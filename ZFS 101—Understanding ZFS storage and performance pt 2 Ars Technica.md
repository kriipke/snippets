# ZFS 101—Understanding ZFS storage and performance pt 2 | Ars Technica
## Copy-on-Write semantics

-   Consider a conventional filesystem, asked to overwrite data in place—it does exactly what you ask it to, and modifies each block just where it lies.
-   Now consider a copy-on-write filesystem asked to modify blocks and files in-place—what it really does is write the new version of the block, then unlink the old version once the new on has been written. Behold, the "data comet!"
-   Now, let's abstract our visualization of copy-on-write—if we ignore the real physical location of blocks, we can simplify our"data comet"to a"data worm" moving from left to right across a map of the available space.
-   Now we can get a good idea of how copy-on-write snapshots work—each block can be owned by multiple snapshots, and will not be unlinked until all referencing snapshots are destroyed.

CoW—Copy on Write—is a fundamental underpinning beneath most of what makes ZFS awesome. The basic concept is simple—if you ask a traditional filesystem to modify a file in-place, it does precisely what you asked it to. If you ask a copy-on-write filesystem to do the same thing, it says "okay"—but it's lying to you.

Instead, the copy-on-write filesystem writes out a new version of the `block` you modified, then updates the file's metadata to unlink the old `block`, and link the new `block` you just wrote.

Unlinking the old `block` and linking in the new is accomplished in a single operation, so it can't be interrupted—if you dump the power after it happens, you have the new version of the file, and if you dump power before, then you have the old version. You're always filesystem-consistent, either way.

Copy-on-write in ZFS isn't only at the filesystem level, it's also at the disk management level. This means that the RAID hole—a condition in which a stripe is only partially written before the system crashes, making the array inconsistent and corrupt after a restart—doesn't affect ZFS. Stripe writes are atomic, the vdev is always consistent, and [Bob's your uncle](https://en.wikipedia.org/wiki/Bob%27s_your_uncle).

### ZIL—the ZFS Intent Log

-   ZFS handles synchronous writes in a special way—it temporarily but immediately stores them in the ZIL, before writing them out permanently along with asynchronous writes later.
-   Normally, data written to the ZIL is never read from again. But it can be replayed out to main storage after a system crash.
-   SLOG, or Secondary LOG device, is just a special—and hopefully very fast—vdev where the ZIL can be kept apart from main storage.
-   After a crash, any dirty data in the ZIL is replayed—in this case, the ZIL is on a SLOG, so that's where it's replayed from.

There are two major categories of write operations—synchronous (sync) and asynchronous (async). For most workloads, the vast majority of write operations are asynchronous—the filesystem is allowed to aggregate them and commit them in batches, reducing fragmentation and tremendously increasing throughput.

Sync writes are an entirely different animal—when an application requests a sync write, it's telling the filesystem"you need to commit this to non volatile storage _now_, and until you do so, I can't do anything else." Sync writes must therefore be committed to disk immediately—and if that increases fragmentation or decreases throughput, so be it.

ZFS handles sync writes differently from normal filesystems—instead of flushing out sync writes to normal storage immediately, ZFS commits them to a special storage area called the ZFS Intent Log, or ZIL. The trick here is, those writes _also_ remain in memory, being aggregated along with normal asynchronous write requests, to later be flushed out to storage as perfectly normal TXGs (Transaction Groups).

In normal operation, the ZIL is written to and never read from again. When writes saved to the ZIL are committed to main storage from RAM in normal TXGs a few moments later, they're unlinked from the ZIL. The only time the ZIL is ever _read_ from is upon pool import.

If ZFS crashes—or the operating system crashes, or there's an unhandled power outage—while there's data in the ZIL, that data will be read from during the next pool import (eg when a crashed system is restarted). Whatever is in the ZIL will be read in, aggregated into TXGs, committed to main storage, and then unlinked from the ZIL during the import process.

One of the classes of support `vdev` available is `LOG`—also known as SLOG, or Secondary LOG device. All the SLOG does is provide the pool with a separate—and hopefully far faster, with very high write endurance—`vdev` to store the `ZIL` in, instead of keeping the `ZIL` on the main storage `vdevs`. In all respects, the `ZIL` behaves the same whether it's on main storage, or on a `LOG` vdev—but if the `LOG` vdev has very high write performance, then sync write returns will happen very quickly.

Adding a `LOG` vdev to a pool absolutely **cannot** and **will not** directly improve asynchronous write performance—even if you force all writes into the ZIL using `zfs set sync=always`, they still get committed to main storage in TXGs in the same way and at the same pace they would have without the `LOG`. The only direct performance improvements are for synchronous write latency (since the `LOG`'s greater speed enables the `sync` call to return faster).

However, in an environment that already requires lots of sync writes, a `LOG` vdev can indirectly accelerate asynchronous writes and uncached reads as well. Offloading ZIL writes to a separate `LOG` vdev means less contention for IOPS on primary storage, thereby increasing performance for all reads and writes to some degree.

### Snapshots

Copy-on-write semantics are also the necessary underpinning for ZFS's atomic snapshots and incremental asynchronous replication. The live filesystem has a tree of [pointers](https://utcc.utoronto.ca/~cks/space/blog/solaris/ZFSBroadDiskStructure) marking all the `records` that contain current data—when you take a snapshot, you simply make a copy of that tree of pointers.

When a record is overwritten in the live filesystem, ZFS writes the new version of the `block` to unused space first. Then it unlinks the old version of the `block` from the current filesystem. But if any `snapshot` references the old `block`, it still remains immutable. The old `block` won't actually be reclaimed as free space until all `snapshots` referencing that `block` have been destroyed!

### Replication

-   My Steam library in 2015 was 158GiB, in 126,927 files. This is pretty close to an optimal situation for rsync—ZFS replication across the network was "only" 750% faster.
-   On the same network, replicating a single 40GiB Windows 7 VM image file is an entirely different story. ZFS replication is 289x faster than rsync—or "only" 161x faster, if you're savvy enough to invoke rsync with --inplace.
-   As the VM image scales up, rsync's problems scale with it. 1.9TiB isn't very large for a modern VM image—but it's large enough for ZFS replication to be 1,148x faster than rsync, even using rsync's --inplace argument.

Once you understand how snapshots work, you're in a good place to understand replication. Since a snapshot is simply a tree of pointers to `records`, it follows that if we `zfs send` a snapshot, we're sending both that tree and all of the associated records. When we pipe that `zfs send` to a `zfs receive` on the target, it writes both the actual `block` contents, and the tree of pointers referencing the `blocks`, into the target dataset.

Things get more interesting on your second `zfs send`. Now that you've got two systems, each containing the snapshot `poolname/datasetname@1`, you can take a new snapshot, `poolname/datasetname@2`. So on the source pool, you have `datasetname@1` and `datasetname@2`, and on the target pool, so far you just have the first snapshot—`datasetname@1`.

Since we have a common snapshot between source and destination—`datasetname@1`—we can build an _incremental_ `zfs send` on top of it. When we ask the system to `zfs send -i poolname/datasetname@1 poolname/datasetname@2`, it compares the two pointer trees. Any pointers which exist only in `@2` obviously reference new `blocks`—so we'll need the contents of those `blocks` as well.

On the remote system, piping in the resulting incremental `send` is similarly easy. First, we write out all the new `records` included in the `send` stream, then we add in the pointers to those `blocks`. Presto, we've got `@2` on the new system!

ZFS asynchronous incremental replication is an enormous improvement over earlier, non-snapshot-based techniques like `rsync`. In both cases, only the changed data needs to get sent over the wire—but `rsync` must first _read_ all the data from disk, on both sides, in order to checksum and compare it. By contrast, ZFS replication doesn't need to read anything but the pointer trees—and any `blocks` that pointer tree contains which _weren't_ already present in the common snapshot.

### Inline compression

Copy-on-write semantics also make it easier to offer inline compression. With a traditional filesystem offering in-place modification, compression is problematic—both the old version and the new version of the modified data must fit in exactly the same space.

If we consider a chunk of data in the middle of a file which begins life as 1MiB of zeroes—`0x00000000` ad nauseam—it would compress down to a single disk sector very easily. But what happens if we replace that 1MiB of zeroes with 1MiB of incompressible data, such as JPEG or pseudo-random noise? Suddenly, that 1MiB of data needs 256 4KiB sectors, not just one—and the hole in the middle of the file is only one sector wide.

ZFS doesn't have this problem, since modified records are always written to unused space—the original `block` only occupies a single 4KiB `sector`, and the new record occupies 256 of them, but that's not an issue—the newly modified chunk from the"middle"of the file would have been written to unused space whether its size changed or not, so for ZFS, this"problem" is just another day at the office.

ZFS's inline compression is off by default, and it offers pluggable algorithms—currently including LZ4, gzip (1-9), LZJB, and ZLE.

-   **LZ4** is a stream algorithm offering extremely rapid compression and decompression, and is a performance win for the majority of use cases—even with very anemic CPUs.
-   **GZIP**is the venerable algorithm all Unix-like users know and love. It can be implemented with compression levels 1-9, with increasing compression ratio and CPU usage as levels approach 9. Gzip may be a win for all-text (or otherwise extremely compressible) use-cases, but frequently results in CPU bottlenecks otherwise—use with caution, particularly at higher levels.
-   **LZJB**is the original algorithm used by ZFS. It is deprecated, and should no longer be used—LZ4 is superior in every metric.
-   **ZLE** is Zero Level Encoding—it leaves normal data alone entirely, but will compress large sequences of zeroes. Useful for entirely incompressible datasets (eg JPEG, MP4, or other already-compressed formats), since it ignores the incompressible data, but compresses slack space on final records.

We recommend LZ4 compression for nearly any conceivable use-case; the performance penalty when it encounters incompressible data is very small, and the performance _gain_ for typical data is significant. Copying a VM image for a new Windows operating system installation (just the installed Windows operating system, no data on it yet) went 27% faster with `compression=lz4` than `compression=none` in [this 2015 test](https://jrs-s.net/2015/02/24/zfs-compression-yes-you-want-this/).

## ARC—the Adaptive Replacement Cache

ZFS is the only modern filesystem we know of which uses its own read cache mechanism, rather than relying on its operating system's page cache to keep copies of recently-read blocks in RAM for it.

Although the separate cache mechanism has its problems—ZFS cannot react to new requests to allocate memory as immediately as the kernel can, and therefore a new `mallocate()` call may fail, if it would need RAM currently occupied by the ARC—there's good reason, at least for now, for putting up with it.

Every well-known modern operating system—including MacOS, Windows, Linux, and BSD—uses the LRU (Least Recently Used) algorithm for its page cache implementation. LRU is a naive algorithm that bumps a cached block up to the "top" of the queue each time it's read, and evicts blocks from the"bottom"of the queue as necessary to add new cache misses (blocks which had to be read from disk, rather than cache) at the"top."

This is fine so far as it goes, but in systems with large working data sets, the LRU can easily end up "thrashing"—evicting very frequently-needed blocks, to make room for blocks that will never get read from cache again.

The ARC is a much less naive [algorithm](https://en.wikipedia.org/wiki/Adaptive_replacement_cache), which can be thought of as a "weighted" cache. Each time a cached block is read, it becomes a little "heavier" and more difficult to evict—and even after an eviction, the evicted block is _tracked_ for a period of time. A block which has been evicted but then must be read back into cache will also become "heavier" and more difficult to evict.

The end result of all this is a cache with typically far greater hit ratios—the ratio between cache hits (reads served from cache) and cache misses (reads served from disk). This is an extremely important statistic—not only are cache hits themselves served orders of magnitude more rapidly, the cache _misses_ can also be served more rapidly, since more cache hits==fewer concurrent requests to disk==lower latency for those remaining misses which _must_ be serviced from disk.

## Conclusion

Now that we've covered the basic semantics of ZFS—how copy-on-write works, and the relationships between pools, vdevs, blocks, sectors and files—we're ready to talk actual performance, with real numbers.

Stay tuned for the next installment of our storage fundamentals series to see the actual performance seen in pools using mirror and RAIDz vdevs, as compared with both one another and the traditional Linux kernel RAID topologies we explored [earlier](https://arstechnica.com/information-technology/2020/04/understanding-raid-how-performance-scales-from-one-disk-to-eight/).

Initially, we're just going to cover the basics—the ZFS topologies themselves—but after _that_, we'll be ready to talk about more advanced ZFS setup and tuning, including the use of support vdev types like L2ARC, SLOG, and Special Allocation. 
 [https://arstechnica.com/information-technology/2020/05/zfs-101-understanding-zfs-storage-and-performance/](https://arstechnica.com/information-technology/2020/05/zfs-101-understanding-zfs-storage-and-performance/)

 [https://arstechnica.com/information-technology/2020/05/zfs-101-understanding-zfs-storage-and-performance/](https://arstechnica.com/information-technology/2020/05/zfs-101-understanding-zfs-storage-and-performance/)

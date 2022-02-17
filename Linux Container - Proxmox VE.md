# Linux Container - Proxmox VE
 Linux Container - Proxmox VE        

# Linux Container

From Proxmox VE

[Jump to navigation](#mw-head) [Jump to search](#searchInput)

## Contents

-   [Technology Overview](#_technology_overview)
-   [Supported Distributions](#pct_supported_distributions)
-   [Container Images](#pct_container_images)
-   [Container Settings](#pct_settings)
-   [Security Considerations](#_security_considerations)
-   [Guest Operating System Configuration](#_guest_operating_system_configuration)
-   [Container Storage](#pct_container_storage)
-   [Backup and Restore](#_backup_and_restore)
-   [Managing Containers with pct](#_managing_containers_with_tt_span_class_monospaced_pct_span_tt)
-   [Migration](#pct_migration)
-   [Configuration](#pct_configuration)
-   [Locks](#_locks)

Containers are a lightweight alternative to fully virtualized machines (VMs). They use the kernel of the host system that they run on, instead of emulating a full operating system (OS). This means that containers can access resources on the host system directly.

The runtime costs for containers is low, usually negligible. However, there are some drawbacks that need be considered:

-   Only Linux distributions can be run in Proxmox Containers. It is not possible to run other operating systems like, for example, FreeBSD or Microsoft Windows inside a container.
-   For security reasons, access to host resources needs to be restricted. Therefore, containers run in their own separate namespaces. Additionally some syscalls (user space requests to the Linux kernel) are not allowed within containers.

Proxmox VE uses [Linux Containers (LXC)](https://linuxcontainers.org/lxc/introduction/) as its underlying container technology. The “Proxmox Container Toolkit” (pct) simplifies the usage and management of LXC, by providing an interface that abstracts complex tasks.

Containers are tightly integrated with Proxmox VE. This means that they are aware of the cluster setup, and they can use the same network and storage resources as virtual machines. You can also use the Proxmox VE firewall, or manage containers using the HA framework.

Our primary goal is to offer an environment that provides the benefits of using a VM, but without the additional overhead. This means that Proxmox Containers can be categorized as “System Containers”, rather than “Application Containers”.

| !\[](data:image/png;base64,
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)
 | If you want to run application containers, for example, _Docker_ images, it is recommended that you run them inside a Proxmox Qemu VM. This will give you all the advantages of application containerization, while also providing the benefits that VMs offer, such as strong isolation from the host and the ability to live-migrate, which otherwise isn’t possible with containers. |

## Technology Overview

-   LXC ([https://linuxcontainers.org/](https://linuxcontainers.org/))
-   Integrated into Proxmox VE graphical web user interface (GUI)
-   Easy to use command line tool pct
-   Access via Proxmox VE REST API
-   _lxcfs_ to provide containerized /proc file system
-   Control groups (_cgroups_) for resource isolation and limitation
-   _AppArmor_ and _seccomp_ to improve security
-   Modern Linux kernels
-   Image based deployment ([templates](/wiki/Linux_Container#pct_supported_distributions))
-   Uses Proxmox VE [storage library](/wiki/Storage#chapter_storage)
-   Container setup from host (network, DNS, storage, etc.)

## Supported Distributions

List of officially supported distributions can be found below.

Templates for the following distributions are available through our repositories. You can use [pveam](/wiki/Linux_Container#pct_container_images) tool or the Graphical User Interface to download them.

### Alpine Linux

Alpine Linux is a security-oriented, lightweight Linux distribution based on musl libc and busybox.

— [https://alpinelinux.org](https://alpinelinux.org)

For currently supported releases see:

[https://alpinelinux.org/releases/](https://alpinelinux.org/releases/)

### Arch Linux

Arch Linux, a lightweight and flexible Linux® distribution that tries to Keep It Simple.

— [https://archlinux.org/](https://archlinux.org/)

Arch Linux is using a rolling-release model, see its wiki for more details:

[https://wiki.archlinux.org/title/Arch_Linux](https://wiki.archlinux.org/title/Arch_Linux)

### CentOS, Almalinux, Rocky Linux

#### CentOS / CentOS Stream

The CentOS Linux distribution is a stable, predictable, manageable and reproducible platform derived from the sources of Red Hat Enterprise Linux (RHEL)

— [https://centos.org](https://centos.org)

For currently supported releases see:

[https://wiki.centos.org/About/Product](https://wiki.centos.org/About/Product)

#### Almalinux

An Open Source, community owned and governed, forever-free enterprise Linux distribution, focused on long-term stability, providing a robust production-grade platform. AlmaLinux OS is 1:1 binary compatible with RHEL® and pre-Stream CentOS.

— [https://almalinux.org](https://almalinux.org)

For currently supported releases see:

[https://en.wikipedia.org/wiki/AlmaLinux#Releases](https://en.wikipedia.org/wiki/AlmaLinux#Releases)

#### Rocky Linux

Rocky Linux is a community enterprise operating system designed to be 100% bug-for-bug compatible with America’s top enterprise Linux distribution now that its downstream partner has shifted direction.

— [https://rockylinux.org](https://rockylinux.org)

For currently supported releases see:

[https://en.wikipedia.org/wiki/Rocky_Linux#Releases](https://en.wikipedia.org/wiki/Rocky_Linux#Releases)

### Debian

Debian is a free operating system, developed and maintained by the Debian project. A free Linux distribution with thousands of applications to meet our users' needs.

— [https://www.debian.org/intro/index#software](https://www.debian.org/intro/index#software)

For currently supported releases see:

[https://www.debian.org/releases/stable/releasenotes](https://www.debian.org/releases/stable/releasenotes)

### Devuan

Devuan GNU+Linux is a fork of Debian without systemd that allows users to reclaim control over their system by avoiding unnecessary entanglements and ensuring Init Freedom.

— [https://www.devuan.org](https://www.devuan.org)

For currently supported releases see:

[https://www.devuan.org/os/releases](https://www.devuan.org/os/releases)

### Fedora

Fedora creates an innovative, free, and open source platform for hardware, clouds, and containers that enables software developers and community members to build tailored solutions for their users.

— [https://getfedora.org](https://getfedora.org)

For currently supported releases see:

[https://fedoraproject.org/wiki/Releases](https://fedoraproject.org/wiki/Releases)

### Gentoo

a highly flexible, source-based Linux distribution.

— [https://www.gentoo.org](https://www.gentoo.org)

Gentoo is using a rolling-release model.

### OpenSUSE

The makers' choice for sysadmins, developers and desktop users.

— [https://www.opensuse.org](https://www.opensuse.org)

For currently supported releases see:

[https://get.opensuse.org/leap/](https://get.opensuse.org/leap/)

### Ubuntu

Ubuntu is the modern, open source operating system on Linux for the enterprise server, desktop, cloud, and IoT.

— [https://ubuntu.com/](https://ubuntu.com/)

For currently supported releases see:

[https://wiki.ubuntu.com/Releases](https://wiki.ubuntu.com/Releases)

## Container Images

Container images, sometimes also referred to as “templates” or “appliances”, are tar archives which contain everything to run a container.

Proxmox VE itself provides a variety of basic templates for the [most common Linux distributions](/wiki/Linux_Container#pct_supported_distributions). They can be downloaded using the GUI or the pveam (short for Proxmox VE Appliance Manager) command line utility. Additionally, [TurnKey Linux](https://www.turnkeylinux.org/) container templates are also available to download.

The list of available templates is updated daily through the _pve-daily-update_ timer. You can also trigger an update manually by executing:

\# pveam update

To view the list of available images run:

\# pveam available

You can restrict this large list by specifying the section you are interested in, for example basic system images:

List available system images

    \# pveam available --section system
    system          alpine-3.12-default\_20200823\_amd64.tar.xz
    system          alpine-3.13-default\_20210419\_amd64.tar.xz
    system          alpine-3.14-default\_20210623\_amd64.tar.xz
    system          archlinux-base\_20210420-1\_amd64.tar.gz
    system          centos-7-default\_20190926\_amd64.tar.xz
    system          centos-8-default\_20201210\_amd64.tar.xz
    system          debian-9.0-standard\_9.7-1\_amd64.tar.gz
    system          debian-10-standard\_10.7-1\_amd64.tar.gz
    system          devuan-3.0-standard\_3.0\_amd64.tar.gz
    system          fedora-33-default\_20201115\_amd64.tar.xz
    system          fedora-34-default\_20210427\_amd64.tar.xz
    system          gentoo-current-default\_20200310\_amd64.tar.xz
    system          opensuse-15.2-default\_20200824\_amd64.tar.xz
    system          ubuntu-16.04-standard\_16.04.5-1\_amd64.tar.gz
    system          ubuntu-18.04-standard\_18.04.1-1\_amd64.tar.gz
    system          ubuntu-20.04-standard\_20.04-1\_amd64.tar.gz
    system          ubuntu-20.10-standard\_20.10-1\_amd64.tar.gz
    system          ubuntu-21.04-standard\_21.04-1\_amd64.tar.gz

Before you can use such a template, you need to download them into one of your storages. If you’re unsure to which one, you can simply use the local named storage for that purpose. For clustered installations, it is preferred to use a shared storage so that all nodes can access those images.

\# pveam download local debian-10.0-standard_10.0-1_amd64.tar.gz

You are now ready to create containers using that image, and you can list all downloaded images on storage local with:

\# pveam list local
local:vztmpl/debian-10.0-standard_10.0-1_amd64.tar.gz  219.95MB

| !\[](data:image/png;base64,
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)
 | You can also use the Proxmox VE web interface GUI to download, list and delete container templates. |

pct uses them to create a new container, for example:

\# pct create 999 local:vztmpl/debian-10.0-standard_10.0-1_amd64.tar.gz

The above command shows you the full Proxmox VE volume identifiers. They include the storage name, and most other Proxmox VE commands can use them. For example you can delete that image later with:

\# pveam remove local:vztmpl/debian-10.0-standard_10.0-1_amd64.tar.gz

## Container Settings

### General Settings

[![](https://pve.proxmox.com/pve-docs/images/screenshot/gui-create-ct-general.png)
](/pve-docs/images/screenshot/gui-create-ct-general.png)

General settings of a container include

-   the **Node** : the physical server on which the container will run
-   the **CT ID**: a unique number in this Proxmox VE installation used to identify your container
-   **Hostname**: the hostname of the container
-   **Resource Pool**: a logical group of containers and VMs
-   **Password**: the root password of the container
-   **SSH Public Key**: a public key for connecting to the root account over SSH
-   **Unprivileged container**: this option allows to choose at creation time if you want to create a privileged or unprivileged container.

#### Unprivileged Containers

Unprivileged containers use a new kernel feature called user namespaces. The root UID 0 inside the container is mapped to an unprivileged user outside the container. This means that most security issues (container escape, resource abuse, etc.) in these containers will affect a random unprivileged user, and would be a generic kernel security bug rather than an LXC issue. The LXC team thinks unprivileged containers are safe by design.

This is the default option when creating a new container.

| !\[](data:image/png;base64,
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)
 | If the container uses systemd as an init system, please be aware the systemd version running inside the container should be equal to or greater than 220. |

#### Privileged Containers

Security in containers is achieved by using mandatory access control _AppArmor_ restrictions, _seccomp_ filters and Linux kernel namespaces. The LXC team considers this kind of container as unsafe, and they will not consider new container escape exploits to be security issues worthy of a CVE and quick fix. That’s why privileged containers should only be used in trusted environments.

### CPU

[![](https://pve.proxmox.com/pve-docs/images/screenshot/gui-create-ct-cpu.png)
](/pve-docs/images/screenshot/gui-create-ct-cpu.png)

You can restrict the number of visible CPUs inside the container using the cores option. This is implemented using the Linux _cpuset_ cgroup (**c**ontrol **group**). A special task inside pvestatd tries to distribute running containers among available CPUs periodically. To view the assigned CPUs run the following command:

\# pct cpusets

* * *

 102:              6 7
 105:      2 3 4 5
 108:  0 1

* * *

Containers use the host kernel directly. All tasks inside a container are handled by the host CPU scheduler. Proxmox VE uses the Linux _CFS_ (**C**ompletely **F**air **S**cheduler) scheduler by default, which has additional bandwidth control options.

| cpulimit:  
 \| 

You can use this option to further limit assigned CPU time. Please note that this is a floating point number, so it is perfectly valid to assign two cores to a container, but restrict overall CPU consumption to half a core.

cores: 2
cpulimit: 0.5

 \|
| cpuunits:  
 \| 

This is a relative weight passed to the kernel scheduler. The larger the number is, the more CPU time this container gets. Number is relative to the weights of all the other running containers. The default is 1024. You can use this setting to prioritize some containers.

 \|

### Memory

[![](https://pve.proxmox.com/pve-docs/images/screenshot/gui-create-ct-memory.png)
](/pve-docs/images/screenshot/gui-create-ct-memory.png)

Container memory is controlled using the cgroup memory controller.

| memory:  
 \| 

Limit overall memory usage. This corresponds to the memory.limit_in_bytes cgroup setting.

 \|
| swap:  
 \| 

Allows the container to use additional swap memory from the host swap space. This corresponds to the memory.memsw.limit_in_bytes cgroup setting, which is set to the sum of both value (memory + swap).

 \|

### Mount Points

[![](https://pve.proxmox.com/pve-docs/images/screenshot/gui-create-ct-root-disk.png)
](/pve-docs/images/screenshot/gui-create-ct-root-disk.png)

The root mount point is configured with the rootfs property. You can configure up to 256 additional mount points. The corresponding options are called mp0 to mp255. They can contain the following settings:

rootfs: \[volume=]<volume> \[,acl=&lt;1|0>] \[,mountoptions=&lt;opt\[;opt...]>] \[,quota=&lt;1|0>] \[,replicate=&lt;1|0>] \[,ro=&lt;1|0>] \[,shared=&lt;1|0>] \[,size=<DiskSize>]

Use volume as container root. See below for a detailed description of all options.

mp\[n]: \[volume=]<volume> ,mp=<Path> \[,acl=&lt;1|0>] \[,backup=&lt;1|0>] \[,mountoptions=&lt;opt\[;opt...]>] \[,quota=&lt;1|0>] \[,replicate=&lt;1|0>] \[,ro=&lt;1|0>] \[,shared=&lt;1|0>] \[,size=<DiskSize>]

Use volume as container mount point. Use the special syntax STORAGE_ID:SIZE_IN_GiB to allocate a new volume.

acl=<boolean>

Explicitly enable or disable ACL support.

backup=<boolean>

Whether to include the mount point in backups (only used for volume mount points).

mountoptions=&lt;opt\[;opt...]>

Extra mount options for rootfs/mps.

mp=<Path>

Path to the mount point as seen from inside the container.

| !\[](data:image/png;base64,
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)
 | Must not contain any symlinks for security reasons. |

quota=<boolean>

Enable user quotas inside the container (not supported with zfs subvolumes)

replicate=<boolean> (_default =_ 1)

Will include this volume to a storage replica job.

ro=<boolean>

Read-only mount point

shared=<boolean> (_default =_ 0)

Mark this non-volume mount point as available on all nodes.

| !\[](data:image/png;base64,
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
/F/lgJiyQFHragAAAABJRU5ErkJggg==)
 | This option does not share the mount point automatically, it assumes it is shared already! |

size=<DiskSize>

Volume size (read only value).

volume=<volume>

Volume, device or directory to mount into the container.

Currently there are three types of mount points: storage backed mount points, bind mounts, and device mounts.

Typical container rootfs configuration

rootfs: thin1:base-100-disk-1,size=8G

#### Storage Backed Mount Points

Storage backed mount points are managed by the Proxmox VE storage subsystem and come in three different flavors:

-   Image based: these are raw images containing a single ext4 formatted file system.
-   ZFS subvolumes: these are technically bind mounts, but with managed storage, and thus allow resizing and snapshotting.
-   Directories: passing size=0 triggers a special case where instead of a raw image a directory is created.

| !\[](data:image/png;base64,
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)
 | The special option syntax STORAGE_ID:SIZE_IN_GB for storage backed mount point volumes will automatically allocate a volume of the specified size on the specified storage. For example, calling |

pct set 100 -mp0 thin1:10,mp=/path/in/container

will allocate a 10GB volume on the storage thin1 and replace the volume ID place holder 10 with the allocated volume ID, and setup the moutpoint in the container at /path/in/container

#### Bind Mount Points

Bind mounts allow you to access arbitrary directories from your Proxmox VE host inside a container. Some potential use cases are:

-   Accessing your home directory in the guest
-   Accessing an USB device directory in the guest
-   Accessing an NFS mount from the host in the guest

Bind mounts are considered to not be managed by the storage subsystem, so you cannot make snapshots or deal with quotas from inside the container. With unprivileged containers you might run into permission problems caused by the user mapping and cannot use ACLs.

| !\[](data:image/png;base64,
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)
 | The contents of bind mount points are not backed up when using vzdump. |

| !\[](data:image/png;base64,
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
/F/lgJiyQFHragAAAABJRU5ErkJggg==)
 | For security reasons, bind mounts should only be established using source directories especially reserved for this purpose, e.g., a directory hierarchy under /mnt/bindmounts. Never bind mount system directories like /, /var or /etc into a container - this poses a great security risk. |

| !\[](data:image/png;base64,
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)
 | The bind mount source path must not contain any symlinks. |

For example, to make the directory /mnt/bindmounts/shared accessible in the container with ID 100 under the path /shared, use a configuration line like mp0: /mnt/bindmounts/shared,mp=/shared in /etc/pve/lxc/100.conf. Alternatively, use pct set 100 -mp0 /mnt/bindmounts/shared,mp=/shared to achieve the same result.

#### Device Mount Points

Device mount points allow to mount block devices of the host directly into the container. Similar to bind mounts, device mounts are not managed by Proxmox VE’s storage subsystem, but the quota and acl options will be honored.

| !\[](data:image/png;base64,
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)
 | Device mount points should only be used under special circumstances. In most cases a storage backed mount point offers the same performance and a lot more features. |

| !\[](data:image/png;base64,
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)
 | The contents of device mount points are not backed up when using vzdump. |

### Network

[![](https://pve.proxmox.com/pve-docs/images/screenshot/gui-create-ct-network.png)
](/pve-docs/images/screenshot/gui-create-ct-network.png)

You can configure up to 10 network interfaces for a single container. The corresponding options are called net0 to net9, and they can contain the following setting:

net\[n]: name=<string> \[,bridge=<bridge>] \[,firewall=&lt;1|0>] \[,gw=<GatewayIPv4>] \[,gw6=<GatewayIPv6>] \[,hwaddr=&lt;XX:XX:XX:XX:XX:XX>] \[,ip=&lt;(IPv4/CIDR|dhcp|manual)>] \[,ip6=&lt;(IPv6/CIDR|auto|dhcp|manual)>] \[,mtu=<integer>] \[,rate=<mbps>] \[,tag=<integer>] \[,trunks=&lt;vlanid\[;vlanid...]>] \[,type=<veth>]

Specifies network interfaces for the container.

bridge=<bridge>

Bridge to attach the network device to.

firewall=<boolean>

Controls whether this interface’s firewall rules should be used.

gw=<GatewayIPv4>

Default gateway for IPv4 traffic.

gw6=<GatewayIPv6>

Default gateway for IPv6 traffic.

hwaddr=&lt;XX:XX:XX:XX:XX:XX>

A common MAC address with the I/G (Individual/Group) bit not set.

ip=&lt;(IPv4/CIDR|dhcp|manual)>

IPv4 address in CIDR format.

ip6=&lt;(IPv6/CIDR|auto|dhcp|manual)>

IPv6 address in CIDR format.

mtu=<integer> (64 - N)

Maximum transfer unit of the interface. (lxc.network.mtu)

name=<string>

Name of the network device as seen from inside the container. (lxc.network.name)

rate=<mbps>

Apply rate limiting to the interface

tag=<integer> (1 - 4094)

VLAN tag for this interface.

trunks=&lt;vlanid\[;vlanid...]>

VLAN ids to pass through the interface

type=<veth>

Network interface type.

### Automatic Start and Shutdown of Containers

To automatically start a container when the host system boots, select the option _Start at boot_ in the _Options_ panel of the container in the web interface or run the following command:

\# pct set CTID -onboot 1

[![](https://pve.proxmox.com/pve-docs/images/screenshot/gui-qemu-edit-start-order.png)
](/pve-docs/images/screenshot/gui-qemu-edit-start-order.png)

##### Start and Shutdown Order

If you want to fine tune the boot order of your containers, you can use the following parameters:

-   **Start/Shutdown order**: Defines the start order priority. For example, set it to 1 if you want the CT to be the first to be started. (We use the reverse startup order for shutdown, so a container with a start order of 1 would be the last to be shut down)
-   **Startup delay**: Defines the interval between this container start and subsequent containers starts. For example, set it to 240 if you want to wait 240 seconds before starting other containers.
-   **Shutdown timeout**: Defines the duration in seconds Proxmox VE should wait for the container to be offline after issuing a shutdown command. By default this value is set to 60, which means that Proxmox VE will issue a shutdown request, wait 60s for the machine to be offline, and if after 60s the machine is still online will notify that the shutdown action failed.

Please note that containers without a Start/Shutdown order parameter will always start after those where the parameter is set, and this parameter only makes sense between the machines running locally on a host, and not cluster-wide.

If you require a delay between the host boot and the booting of the first container, see the section on [Proxmox VE Node Management](/wiki/Proxmox_Node_Management#first_guest_boot_delay).

### Hookscripts

You can add a hook script to CTs with the config property hookscript.

\# pct set 100 -hookscript local:snippets/hookscript.pl

It will be called during various phases of the guests lifetime. For an example and documentation see the example script under /usr/share/pve-docs/examples/guest-example-hookscript.pl.

## Security Considerations

Containers use the kernel of the host system. This exposes an attack surface for malicious users. In general, full virtual machines provide better isolation. This should be considered if containers are provided to unknown or untrusted people.

To reduce the attack surface, LXC uses many security features like AppArmor, CGroups and kernel namespaces.

### AppArmor

AppArmor profiles are used to restrict access to possibly dangerous actions. Some system calls, i.e. mount, are prohibited from execution.

To trace AppArmor activity, use:

\# dmesg | grep apparmor

Although it is not recommended, AppArmor can be disabled for a container. This brings security risks with it. Some syscalls can lead to privilege escalation when executed within a container if the system is misconfigured or if a LXC or Linux Kernel vulnerability exists.

To disable AppArmor for a container, add the following line to the container configuration file located at /etc/pve/lxc/CTID.conf:

lxc.apparmor.profile = unconfined

| !\[](data:image/png;base64,
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
/F/lgJiyQFHragAAAABJRU5ErkJggg==)
 | Please note that this is not recommended for production use. |

### Control Groups (_cgroup_)

_cgroup_ is a kernel mechanism used to hierarchically organize processes and distribute system resources.

The main resources controlled via _cgroups_ are CPU time, memory and swap limits, and access to device nodes. _cgroups_ are also used to "freeze" a container before taking snapshots.

There are 2 versions of _cgroups_ currently available, [legacy](https://www.kernel.org/doc/html/v5.11/admin-guide/cgroup-v1/index.html) and [_cgroupv2_](https://www.kernel.org/doc/html/v5.11/admin-guide/cgroup-v2.html).

Since Proxmox VE 7.0, the default is a pure _cgroupv2_ environment. Previously a "hybrid" setup was used, where resource control was mainly done in _cgroupv1_ with an additional _cgroupv2_ controller which could take over some subsystems via the _cgroup_no_v1_ kernel command line parameter. (See the [kernel parameter documentation](https://www.kernel.org/doc/html/latest/admin-guide/kernel-parameters.html) for details.)

#### CGroup Version Compatibility

The main difference between pure _cgroupv2_ and the old hybrid environments regarding Proxmox VE is that with _cgroupv2_ memory and swap are now controlled independently. The memory and swap settings for containers can map directly to these values, whereas previously only the memory limit and the limit of the **sum** of memory and swap could be limited.

Another important difference is that the _devices_ controller is configured in a completely different way. Because of this, file system quotas are currently not supported in a pure _cgroupv2_ environment.

_cgroupv2_ support by the container’s OS is needed to run in a pure _cgroupv2_ environment. Containers running _systemd_ version 231 or newer support _cgroupv2_ \[[1](#_footnote_1 "View footnote")], as do containers not using _systemd_ as init system \[[2](#_footnote_2 "View footnote")].

| !\[](data:image/png;base64,
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)
 \| 

CentOS 7 and Ubuntu 16.10 are two prominent Linux distributions releases, which have a _systemd_ version that is too old to run in a _cgroupv2_ environment, you can either

-   Upgrade the whole distribution to a newer release. For the examples above, that could be Ubuntu 18.04 or 20.04, and CentOS 8 (or RHEL/CentOS derivatives like AlmaLinux or Rocky Linux). This has the benefit to get the newest bug and security fixes, often also new features, and moving the EOL date in the future.
-   Upgrade the Containers systemd version. If the distribution provides a backports repository this can be an easy and quick stop-gap measurement.
-   Move the container, or its services, to a Virtual Machine. Virtual Machines have a much less interaction with the host, that’s why one can install decades old OS versions just fine there.
-   Switch back to the legacy _cgroup_ controller. Note that while it can be a valid solution, it’s not a permanent one. There’s a high likelihood that a future Proxmox VE major release, for example 8.0, cannot support the legacy controller anymore.

 \|

#### Changing CGroup Version

| !\[](data:image/png;base64,
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)
 | If file system quotas are not required and all containers support _cgroupv2_, it is recommended to stick to the new default. |

To switch back to the previous version the following kernel command line parameter can be used:

systemd.unified_cgroup_hierarchy=0

See [this section](/wiki/Host_Bootloader#sysboot_edit_kernel_cmdline) on editing the kernel boot command line on where to add the parameter.

## Guest Operating System Configuration

Proxmox VE tries to detect the Linux distribution in the container, and modifies some files. Here is a short list of things done at container startup:

set /etc/hostname

to set the container name

modify /etc/hosts

to allow lookup of the local hostname

network setup

pass the complete network setup to the container

configure DNS

pass information about DNS servers

adapt the init system

for example, fix the number of spawned getty processes

set the root password

when creating a new container

rewrite ssh_host_keys

so that each container has unique keys

randomize crontab

so that cron does not start at the same time on all containers

Changes made by Proxmox VE are enclosed by comment markers:

\# --- BEGIN PVE ---
<data>
\# --- END PVE ---

Those markers will be inserted at a reasonable location in the file. If such a section already exists, it will be updated in place and will not be moved.

Modification of a file can be prevented by adding a .pve-ignore. file for it. For instance, if the file /etc/.pve-ignore.hosts exists then the /etc/hosts file will not be touched. This can be a simple empty file created via:

\# touch /etc/.pve-ignore.hosts

Most modifications are OS dependent, so they differ between different distributions and versions. You can completely disable modifications by manually setting the ostype to unmanaged.

OS type detection is done by testing for certain files inside the container. Proxmox VE first checks the /etc/os-release file \[[3](#_footnote_3 "View footnote")]. If that file is not present, or it does not contain a clearly recognizable distribution identifier the following distribution specific release files are checked.

Ubuntu

inspect /etc/lsb-release (DISTRIB_ID=Ubuntu)

Debian

test /etc/debian_version

Fedora

test /etc/fedora-release

RedHat or CentOS

test /etc/redhat-release

ArchLinux

test /etc/arch-release

Alpine

test /etc/alpine-release

Gentoo

test /etc/gentoo-release

| !\[](data:image/png;base64,
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)
 | Container start fails if the configured ostype differs from the auto detected type. |

## Container Storage

The Proxmox VE LXC container storage model is more flexible than traditional container storage models. A container can have multiple mount points. This makes it possible to use the best suited storage for each application.

For example the root file system of the container can be on slow and cheap storage while the database can be on fast and distributed storage via a second mount point. See section [Mount Points](/wiki/Linux_Container#pct_mount_points) for further details.

Any storage type supported by the Proxmox VE storage library can be used. This means that containers can be stored on local (for example lvm, zfs or directory), shared external (like iSCSI, NFS) or even distributed storage systems like Ceph. Advanced storage features like snapshots or clones can be used if the underlying storage supports them. The vzdump backup tool can use snapshots to provide consistent container backups.

Furthermore, local devices or local directories can be mounted directly using _bind mounts_. This gives access to local resources inside a container with practically zero overhead. Bind mounts can be used as an easy way to share data between containers.

### FUSE Mounts

| !\[](data:image/png;base64,
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
/F/lgJiyQFHragAAAABJRU5ErkJggg==)
 | Because of existing issues in the Linux kernel’s freezer subsystem the usage of FUSE mounts inside a container is strongly advised against, as containers need to be frozen for suspend or snapshot mode backups. |

If FUSE mounts cannot be replaced by other mounting mechanisms or storage technologies, it is possible to establish the FUSE mount on the Proxmox host and use a bind mount point to make it accessible inside the container.

### Using Quotas Inside Containers

Quotas allow to set limits inside a container for the amount of disk space that each user can use.

| !\[](data:image/png;base64,
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)
 | This currently requires the use of legacy _cgroups_. |

| !\[](data:image/png;base64,
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)
 | This only works on ext4 image based storage types and currently only works with privileged containers. |

Activating the quota option causes the following mount options to be used for a mount point: usrjquota=aquota.user,grpjquota=aquota.group,jqfmt=vfsv0

This allows quotas to be used like on any other system. You can initialize the /aquota.user and /aquota.group files by running:

\# quotacheck -cmug /
\# quotaon /

Then edit the quotas using the edquota command. Refer to the documentation of the distribution running inside the container for details.

| !\[](data:image/png;base64,
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)
 | You need to run the above commands for every mount point by passing the mount point’s path instead of just /. |

### Using ACLs Inside Containers

The standard Posix **A**ccess **C**ontrol **L**ists are also available inside containers. ACLs allow you to set more detailed file ownership than the traditional user/group/others model.

### Backup of Container mount points

To include a mount point in backups, enable the backup option for it in the container configuration. For an existing mount point mp0

mp0: guests:subvol-100-disk-1,mp=/root/files,size=8G

add backup=1 to enable it.

mp0: guests:subvol-100-disk-1,mp=/root/files,size=8G,backup=1

| !\[](data:image/png;base64,
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)
 | When creating a new mount point in the GUI, this option is enabled by default. |

To disable backups for a mount point, add backup=0 in the way described above, or uncheck the **Backup** checkbox on the GUI.

### Replication of Containers mount points

By default, additional mount points are replicated when the Root Disk is replicated. If you want the Proxmox VE storage replication mechanism to skip a mount point, you can set the **Skip replication** option for that mount point. As of Proxmox VE 5.0, replication requires a storage of type zfspool. Adding a mount point to a different type of storage when the container has replication configured requires to have **Skip replication** enabled for that mount point.

## Backup and Restore

### Container Backup

It is possible to use the vzdump tool for container backup. Please refer to the vzdump manual page for details.

### Restoring Container Backups

Restoring container backups made with vzdump is possible using the pct restore command. By default, pct restore will attempt to restore as much of the backed up container configuration as possible. It is possible to override the backed up configuration by manually setting container options on the command line (see the pct manual page for details).

| !\[](data:image/png;base64,
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)
 | pvesm extractconfig can be used to view the backed up configuration contained in a vzdump archive. |

There are two basic restore modes, only differing by their handling of mount points:

#### “Simple” Restore Mode

If neither the rootfs parameter nor any of the optional mpX parameters are explicitly set, the mount point configuration from the backed up configuration file is restored using the following steps:

1.  Extract mount points and their options from backup
2.  Create volumes for storage backed mount points on the storage provided with the storage parameter (default: local).
3.  Extract files from backup archive
4.  Add bind and device mount points to restored configuration (limited to root user)

| !\[](data:image/png;base64,
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)
 | Since bind and device mount points are never backed up, no files are restored in the last step, but only the configuration options. The assumption is that such mount points are either backed up with another mechanism (e.g., NFS space that is bind mounted into many containers), or not intended to be backed up at all. |

This simple mode is also used by the container restore operations in the web interface.

#### “Advanced” Restore Mode

By setting the rootfs parameter (and optionally, any combination of mpX parameters), the pct restore command is automatically switched into an advanced mode. This advanced mode completely ignores the rootfs and mpX configuration options contained in the backup archive, and instead only uses the options explicitly provided as parameters.

This mode allows flexible configuration of mount point settings at restore time, for example:

-   Set target storages, volume sizes and other options for each mount point individually
-   Redistribute backed up files according to new mount point scheme
-   Restore to device and/or bind mount points (limited to root user)

## Managing Containers with pct

The “Proxmox Container Toolkit” (pct) is the command line tool to manage Proxmox VE containers. It enables you to create or destroy containers, as well as control the container execution (start, stop, reboot, migrate, etc.). It can be used to set parameters in the config file of a container, for example the network configuration or memory limits.

### CLI Usage Examples

Create a container based on a Debian template (provided you have already downloaded the template via the web interface)

\# pct create 100 /var/lib/vz/template/cache/debian-10.0-standard_10.0-1_amd64.tar.gz

Start container 100

\# pct start 100

Start a login session via getty

\# pct console 100

Enter the LXC namespace and run a shell as root user

\# pct enter 100

Display the configuration

\# pct config 100

Add a network interface called eth0, bridged to the host bridge vmbr0, set the address and gateway, while it’s running

\# pct set 100 -net0 name=eth0,bridge=vmbr0,ip=192.168.15.147/24,gw=192.168.15.1

Reduce the memory of the container to 512MB

\# pct set 100 -memory 512

Destroying a container always removes it from Access Control Lists and it always removes the firewall configuration of the container. You have to activate _--purge_, if you want to additionally remove the container from replication jobs, backup jobs and HA resource configurations.

\# pct destroy 100 --purge

Move a mount point volume to a different storage.

\# pct move-volume 100 mp0 other-storage

Reassign a volume to a different CT. This will remove the volume mp0 from the source CT and attaches it as mp1 to the target CT. In the background the volume is being renamed so that the name matches the new owner.

\#  pct move-volume 100 mp0 --target-vmid 200 --target-volume mp1

### Obtaining Debugging Logs

In case pct start is unable to start a specific container, it might be helpful to collect debugging output by passing the --debug flag (replace CTID with the container’s CTID):

\# pct start CTID --debug

Alternatively, you can use the following lxc-start command, which will save the debug log to the file specified by the -o output option:

\# lxc-start -n CTID -F -l DEBUG -o /tmp/lxc-CTID.log

This command will attempt to start the container in foreground mode, to stop the container run pct shutdown CTID or pct stop CTID in a second terminal.

The collected debug log is written to /tmp/lxc-CTID.log.

| !\[](data:image/png;base64,
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)
 | If you have changed the container’s configuration since the last start attempt with pct start, you need to run pct start at least once to also update the configuration used by lxc-start. |

## Migration

If you have a cluster, you can migrate your Containers with

\# pct migrate <ctid> <target>

This works as long as your Container is offline. If it has local volumes or mount points defined, the migration will copy the content over the network to the target host if the same storage is defined there.

Running containers cannot live-migrated due to technical limitations. You can do a restart migration, which shuts down, moves and then starts a container again on the target node. As containers are very lightweight, this results normally only in a downtime of some hundreds of milliseconds.

A restart migration can be done through the web interface or by using the --restart flag with the pct migrate command.

A restart migration will shut down the Container and kill it after the specified timeout (the default is 180 seconds). Then it will migrate the Container like an offline migration and when finished, it starts the Container on the target node.

## Configuration

The /etc/pve/lxc/<CTID>.conf file stores container configuration, where <CTID> is the numeric ID of the given container. Like all other files stored inside /etc/pve/, they get automatically replicated to all other cluster nodes.

| !\[](data:image/png;base64,
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)
 | CTIDs &lt; 100 are reserved for internal purposes, and CTIDs need to be unique cluster wide. |

Example Container Configuration

ostype: debian
arch: amd64
hostname: www
memory: 512
swap: 512
net0: bridge=vmbr0,hwaddr=66:64:66:64:64:36,ip=dhcp,name=eth0,type=veth
rootfs: local:107/vm-107-disk-1.raw,size=7G

The configuration files are simple text files. You can edit them using a normal text editor, for example, vi or nano. This is sometimes useful to do small corrections, but keep in mind that you need to restart the container to apply such changes.

For that reason, it is usually better to use the pct command to generate and modify those files, or do the whole thing using the GUI. Our toolkit is smart enough to instantaneously apply most changes to running containers. This feature is called “hot plug”, and there is no need to restart the container in that case.

In cases where a change cannot be hot-plugged, it will be registered as a pending change (shown in red color in the GUI). They will only be applied after rebooting the container.

### File Format

The container configuration file uses a simple colon separated key/value format. Each line has the following format:

\# this is a comment
OPTION: value

Blank lines in those files are ignored, and lines starting with a # character are treated as comments and are also ignored.

It is possible to add low-level, LXC style configuration directly, for example:

lxc.init_cmd: /sbin/my_own_init

or

lxc.init_cmd = /sbin/my_own_init

The settings are passed directly to the LXC low-level tools.

### Snapshots

When you create a snapshot, pct stores the configuration at snapshot time into a separate snapshot section within the same configuration file. For example, after creating a snapshot called “testsnapshot”, your configuration file will look like this:

Container configuration with snapshot

memory: 512
swap: 512
parent: testsnaphot
...

\[testsnaphot]
memory: 512
swap: 512
snaptime: 1457170803
...

There are a few snapshot related properties like parent and snaptime. The parent property is used to store the parent/child relationship between snapshots. snaptime is the snapshot creation time stamp (Unix epoch).

### Options

arch: &lt;amd64 | arm64 | armhf | i386> (_default =_ amd64)

OS architecture type.

cmode: &lt;console | shell | tty> (_default =_ tty)

Console mode. By default, the console command tries to open a connection to one of the available tty devices. By setting cmode to _console_ it tries to attach to /dev/console instead. If you set cmode to _shell_, it simply invokes a shell inside the container (no login).

console: <boolean> (_default =_ 1)

Attach a console device (/dev/console) to the container.

cores: <integer> (1 - 8192)

The number of cores assigned to the container. A container can use all available cores by default.

cpulimit: <number> (0 - 8192) (_default =_ 0)

Limit of CPU usage.

| !\[](data:image/png;base64,
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)
 | If the computer has 2 CPUs, it has a total of _2_ CPU time. Value _0_ indicates no CPU limit. |

cpuunits: <integer> (0 - 500000) (_default =_ 1024)

CPU weight for a VM. Argument is used in the kernel fair scheduler. The larger the number is, the more CPU time this VM gets. Number is relative to the weights of all the other running VMs.

| !\[](data:image/png;base64,
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)
 | You can disable fair-scheduler configuration by setting this to 0. |

debug: <boolean> (_default =_ 0)

Try to be more verbose. For now this only enables debug log-level on start.

description: <string>

Description for the Container. Shown in the web-interface CT’s summary. This is saved as comment inside the configuration file.

features: \[force_rw_sys=&lt;1|0>] \[,fuse=&lt;1|0>] \[,keyctl=&lt;1|0>] \[,mknod=&lt;1|0>] \[,mount=&lt;fstype;fstype;...>] \[,nesting=&lt;1|0>]

Allow containers access to advanced features.

force_rw_sys=<boolean> (_default =_ 0)

Mount /sys in unprivileged containers as rw instead of mixed. This can break networking under newer (>= v245) systemd-network use.

fuse=<boolean> (_default =_ 0)

Allow using _fuse_ file systems in a container. Note that interactions between fuse and the freezer cgroup can potentially cause I/O deadlocks.

keyctl=<boolean> (_default =_ 0)

For unprivileged containers only: Allow the use of the keyctl() system call. This is required to use docker inside a container. By default unprivileged containers will see this system call as non-existent. This is mostly a workaround for systemd-networkd, as it will treat it as a fatal error when some keyctl() operations are denied by the kernel due to lacking permissions. Essentially, you can choose between running systemd-networkd or docker.

mknod=<boolean> (_default =_ 0)

Allow unprivileged containers to use mknod() to add certain device nodes. This requires a kernel with seccomp trap to user space support (5.3 or newer). This is experimental.

mount=&lt;fstype;fstype;...>

Allow mounting file systems of specific types. This should be a list of file system types as used with the mount command. Note that this can have negative effects on the container’s security. With access to a loop device, mounting a file can circumvent the mknod permission of the devices cgroup, mounting an NFS file system can block the host’s I/O completely and prevent it from rebooting, etc.

nesting=<boolean> (_default =_ 0)

Allow nesting. Best used with unprivileged containers with additional id mapping. Note that this will expose procfs and sysfs contents of the host to the guest.

hookscript: <string>

Script that will be exectued during various steps in the containers lifetime.

hostname: <string>

Set a host name for the container.

lock: &lt;backup | create | destroyed | disk | fstrim | migrate | mounted | rollback | snapshot | snapshot-delete>

Lock/unlock the VM.

memory: <integer> (16 - N) (_default =_ 512)

Amount of RAM for the VM in MB.

mp\[n]: \[volume=]<volume> ,mp=<Path> \[,acl=&lt;1|0>] \[,backup=&lt;1|0>] \[,mountoptions=&lt;opt\[;opt...]>] \[,quota=&lt;1|0>] \[,replicate=&lt;1|0>] \[,ro=&lt;1|0>] \[,shared=&lt;1|0>] \[,size=<DiskSize>]

Use volume as container mount point. Use the special syntax STORAGE_ID:SIZE_IN_GiB to allocate a new volume.

acl=<boolean>

Explicitly enable or disable ACL support.

backup=<boolean>

Whether to include the mount point in backups (only used for volume mount points).

mountoptions=&lt;opt\[;opt...]>

Extra mount options for rootfs/mps.

mp=<Path>

Path to the mount point as seen from inside the container.

| !\[](data:image/png;base64,
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)
 | Must not contain any symlinks for security reasons. |

quota=<boolean>

Enable user quotas inside the container (not supported with zfs subvolumes)

replicate=<boolean> (_default =_ 1)

Will include this volume to a storage replica job.

ro=<boolean>

Read-only mount point

shared=<boolean> (_default =_ 0)

Mark this non-volume mount point as available on all nodes.

| !\[](data:image/png;base64,
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
/F/lgJiyQFHragAAAABJRU5ErkJggg==)
 | This option does not share the mount point automatically, it assumes it is shared already! |

size=<DiskSize>

Volume size (read only value).

volume=<volume>

Volume, device or directory to mount into the container.

nameserver: <string>

Sets DNS server IP address for a container. Create will automatically use the setting from the host if you neither set searchdomain nor nameserver.

net\[n]: name=<string> \[,bridge=<bridge>] \[,firewall=&lt;1|0>] \[,gw=<GatewayIPv4>] \[,gw6=<GatewayIPv6>] \[,hwaddr=&lt;XX:XX:XX:XX:XX:XX>] \[,ip=&lt;(IPv4/CIDR|dhcp|manual)>] \[,ip6=&lt;(IPv6/CIDR|auto|dhcp|manual)>] \[,mtu=<integer>] \[,rate=<mbps>] \[,tag=<integer>] \[,trunks=&lt;vlanid\[;vlanid...]>] \[,type=<veth>]

Specifies network interfaces for the container.

bridge=<bridge>

Bridge to attach the network device to.

firewall=<boolean>

Controls whether this interface’s firewall rules should be used.

gw=<GatewayIPv4>

Default gateway for IPv4 traffic.

gw6=<GatewayIPv6>

Default gateway for IPv6 traffic.

hwaddr=&lt;XX:XX:XX:XX:XX:XX>

A common MAC address with the I/G (Individual/Group) bit not set.

ip=&lt;(IPv4/CIDR|dhcp|manual)>

IPv4 address in CIDR format.

ip6=&lt;(IPv6/CIDR|auto|dhcp|manual)>

IPv6 address in CIDR format.

mtu=<integer> (64 - N)

Maximum transfer unit of the interface. (lxc.network.mtu)

name=<string>

Name of the network device as seen from inside the container. (lxc.network.name)

rate=<mbps>

Apply rate limiting to the interface

tag=<integer> (1 - 4094)

VLAN tag for this interface.

trunks=&lt;vlanid\[;vlanid...]>

VLAN ids to pass through the interface

type=<veth>

Network interface type.

onboot: <boolean> (_default =_ 0)

Specifies whether a VM will be started during system bootup.

ostype: &lt;alpine | archlinux | centos | debian | devuan | fedora | gentoo | opensuse | ubuntu | unmanaged>

OS type. This is used to setup configuration inside the container, and corresponds to lxc setup scripts in /usr/share/lxc/config/<ostype>.common.conf. Value _unmanaged_ can be used to skip and OS specific setup.

protection: <boolean> (_default =_ 0)

Sets the protection flag of the container. This will prevent the CT or CT’s disk remove/update operation.

rootfs: \[volume=]<volume> \[,acl=&lt;1|0>] \[,mountoptions=&lt;opt\[;opt...]>] \[,quota=&lt;1|0>] \[,replicate=&lt;1|0>] \[,ro=&lt;1|0>] \[,shared=&lt;1|0>] \[,size=<DiskSize>]

Use volume as container root.

acl=<boolean>

Explicitly enable or disable ACL support.

mountoptions=&lt;opt\[;opt...]>

Extra mount options for rootfs/mps.

quota=<boolean>

Enable user quotas inside the container (not supported with zfs subvolumes)

replicate=<boolean> (_default =_ 1)

Will include this volume to a storage replica job.

ro=<boolean>

Read-only mount point

shared=<boolean> (_default =_ 0)

Mark this non-volume mount point as available on all nodes.

| !\[](data:image/png;base64,
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
/F/lgJiyQFHragAAAABJRU5ErkJggg==)
 | This option does not share the mount point automatically, it assumes it is shared already! |

size=<DiskSize>

Volume size (read only value).

volume=<volume>

Volume, device or directory to mount into the container.

searchdomain: <string>

Sets DNS search domains for a container. Create will automatically use the setting from the host if you neither set searchdomain nor nameserver.

startup: `\[\[order=\]\\d+\] \[,up=\\d+\] \[,down=\\d+\] `

Startup and shutdown behavior. Order is a non-negative number defining the general startup order. Shutdown in done with reverse ordering. Additionally you can set the _up_ or _down_ delay in seconds, which specifies a delay to wait before the next VM is started or stopped.

swap: <integer> (0 - N) (_default =_ 512)

Amount of SWAP for the VM in MB.

tags: <string>

Tags of the Container. This is only meta information.

template: <boolean> (_default =_ 0)

Enable/disable Template.

timezone: <string>

Time zone to use in the container. If option isn’t set, then nothing will be done. Can be set to _host_ to match the host time zone, or an arbitrary time zone option from /usr/share/zoneinfo/zone.tab

tty: <integer> (0 - 6) (_default =_ 2)

Specify the number of tty available to the container

unprivileged: <boolean> (_default =_ 0)

Makes the container run as unprivileged user. (Should not be modified manually.)

unused\[n]: \[volume=]<volume>

Reference to unused volumes. This is used internally, and should not be modified manually.

volume=<volume>

The volume that is not used currently.

## Locks

Container migrations, snapshots and backups (vzdump) set a lock to prevent incompatible concurrent actions on the affected container. Sometimes you need to remove such a lock manually (e.g., after a power failure).

\# pct unlock <CTID>

| !\[](data:image/png;base64,
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)
 | Only do this if you are sure the action which set the lock is no longer running. |

* * *

[1](#_footnoteref_1 "Return to text"). this includes all newest major versions of container templates shipped by Proxmox VE

[2](#_footnoteref_2 "Return to text"). for example Alpine Linux

[3](#_footnoteref_3 "Return to text"). /etc/os-release replaces the multitude of per-distribution release files [https://manpages.debian.org/stable/systemd/os-release.5.en.html](https://manpages.debian.org/stable/systemd/os-release.5.en.html)

## Migrate container from OpenVZ to Linux container

Follow this howto:

-   [Convert OpenVZ to LXC](/wiki/Convert_OpenVZ_to_LXC "Convert OpenVZ to LXC")

## References

-   [Wikipedia Linux Container](https://en.wikipedia.org/wiki/LXC)
-   [Linux Container](https://linuxcontainers.org/)
-   [GIT Linux Container](https://github.com/lxc)

Retrieved from "[https://pve.proxmox.com/mediawiki/index.php?title=Linux_Container&oldid=11234](https://pve.proxmox.com/mediawiki/index.php?title=Linux_Container&oldid=11234)"

[Categories](/wiki/Special:Categories "Special:Categories"):

-   [Reference Documentation](/wiki/Category:Reference_Documentation "Category:Reference Documentation")
-   [Installation](/wiki/Category:Installation "Category:Installation")
-   [HOWTO](/wiki/Category:HOWTO "Category:HOWTO")

## Navigation menu

### Personal tools

-   [Log in](/mediawiki/index.php?title=Special:UserLogin&returnto=Linux+Container "You are encouraged to log in; however, it is not mandatory \[Alt+Shift+o]")

### Namespaces

-   [Page](/wiki/Linux_Container "View the content page \[Alt+Shift+c]")
-   [Discussion](/mediawiki/index.php?title=Talk:Linux_Container&action=edit&redlink=1 "Discussion about the content page (page does not exist) \[Alt+Shift+t]")

### Variants

### Views

-   [Read](/wiki/Linux_Container)
-   [View source](/mediawiki/index.php?title=Linux_Container&action=edit "This page is protected.
    You can view its source \[Alt+Shift+e]")
-   [View history](/mediawiki/index.php?title=Linux_Container&action=history "Past revisions of this page \[Alt+Shift+h]")

### More

### Search

### Navigation

-   [Proxmox VE](/wiki/Main_Page)
-   [Documentation (current)](https://pve.proxmox.com/pve-docs/)
-   [Documentation (6.x)](https://pve.proxmox.com/pve-docs-6/)
-   [Downloads](/wiki/Downloads)
-   [Installation](/wiki/Installation)
-   [Get support](/wiki/Get_support)

### Sites

-   [proxmox.com](https://www.proxmox.com)
-   [Support forum](https://forum.proxmox.com)
-   [Bugtracker](https://bugzilla.proxmox.com)
-   [Source code](https://git.proxmox.com)
-   [FAQ](https://pve.proxmox.com/wiki/FAQ)

### Tools

-   [What links here](/wiki/Special:WhatLinksHere/Linux_Container "A list of all wiki pages that link here \[Alt+Shift+j]")

-   [Related changes](/wiki/Special:RecentChangesLinked/Linux_Container "Recent changes in pages linked from this page \[Alt+Shift+k]")

-   [Special pages](/wiki/Special:SpecialPages "A list of all special pages \[Alt+Shift+q]")

-   [Printable version](javascript:print(); "Printable version of this page \[Alt+Shift+p]")

-   [Permanent link](/mediawiki/index.php?title=Linux_Container&oldid=11234 "Permanent link to this revision of the page")

-   [Page information](/mediawiki/index.php?title=Linux_Container&action=info "More information about this page")

-   [Cite this page](/mediawiki/index.php?title=Special:CiteThisPage&page=Linux_Container&id=11234&wpFormIdentifier=titleform "Information on how to cite this page")

-   This page was last edited on 17 November 2021, at 10:27.

-   [Privacy policy](/wiki/Proxmox_VE:Privacy_policy "Proxmox VE:Privacy policy")

-   [About Proxmox VE](/wiki/Proxmox_VE:About "Proxmox VE:About")

-   [Disclaimers](/wiki/Proxmox_VE:General_disclaimer "Proxmox VE:General disclaimer")

-   [![](https://pve.proxmox.com/mediawiki/resources/assets/poweredby_mediawiki_88x31.png)
       ](https://www.mediawiki.org/) 
    [https://pve.proxmox.com/wiki/Linux_Container#pct_container_images](https://pve.proxmox.com/wiki/Linux_Container#pct_container_images)

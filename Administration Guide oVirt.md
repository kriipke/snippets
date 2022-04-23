# Administration Guide | oVirt
#### 2.6.1. About oVirt storage

oVirt uses a centralized storage system for virtual disks, ISO files and snapshots. Storage networking can be implemented using:

-   Network File System (NFS)
-   GlusterFS exports
-   Other POSIX compliant file systems
-   Internet Small Computer System Interface (iSCSI)
-   Local storage attached directly to the virtualization hosts
-   Fibre Channel Protocol (FCP)
-   Parallel NFS (pNFS)

Setting up storage is a prerequisite for a new data center because a data center cannot be initialized unless storage domains are attached and activated.

As a oVirt system administrator, you need to create, configure, attach and maintain storage for the virtualized enterprise. You should be familiar with the storage types and their use. Read your storage array vendor’s guides, and see the [Enterprise Linux Storage Administration Guide](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/Storage_Administration_Guide/) for more information on the concepts, protocols, requirements or general usage of storage.

To add storage domains you must be able to successfully access the Administration Portal, and there must be at least one host connected with a status of **Up**.

oVirt has three types of storage domains:

-   **Data Domain:** A data domain holds the virtual hard disks and OVF files of all the virtual machines and templates in a data center. In addition, snapshots of the virtual machines are also stored in the data domain.

    The data domain cannot be shared across data centers. Data domains of multiple types (iSCSI, NFS, FC, POSIX, and Gluster) can be added to the same data center, provided they are all shared, rather than local, domains.

    You must attach a data domain to a data center before you can attach domains of other types to it.
-   **ISO Domain:** ISO domains store ISO files (or logical CDs) used to install and boot operating systems and applications for the virtual machines. An ISO domain removes the data center’s need for physical media. An ISO domain can be shared across different data centers. ISO domains can only be NFS-based. Only one ISO domain can be added to a data center.
-   **Export Domain:** Export domains are temporary storage repositories that are used to copy and move images between data centers and oVirt environments. Export domains can be used to backup virtual machines. An export domain can be moved between data centers, however, it can only be active in one data center at a time. Export domains can only be NFS-based. Only one export domain can be added to a data center.

    \|  \| 

    The export storage domain is deprecated. Storage data domains can be unattached from a data center and imported to another data center in the same environment, or in a different environment. Virtual machines, floating virtual disks, and templates can then be uploaded from the imported storage domain to the attached data center. See [Importing Existing Storage Domains](#sect-Importing_Existing_Storage_Domains) for information on importing storage domains.

     \|

\|  \| 

Only commence configuring and attaching storage for your oVirt environment once you have determined the storage needs of your data center(s).

 \| 
 [https://ovirt.org/documentation/administration_guide/index.html#about-storage](https://ovirt.org/documentation/administration_guide/index.html#about-storage)

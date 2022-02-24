# Loading System Images and Config. Files  [Cisco IOS Software Releases 11.0] - Cisco Systems
## Table Of Contents

[Loading System Images and Configuration Files](#wp3634)

[System Image and Configuration File Load Task List](#wp10112)

[Retrieve System Images and Configuration Files](#wp10968)

[Retrieve System Images and Configuration File Task List](#wp10971)

[Copy System Images from a Network Server to Flash Memory](#wp11017)

[Copy from a TFTP Server to Flash Memory](#wp11040)

[Copy from an rcp Server to Flash Memory](#wp11175)

[Copy from a MOP Server to Flash Memory](#wp11256)

[Copy Configuration Files from a Network Server to the Access Server](#wp11294)

[Copy from a TFTP Server to the Access Server](#wp11320)

[Copy from an rcp Server to the Access Server](#wp11347)

[Change the Buffer Size for Loading Configuration Files](#wp11520)

[Compress Configuration Files](#wp14424)

[Verify the Image in Flash Memory](#wp11560)

[Display System Image and Configuration Information](#wp11577)

[Reexecute the Configuration Commands in Nonvolatile Memory](#wp11644)

[Clear the Configuration Information](#wp11669)

[Perform General Startup Tasks](#wp11686)

[General Startup Task List](#wp11697)

[Enter Global Configuration Mode and Select a Configuration Source](#wp11723)

[Configure the Access Server from the Terminal](#wp11750)

[Configure the Access Server from Memory](#wp11786)

[Configure the Access Server from the Network](#wp11800)

[Copy a Configuration File Directly to the Startup Configuration](#wp11861)

[Modify the Configuration Register Boot Field](#wp11889)

[How the Access Server Uses the Boot Field](#wp11893)

[Setting the Boot Field](#wp11903)

[Perform the Boot Field Modification Tasks](#wp11913)

[Specify the Startup System Image](#wp11984)

[Load from Flash Memory](#wp11992)

[Flash Memory Configuration](#wp12019)

[Specify the Startup Configuration File](#wp12319)

[Download the Network Configuration File](#wp12328)

[Download the Host Configuration File](#wp12363)

[Store System Images and Configuration Files](#wp11736)

[Store System Images and Configuration Files Task List](#wp12428)

[Copy System Images from Flash Memory to a Network Server](#wp12589)

[Copy from Flash Memory to a TFTP Server](#wp12443)

[Copy from Flash Memory to an rcp Server](#wp12607)

[Copy Configuration Files from the Access Server to a Network Server](#wp12512)

[Copy from the Access Server to a TFTP Server](#wp12740)

[Copy from the Access Server to an rcp Server](#wp12779)

[Configure the Access Server as a Network Server](#wp12948)

[Configure an Access Server as a Network Server Task List](#wp13015)

[Configure an Access Server as a TFTP Server](#wp12990)

[Configure Flash Memory as a TFTP Server](#wp13079)

[Configure Flash Memory as a TFTP Server Task List](#wp13085)

[Perform Prerequisite Tasks](#wp13106)

[Configure the Flash Server](#wp13120)

[Configure the Client Access Server](#wp13148)

[Configure an Access Server as a RARP Server](#wp13040)

[Configure for Other Types of Servers](#wp13187)

[Configure for Other Types of Servers Task List](#wp13189)

[Specify Asynchronous Interface Extended BOOTP Requests](#wp13208)

[Specify MOP Server Boot Requests](#wp13233)

[Perform Startup Tasks](#wp11639)

[Partition Flash Memory Using Dual Flash Bank](#wp13288)

[Understand Relocatable Images](#wp13308)

[Dual Flash Bank Configuration Task List](#wp13331)

[Partition Flash Memory](#wp13360)

[Download a File into a Flash Partition](#wp13374)

[Manually Boot from Flash Memory](#wp13407)

[Configure the Access Server to Automatically Boot from Flash](#wp13459)

[Configure a Flash Partition as a TFTP Server](#wp13489)

[Use Flash Load Helper to Upgrade Software on Run-from-Flash Systems](#wp13521)

[Flash Load Helper Configuration Task List](#wp13552)

[Download a File Using Flash Load Helper](#wp13563)

[Monitor Flash Load Helper](#wp13643)

[Configure for Remote Shell (rsh) and Remote Copy (rcp) Functions](#wp13270)

[Cisco's Implementation of Remote Shell (rsh) and Remote Copy (rcp)](#wp11742)

[Using rsh](#wp10124)

[Maintaining rsh Security](#wp10131)

[Using rcp](#wp10136)

[Configure for rsh and rcp Task List](#wp13681)

[Configure an Access Server to Support Incoming rcp Requests and rsh Commands](#wp13700)

[Configure the Access Server to Accept rcp Requests from Remote Users](#wp13719)

[Configure the System to Allow Remote Users to Execute Commands Using rsh](#wp13752)

[Turn Off DNS Lookups for rcp and rsh](#wp13803)

[Configure the Remote Username for rcp Requests](#wp13826)

[Remotely Execute Commands Using rsh](#wp13861)

[Use the AutoInstall Procedure](#wp13905)

[AutoInstall Requirements](#wp13913)

[Using a DOS-Based TFTP Server](#wp13935)

[How AutoInstall Works](#wp13944)

[Acquiring the New Access Server's IP Address](#wp13951)

[Resolving the IP Address to the Host Name](#wp13988)

[Downloading the New Access Server's Host Configuration File](#wp14011)

[Perform the AutoInstall Procedure](#wp14026)

[Modifying the Existing Access Server's Configuration](#wp14033)

[Set up the TFTP Server](#wp14186)

[Set Up the BOOTP or RARP](#wp14231)

[Connect the New Access Server to the Network](#wp14255)

[Manually Load a System Image from ROM Monitor](#wp4770)

[Manually Boot from Flash Memory](#wp4775)

[Manually Boot from a Network File](#wp4877)

[Manually Boot from ROM](#wp4901)

[Use the System Image Instead of Reloading](#wp10776)

## Loading System Images and Configuration Files

* * *

This chapter describes how to load system images and configuration files. The system images contain the system software, and the configuration files contain commands for customizing the access server.

The instructions in this chapter describe how to copy system images from access servers to network servers (and vice versa), display and compare different configuration files, and list the system software version running on the access server.

This chapter also describes the AutoInstall procedure, which you can use to automatically configure and enable a new access server upon startup. It also explains how to manually load system images from ROM monitor so that you can successfully boot the access server when typical startup processes malfunction.

To benefit most from the instructions and organization in this chapter, your access server must contain a minimal configuration that allows you to interact with the system software. You can create a basic configuration file using the setup command facility.

For a complete description of the commands mentioned in this chapter, refer to the "System Image and Configuration File Load Commands" chapter in the _Access and Communication Servers Command Reference_ publication.

![](https://www.cisco.com/en/US/i/templates/note.gif)

* * *

**Note**![](https://www.cisco.com/en/US/i/templates/blank.gif)
One or more commands in this chapter have changed from previous releases. Commands in this chapter that have been replaced by new commands continue to perform their normal functions in the current release but are no longer documented. Support for these commands will cease in a future release. See the _Access and Communication Servers Command Reference_ publication for detailed command information.

* * *

Table 3-1 Mapping Old Commands to New Commands

\| 

Old Command

 \| 

New Command

 \|
\| 

configure network

 \| 

copy rcp running-config (for an rcp server)

copy tftp running-config (for a TFTP server)

 \|
\| 

configure overwrite-network

 \| 

copy rcp startup-config (for an rcp server)

copy tftp startup-config (for a TFTP server)

 \|
\| 

copy erase flash

 \| 

erase flash

 \|
\| 

copy verify or copy verify flash

 \| 

verify flash

 \|
\| 

copy verify bootflash

 \| 

verify bootflash

 \|
\| 

show configuration

 \| 

show startup-config

 \|
\| 

tftp-server system

 \| 

tftp-server

 \|
\| 

write erase

 \| 

erase startup-config

 \|
\| 

write memory

 \| 

copy running-config startup-config

 \|
\| 

write network

 \| 

copy running-config rcp (for an rcp server)

copy running-config tftp (for a TFTP server)

 \|
\| 

write terminal

 \| 

show running-config

 \|

## System Image and Configuration File Load Task List

To load and maintain system images, configuration files, and microcode images needed for access server startup, complete the tasks in the following sections.

![](https://www.cisco.com/en/US/i/templates/note.gif)

* * *

**Note**![](https://www.cisco.com/en/US/i/templates/blank.gif)
The organization of tasks assumes you have a minimal configuration that you want to modify.

* * *

The tasks in the first three sections are typical tasks for all access servers. Perform the tasks in the remaining sections as needed for your particular network environment.

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Retrieve System Images and Configuration Files](#wp10968)

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Perform General Startup Tasks](#wp11686)

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Store System Images and Configuration Files](#wp11736)

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Configure the Access Server as a Network Server](#wp12948)

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Configure for Other Types of Servers](#wp13187)

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Perform Startup Tasks](#wp11639)

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Configure for Remote Shell (rsh) and Remote Copy (rcp) Functions](#wp13270)

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Use the AutoInstall Procedure](#wp13905)

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Manually Load a System Image from ROM Monitor](#wp4770)

## Retrieve System Images and Configuration Files

If you have a minimal configuration that allows you to interact with the system software, you can retrieve other system images and configuration files from a network server and modify them for use in your particular network environment. This section describes tasks related to retrieving system images and configuration files for modification.

### Retrieve System Images and Configuration File Task List

When retrieving system images and configuration files, you can perform the following tasks. None of the tasks are mandatory.

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Copy System Images from a Network Server to Flash Memory](#wp11017)

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Copy Configuration Files from a Network Server to the Access Server](#wp11294)

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Change the Buffer Size for Loading Configuration Files](#wp11520)

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Compress Configuration Files](#wp14424)

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Verify the Image in Flash Memory](#wp11560)

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Display System Image and Configuration Information](#wp11577)

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Reexecute the Configuration Commands in Nonvolatile Memory](#wp11644)

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Clear the Configuration Information](#wp11669)

### Copy System Images from a Network Server to Flash Memory

You can copy system images from a TFTP server, an rcp server, or a MOP server. The following sections describe these tasks:

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Copy from a TFTP Server to Flash Memory](#wp11040)

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Copy from an rcp Server to Flash Memory](#wp11175)

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Copy from a MOP Server to Flash Memory](#wp11256)

### Copy from a TFTP Server to Flash Memory

You can copy a system image from a network server to Flash memory using TFTP by completing the following tasks:

\| 

Task

 \| 

Command

 \|
\| 

**Step 1** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Make a backup copy of the current system software image.

 \| 

See the section "[Copy from Flash Memory to a TFTP Server](#wp12443)" later in this chapter.

 \|
\| 

**Step 2** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Copy a system image to Flash memory.

 \| 

copy tftp flash

 \|
\| 

**Step 3** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
When prompted, enter the IP address or domain name of the server.

 \| 

ip-address or name

 \|
\| 

**Step 4** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
When prompted, enter the filename of the server system image.

 \| 

filename

 \|

![](https://www.cisco.com/en/US/i/templates/note.gif)

* * *

**Note**![](https://www.cisco.com/en/US/i/templates/blank.gif)
Be sure there is ample space available before copying a file to Flash. Use the **show flash** command and compare the size of the file you want to copy to the amount of available Flash memory shown. If the space available is less than the space required by the file you want to copy, the copy process will continue, but the entire file will not be copied into Flash. A failure message, "buffer overflow - xxxx/xxxx," will appear, where xxxx/xxxx is the number of bytes read in number of bytes available.

* * *

The server system image copied to the Flash memory of the Cisco 2500 series access server must be at least Software release 9.21 or later.

After you issue the **copy tftp flash** command, the system prompts you for the IP address (or domain name) of the server. This can be another access server serving ROM or Flash system software images. You are then prompted for the filename of the software image. When there is free space available in Flash memory, you are given the option of erasing the existing Flash memory before writing onto it. If no free Flash memory space is available, or if the Flash memory has never been written to, the erase routine is required before new files can be copied. The system will inform you of these conditions and prompt you for a response. Note that the Flash memory is erased at the factory before shipment.

If you attempt to copy a file into Flash memory that is already there, a prompt will tell you that a file with the same name already exists. This file is "deleted" when you copy the new file into Flash. The first copy of the file still resides within Flash memory, but is rendered unusable in favor of the newest version, and will be listed with the "deleted" tag when you use the **show flash** command. If you terminate the copy process, the newer file will be marked "deleted" because the entire file was not copied and is, therefore, not valid. In this case, the original file in Flash memory is valid and available to the system.

Following is sample output (copying a system image named _igs-bfpx.102.1_) of the prompt you will see when you use the **copy tftp flash** command and Flash memory is too full to copy the file. The filename _igs-bfpx.102.1_ can be in either lowercase or uppercase; the system will interpret _IGS-BFPX.102.1_ as _igs-bfpx.102.1_. If more than one file of the same name is copied to Flash, regardless of case, the last file copied will become the valid file.

env-chassis# copy tftp flash 

IP address or name of remote host \[255.255.255.255]? dirt 

Translating "DIRT"...domain server (255.255.255.255) \[OK]

Name of file to copy ? igs-bfpx.102.1 

Copy igs-bfpx.102.1 from 172.30.13.111 into flash memory? \[confirm]

Flash is filled to capacity.

Erasure is needed before flash may be written.

Erase flash before writing? \[confirm]

Erasing flash EPROMs bank 0

Zeroing bank...zzzzzzzzzzzzzzzz

Verify zeroed...vvvvvvvvvvvvvvvv

Erasing bank...eeeeeeeeeeeeeeee

Erasing flash EPROMs bank 1

Zeroing bank...zzzzzzzzzzzzzzzz

Verify zeroed...vvvvvvvvvvvvvvvv

Erasing bank...eeeeeeeeeeeeeeee

Erasing flash EPROMs bank 2

Zeroing bank...zzzzzzzzzzzzzzzz

Verify zeroed...vvvvvvvvvvvvvvvv

Erasing bank...eeeeeeeeeeeeeeee

Erasing flash EPROMs bank 3

Zeroing bank...zzzzzzzzzzzzzzzz

Verify zeroed...vvvvvvvvvvvvvvvv

Erasing bank...eeeeeeeeeeeeeeee

Loading from 172.30.1.111: 

 \[OK - 1906676/4194240 bytes]

Verifying via checksum...

vvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvv

vvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvv

vvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvv

vvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvv

vvvvvvvvvvvvvvvvvvvvvvvvvvvvv

Flash verification successful. Length = 1906676, checksum = 0x12AD

![](https://www.cisco.com/en/US/i/templates/note.gif)

* * *

**Note**![](https://www.cisco.com/en/US/i/templates/blank.gif)
 If you enter **n** after the "Erase flash before writing?" prompt, the copy process continues. If you enter **y**, the erase routine begins. Make certain you have ample Flash memory space before entering **n** at the erasure prompt.

* * *

Following is sample output from copying a system image named _igs-bfpx.102.1_ into the current Flash configuration, in which a file of the name _igs-bfpx.102.1_ already exists:

env-chassis# copy tftp flash 

IP address or name of remote host \[172.30.13.111]?

Name of file to copy ? igs-bfpx.102.1 

File igs-bfpx.102.1 already exists; it will be invalidated!

Copy igs-bfpx.102.1 from 172.30.13.111 into flash memory? \[confirm]

2287500 bytes available for writing without erasure.

Erase flash before writing? \[confirm]n

Loading from 172.30.1.111: 

\[OK - 1906676/2287500 bytes]

Verifying via checksum...

vvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvv

vvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvv

vvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvv

vvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvv

vvvvvvvvvvvvvvvvvvvvvvvvvvvvv

Flash verification successful. Length = 1902192, checksum = 0x12AD 

In the following example, the Flash security jumper is not installed, so you cannot write files to Flash memory.

Flash: embedded flash security jumper(12V)

       must be strapped to modify flash memory

![](https://www.cisco.com/en/US/i/templates/note.gif)

* * *

**Note**![](https://www.cisco.com/en/US/i/templates/blank.gif)
To terminate this copy process, press **Ctrl-^** (the Ctrl, Shift, and 6 keys on a standard keyboard) simultaneously. Although the process will stop, the partial file copied before the termination will remain until the entire Flash memory is erased. Refer to the Troubleshooting Internetworking Systems publication for procedures on how to resolve Flash memory problems.

* * *

You can copy normal or compressed images to Flash memory. You can produce a compressed system image on any UNIX platform using the **compress** command. Refer to your UNIX platform's documentation for the exact usage of the **compress** command.

The following example shows sample output from copying a system image named _IJ09140Z_ into the current Flash configuration.

IP address or name of remote host \[255.255.255.255]? server1 

Name of tftp filename to copy into flash \[]? IJ09140Z

copy IJ09140Z from 172.30.101.101 into flash memory? \[confirm] <Return> 

xxxxxxxx bytes available for writing without erasure.

erase flash before writing? \[confirm] <Return> 

Clearing and initializing flash memory (please wait)Building configuration...

Loading from 172.30.13.110: -

... \[OK - 324572/524212 bytes]

VVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVVV...

Flash verification successful. Length = 1204637, checksum = 0x95D9

The Building configuration... indicates that each Flash device is being cleared and initialized, one per device. Different access server platforms use different ways of indicating that Flash is being cleared. The spinning star (represented in the sample output by a dash) indicates the copy process. The characters/series of Vs indicates that a checksum is calculated. An O indicates an out-of-order packet. A period (.) indicates a timeout. The last line in the sample configuration indicates that the copy is successful.

### Copy from an rcp Server to Flash Memory

You can copy a system image from a network server to Flash memory using rcp. For the rcp command to execute properly, an account must be defined on the network server for the remote username. You can override the default remote username sent on the rcp copy request by configuring the remote username. For example, if the system image resides in the home directory of a user on the server, you can specify that user's name as the remote username. The rcp protocol implementation copies the system image from the remote server relative to the directory of the remote username.

To copy a system image from an rcp server to Flash memory, complete the following task

\| 

Tasks

 \| 

Command

 \|
\| 

**Step 1** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Make a backup copy of the current system software image.

 \| 

See the section "Copy System Images from Flash Memory to a Network Server" in this chapter.

 \|
\| 

**Step 2** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Enter global configuration mode from the terminal.

This step is only required to override the default remote username (see Step 3).

 \| 

configure terminal

 \|
\| 

**Step 3** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Specify the remote username. This step is optional but recommended.

 \| 

ip rcmd remote-username username

 \|
\| 

**Step 4** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Exit global configuration mode.

 \| 

Ctrl-Z

 \|
\| 

**Step 5** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Copy the system image from the network server to Flash memory using rcp.

 \| 

copy rcp flash

 \|
\| 

**Step 6** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
When prompted, enter the IP address or domain name of the network server.

 \| 

ip-address or name

 \|
\| 

**Step 7** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
When prompted, enter the filename of the server system image to be copied.

 \| 

filename

 \|

s:

![](https://www.cisco.com/en/US/i/templates/note.gif)

* * *

**Note**![](https://www.cisco.com/en/US/i/templates/blank.gif)
Be sure there is ample space available before copying a file to Flash. Use the **show flash** command and compare the size of the file you want to copy to the amount of available Flash memory shown. If the space available is less than the space required by the file you want to copy, the copy process will continue, but the entire file will not be copied into Flash. A failure message, "_buffer overflow - xxxx/xxxx_," will appear, where _xxxx/xxxx_ is the number of bytes read in or the number of bytes available.

* * *

The server system image copied to the Flash memory of the ASM-CS must be at least Software Release 9.0 or later. For Cisco 2500 series access servers, the system image must be at least, 10.2.

When you issue the **copy rcp flash** command, the system prompts you for the IP address (or domain name) of the server. This can be another access server serving ROM or Flash system software images. You are then prompted for the filename of the software image; when there is free space available in Flash memory, you are given the option of erasing the existing Flash memory before writing onto it. If no free Flash memory space is available, or if the Flash memory has never been written to, the erase routine is required before new files can be copied. The system will inform you of these conditions and prompt you for a response. If you accept the erasure, the system will prompt you again to confirm before erasing. Note that the Flash memory is erased at the factory before shipment.

If you attempt to copy a file into Flash memory that is already there, a prompt will tell you that a file with the same name already exists. This file is "deleted" when you copy the new file into Flash. The first copy of the file still resides within Flash memory, but is rendered unusable in favor of the newest version, and will be listed with the "deleted" tag when you use the **show flash** command. If you terminate the copy process, the newer file will be marked "deleted" because the entire file was not copied and is, therefore, not valid. In this case, the original file in Flash memory is valid and available to the system.

The following example copies a system image named _IJ09140z_ from the _netadmin1_ directory on the remote server named _SERVER1.CISCO.COM_ with an IP address of 172.30.101.101 to the access server's Flash memory. To ensure that enough Flash memory is available to accommodate the system image to be copied, the Cisco IOS software allows you to erase the contents of Flash memory first.

cs1# ip rcmd remote-username netadmin1 

File name/status 
 1 IJ09140Z 
\[2076072 bytes used, 21080 bytes available]

Address or name of remote host\[UNKNOWN]? 172.30.101.101 

Name of file to copy? IJ09140Z 

Copy IJ09140z from SERVER1.CISCO.COM?\[confirm]

Checking for file \`IJ09140Z' on SERVER1.CISCO.COM...\[OK]

Erase flash device before writing?\[confirm]

Erasing device...ezeeze...erased.

Connected to 172.30.101.101

Loading 2076007 byte file IJ09140Z:

Verifying checksum... (0x87FD)...\[OK] 

The spinning star (represented in the sample output by a dash) indicates that the copy process is taking place.

![](https://www.cisco.com/en/US/i/templates/note.gif)

* * *

**Note**![](https://www.cisco.com/en/US/i/templates/blank.gif)
If you enter **n** after the "Erase flash device before writing?" prompt, the copy process continues. If you enter **y** and you confirm the erasure, the erase routine begins. Make certain you have ample Flash memory space before entering **n** at the erasure prompt.

* * *

You can copy normal or compressed images to Flash memory. You can produce a compressed system image on any UNIX platform using the **compress** command. Refer to your UNIX platform's documentation for the exact usage of the **compress** command.

### Copy from a MOP Server to Flash Memory

You can copy a system image from a MOP server to Flash memory. To do so, perform the following task in EXEC mode:

\| 

Task

 \| 

Command

 \|
\| 

Copy a boot image using MOP.

 \| 

copy mop flash

 \|

The following example shows a sample output from the **copy mop flash** command. In this example, the system image _junk_, which already exists in Flash memory, is copied to Flash memory, and there is enough memory to copy the file without erasing any existing files.

\[2096 bytes used, 8386512 available, 8388608 total]

Destination file name \[junk]?

Erase flash device before writing? \[confirm]

Flash contains files. Are you sure you want to erase? \[confirm]

  as 'junk' into Flash WITH erase? \[yes/no]yes 

Erasing device... eeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee ...erased

Loading junk from 1234.5678.9abc via Ethernet0: !

Verifying checksum...  OK (0x14B3)

Flash copy took 0:00:01 \[hh:mm:ss]

### Copy Configuration Files from a Network Server to the Access Server

You can also copy configuration files from a TFTP server or an rcp server to the access server. You might use this process to restore a configuration file to the access server if you have backed up the file to a server. If you replace an access server and want to use the configuration file that you created for the original access server, you can restore that file instead of recreating it. You can also use this process to copy to the access server a different configuration that is stored on a network server.

The following sections describe these tasks:

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Copy from a TFTP Server to the Access Server](#wp11320)

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Copy from an rcp Server to the Access Server](#wp11347)

### Copy from a TFTP Server to the Access Server

You can copy a configuration file from a TFTP server to the running configuration or to the startup configuration. When you copy a configuration file to the running configuration, you copy to and run the file from RAM.

When you copy a configuration file to the startup configuration, you copy it to the nonvolatile random-access memory (NVRAM).

To copy a configuration file from a TFTP server to the access server, complete the following tasks:

\| 

Task

 \| 

Command

 \|
\| 

**Step 1** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Copy a file from a TFTP server to the access server.

 \| 

copy tftp running-config

or

copy tftp startup-config

 \|
\| 

**Step 2** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
When prompted, enter the IP address or domain name of the server.

 \| 

ip-address or name

 \|
\| 

**Step 3** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
If prompted, enter the filename of the server system image.

 \| 

filename

 \|

### Copy from an rcp Server to the Access Server

You can copy a configuration file from an rcp server to the local access server. As with TFTP, you can copy a configuration file to the running configuration or to the startup configuration. When you copy a configuration file to the running configuration, you copy to and run the file from RAM.

When you copy a configuration file to the startup configuration, you copy it to NVRAM.

The rcp protocol requires that a client send the remote username on each rcp request to a network server. When you issue a request to copy a configuration file from an rcp network server, the access server sends a default remote username unless you override the default by configuring a remote username. As the default value of the remote username, the Cisco IOS software sends the remote username associated with the current TTY process, if that name is valid. If the TTY username is invalid, the software uses the access server host name as both the remote and local usernames. You can also specify the path of an existing directory along with the remote username.

For the rcp copy request to execute successfully, an account must be defined on the network server for the remote username. If you copy the configuration file from a personal computer used as a file server, the remote host computer must support the remote shell protocol.

### Copy a Configuration File to the Running Configuration

You can copy a configuration file from an rcp server to the running configuration.

A host configuration file contains commands that apply to one network server in particular. A network configuration file contains commands that apply to all network servers on a network.

To copy a configuration file from an rcp server to the running configuration, perform the following tasks:

\| 

Task

 \| 

Command

 \|
\| 

**Step 1** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Enter configuration mode from the terminal. This step is required only if you override the default remote username (see Step 2).

 \| 

configure terminal

 \|
\| 

**Step 2** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Specify the remote username. This step is optional but recommended.

 \| 

ip rcmd remote-username username

 \|
\| 

**Step 3** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Exit configuration mode.

 \| 

^Z

 \|
\| 

**Step 4** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Using rcp, copy the configuration file from a network server to the access server's running memory.

 \| 

copy rcp running-config

 \|
\| 

**Step 5** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
When prompted, enter the IP address of the server.

 \| 

ip-address

 \|
\| 

**Step 6** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
When prompted, enter the name of the configuration file.

 \| 

filename

 \|

The following example copies a host configuration file named _host1-confg_ from the _netadmin1_ directory on the remote server with an IP address of 172.108.101.101, and loads and runs that file on the access server:

Router# configure terminal 

Router(config)# ip rcmd remote-username netadmin1 

Router# copy rcp running-config 

Host or network configuration file \[host]?

Address of remote host \[255.255.255.255]? 172.108.101.101 

Name of configuration file \[Router-confg]? host1-confg 

Configure using host1-confg from 172.108.101.101? \[confirm]

Connected to 172.108.101.101

Loading 1112 byte file host1-confg:!\[OK]

%SYS-5-CONFIG: Configured from host1-config by rcp from 172.108.101.101

### Copy a Configuration File to the Startup Configuration

You can retrieve the commands stored in a configuration file on a server and write them to the startup configuration.

A host configuration file contains commands that apply to one network server in particular. A network configuration file contains commands that apply to all network servers on a network.

To copy a configuration file from an rcp server to the startup configuration, perform the following tasks:

\| 

Task

 \| 

Command

 \|
\| 

**Step 1** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Enter configuration mode from the terminal.

This step is required only if you override the default remote username (see Step 2).

 \| 

configure terminal

 \|
\| 

**Step 2** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Specify the remote username. This step is optional but recommended.

 \| 

ip rcmd remote-username username

 \|
\| 

**Step 3** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Exit configuration mode.

 \| 

^Z

 \|
\| 

**Step 4** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Using rcp, copy the configuration file from a network server to the access server's startup configuration.

 \| 

copy rcp startup-config

 \|
\| 

**Step 5** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
When prompted, enter the IP address of the network server.

 \| 

ip-address

 \|
\| 

**Step 6** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
When prompted, enter the name of the configuration file.

 \| 

filename

 \|

The following example specifies a remote username of _netadmin1_. Then it copies a host configuration file _host2-confg_ from the _netadmin1_ directory on the remote server with an IP address of 172.108.101.101 to the access server's NVRAM.

Rtr2# ip rcmd remote-username netadmin1 

Rtr2# copy rcp startup-config 

Host or network configuration file \[host]?

Address of remote host \[255.255.255.255]? 172.108.101.101 

Name of configuration file\[rtr2-confg]? host2-confg 

Configure using rtr2-confg from 172.131.101.101?\[confirm]

Connected to 172.131.101.101

Loading 1112 byte file rtr2-confg:!\[OK]

%SYS-5-CONFIG_NV:Non-volatile store configured from rtr2-config by rcp from 
172.108.101.101

### Change the Buffer Size for Loading Configuration Files

The buffer that holds the configuration commands is generally the size of NVRAM. Complex configurations might need a larger configuration file buffer size. To change the buffer size, complete the following tasks:

\| 

Task

 \| 

Command

 \|
\| 

**Step 1** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Enter configuration mode from the terminal.

 \| 

configure terminal

 \|
\| 

**Step 2** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Change the buffer size to use for booting a host or network configuration file from a network server.

 \| 

boot buffersize bytes

 \|
\| 

**Step 3** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Exit configuration mode.

 \| 

^Z

 \|
\| 

**Step 4** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Save the configuration file to your startup configuration. On most platforms, this step saves the configuration to NVRAM.

 \| 

copy running-config startup-config

 \|

In the following example, the buffer size is set to 50000 bytes:

Router1# configure terminal 

Router1(config)# boot buffersize 50000 

Router1# copy running-config startup-config 

### Compress Configuration Files

On access servers that are equipped with nonvolatile memory, you can compress configuration files. To compress configuration files, perform the following tasks:

\| 

Task

 \| 

Command

 \|
\| 

**Step 1** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Install the new ROMs.

 \| 

Refer to the appropriate hardware installation and maintenance publication.

 \|
\| 

**Step 2** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Specify that the configuration file is to be compressed.

 \| 

service compress-config

 \|
\| 

Task

 \| 

Command

 \|
\| 

**Step 3** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Enter the new configuration.

 \| 

Use TFTP or rcp to copy the new configuration. If you try to load a configuration that is more than three times larger than the nonvolatile memory size, the following error message is displayed: \[buffer overflow - file-size/buffer-size bytes].

or

configure terminal

 \|
\| 

**Step 4** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Save the new configuration.

 \| 

copy running-config startup-config

 \|

Installing new ROMs is a one-time operation, and is only necessary if you do not already have Cisco Internetwork Operating System (Cisco IOS) Release 10.2 in ROM. Before you can load a configuration file that is larger than the size of nonvolatile memory, you must issue the **service compress-config** command. The **configure terminal** command works only if you have Cisco IOS Release 10.2 boot ROMs.

### Verify the Image in Flash Memory

Before booting from Flash memory, verify that the checksum of the image in Flash memory matches the checksum listed in the README file that was distributed with the system software image. The checksum of the image in Flash memory is displayed at the bottom of the screen when you issue the **copy tftp flash**, **copy rcp flash**, or **copy rcp bootflash** commands. The README file was copied to the network server automatically when you installed the system software image on the server.

![](https://www.cisco.com/en/US/i/templates/caut.gif)

* * *

**Caution**![](https://www.cisco.com/en/US/i/templates/blank.gif)

![](https://www.cisco.com/en/US/docs/ios/11_0/access/configuration/guide/acsysimg_files/acsysimg-1.jpg)

If the checksum value does not match the value in the README file, do not reboot the router. Instead, issue the copy request and compare the checksums again. If the checksum is repeatedly wrong, copy the original system software image bootstrap image back into Flash memory before you reboot the router from Flash memory. If you have a corrupted image in Flash memory and try to boot from Flash, the router will start the system image contained in ROM (assuming booting from a network server is not configured). If ROM does not contain a fully functional system image, the router will not function and must be reconfigured through a direct console port connection.

* * *

### Display System Image and Configuration Information

Perform the following tasks in EXEC mode to display information about system software, system image files, and configuration files:

\| 

Task

 \| 

Command

 \|
\| 

List the system software release version, configuration register setting, and so on.

 \| 

show version

 \|
\| 

List the configuration information stored in nonvolatile memory.

 \| 

show startup-config

 \|
\| 

List the configuration information in running memory.

 \| 

show running-config

 \|
\| 

List information about Flash memory, including system image filenames and amounts of memory used and remaining.

 \| 

show flash

 \|
\| 

List information about Flash memory, including system image filenames, amounts of memory used and remaining, and Flash partitions.

 \| 

show flash \[all |  chips | detailed  |  err |  
partition  number  \[all |  chips | detailed  |  err ] | summary]

 \|
\| 

View the console output generated during the Flash loadhelper operation.

 \| 

show flh-log

 \|

You can also use the o command in ROM monitor mode to list the configuration register settings on some models.

The Flash memory content listing does not include the checksum of individual files. To recompute and verify the image checksum after the image is copied into Flash memory, complete the following task in EXEC mode:

\| 

Task

 \| 

Command

 \|
\| 

Recompute and verify the image checksum after the image is copied into Flash memory.

 \| 

verify flash

 \|

When you enter this command, the screen prompts you for the filename to verify. By default, it prompts for the last (most recent) file in Flash memory. Press Return to recompute the default file checksum or enter the filename of a different file at the prompt. Note that the checksum for microcode images is always 0x0000. The following example illustrates how to use this command:

Router# verify flash Name of file to verify \[gsxx]? 
Verifying via checksum... 
vvvvvvvvvvvvvvvvvvvvvvvvvvvvv 

Flash verification successful. Length = 1923712, checksum = 0xA0C1 
Router#

### Reexecute the Configuration Commands in Nonvolatile Memory

To reexecute the configuration commands in nonvolatile memory, perform the following task in EXEC mode:

\| 

Task

 \| 

Command

 \|
\| 

Reexecute the configuration commands in nonvolatile memory.

 \| 

configure memory

 \|

### Clear the Configuration Information

To clear the contents of your startup configuration, perform the following task in EXEC mode:

\| 

Task

 \| 

Command

 \|
\| 

Clear the contents of your startup configuration.

 \| 

erase startup-config

 \|

## Perform General Startup Tasks

When modifying your network environment, you perform some general startup tasks. For example, to modify a configuration file, you enter configuration mode. You also modify the configuration register boot field to tell the access server if and how to load a system image upon startup. Also, instead of using the default system image and configuration file to start up, you can specify a particular system image and configuration file that the access server uses to start up.

### General Startup Task List

General startup tasks include the following:

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Enter Global Configuration Mode and Select a Configuration Source](#wp11723)

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Modify the Configuration Register Boot Field](#wp11889)

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Specify the Startup System Image](#wp11984)

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Specify the Startup Configuration File](#wp12319)

Additionally, you can take this startup option:

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Copy a Configuration File Directly to the Startup Configuration](#wp11861)

### Enter Global Configuration Mode and Select a Configuration Source

To enter global configuration mode, enter the EXEC command **configure** at the privileged EXEC prompt. The access server responds with the following prompt asking you to specify the terminal or nonvolatile memory, or a file stored on a network server as the source of configuration commands:

Configuring from terminal, memory, or network \[terminal]?

These three methods are described in the next three sections:

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Configure the Access Server from the Terminal](#wp11750)

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Configure the Access Server from Memory](#wp11786)

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Configure the Access Server from the Network](#wp11800)

The access server accepts one configuration command per line. You can enter as many configuration commands as you want.

You can add comments to a configuration file describing the commands you have entered. Precede a comment with an exclamation point (!). Comments are _not_ stored in nonvolatile memory or in the active copy of the configuration file. In other words, comments do not appear when you list the active configuration with the **show running-config** EXEC command or list the configuration in nonvolatile memory with the **show configuration** EXEC command. Comments are stripped out of the configuration file when it is loaded to the access server. However, you can list the comments in configuration files stored on a TFTP or MOP server.

### Configure the Access Server from the Terminal

To configure the access server from the terminal, complete the following tasks:

\| 

Task

 \| 

Command

 \|
\| 

**Step 1** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
From privileged EXEC mode, enter global configuration mode and select the terminal option.

 \| 

configure terminal

 \|
\| 

**Step 2** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Enter the necessary configuration commands.

 \| 

See the appropriate chapter for specific configuration commands.

 \|
\| 

**Step 3** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Exit global configuration mode.

 \| 

Ctrl-Z

 \|
\| 

**Step 4** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Save the configuration file modifications to nonvolatile memory.

 \| 

copy running-config startup-config

 \|

In the following example, the access server is configured from the terminal. The comment "The following command provides the access server host name" identifies the purpose of the next command line. The **hostname** command changes the access server name from cs1 to cs2. By pressing Ctrl-Z, the user quits configuration mode. The command **write memory** loads the configuration changes into nonvolatile memory.

cs1(config)# !The following command provides the communication server host name. 

cs1(config)# hostname cs2 

cs2# copy running-config startup-config 

When the startup configuration is NVRAM, it stores the current configuration information in text format as configuration commands, recording only nondefault settings. The memory is checksummed to guard against corrupted data.

As part of its startup sequence, the access server startup software always checks for configuration information in nonvolatile memory. If nonvolatile memory holds valid configuration commands, the access server executes the commands automatically at startup. If the access server detects a problem with the nonvolatile memory or the configuration it contains, it enters **setup** mode and prompts for configuration. Problems can include a bad checksum for the information in nonvolatile memory or the absence of critical configuration information. See the publication _Troubleshooting Internetworking_ _Systems_ for troubleshooting procedures. See the _Access and Communication Servers Getting Started Gui\_\_de_ for details on setup.

### Configure the Access Server from Memory

You can configure the access server from nonvolatile memory by reexecuting the configuration commands stored in nonvolatile memory. To do so, complete the following task in privileged EXEC mode:

\| 

Task

 \| 

Command

 \|
\| 

Configure the access server from nonvolatile memory.

 \| 

configure memory

 \|

### Configure the Access Server from the Network

You can configure the access server by retrieving a configuration file from one of your network servers and loading it to the access server's running configuration. To do so, complete the following tasks:

\| 

Task

 \| 

Command

 \|
\| 

**Step 1** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Enter global configuration mode with the copy running-config command.

 \| 

copy \[tftp | rcp] running-config

 \|
\| 

**Step 2** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
At the system prompt, select a host or network configuration file. The network configuration file contains commands that apply to all network servers and terminal servers on the network. The host configuration file contains commands that apply to one network server in particular.

 \| 

host or network

 \|
\| 

**Step 3** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
At the system prompt, enter the optional IP address of the remote host from which you are retrieving the configuration file.

 \| 

ip-address

 \|
\| 

**Step 4** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
At the system prompt, enter the name of the configuration file or accept the default name.

 \| 

filename

 \|
\| 

**Step 5** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Confirm the configuration filename that the system supplies.

 \| 

y

 \|

In the following example, the access server is configured from the file _tokyo-config_ at IP address 172.30.2.155:

Router# copy tftp running-config 

Host or network configuration file \[host]?

IP address of remote host \[255.255.255.255]? 172.30.2.155 

Name of configuration file \[tokyo-confg]?

Configure using tokyo-confg from 172.30.2.155? \[confirm] y 

Booting tokyo-confg from 172.30.2.155:!! \[OK - 874/16000 bytes]

### Copy a Configuration File Directly to the Startup Configuration

You can copy a configuration file directly to your startup configuration without affecting the running configuration. This task loads a configuration file directly into NVRAM without affecting the running configuration.

To copy a configuration file directly to the startup configuration, perform the following task in EXEC mode:

\| 

Task

 \| 

Command

 \|
\| 

Load a configuration file directly into NVRAM without affecting the running configuration.

 \| 

copy \[tftp | rcp] startup-config

 \|

### Modify the Configuration Register Boot Field

The configuration register boot field determines whether or not the router loads an operating system image, and if so, where it obtains this system image. The following sections describe the access server's process for using the configuration register boot field, your process for setting this field, and the tasks you must perform to modify the configuration register boot field.

### How the Access Server Uses the Boot Field

The lowest four bits of the 16-bit configuration register (bits 3, 2, 1, and 0) form the boot field. Bit zero (0) of the boot field specifies whether or not the access server loads an operating system image, according to the following criteria:

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
When bit zero equals 0-0-0-0, the access server does not load a system image. Instead, the access server enters ROM monitor or "maintenance" mode from which you can enter ROM monitor commands to manually load a system image.

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
When bit zero equals 0-0-0-1, the access server loads a system image.

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
When the entire boot field equals a value between 0-0-1-0 and 1-1-1-1, the access server loads the system image specified by **boot system** commands in the startup configuration file. When the startup configuration file does not contain **boot system** commands, the access server loads a default system image stored on a network server.

When loading a default system image from a network server, the router uses the configuration register settings to determine the default system image filename for booting from a network server. The router forms the default boot filename by starting with the word _cisco_ and then appending the octal equivalent of the boot field number in the configuration register, followed by a hyphen (-) and the processor type name (cisconn-cpu). See the appropriate hardware installation guide for details on the configuration register and default filename.

### Setting the Boot Field

You must correctly set the configuration register boot field to ensure that your access server loads the operating system image as you intend. To set the boot field, follow this general procedure:

* * *

**Step 1** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Obtain the current configuration register setting. This setting is a hexadecimal value.

**Step 2** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Modify the current configuration register setting to reflect the way in which you want the access server to load a system image. To do so, change the least significant hexadecimal digit to one of the following:

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
0 to load the system image manually using the **b** command in ROM monitor mode.

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
1 to load the system image from boot ROMs.

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
2-F to load the system image from **boot system** commands in the startup configuration file or from a default system image stored on a network server.

For example, if the current configuration register setting is 0x2010 and you want to load a system image from boot ROMs rather than manually from ROM monitor mode, you would change the configuration register setting to 0x2011.

**Step 3** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Reboot the access server to make your changes to the configuration register take effect.

### Perform the Boot Field Modification Tasks

For the ASM-CS and Cisco 2500 series running Software Release 9.1 or later, you can change the configuration register boot field by completing the following tasks:

\| 

Task

 \| 

Command

 \|
\| 

**Step 1** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Obtain the current configuration register setting.

 \| 

show version

 \|
\| 

**Step 2** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Enter global configuration mode and select the terminal option.

 \| 

configure terminal

 \|
\| 

**Step 3** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Modify the default configuration register setting.

 \| 

config-register value

 \|
\| 

**Step 4** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Exit global configuration mode.

 \| 

Ctrl-Z

 \|
\| 

**Step 5** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Reboot the router to make your changes take effect.

 \| 

reload

 \|

In the following example, the **show version** command indicates that the current configuration register is set so that the access server does not automatically load an operating system image. Instead, it enters ROM monitor mode and waits for user-entered ROM monitor commands. The new setting instructs the access server to a load a system image from commands in the startup configuration file or from a default system image stored on a network server.

GS Software, Version 9.0(1)

Copyright (c) 1986-1992 by cisco Systems, Inc.

Compiled Fri 14-Feb-92 12:37

System Bootstrap, Version 4.3

Router1 uptime is 2 days, 10 hours, 0 minutes

System restarted by reload

System image file is unknown, booted via tftp from 131.108.13.111

Host configuration file is "thor-boots", booted via tftp from 131.108.13.111

Network configuration file is "network-confg", booted via tftp from

CSC3 (68020) processor with 4096K bytes of memory.

1 MCI controller (2 Ethernet, 2 Serial).

2 Ethernet/IEEE 802.3 interface.

2 Serial network interface.

32K bytes of non-volatile configuration memory.

Configuration register is 0x0

Router1# configure terminal 

Router1(config)# config-register 0xF 

### Specify the Startup System Image

You can enter multiple boot commands in nonvolatile memory configuration to provide backup methods for loading a system image onto the access server. There are three ways to load a system image:

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
From Flash memory—Flash allows you to copy new system images without changing erasable programmable read-only memory (EPROM). Information stored in Flash is not vulnerable to network failures that might occur when loading system images from servers.

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
From a network server—In case Flash memory becomes corrupted, specifying a system image to be loaded from a network server using TFTP, rcp, or MOP provides a backup boot method for the access server.

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
From ROM—In case of both network failure and Flash memory corruption, specifying a system image to be loaded from ROM provides a final backup boot method. System images stored in ROM may not always be as complete as those stored in Flash memory or on network servers.

You can enter the different types of boot commands in any order in nonvolatile memory configuration. If you enter multiple boot commands, the access server tries them in the order they are entered.

### Load from Flash Memory

Flash memory is available for the ASM-CS. With a CSC-MC+ Flash memory card and CSC-MCI controller and appropriate cables, system software images can be written to Flash memory for booting. Depending on the hardware platform, Flash memory might be available as EPROMs, single in-line memory modules (SIMMs), or memory cards. Check the appropriate hardware installation and maintenance guide for information about types of Flash memory available on a specific platform.

Software images can be stored, booted, and rewritten into Flash memory as necessary. Flash memory can reduce the effects of network failure by reducing dependency on files that can only be accessed over the network.

Flash memory allows you to do the following:

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
Copy the system image to Flash memory using TFTP.

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
Copy the system image to Flash memory using rcp.

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
Boot an access server from Flash memory either automatically or manually.

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
Copy the Flash memory image to a network server using TFTP or rcp.

![](https://www.cisco.com/en/US/i/templates/note.gif)

* * *

**Note**![](https://www.cisco.com/en/US/i/templates/blank.gif)
Use of Flash memory is subject to the terms and conditions of the software license agreement that accompanies your product.

* * *

Flash memory features include the following:

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
Flash memory can be remotely loaded with multiple system software images through TFTP or rcp transfers (one transfer for each file loaded).

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
You can boot an access server manually or automatically from a system software image stored in Flash memory. You can also boot directly from ROM, or you can boot from a network server using TFTP or rcp.

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
Flash memory provides write protection against accidental erasing or reprogramming.

![](https://www.cisco.com/en/US/i/templates/note.gif)

* * *

**Note**![](https://www.cisco.com/en/US/i/templates/blank.gif)
Booting from ROM is faster than booting from Flash memory. However, if you are booting from a network server, Flash memory is faster and more reliable.

* * *

### Security Precautions

Take the following precautions when loading from Flash memory:

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
Flash memory provides write protection against accidental erasing or reprogramming.

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
Flash memory cards contain a write protect switch that you can use to protect data. You must set the switch to _unprotected_ to write data to the Flash memory card.

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
The system image stored in Flash memory can be changed only from a privileged EXEC command session on the console terminal.

### Flash Memory Configuration

The following list is an overview of how to configure your system to boot from Flash memory. It is not a step-by-step set of instructions; rather, it is an overview of the process of using the Flash capability.

Refer to the appropriate Cisco hardware installation and maintenance publication for complete instructions for installing the hardware and for information about the jumper settings required for your configuration.

* * *

**Step 1** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Perform _one_ of the following:

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
Partition Flash memory or use Flash load helper to allow the system to run from Flash memory while you copy to it. See the "[Partition Flash Memory Using Dual Flash Bank](#wp13288)"and"[Use Flash Load Helper to Upgrade Software on Run-from-Flash Systems](#wp13521)" sections for more information.

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
Set the system to load and run a system image from boot ROMs. See the "[Modify the Configuration Register Boot Field](#wp11889)" section for more information.

**Step 2** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
If you ran the image from boot ROMs, reload the system image.

**Step 3** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Copy a system image to Flash memory using TFTP or rcp. See the "[Copy System Images from a Network Server to Flash Memory](#wp11017)" section for more information on performing this step.

**Step 4** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Configure the system to automatically boot from the desired file in Flash memory. You might need to change the configuration register value. See the "[Modify the Configuration Register Boot Field](#wp11889)" section for more information on the modifying configuration register.

**Step 5** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Save your configurations.

**Step 6** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Power-cycle and reboot your system to ensure that all is working as expected.

### Configure the Access Server to Automatically Boot from an Image in Flash Memory

Once you have successfully configured Flash memory, you might want to configure the system with the **no boot system flash** command to revert back to booting from ROM.

To configure the access server to boot automatically from an image in Flash memory, complete the following tasks:

\| 

Task

 \| 

Command

 \|
\| 

**Step 1** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Enter global configuration mode from the terminal.

 \| 

configure terminal

 \|
\| 

**Step 2** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Enter the filename of an image stored in Flash memory.

 \| 

boot system flash \[filename]

 \|
\| 

**Step 3** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Set the configuration register to enable loading of the system image from Flash memory.

 \| 

config-register value

 \|
\| 

**Step 4** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Exit global configuration mode.

 \| 

Ctrl-Z

 \|
\| 

**Step 5** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Save the configuration information to nonvolatile memory.

 \| 

copy running-config startup-config

 \|
\| 

**Step 6** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Power-cycle and reboot the system to ensure that all works as expected.

 \| 

reload

 \|

Automatically booting from Flash memory requires changing the processor's configuration register. See the section entitled"Modifying the Configuration Register Boot Field" earlier in this chapter. Use the **show version** command to list the current configuration register setting.

If you enter more than one image filename, the access server tries them in the order entered.

If a filename already appears in the configuration file and you want to specify a new filename, remove the existing filename with the **no** **boot system flash** _filename_ command.

![](https://www.cisco.com/en/US/i/templates/note.gif)

* * *

**Note**![](https://www.cisco.com/en/US/i/templates/blank.gif)
The **no boot system** configuration command disables  all **boot system** configuration commands regardless of argument. Specifying the **flash** keyword or the _filename_ argument with the **no boot system** command disables only the commands specified by these arguments.

* * *

The following example shows how to configure the access server to automatically boot from an image in Flash memory on a Cisco 2500:

Router# configure terminal 

Enter configuration commands, one per line.  End with CNTL/Z.

Router(config)# boot system flash igs-bfpx.102.1 

Router(config)# config-register 0x2 

Router# copy running-config startup-config 

System Bootstrap, Version (3.3), SOFTWARE

Copyright (c) 1986-1995 by Cisco Systems

2500 processor with 1024 Kbytes of main memory

Booting igs-bfpx.102.1 from Flash address space

F3: 3911536+96836+319604 at 0x3000060

Use, duplication, or disclosure by the Government is

subject to restrictions as set forth in subparagraph

(c) of the Commercial Computer Software - Restricted

Rights clause at FAR sec. 52.227-19 and subparagraph

(c) (1) (ii) of the Rights in Technical Data and Computer

Software clause at DFARS sec. 252.227-7013.

                 San Jose, California 95134-1706

3000 Software (IGS-BFPX), Version 10.2

Copyright (c) 1986-1994 by Cisco Systems, Inc.

Compiled Tue 05-Jul-94 16:14

% System running from device (System flash) being initialized.

   Setting System flash access to read-only.

SNMP Research SNMP Agent Resident Module Version 12.2.0.0

Copyright 1989, 1990, 1991, 1992, 1993, 1994 SNMP Research, Inc.

cisco 2500 (68030) processor (revision A) with 1024K/1024K bytes of memory.

Processor board serial number 01244583

X.25 software, Version 2.0, NET2, BFE and GOSIP compliant.

SuperLAT software (copyright 1990 by Meridian Technology Corp).

Authorized for Enterprise software set.  (0x0)

1 Ethernet/IEEE 802.3 interface.

2 Serial network interfaces.

32K bytes of non-volatile configuration memory.

4096K bytes of processor board System flash. (Read only mode)

Press RETURN to get started!

### Load from a Network Server

You can configure the access server to load a system image from a network server using TFTP, rcp, or MOP to copy the system image file.

To do this, the configuration register boot field must be set to the correct value. See "Modify the Configuration Register Boot Field" later in this chapter. Use the **show version** command to list the current configuration register setting.

If you do not boot from a network server using MOP and you do not specify the TFTP or  rcp server, by default the system image that you specify is booted from a network server using TFTP.

![](https://www.cisco.com/en/US/i/templates/note.gif)

* * *

**Note**![](https://www.cisco.com/en/US/i/templates/blank.gif)
If you are using a Sun workstation as a network server and TFTP to transfer the file, set up the workstation to enable verification and generation of UDP checksums. See the Sun documentation for details.

* * *

For increased performance and reliability, boot from a system image from a network server using rcp. The rcp protocol implementation uses the Transmission Control Protocol (TCP), which ensures reliable delivery of data.

If you boot the access server from a network server using rcp, the Cisco IOS software searches for the system image on the server relative to the directory of the remote username.You cannot explicitly specify a remote username when you issue the boot command. Instead, the host name configured for the access server is used.

You can also boot from a compressed image on a network server. One reason to use a compressed image is to ensure that there is enough memory available for storage. On access servers that do not contain a run-from-ROM image in EPROM, when the access server boots software from a network server, the image being booted and the running image both must fit into memory. If the running image is large, there might not be room in memory for the image being booted from the network server.

If there is not enough room in memory to boot a regular image from a network server, you can produce a compressed software image on any UNIX platform using the **compress** command. Refer to your UNIX platform's documentation for the exact usage of the **compress** command.

To specify the loading of a system image from a network server, complete the following tasks.

\| 

Task

 \| 

Command

 \|
\| 

**Step 1** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Enter global configuration mode from the terminal.

 \| 

configure terminal

 \|
\| 

**Step 2** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Specify the system image file to be booted from a network server using TFTP, rcp, or MOP server.

 \| 

boot system \[tftp | rcp] filename \[ip-address]  
boot system mop filename \[mac-address] \[interface]

 \|
\| 

**Step 3** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Set the configuration register to enable loading of the system image from a network server.

 \| 

config-register value

 \|
\| 

**Step 4** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Exit global configuration mode.

 \| 

Ctrl-Z

 \|
\| 

**Step 5** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Write the configuration information to nonvolatile memory.

 \| 

copy running-config startup-config

 \|

In the following example, the access server is configured to use rcp to netboot from the _testme5.tester_ system image file on a network server at IP address 172.30.0.1:

Router# configure terminal 

Router(config)# boot system rcp testme5.tester 172.30.0.1 

Router# copy running-config startup-config

### Load from ROM

To specify the use of the ROM system image as a backup to other boot instructions in the configuration file, complete the following tasks:

\| 

Task

 \| 

Command

 \|
\| 

**Step 1** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Enter global configuration mode from the terminal.

 \| 

configure terminal

 \|
\| 

**Step 2** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Specify use of the ROM system image as a backup image.

 \| 

boot system rom

 \|
\| 

**Step 3** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Set the configuration register to enable loading of the system image from ROM.

 \| 

config-register value  

 \|
\| 

**Step 4** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Exit global configuration mode.

 \| 

Ctrl-Z

 \|
\| 

**Step 5** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Save the configuration information to nonvolatile memory.

 \| 

copy running-config startup-config

 \|

In the following example, the access server is configured to boot a Flash image called _image1_ first. If the Flash image fails, the access server boots the configuration file _backup1_ from a network server. If this method fails, then the system will boot from ROM.

Router# configure terminal 

Router(config)# boot system flash image1 

Router(config)# boot system backup1 172.30.20.4 

Router(config)# boot system rom 

Router# copy running-config startup-config

### Use a Fault-Tolerant Boot Strategy

Occasionally, network failures make netbooting impossible. To lessen the effects of network failure, consider the following boot strategy. After Flash is installed and configured, you might want to configure the access server to boot in the following order:

**1** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Boot an image from Flash.

**2** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Boot an image from a system filename (netboot).

**3** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Boot from ROM image.

This boot order provides the most fault-tolerant alternative in the netbooting environment. Use the following commands in your configuration to allow you to boot first from Flash, then from a system file, and finally from ROM:

\| 

Task

 \| 

Command

 \|
\| 

**Step 1** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Enter global configuration mode from the terminal.

 \| 

configure terminal

 \|
\| 

**Step 2** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Configure the access server to boot from Flash memory.

 \| 

boot system flash  \[filename]

 \|
\| 

**Step 3** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Configure the access server to boot from a system filename.

 \| 

boot system \[rcp  | tftp]  filename \[ip-address]

 \|
\| 

**Step 4** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Configure the access server to boot from ROM.

 \| 

boot system rom

 \|
\| 

**Step 5** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Set the configuration register to enable loading of the system image from a network server or Flash.

 \| 

config-register value

 \|
\| 

**Step 6** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Exit global configuration mode.

 \| 

Ctrl-Z

 \|
\| 

**Step 7** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Save the configuration information to nonvolatile memory.

 \| 

copy running-config startup-config

 \|

The order of the commands needed to implement this strategy is shown in the following example:

Router# configure terminal 

Router(config)# boot system flash gsxx 

Router(config)# boot system gsxx  172.30.101.101 

Router(config)# boot system rom 

Router# copy running-config startup-config 

Using this strategy, an access server used primarily in a netbooting environment has three alternative sources from which to boot. These alternative sources help lessen the negative effects of a failure with the TFTP file server and of the network in general.

### Specify the Startup Configuration File

Configuration files can be stored on network servers. You can configure the access server to automatically request and receive two configuration files from the network server:

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Download the Network Configuration File](#wp12328)

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Download the Host Configuration File](#wp12363)

The first file the server attempts to load is the network configuration file. The  network configuration file contains information that is shared among several access servers. For example, it can be used to provide mapping between IP addresses and host names.

The second file the server attempts to load is the _host configuration_ file. This file contains commands that apply to one access server in particular. Both the network and host configuration files must reside on a network server reachable using TFTP, rcp, or MOP and be readable.

You can specify an ordered list of network configuration and host configuration filenames. The access server scans this list until it successfully loads the appropriate network or host configuration file.

### Download the Network Configuration File

To configure the access server to download a network configuration file from a server upon restart, complete the following tasks.

\| 

Task

 \| 

Command

 \|
\| 

**Step 1** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Enter global configuration mode from the terminal.

 \| 

configure terminal

 \|
\| 

**Step 2** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Enter the network configuration filename.

 \| 

boot network mop filename \[mac-address] \[interface]  
boot network \[tftp | rcp] filename \[ip-address]

 \|
\| 

**Step 3** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Enable the access server to automatically load the network file upon restart.

 \| 

service config

 \|
\| 

**Step 4** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Exit global configuration mode.

 \| 

Ctrl-Z

 \|
\| 

**Step 5** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Save the configuration information to nonvolatile memory.

 \| 

copy running-config startup-config

 \|

For Step 2, if you do not specify a network configuration filename, the access server uses the default filename _network-confg_. If you do not specify a TFTP or rcp server, the access server assumes that you are using TFTP to transfer the file and that the server whose IP address you specify supports TFTP.

If you configure the access server to download the network configuration file from a network server using rcp, the Cisco IOS software searches for the system image on the server relative to the directory of the remote username. The access server host name is used as the remote username.

You can specify more than one network configuration file. The access server tries each of them in order until it loads one successfully. This procedure can be useful for keeping files with different configuration information loaded on a network server.

### Download the Host Configuration File

To configure the access server to download a host configuration file from a server upon restart, complete the following tasks. Step 2 is optional. If you do not specify a host configuration filename, the access server uses its own name to form a host configuration filename by converting the access server name to all lowercase letters, removing all domain information, and appending _-confg_. If no host name information is available, the access server uses the default host configuration filename _cs-confg_.

\| 

Task

 \| 

Command

 \|
\| 

**Step 1** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Enter global configuration mode from the terminal.

 \| 

configure terminal

 \|
\| 

**Step 2** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Optionally, enter the host configuration filename.

 \| 

boot host mop filename \[mac-address] \[interface]  
boot host \[tftp | rcp] filename \[ip-address]

 \|
\| 

**Step 3** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Enable the access server to automatically load the host file upon restart.

 \| 

service config

 \|
\| 

**Step 4** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Exit global configuration mode.

 \| 

Ctrl-Z

 \|
\| 

**Step 5** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Save the configuration information to nonvolatile memory.

 \| 

copy running-config startup-config

 \|
\| 

**Step 6** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Reset the access server with the new configuration information.

 \| 

reload

 \|

You can specify more than one host configuration file. The access server tries them in order until it loads one successfully. This procedure can be useful for keeping files with different configuration information loaded on a network server.

In the following example, the access server is configured to boot from the host configuration file _hostfile1_ and from the network configuration file _networkfile1_:

Router# configure terminal 

Router(config)# boot host hostfile1 

Router(config)# boot network networkfile1 

Router(config)# service config 

Router# copy running-config startup-config 

If the network server fails to load a configuration file during startup, it tries again every ten minutes (default setting) until a host provides the requested files. With each failed attempt, the network server displays a message on the console terminal. If the network server is unable to load the specified file, it displays the following message:

Booting host-confg... \[timed out]

Refer to the Troubleshooting Internetworking Systems publication for troubleshooting procedures. If there are any problems with the configuration file pointed to in nonvolatile memory, or the configuration register is set to ignore nonvolatile memory, the access server will enter the **setup** command facility. See the _Access and Communication Servers Getting Started Guide_ for details on the **setup** command.

## Store System Images and Configuration Files

After modifying and saving your access server's unique configurations, you can store them on a network server to use as backup copies.

### Store System Images and Configuration Files Task List

To store system images and configuration files, perform the following tasks:

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Copy System Images from Flash Memory to a Network Server](#wp12589)

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Copy Configuration Files from the Access Server to a Network Server](#wp12512)

### Copy System Images from Flash Memory to a Network Server

You can copy system images from Flash memory to a TFTP server or to an rcp server. You can use this server copy of the system image as a backup copy, or you can use it to verify that the copy in Flash is the same as the original file on disk. The following sections describe these tasks:

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Copy from Flash Memory to a TFTP Server](#wp12443)

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Copy from Flash Memory to an rcp Server](#wp12607)

### Copy from Flash Memory to a TFTP Server

You can use TFTP to copy a system image back to a network server. This copy of the system image can serve as a backup copy and also can be used to verify that the copy in Flash is the same as on the original file on disk. In some implementations of TFTP, you must first create a "dummy" file on the TFTP server and give it read, write, and execute permissions before copying a file over it. Refer to your TFTP documentation for more information.To copy the system image to a network server, perform the following task:

\| 

Task

 \| 

Command

 \|
\| 

**Step 1** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Determine the exact spelling of the system image in Flash memory.

 \| 

show flash \[all]

 \|
\| 

**Step 2** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Copy the system image in Flash memory to a TFTP server.

 \| 

copy flash tftp

 \|
\| 

**Step 3** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
When prompted, enter the IP address or domain name of the TFTP server.

 \| 

ip-address or name

 \|
\| 

**Step 4** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
When prompted, enter the filename of the system image in Flash memory.

 \| 

filename

 \|

The following example uses the **show flash all** command to determine the name of the system image file and the **copy flash tftp** command to copy the system image to a TFTP server. The name of the system image file (_xk09140z_) is listed near the end of the **show flash all** output.

        addr     length    fcksum  ccksum

        0x40     4008404   0x35B3  0x35B3

\[4008468 bytes used, 185836 bytes available]

4096K bytes of processor board System flash. (Read only mode)

System flash chips could not be identified.

Check the Vpp (12V) jumper installation (if present)

and/or the chips/SIMMs installed.

Flash chips supported by system :

   Code   Chip-Sz     Cmd-grp   Chip-name

   89B4   0x20000        1      INTEL 28F010

   89BD   0x40000        1      INTEL 28F020

   01A7   0x20000        1      AMD   28F010

   012A   0x40000        1      AMD   28F020

   1CD0   0x40000        1      M5M   28F101P

   89A2   0x100000       2      INTEL 28F008SA

2048K bytes of flash memory on embedded flash (in XX).

   ROM   socket    code    bytes       name

    0      U42     89BD   0x40000     INTEL 28F020

    1      U44     89BD   0x40000     INTEL 28F020

    2      U46     89BD   0x40000     INTEL 28F020

    3      U48     89BD   0x40000     INTEL 28F020

    4      U41     89BD   0x40000     INTEL 28F020

    5      U43     89BD   0x40000     INTEL 28F020

    6      U45     89BD   0x40000     INTEL 28F020

    7      U47     89BD   0x40000     INTEL 28F020

  security jumper(12V) is installed,

  flash memory is programmable.

  \[903848/2097152 bytes free]

IP address of remote host \[255.255.255.255]? 172.30.13.110

filename to write on tftp host? igs-bfpx.102.1 

To stop the copy process, press **Ctrl-^**. Refer to the Troubleshooting Internetworking Systems publication for procedures on how to resolve Flash memory problems.

Once you have configured Flash memory, you might want to configure the system (using the **configure terminal** command) with the **no boot system flash** configuration command to revert to booting from ROM (for example, if you do not yet need this functionality, if you choose to netboot, or if you do not have the proper image in Flash memory). After you enter the **no boot system flash** command, use the **copy running-config startup-config** command to save the new configuration command to nonvolatile memory.

### Copy from Flash Memory to an rcp Server

You can also copy a system image from Flash memory to an rcp network server.

The rcp protocol requires that a client send the remote username on each rcp request to the server. When you copy an image from Flash memory to a network server using rcp, the Cisco IOS software sends the remote username associated with the current TTY (terminal) process, if that name is valid. If the TTY remote username is invalid, the software uses the host name as both the remote and local usernames.

![](https://www.cisco.com/en/US/i/templates/note.gif)

* * *

**Note**![](https://www.cisco.com/en/US/i/templates/blank.gif)
For Cisco, TTYs are commonly used in communication servers. The concept of TTY originated with UNIX. For UNIX systems, each physical device is represented in the file system. Terminals are called _TTY devices_, which stands for _teletype_, the original UNIX terminal.

* * *

You can configure a different remote username to be sent to the server. The rcp protocol implementation writes the system image relative to the directory associated with the remote username on the network server, if the server has a directory structure (for example, UNIX directory structures).

For the rcp command to execute properly, an account must be defined on the destination server for the remote username.

To stop the copy process, press **Ctrl**-**^**. Refer to the Troubleshooting Internetworking Systems publication for procedures on how to resolve Flash memory problems.

If you copy the system image to a personal computer used as a file server, the computer must support the rcp protocol.

To copy the system image from Flash memory to a network server, perform the following tasks:

\| 

Task

 \| 

Command

 \|
\| 

**Step 1** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
(Optional) If you do not already know it, determine the exact spelling of the system image in Flash memory.

 \| 

show flash all

 \|
\| 

**Step 2** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Enter configuration mode from the terminal. This step is required only if you are going to override the default remote username (see step 2).

 \| 

configure terminal

 \|
\| 

**Step 3** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Specify the remote username. This step is optional but recommended.

 \| 

ip rcmd remote-username username

 \|
\| 

**Step 4** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Exit configuration mode.

 \| 

^Z

 \|
\| 

**Step 5** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Using rcp, copy the system image in Flash memory to a network server.

 \| 

copy flash rcp

 \|
\| 

**Step 6** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
When prompted, enter the IP address or domain name of the rcp server.

 \| 

ip-address or name

 \|
\| 

**Step 7** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
When prompted, enter the filename of the system image in Flash memory.

 \| 

filename

 \|

The following example copies the system image _gsxx_ to a network server using rcp:

Router# configure terminal 

Router(config)# ip rcmd remote-username netadmin1 

^Z Router# copy flash rcp 

File name/status 
 1 gsxx 
\[2076072 bytes used, 21080 bytes available]

Name of file to copy? gsxx 

Address or name of remote host \[UNKNOWN]? 131.108.1.111 

File name to write to? gsxx 

Verifying checksum for \`gsxx' (file # 1)...\[OK] 

The spinning star (represented by a dash in the sample output) indicates that the copy process is taking place.

### Copy Configuration Files from the Access Server to a Network Server

You can copy configuration files from the access server to a TFTP server or rcp server. You can do this to back up a current configuration file to a server before changing its contents, thereby allowing you to later restore the original configuration file from the server. The following sections describe these tasks:

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Copy from the Access Server to a TFTP Server](#wp12740)

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Copy from the Access Server to an rcp Server](#wp12779)

### Copy from the Access Server to a TFTP Server

Usually, the configuration file that you copy to must already exist on the TFTP server and be globally writable before the TFTP server allows you to write to it.

To store configuration information on a TFTP network server, complete the following tasks in the EXEC mode:

\| 

Task

 \| 

Command

 \|
\| 

**Step 1** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Specify that the running or startup configuration file be stored on a network server.

 \| 

copy running-config tftp

or

copy startup-config tftp

 \|
\| 

**Step 2** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Enter the IP address of the network server.

 \| 

ip-address

 \|
\| 

**Step 3** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Enter the name of the configuration file to store on the server.

 \| 

filename

 \|
\| 

**Step 4** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Confirm the entry.

 \| 

y

 \|

The command prompts you for the destination host's address and a filename, as the following example illustrates.

The following example copies a configuration file from an access server to a TFTP server:

Tokyo# copy running-config tftp 

Remote host \[172.108.2.155]?

Name of configuration file to write \[tokyo-confg]?

Write file tokyo-confg on host 172.108.2.155? \[confirm] y 

Writing tokyo-confg - \[OK]

### Copy from the Access Server to an rcp Server

You can use rcp to copy configuration files from the local access server to a network server. You can copy a running configuration file or a startup configuration file to the server.

The rcp protocol requires that a client send the remote username on each rcp request to a server. When you issue a command to copy a configuration file from the access server to a server using rcp, the router sends a default remote username unless you override the default by configuring a remote username. As the default value of the remote username, the Cisco IOS software sends the remote username associated with the current TTY (terminal) process, if that name is valid.

![](https://www.cisco.com/en/US/i/templates/note.gif)

* * *

**Note**![](https://www.cisco.com/en/US/i/templates/blank.gif)
For UNIX systems, each physical device is represented in the file system. Terminals are called _TTY devices_, which stands for _teletype_, the original UNIX terminal.

* * *

If the TTY remote username is invalid, the Cisco IOS software uses the host name as both the remote and local usernames. The rcp protocol implementation writes the configuration file relative to the directory associated with the remote username on the server, if the server has a directory structure, as do UNIX systems.

For the rcp copy request to execute successfully, an account must be defined on the network server for the remote username.

If you copy the configuration file to a personal computer used as a file server, the computer must support rcp.

To copy a startup configuration file or a running configuration file from the router to an rcp server, use one of following tasks:

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Copy a Running Configuration File to an rcp Server](#wp12797)

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Copy a Startup Configuration File to an rcp Server](#wp12846)

### Copy a Running Configuration File to an rcp Server

You can copy the running configuration file to an rcp server. The copied file can serve as a backup configuration file.

To store a running configuration file on a server, complete the following tasks:

\| 

Task

 \| 

Command

 \|
\| 

**Step 1** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Enter configuration mode from the terminal.

This step is required only if you override the default remote username (see step 2).

 \| 

configure terminal

 \|
\| 

**Step 2** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Specify the remote username. This step is optional. but recommended.

 \| 

ip rcmd remote-username username

 \|
\| 

**Step 3** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Specify that the access server's running configuration file is stored on a network server.

 \| 

copy running-config rcp

 \|
\| 

**Step 4** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Enter the IP address of the network server.

 \| 

ip-address

 \|
\| 

**Step 5** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Enter the name of the configuration file to store on the server.

 \| 

filename

 \|
\| 

**Step 6** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Confirm the entry.

 \| 

y

 \|

The following example copies the running configuration file named _Rtr2-confg_ to the _netadmin1_ directory on the remote host with an IP address of 172.108.101.101:

Rtr2# ip rcmd remote-username netadmin1 

Rtr2# copy running-config rcp 

Remote host\[]? 172.108.101.101 

Name of configuration file to write \[Rtr2-confg]?

Write file rtr2-confg on host 172.108.101.101?\[confirm]

Connected to 172.108.101.101

### Copy a Startup Configuration File to an rcp Server

You can copy the contents of the startup configuration file to an rcp server. The copied file can be used as a backup configuration file.

To copy a startup configuration file to a network server using rcp, complete the following tasks:

\| 

Task

 \| 

Command

 \|
\| 

**Step 1** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Enter configuration mode from the terminal.

This step is required only if you override the default remote username (see step 2).

 \| 

configure terminal

 \|
\| 

**Step 2** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Specify the remote username. This step is optional. but recommended.

 \| 

ip rcmd remote-username username

 \|
\| 

**Step 3** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Specify that the startup configuration file is copied to an rcp server. This step copies the configuration from NVRAM to the rcp server.

 \| 

copy startup-config rcp

 \|
\| 

**Step 4** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Enter the IP address of the network server.

 \| 

ip-address

 \|
\| 

**Step 5** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Enter the name of the configuration file to store on the server.

 \| 

filename

 \|
\| 

**Step 6** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Confirm the entry.

 \| 

y

 \|

The following example shows how to store a startup configuration file on a server by using rcp to copy the file:

Rtr2# ip rcmd remote-username netadmin2 

Rtr2# copy startup-config rcp 

Remote host\[]? 172.108.101.101 

Name of configuration file to write \[rtr2-confg]?

Write file rtr2-confg on host 172.108.101.101?\[confirm]

## Configure the Access Server as a Network Server

You can configure an access server as a network server to cut costs and time delays in your network. Typically, the access server configured as a server delivers operating system images from its Flash memory to other access servers or routers. You can also configure the access server to respond to other types of service requests, such as Reverse Address Resolution Protocol (RARP).

### Configure an Access Server as a Network Server Task List

To configure the router as a server, perform any of the following tasks. The tasks are not mutually exclusive.

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Configure an Access Server as a TFTP Server](#wp12990)

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Configure Flash Memory as a TFTP Server](#wp13079)

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Configure an Access Server as a RARP Server](#wp13040)

### Configure an Access Server as a TFTP Server

As a TFTP server host, the access server responds to TFTP Read Request messages by sending a copy of the system image contained in ROM or one of the system images contained in Flash to the requesting host. The TFTP Read Request message must use one of the filenames that are specified in the access server's configuration.

You can specify multiple filenames by repeating the **tftp-server** command. The system sends a copy of the system image contained in ROM or one of the system images contained in Flash memory to any host that issues a TFTP read request with this filename.

If the specified _filename1_ or _filename2_ exists in Flash memory, a copy of the Flash image is sent. On systems that contain a complete image in ROM, the system sends the ROM image if the specified _filename1_ or _filename2_ is not found in Flash memory.

Images that run from ROM cannot be loaded over the network. Therefore, do not use TFTP to offer the ROMs on these images.

To specify TFTP server operation for an access server, complete the following tasks:

\| 

Task

 \| 

Command

 \|
\| 

**Step 1** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Enter global configuration mode from the terminal.

 \| 

configure terminal

 \|
\| 

**Step 2** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Specify TFTP server operation.

 \| 

tftp-server flash  \[partition-number:] filename1 \[alias filename2] \[access-list-number]

 \|
\| 

**Step 3** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Exit global configuration mode.

 \| 

Ctrl-Z

 \|
\| 

**Step 4** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Save the configuration information to nonvolatile memory.

 \| 

copy running-config startup-config

 \|

The TFTP session can sometimes fail. TFTP generates the following special characters to help you determine why a TFTP session fails:

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
"E" character indicates that the TFTP server received an erroneous packet

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
"O" character indicates that the TFTP server received an out-of-sequence packet

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
A period (.) indicates a timeout

The transfer session might still succeed even if TFTP generates these characters, but the output is useful for diagnosing the transfer failure. For troubleshooting procedures, refer to the _Troubleshooting Internetworking Systems_ publication_._

In the following example, the system uses TFTP to send a copy of the _version-10.3_ file located in Flash memory in response to a TFTP Read Request for that file. The requesting host is checked against access list 22.

tftp-server flash version-10.3 22

### Configure Flash Memory as a TFTP Server

Flash memory can be used as a TFTP file server for other access servers on the network. This feature allows you to boot a remote access server with an image that resides in the Flash server memory.

In the description that follows, one access server is referred to as the Flash server, and all other access servers are referred to as client access servers. Examples of configurations for the Flash server and client access servers include commands as necessary.

### Configure Flash Memory as a TFTP Server Task List

To configure Flash memory as a TFTP server, perform the following tasks:

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Perform Prerequisite Tasks](#wp13106)

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Configure the Flash Server](#wp13120)

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Configure the Client Access Server](#wp13148)

### Perform Prerequisite Tasks

The Flash server and client access server must be able to reach one another before the TFTP function can be implemented. Verify this connection by pinging between the Flash server and client access server (in either direction) using the **ping** command, as in the following example:

router# ping 172.30.101.101 <Return> 

In this example, the IP address of 172.30.101.101 belongs to the client access server. Connectivity is indicated by !!!!!, and ... \[timed out] or \[failed] indicates no connection. If the connection fails, reconfigure the interface, check the physical connection between the Flash server and client access server, and ping again.

After you verify the connection, ensure that a TFTP-bootable image is present in Flash memory. This is the system software image the client access server will boot. Note the name of this software image so you can verify it after the first client boot.

![](https://www.cisco.com/en/US/i/templates/note.gif)

* * *

**Note**![](https://www.cisco.com/en/US/i/templates/blank.gif)
The filename used must represent a software image that is present in Flash memory. If no image resides in Flash memory, the client access server will boot the server's ROM image as a default.

* * *

![](https://www.cisco.com/en/US/i/templates/caut.gif)

* * *

**Caution**![](https://www.cisco.com/en/US/i/templates/blank.gif)

![](https://www.cisco.com/en/US/docs/ios/11_0/access/configuration/guide/acsysimg_files/acsysimg-2.jpg)

For full functionality, the software residing in the Flash memory must be the same type as the ROM software installed on the client access server. For example, if the server has X.25 software, and the client does not have X.25 software in ROM, the client will not have X.25 capabilities after booting from the server's Flash memory.

* * *

### Configure the Flash Server

Perform the following task to configure the Flash server:

\| 

Task

 \| 

Command

 \|
\| 

**Step 1** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Enter configuration mode from the terminal.

 \| 

configure terminal

 \|
\| 

**Step 2** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Specify the TFTP server operation for a router.

 \| 

tftp-server flash \[partition-number:]filename1 \[alias filename2] \[access-list-number]

 \|

The following example illustrates how to configure the Flash server. This example gives the filename of the software image in the Flash server and one access list (labeled 1). The access list must include the network where the client access server resides. Thus, in the example, the network 172.30.101.0 and any client access servers on it are permitted access to the Flash server filename _igs-bfpx.102.1_.

Server# configure terminal 

Enter configuration commands, one per line.

Edit with DELETE, CRTL/W, and CRTL/U; end with CTRL/Z

Server# tftp-server flash igs-bfpx.102.1 

Server# access-list 1 permit 172.30.101.0 0.0.0.255 

Server# copy running-config startup-config <Return> 

### Configure the Client Access Server

Configure the client access server to first load a system image from the Flash server. As a backup, configure the client access server to then load its own ROM image if the load from a Flash server fails.

Perform the following task to configure the Flash server:

\| 

Task

 \| 

Command

 \|
\| 

**Step 1** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Enter configuration mode from the terminal.

 \| 

configure terminal

 \|
\| 

**Step 2** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Remove all previous boot system statements from the configuration file.

 \| 

no boot system

 \|
\| 

**Step 3** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Specify that the client access server load a system image from the Flash server.

 \| 

boot system \[rcp | tftp] filename \[ip-address]

 \|
\| 

**Step 4** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
As a backup, specify that the client access server loads its own ROM image.

 \| 

boot system rom

 \|
\| 

**Step 5** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Set the configuration register to enable the client access server to load a system image from a network server.

 \| 

config-register value

 \|
\| 

**Step 6** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Exit configuration mode.

 \| 

^z

 \|
\| 

**Step 7** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Save the configuration file to your startup configuration.

 \| 

copy running-config startup-config

 \|
\| 

**Step 8** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Reload the access server to enable your changes.

 \| 

reload

 \|

![](https://www.cisco.com/en/US/i/templates/caut.gif)

* * *

**Caution**![](https://www.cisco.com/en/US/i/templates/blank.gif)

![](https://www.cisco.com/en/US/docs/ios/11_0/access/configuration/guide/acsysimg_files/acsysimg-3.jpg)

Using the no boot system command in the following example will invalidate all other boot system commands currently in the client access server system configuration. Before proceeding, determine whether the system configuration stored in the client access server should first be saved (uploaded) to a TFTP file server to be used as a backup copy.

* * *

The following example illustrates how to use these commands:

Server# configure terminal 

Enter configuration commands, one per line.

Edit with DELETE, CRTL/W, and CRTL/U; end with CTRL/Z

Server# boot system gs7-k.11.0 172.36.98.11 

Server# config register 0x010F 

Server# copy running-config startup-config 

In this example, the **no boot system** command invalidates all other **boot system** commands currently in the configuration memory, and any **boot system** commands entered after this command will be executed first. The second command, **boot system** _filename_ _address_, tells the client access server to look for the file _gs7-k11.0_ in the Flash server with an IP address of 172.36.98.11. If this fails, the client access server will boot from its system ROM upon the **boot system rom** command, which is included as a backup in case of a network problem. The **copy running-config startup-config** command copies the configuration to NVRAM.

![](https://www.cisco.com/en/US/i/templates/caut.gif)

* * *

**Caution**![](https://www.cisco.com/en/US/i/templates/blank.gif)

![](https://www.cisco.com/en/US/docs/ios/11_0/access/configuration/guide/acsysimg_files/acsysimg-4.jpg)

The system software to be booted from the Flash server must reside in Flash memory on the server. If it is not in the Flash memory, the client access server will boot the Flash server's system ROM.

* * *

Use the **show version** command on the client access server to verify that the software image booted from the Flash server is the image present in Flash memory.

Following is sample output of the **show version** command:

env-chassis> show version 

GS Software (GS7), Version 9.1.17

Copyright (c) 1986-1992 by cisco Systems, Inc.

Compiled Wed 21-Oct-92 22:49

System Bootstrap, Version 4.6(0.15) 

Current date and time is Thu 10-22-1992 13:15:03

Boot date and time is Thu 10-22-1992 13:06:55

env-chassis uptime is 9 minutes

System restarted by power-on

System image file is "gs7-k.9.17", booted via tftp from 131.131.111.111

RP1 (68040) processor with 16384K bytes of memory.

1 EIP controller (6 Ethernet).

6 Ethernet/IEEE 802.3 interface.

128K bytes of non-volatile configuration memory.

4096K bytes of flash memory on embedded flash (in RP1).

Configuration register is 0x010F

The important information in this example is contained in the first line (GS Software...) and in the line that begins with "System image file...." The "GS Software..." line shows the version of the operating system in the client router's RAM. The"System image file...." line show the filename of the system image loaded from the Flash server.

![](https://www.cisco.com/en/US/i/templates/note.gif)

* * *

**Note**![](https://www.cisco.com/en/US/i/templates/blank.gif)
If no bootable image is present in the Flash server memory when the client server is booted, the version currently running (the first line of the **show version** output) is the system ROM version of the Flash server by default.

* * *

Verify that the software shown in the first line of the show version output is the software residing in the Flash server memory.

### Configure an Access Server as a RARP Server

You can configure the access server as a RARP server. This feature enables the access server to answer RARP requests, making diskless booting of various systems possible (for example, Sun workstations or PCs on networks where the client and server are on separate subnets).

To configure the access server as a RARP server, perform the following task in interface configuration mode:

\| 

Task

 \| 

Command

 \|
\| 

Configure the access server as a RARP server.

 \| 

ip rarp-server ip-address

 \|

Figure 3-1 Configuring an Access Server as a RARP Server

![](https://www.cisco.com/en/US/docs/ios/11_0/access/configuration/guide/acsysimg_files/S3064.jpg)

In the following example, the access server is configured to act as a RARP server.  
[](#wp13054)illustrates the network configuration.

! Allow the access server to forward broadcast portmapper requests

 ip forward-protocol udp 111

! Provide the access server with the IP address of the diskless sun

 arp 172.30.2.5 0800.2002.ff5b arpa

! Configure the access server to act as a RARP server, using the Sun Server's

! IP address in the RARP response packet.

 ip rarp-server 172.30.3.100

! Portmapper broadcasts from this interface are sent to the Sun Server.

 ip helper-address 172.30.3.100

The Sun client and server machine's IP addresses must use the same major network number due to a limitation of the current SunOS _rpc.BOOTParamd_ daemon.

## Configure for Other Types of Servers

You can configure the access server to work with various types of servers to forward different types of service requests.

### Configure for Other Types of Servers Task List

You can configure the router to forward extended BOOTP requests over asynchronous interfaces and MOP server boot requests. The following sections describe these tasks. The tasks are not mutually exclusive.

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Specify Asynchronous Interface Extended BOOTP Requests](#wp13208)

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Specify MOP Server Boot Requests](#wp13233)

### Specify Asynchronous Interface Extended BOOTP Requests

The Boot Protocol (BOOTP) server for asynchronous interfaces supports the extended BOOTP requests specified in RFC 1084. The following command is useful when using the auxiliary port as an asynchronous interface. To configure extended BOOTP requests, perform the following task in global configuration mode:

\| 

Task

 \| 

Command

 \|
\| 

Configure extended BOOTP requests for SLIP.

 \| 

async-bootp tag \[:hostname] data

 \|

You can display the extended BOOTP requests by performing the following task in EXEC mode:

\| 

Task

 \| 

Command

 \|
\| 

Show parameters for BOOTP requests.

 \| 

show async bootp

 \|

### Specify MOP Server Boot Requests

To change the access server's parameters for retransmitting boot requests to a MOP server, complete the following tasks:

\| 

Task

 \| 

Command

 \|
\| 

**Step 1** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Enter global configuration mode from the terminal.

 \| 

configure terminal

 \|
\| 

**Step 2** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Specify the code for the MOP server.

 \| 

mop device-code {cisco | ds200}  

 \|
\| 

**Step 3** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Set the length of time that the access server waits before retransmitting a message.

 \| 

mop retransmit-timer seconds

 \|
\| 

**Step 4** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Specify the number of times an access server retransmits MOP boot requests.

 \| 

mop retries count

 \|
\| 

**Step 5** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Exit global configuration mode.

 \| 

Ctrl-Z or exit

 \|
\| 

**Step 6** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Save the configuration information to nonvolatile memory.

 \| 

copy running-config startup-config

 \|

By default, when the access server transmits a request that requires a response from a MOP boot server and the server does not respond, the message will be retransmitted after four seconds. If the MOP boot server and access server are separated by a slow serial link, it might take longer than four seconds for the access server to receive a response to its message. Therefore, you might want to configure the access server to wait longer than four seconds before retransmitting the message if you are using such a link.

In the following example, if the MOP boot server does not respond within ten seconds after the access server sends a message, the access server will retransmit the message:

## Perform Startup Tasks

You can perform the following optional startup tasks for the access server:

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Partition Flash Memory Using Dual Flash Bank](#wp13288)

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Use Flash Load Helper to Upgrade Software on Run-from-Flash Systems](#wp13521)

### Partition Flash Memory Using Dual Flash Bank

Dual Flash bank allows you to partition the two banks of Flash memory into two separate, logical devices so that the access server can hold and maintain two different software images. No downtime is required to write software into Flash memory while running software that is in another bank of Flash memory.

Dual Flash bank is supported on low-end systems that have at least two banks of Flash memory, including systems that support a single SIMM that has two banks of Flash memory. The Cisco 2500 series supports dual Flash bank.

To use dual Flash bank, you must have at least two banks of Flash memory; a bank is a set of four chips. The minimum partition size is the size of a bank.

There are several benefits to partitioning Flash memory:

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
For any system, partitioning—rather than having one logical Flash memory device—provides a cleaner way of managing different files in Flash memory, especially if the Flash memory size is large.

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
For systems that execute code out of Flash memory, partitioning allows you to download a new image into the file system in one Flash memory bank while an image is being executed from the file system in the other bank. The download is simple, and it causes no network disruption or downtime. After the download is complete, you can switch over at a convenient time.

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
One system can hold two different images, one image acting as a backup for the other. Therefore, if a downloaded image fails to boot for some reason, the earlier running, good image is still available. Each bank is treated as a separate device.

You might use load helper rather than dual Flash bank for one of the following reasons:

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
If you want to download a new file into the same bank from which the current system image is executing.

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
If you want to download a file that is larger than the size of a bank, and therefore you need to switch to a single-bank mode.

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
If you have only one single-bank Flash SIMM installed. In this case, Flash load helper is the best option for upgrading your software.

See the "[Use Flash Load Helper to Upgrade Software on Run-from-Flash Systems](#wp13521)" section in this chapter for information about using Flash load helper.

### Understand Relocatable Images

Partitioning requires that run-from-Flash images be loaded into different Flash memory banks at different physical addresses. This means that images must be relocatable. A relocatable image is an image that contains special relocation information that allows the following:

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
The image to relocate itself whenever it is loaded into RAM for execution

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
A download program with appropriate support to relocate the image before storage in Flash memory, so that the image can run in place in Flash memory, regardless of where in Flash memory it is stored

Run-from-Flash systems used to run nonrelocatable images execute images that need to be stored in Flash memory at a specific address. This means that the image is stored at any other location in Flash memory, it could not be executed in Flash memory, nor could the image be executed from RAM. The relocatable image overcomes this limitation.

With Flash partitioning, the nonrelocatable run-from-Flash images will not work unless loaded into the first device as the first file. This requirement defeats the purpose of partitioning. However, relocatable images can be loaded into any Flash partition (and not necessarily as the first file within the partition) and executed in place.

Note that unless downloaded as the first file in the first partition, this download must be done by an image that recognizes relocatable images.

A relocatable image is an image that is Cisco IOS Release 10.0(6), 10.2(2), or later. All Cisco 2500 series access servers have relocatable images. Communication servers may have one of the following nonrelocatable images:

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
Any image from a release prior to Cisco IOS Release 10.0

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
Any 10.0 image prior to Cisco IOS Release 10.0(6)

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
Cisco IOS Release 10.2

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
Cisco IOS Release 10.3

You can identify a relocatable image by its name. The naming convention for image names for storage on a UNIX system is as follows:

_platform_-_capabilities_-_type_

The letter "l" in the _type_ field indicates a relocatable image. The following are examples of some relocatable image names:

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
igs-i-l—Cisco IOS IP image

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
igs-d-l—Desktop feature image

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
igs-bpx-l—Cisco IOS enterprise image

Only the "igs" prefix images are available as relocatable images. Images distributed on floppy diskettes might have different naming conventions.

For backward compatibility, the relocatable images have been linked to execute as the first file in the first Flash memory bank. This makes the images similar to previous Flash images. Thus, if you download a relocatable image into a nonrelocatable image system, the image will run correctly from Flash memory.

### Dual Flash Bank Configuration Task List

To use dual Flash bank, perform the tasks in one or more of the following sections:

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Partition Flash Memory](#wp13360)

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Download a File into a Flash Partition](#wp13374)

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Manually Boot from Flash Memory](#wp13407)

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Configure the Access Server to Automatically Boot from Flash](#wp13459)

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Configure a Flash Partition as a TFTP Server](#wp13489)

See the "[Clear the Configuration Information](#wp11669)" section in this chapter for information about monitoring dual Flash bank.

To upgrade your software, you must erase Flash memory when you are prompted during the download. This is to ensure that the image is downloaded as the first file in Flash memory.

### Partition Flash Memory

To partition Flash memory, perform the following task in global configuration mode:

\| 

Task

 \| 

Command

 \|
\| 

Partition Flash memory.

 \| 

partition flash partitions \[size1 size2]

 \|

This task will succeed only if the system has at least two banks of Flash memory and the partitioning does not cause an existing file in Flash memory to be split across the two partitions.

### Download a File into a Flash Partition

To download a file into a Flash partition, perform one of the following tasks in EXEC mode:

\| 

Task

 \| 

Command

 \|
\| 

Download a file from a TFTP server into a Flash partition.

 \| 

copy tftp flash

 \|
\| 

Download a file from a MOP server into a Flash partition.

 \| 

copy mop flash

 \|
\| 

Download a file from an rcp server into a Flash partition.

 \| 

copy rcp flash

 \|

The prompts displayed after you execute these tasks indicate the method by which the download can be done into each partition. The possible methods are as follows:

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
None—There is no way to copy into the partition.

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
RXBOOT-Manual—You must manually reload to the rxboot image in ROM in order to copy the image.

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
RXBOOT-FLH—The copy will be done using the Flash load helper software in boot ROM; that is, it will be done automatically.

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
Direct—The copy can be done directly.

If the image download can be done into more than one partition, you are prompted for the partition number. Enter any of the following at the partition number prompt to obtain help:

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
**?**—Display the directory listings of all partitions.

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
**?1**—Display the directory of the first partition.

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
**?2**—Display the directory of the second partition.

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
**q**—Quit the copy command.

### Manually Boot from Flash Memory

To manually boot the access server from Flash memory, perform one of the following tasks in ROM monitor mode:

\| 

Task

 \| 

Command

 \|
\| 

Boot the first bootable file found in any partition.

 \| 

b flash  
or  
boot flash flash:

 \|
\| 

Boot the first bootable file from the specified partition.

 \| 

b flash partition-number:  
or  
boot flash flash:partition-number:

 \|
\| 

Boot a specified file from the first partition.

 \| 

b flash filename  
or  
boot flash flash:filename

 \|
\| 

Boot a specified file from the specified partition.

 \| 

b flash partition-number:filename  
or  
boot flash flash:partition-number:filename

 \|

The result of booting a relocatable image from Flash memory depends on where and how the image was downloaded into Flash memory. [](#wp13457)describes the various ways an image might be downloaded and the corresponding result of booting from Flash memory.

Table 3-2 Downloading an Image and Booting from Flash

\| 

Download Method

 \| 

Result of Booting from Flash Memory

 \|
\| 

The image was downloaded as the first file by a nonrelocatable image.

 \| 

The image will execute in place from Flash memory just like a run-from-Flash image.

 \|
\| 

The image was downloaded as a subsequent file by a nonrelocatable image.

 \| 

The nonrelocatable image would not have relocated the image before storage in Flash memory. This image will not be booted.

 \|
\| 

The image was downloaded as the first file by a relocatable image.

 \| 

The image will execute in place from Flash memory like a run-from-Flash image.

 \|
\| 

The image was downloaded as a subsequent file by a relocatable image (including download into the second partition).

 \| 

The relocatable image relocates the image before storage in Flash memory. Hence, the image will execute in place from Flash memory, like any other run-from-Flash image.

 \|

### Configure the Access Server to Automatically Boot from Flash

To configure the access server to boot automatically from Flash memory, perform one of the following tasks in global configuration mode:

\| 

Task

 \| 

Command

 \|
\| 

Boot the first bootable file found in any partition.

 \| 

boot system flash  
or  
boot system flash:

 \|
\| 

Boot the first bootable file from the specified partition.

 \| 

boot system flash partition-number:  
or  
boot system flash flash:partition-number:

 \|
\| 

Boot the specified file from the first partition.

 \| 

boot system flash filename  
or  
boot system flash flash:filename

 \|
\| 

Boot the specified file from the specified partition.

 \| 

boot system flash partition-number:filename  
or  
boot system flash flash:partition-number:filename

 \|

The result of booting a relocatable image from Flash memory depends on where and how the image was downloaded into Flash memory. [](#wp13457), shown earlier, describes the various means ways an image might be downloaded and the corresponding results of booting from Flash memory.

### Configure a Flash Partition as a TFTP Server

To configure a Flash partition as a TFTP server, perform one of the following tasks in global configuration mode:

\| 

Task

 \| 

Command

 \|
\| 

Specify a file.

 \| 

tftp-server filename

 \|
\| 

Specify a file in the first partition of Flash.

 \| 

tftp-server flash:filename

 \|
\| 

Specify a file in the specified partition of Flash.

 \| 

tftp-server partition-number:filename

 \|
\| 

Specify a file in the specified partition of Flash memory.

 \| 

tftp-server flash:partition-number:filename

 \|

Once you have specified TFTP server operation, exit configuration mode and save the configuration information to nonvolatile memory.

### Use Flash Load Helper to Upgrade Software on Run-from-Flash Systems

The Flash load helper software option allows users to upgrade their system software on run-from-Flash systems. Flash load helper simplifies the upgrade procedure without requiring additional hardware; however, it requires some brief network downtime. A system image running from Flash can use Flash load helper only if the boot ROMs support Flash load helper. If the boot ROMs do not support Flash load helper, you must perform the Flash upgrade manually.

The Flash load helper software upgrade process is simple and does not require additional hardware; however, it does require some brief network downtime. A system image running from Flash can use Flash load helper only if the boot ROMs support Flash load helper. If the boot ROMs do not support Flash load helper, you must perform the Flash upgrade manually. See the "[Manually Boot from Flash Memory](#wp4775)" section in this chapter.

Flash load helper is an automated procedure that reloads the ROM-based image, downloads the software to Flash memory, and reboots to the system image in Flash memory. Flash load helper performs checks and validations to maximize the success of a Flash upgrade and minimize the chance of leaving Flash memory in either an erased state or with a file that cannot boot.

In run-from-Flash systems, the software image in the access server is stored in and executed from the Flash EPROM (as opposed to being executed from RAM). This method reduces memory cost. A run- from-Flash system requires enough Flash EPROM to hold the image and enough main system RAM to hold the access server tables and data structures. The system does not need the same amount of main system RAM as a run-from-RAM system because the full image does not reside in RAM. Run-from-Flash systems include the Cisco 2500 series.

Flash load helper includes the following features:

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
Confirms access to the specified source file on the specified server before erasing Flash memory and reloading to the ROM image for the actual upgrade.

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
Warns you if the image being downloaded is not appropriate for the system.

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
Prevents reloads to the ROM image for a Flash upgrade if the system is not set up for automatic booting and the user is not on the console terminal. In the event of a catastrophic failure during the upgrade, Flash load helper can bring up the boot ROM image as a last resort rather than have the system wait at the ROM monitor's prompt for input from the console terminal.

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
Retries Flash downloads automatically up to six times. The retry sequence is as follows:

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
First try

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
Immediate retry

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
Retry after 30 seconds

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
Reload ROM image and retry

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
Immediate retry

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
Retry after 30 seconds

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
Allows you to save any configuration changes made before you exit out of the system image.

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
Notifies users logged in to the system of the impending switch to the boot ROM image so that they do not lose their connections unexpectedly.

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
Logs console output during the Flash load helper operation into a buffer that is preserved through system reloads. You can retrieve the buffer contents from a running image. The output is useful where console access is unavailable or there is a failure in the download operation.

Flash load helper can also be used on systems with multiple banks of Flash memory that support Flash memory partitioning. Flash load helper enables you to download a new file into the same partition from which the system is executing an image.

For information about how to partition multiple banks of Flash memory so your system can hold two different images, see the "[Partition Flash Memory Using Dual Flash Bank](#wp13288)" section in this chapter.

### Flash Load Helper Configuration Task List

Perform the tasks in the following sections to use and monitor Flash load helper:

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Download a File Using Flash Load Helper](#wp13563)

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Monitor Flash Load Helper](#wp13643)

### Download a File Using Flash Load Helper

To download a new file to Flash memory using Flash load helper, check to make sure that your boot ROMs support Flash load helper, then perform the following tasks in privileged EXEC mode:

\| 

Task

 \| 

Command

 \|
\| 

Download a new file to Flash memory.

 \| 

copy tftp flash

or

copy mop flash

 \|

The following error message displays if you are in a Telnet session and the system is set for manual booting (the boot bits in the configuration register are zero):

ERR: Config register boot bits set for manual booting

In case of any catastrophic failure in the Flash memory upgrade, this error message helps to minimize the chance of the system going down to the ROM monitor prompt and being taken out of the remote Telnet user's control.

The system tries to bring up at least the boot ROM image if it cannot boot an image from Flash memory. Before reinitiating the **copy tftp flash** command, you must set the boot bits to a nonzero value, using the **config-register** global configuration command.

The **copy tftp flash** command initiates a series of prompts to which you must provide responses. The dialog is similar to the following:

\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\* NOTICE **************\*\*\*************** 
Flash load helper v1.0

This process will accept the TFTP copy options and then terminate the current system image 
to use the ROM based image for the copy. Router functionality will not be available during 
that time. If you are logged in via telnet, this connection will terminate. Users with 
console access can see the results of the copy operation.

********************************\*********************************\* 

If any terminals other than the one on which this command is being executed are active, the following message appears:

There are active users logged into the system. 

File Length  Name/status 
1    2251320 abc/igs-kf.103 
\[2251384 bytes used, 1942920 available, 4194304 total]

Enter the IP address or name of the remote host you are copying from:

Address or name of remote host \[255.255.255.255]? 172.108.1.111 

Enter the name of the file you want to copy:

Source file name? abc/igs-kf.103 

Enter the name of the destination file:

Destination file name \[default = source name]? <Return> 

Accessing file \`abc/igs-kf.103' on 172.30.1.111....

Loading from 172.30.13.111:

Erase flash device before writing? \[confirm] <Return> 

If you choose to erase Flash memory, the dialog continues as follows. The **copy tftp flash** operation verifies the request from the running image by trying to TFTP a single block via TFTP from the remote TFTP server. Then Flash load helper is executed, causing the system to reload to the ROM-based system image.

Erase flash device before writing? \[confirm] y 

Flash contains files. Are you sure? \[confirm] y 

If the file does not seem to be a valid image for the system, a warning appears; you must issue confirmation.

Copy \`abc/igs-kf.103' from TFTP server

as \`abc/igs-kf.103' into Flash WITH erase? y 

%SYS-5-RELOAD: Reload requested

%FLH: rxboot/igs-kf.103r from 172.30.1.111 to flash ...

If you choose not to erase Flash memory, and there was file duplication, the dialog would have continued as follows:

Erase flash device before writing? \[confirm] n 

Copy \`abc/igs-kf.103' from TFTP server

as \`abc/igs-kf.103' into Flash WITHOUT erase? y 

If you choose not to erase Flash memory, and there was file duplication, the dialog would have continued as follows:

Erase flash device before writing? \[confirm] n 

File \`abc/igs-kf.103' already exists; it will be invalidated!

Invalidate existing copy of \`abc/igs-kf' in flash memory? \[confirm] y 

Copy \`abc/igs-kf.103' from TFTP server

as \`abc/igs-kf.103' into Flash WITHOUT erase? y 

If the configuration has been modified but not yet saved, you are prompted to save the configuration:

System configuration has been modified. Save? \[confirm]

If you confirm to save the configuration, you might also receive this message:

Warning: Attempting to overwrite an NVRAM configuration previously written by a different 
version of the system image. Overwrite the previous NVRAM configuration? \[confirm]

Users with open Telnet connections are notified of the system reload, as follows:

\*\*System going down for Flash upgrade\*\*

If the TFTP process fails, the copy operation is retried up to three times. If the failure happens in the middle of a copy (part of the file has been written to Flash memory), the retry does not erase Flash memory unless you specified an erase. The partly written file is marked as deleted and a new file is opened with the same name. If Flash memory runs out of free space in this process, the copy is terminated.

After Flash load helper finishes its copy (whether the copy operation is successful or not), it automatically attempts an autoboot or a manual boot, depending on the value of the boot bits in the configuration register. If the boot bits are zero, the system attempts a default boot from Flash memory (equivalent to a manual **b flash** command at the ROM monitor prompt) to load up the first bootable file in Flash memory.

If the boot bits are nonzero, the system attempts to boot based on the boot configuration commands. If no boot configuration commands exist, the system attempts a default boot from Flash memory, that is, it attempts to load the first bootable file in Flash memory.

### Monitor Flash Load Helper

To view the system console output generated during the Flash load helper operation, use the image that has been booted up after the Flash memory upgrade. Perform the following task in privileged EXEC mode:

\| 

Task

 \| 

Command

 \|
\| 

View the system console output generated by Flash load helper.

 \| 

show flh-log

 \|

If you are a remote Telnet user performing the Flash upgrade without a console connection, this task allows you to retrieve console output when your Telnet connection has terminated due to the switch to the ROM image. The output indicates what happened during the download, and is particularly useful if the download fails.

## Configure for Remote Shell (rsh) and Remote Copy (rcp) Functions

You can configure your access server for rsh and rcp functions. This feature allows you to execute commands on remote access servers or routers and to remotely copy system images and configuration files to and from a network server or an access server.

### Cisco's Implementation of Remote Shell (rsh) and Remote Copy (rcp)

One of the first attempts to use the network as a resource in the UNIX community resulted in the design and implementation of the remote shell protocol, which included the remote shell (rsh) and remote copy (rcp). Rsh and rcp allows users to execute commands remotely and copy files to and from a file system residing on a remote host or server on the network. Cisco's implementation of rsh and rcp interoperates with standard implementations.

### Using rsh

From the access server, you can use rsh to execute commands on remote systems to which you have access. When you issue the rsh command, a shell is started on the remote system. The shell allows you to execute commands on the remote system without having to log into the system. In other words, if you use rsh you do not need to connect to the system or access server and then disconnect after you execute a command if you use rsh. For example, you can use rsh to remotely look at the status of other access servers without connecting to the access server, executing the command, and then disconnecting from the access server. This is useful for looking at statistics on many different access servers.

### Maintaining rsh Security

To gain access to a remote system running rsh, such as a UNIX host, you must be configured in the system's _.rhosts_ file or its equivalent. On UNIX systems, the _.rhosts_ file identifies trusted users who can remotely execute commands on the system.

You can enable rsh support on an access server to allow users on remote systems to execute commands on the access server. However, our implementation of rsh does not support an _.rhosts_ file. Instead, you configure a local authentication database to control access to the access server by users attempting to execute commands remotely using rsh. A local authentication database is similar in concept and use to a UNIX _.rhosts_ file. Each entry that you configure in the authentication database identifies the local user, the remote host, and the remote user.

### Using rcp

The rcp copy commands rely on the rsh server (or daemon) on the remote system. To copy files using rcp, you do not need to create a server for file distribution, as you do with TFTP. You only need to have access to a server that supports the remote shell (rsh). (Most UNIX systems support rsh.) Because you are copying a file from one place to another, you must have read permission on the source file and write permission on the destination file. If the destination file does not exist, rcp creates it for you.

Although Cisco rcp implementation emulates the behavior of the UNIX rcp implementation—copying files among systems on the network—our command syntax differs from the UNIX rcp command syntax. Our rcp support offers a set of copy commands that use rcp as the transport mechanism. These rcp copy commands are similar in style to our TFTP copy commands, but they offer an alternative that provides faster performance and reliable delivery of data. This is because the rcp transport mechanism is built on and uses the Transmission Control Protocol/Internet Protocol (TCP/IP) stack, which is connection-oriented. You can use the Cisco rcp commands to copy system images and configuration files from the access server to a network server and vice versa.

You can also enable rcp support on the access server to allow users on remote systems to copy files to and from the access server.

### Configure for rsh and rcp Task List

To configure the router for rsh and rcp, perform the following tasks:

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Configure an Access Server to Support Incoming rcp Requests and rsh Commands](#wp13700)

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Configure the Access Server to Accept rcp Requests from Remote Users](#wp13719)

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
[Configure the System to Allow Remote Users to Execute Commands Using rsh](#wp13752)

### Configure an Access Server to Support Incoming rcp Requests and rsh Commands

You configure a local authentication database to control access to the access server by remote users. A local authentication database is similar in concept and use to a UNIX _.rhosts_ file. To allow remote users to execute rcp or rsh commands on the access server, configure entries for those users in the access server's authentication database.

Each entry configured in the authentication database identifies the local user, the remote host, and the remote user. To be allowed to remotely execute commands on the access server, the remote user must specify all three values—the local username, the remote host name, and the remote username. For rsh users, you can also grant a user permission to execute privileged EXEC commands remotely.

An entry that you configure in the access server authentication database differs from an entry in a UNIX _.rhosts_ file in several ways, for example, the entry must include a local username. Because the _.rhosts_ file on a UNIX system resides in a local user's home directory, an entry in a UNIX _.rhosts_ file does not need to include the local username. The local username is determined from the user account. You must specify the local username along with the remote host name and the remote username in each authentication database entry that you configure. You can specify the access server host name as the local username.

To make the local username available to remote users, you need to communicate the username to the network administrator or the remote user. To allow a remote user to execute a command on the access server, the local username sent by the remote user needs to match the local username configured in the database entry.

The Cisco IOS software uses Domain Name System to authenticate the remote host's name and address. Because DNS can return several valid IP addresses for a host name, the Cisco IOS software checks the address of the requesting client against all of the IP addresses for the named host returned by DNS. If the address sent by the requester is considered invalid, because, it does not match any address listed with DNS for the host name, then the Cisco IOS software will reject the remote-command execution request.

If no DNS servers are configured for the access server, then the access server cannot authenticate the host with the authentication database. In this case, the Cisco IOS software will send a broadcast request to attempt to gain access to DNS services on another server. If DNS services are not available, you must use the **no ip domain-lookup** command to disable the access server's attempt to gain access to a DNS server by sending a broadcast request.

If DNS services are not available and, therefore, you bypass the DNS security check, the Cisco IOS software will accept the request to remotely execute a command _only_ if all three values sent with the request match exactly the values configured for an entry in the local authentication file.

If DNS is enabled but you do not want to use DNS for rcmd queries, use the **no ip rcmd domain-lookup** command.

To ensure security, the access server is _not_ enabled to support rcp requests from remote users by default. When the access server is not enabled to support rcp, the authorization database has no effect.

### Configure the Access Server to Accept rcp Requests from Remote Users

To configure the access server to support incoming rcp requests, complete the following tasks, starting in privileged EXEC mode:

\| 

Task

 \| 

Command

 \|
\| 

**Step 1** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Enter configuration mode from the terminal.

 \| 

configure terminal

 \|
\| 

**Step 2** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Create an entry in the local authentication database for each remote user who is allowed to execute rcp commands on the access server.

 \| 

ip rcmd remote-host  local-username {ip-address | host} remote-username

 \|
\| 

**Step 3** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Enable the access server to support incoming rcp requests.

 \| 

ip rcmd rcp-enable

 \|

![](https://www.cisco.com/en/US/i/templates/note.gif)

* * *

**Note**![](https://www.cisco.com/en/US/i/templates/blank.gif)
In Cisco IOS Release 10.3, the **ip** keyword has been added to **rcmd** commands. If you are upgrading from Cisco IOS Release 10.2 to 11.0, this keyword will automatically be added to any **rcmd** commands that you have in your Cisco IOS Release 10.2 configuration files.

* * *

To disable the access server from supporting incoming rcp requests, use the **no ip rcmd rcp-enable** command.

![](https://www.cisco.com/en/US/i/templates/note.gif)

* * *

**Note**![](https://www.cisco.com/en/US/i/templates/blank.gif)
 When the access server's support for incoming rcp requests is disabled, you can still use the rcp commands to copy images from remote servers. The access server's support for incoming rcp requests is distinct from its ability to handle for outgoing rcp requests.

* * *

The following example shows how to add two entries for remote users to the access server's authentication database and then enable the access server to support remote copy requests from remote users. The users, named _netadmin1_ on the remote host at IP address 172.30.15.55 and _netadmin3_ on remote host at IP address 131.108.101.101, are both allowed to connect to the access server and remotely execute rcp commands on it after the access server is enabled to support rcp. Both authentication database entries give the access server's host name _commserver1_ as the local username. The fourth command enables the access server to support rcp requests from remote users.

 ip rcmd remote-host commserver1 172.30.15.55 netadmin1

 ip rcmd remote-host commserver1 172.30.101.101 netadmin3 

### Configure the System to Allow Remote Users to Execute Commands Using rsh

To configure the access server as an rsh server, complete the following tasks, starting in privileged EXEC mode:

\| 

Task

 \| 

Command

 \|
\| 

**Step 1** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Enter configuration mode from the terminal.

 \| 

configure terminal

 \|
\| 

**Step 2** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Create an entry in the local authentication database for each remote user who is allowed to execute rsh commands on the access server.

 \| 

ip rcmd remote-host  local-username {ip-address | host} remote-username \[enable]

 \|
\| 

**Step 3** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Enable the access server to support incoming rsh commands.

 \| 

ip rcmd rsh-enable

 \|

![](https://www.cisco.com/en/US/i/templates/note.gif)

* * *

**Note**![](https://www.cisco.com/en/US/i/templates/blank.gif)
In Cisco IOS Release 10.3, the **ip** keyword has been added to **rcmd** commands. If you are upgrading from Cisco IOS Release 10.2 to 11.0, this keyword will automatically be added to any rcmd commands you have in your Cisco IOS Release 10.2 configuration files.

* * *

To disable the access server from supporting incoming rsh commands, use the no ip rcmd rsh-enable command.

![](https://www.cisco.com/en/US/i/templates/note.gif)

* * *

**Note**![](https://www.cisco.com/en/US/i/templates/blank.gif)
When the access server is disabled, you can still issue a remote shell command to be executed on other access servers that support rcp and on UNIX hosts on the network.

* * *

The following example shows how to add two entries for remote users to the access server's authentication database, and enable the access server to support rsh commands from remote users. The users, named _rmtnetad1_ and _netadmin4_, are both on the remote host at IP address 172.30.101.101. Although both users are on the same remote host, you must include a unique entry for each user. Both users are allowed to connect to the access server and remotely execute rsh commands on it after the access server is enabled for rsh. The user named _netadmin4_ is allowed to execute privileged EXEC mode commands on the access server. Both authentication database entries give the access server's host name _commserver1_ as the local username. The fourth command enables the access server to support rsh commands issued by remote users.

 ip rcmd remote-host commserver1 172.30.101.101 rmtnetad1

 ip rcmd remote-host commserver1 172.30.101.101 netadmin4 enable

### Turn Off DNS Lookups for rcp and rsh

To bypass the DNS security check when DNS services are configured but not available, perform the following task in global configuration mode:

\| 

Task

 \| 

Command

 \|
\| 

Bypass the DNS security check.

 \| 

no ip rcmd domain-lookup

 \|

The access server software will accept the request to remotely execute a command only if all three values sent with the request match exactly the values configured for an entry in the local authentication file.

### Configure the Remote Username for rcp Requests

From the access server, you can use rcp to remotely copy files to and from network servers and hosts if those systems support rcp. You do not need to configure the access server to issue remote copy requests from the access server using rcp. However, to prepare to use rcp from the access server for remote copying, you can perform an optional configuration process to specify the remote username to be sent on each rcp request.

The rcp protocol requires a client to send the remote username on an rcp request. By default, the Cisco IOS software sends the remote username associated with the current TTY (terminal) process, if that name is valid, for rcp commands.

![](https://www.cisco.com/en/US/i/templates/note.gif)

* * *

**Note**![](https://www.cisco.com/en/US/i/templates/blank.gif)
For UNIX systems, each physical device is represented in the file system. Terminals, or serial lines, are called TTY devices. (TTY stands for teletype, the original UNIX terminal.)

* * *

If the username for the current TTY process is not valid, the Cisco IOS software sends the host name as the remote username. For boot commands using rcp, the Cisco IOS software sends the access server host name by default. You cannot explicitly configure the remote username.

When copying from the remote server, rcp searches for the system image or configuration file to be copied relative to the directory of the remote username. When copying to the remote server, rcp writes the system image or configuration file to be copied relative to the directory of the remote username. When booting an image, rcp searches for the image file on the remote server relative to the directory of the remote username.

To override the default remote username sent on rcp requests, complete the following tasks:

\| 

Task

 \| 

Command

 \|
\| 

**Step 1** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Enter configuration mode from the terminal.

 \| 

configure terminal

 \|
\| 

**Step 2** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Specify the remote username.

 \| 

ip rcmd remote-username username

 \|
\| 

**Step 3** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Exit configuration mode.

 \| 

Ctrl-Z

 \|

To remove the remote username and return to the default value, use the **no ip rcmd remote-username** command.

### Remotely Execute Commands Using rsh

You can use rsh to execute commands remotely on network servers that support the remote shell protocol. To use this command, the _.rhosts_ files on the network server must include an entry that permits you to remotely execute commands on that host.

The rsh command that you issue is remotely executed from the directory of the account for the remote user that you specify through the **/user** _username_ parameter.

If you do not specify the _/user_ keyword and argument, the access server sends a default remote username unless you override the default by configuring a remote username. As the default value of the remote username, the Cisco IOS software sends the remote username associated with the current TTY process, if that name is valid. If the TTY remote username is invalid, the Cisco IOS software uses the access server host name as the both the remote and local usernames.

To execute a command remotely on a network server using rsh, perform the following task from EXEC mode:

\| 

Task

 \| 

Command

 \|
\| 

**Step 1** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
If you do not specify the /user keyword and argument in step 2, configure the remote username to be used. This step is optional, but recommended.

 \| 

ip rcmd remote-username username

 \|
\| 

**Step 2** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Enter the rsh command to be executed remotely.

 \| 

rsh {ip-address | host} \[/user username] remote-command

 \|

The following example shows how to execute commands remotely using rsh:

Router# exec Router# rsh mysys.cisco.com /u sharon ls -a 

## Use the AutoInstall Procedure

This section provides information about AutoInstall, a procedure that allows you to configure a new access server automatically and dynamically. You use the AutoInstall procedure to connect a new access server to a network on which there is an existing preconfigured access server, turning on the new access server, and enable it with a configuration file that is automatically downloaded from a Trivial File Transfer Protocol (TFTP) server.

The following sections provide the requirements for AutoInstall and an overview of how the procedure works. To start the procedure, see "[Perform the AutoInstall Procedure](#wp14026)" later in this section.

### AutoInstall Requirements

For the AutoInstall procedure to work, your system must meet the following requirements:

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
Both access servers must be physically attached to the network using one or more of the following interface types: Ethernet, Token Ring, serial with High-Level Data Link Control (HDLC) encapsulation, or serial with Frame Relay encapsulation. HDLC is the default encapsulation. If the AutoInstall process fails over HDLC, the access server automatically configures Frame Relay encapsulation.

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
The existing preconfigured access server must be running Software Release 9.1 or later. For AutoInstall over Frame Relay, access servers on both sides must be running Cisco IOS Release 10.3 or later.

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
The new access server must be running Software Release 9.1 or later. For AutoInstall over Frame Relay, the new access server must be running Software Release 10.3 or later.

![](https://www.cisco.com/en/US/i/templates/note.gif)

* * *

**Note**![](https://www.cisco.com/en/US/i/templates/blank.gif)
Only Token Ring cards that set ring speed with physical jumpers support AutoInstall. AutoInstall does not work with Token Ring interfaces for which the ring speed must be set using software configuration commands. If the ring speed is not set, the interface is set to shutdown mode.

* * *

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
You must complete procedures 1 and either 2 or 3:

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
Procedure 1: A configuration file for the new access server must reside on a TFTP server. This file can contain the new access server's full configuration or the minimum needed for the administrator to connect to the new access server for configuration using Telnet.

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
Procedure 2: A file named network-confg also must reside on the server. The file must have an Internet Protocol (IP) host name entry for the new access server. The server must be reachable from the existing access server.

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
Procedure 3: An IP address-to-host name mapping for the new access server must be added to a Domain Name System (DNS) database file.

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
If the existing access server is to help automatically install the new access server via an HDLC-encapsulated serial interface using Serial Line Address Resolution Protocol (SLARP), that interface must be configured with an IP address whose host portion has the value 1 or 2. (AutoInstall over Frame Relay does not have this address constraint.) Subnet masks of any size are supported.

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
If the existing access server is to help automatically install the new access server using a Frame Relay-encapsulated serial interface, that interface must be configured with the following:

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
An IP helper address pointing to the TFTP server. In the following example, 172.16.2.75 is the address of the TFTP server:

ip helper 172.16.2.75

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
A Frame Relay map pointing back to the new access server. In the following example, 172.21.177.100 is the IP address of _newrcommserver_'s serial interface. 100 is the PVC identifier:

frame-relay map ip 172.21.177.100 100 dlci

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
If the existing access server is to help automatically install the new access server via an Ethernet or Token Ring using BOOTP or Reverse Address Resolution Protocol (RARP), a BOOTP or RARP server also must be set up to map the new access server's Media Access Control (MAC) address to its IP address.

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
IP helper addresses might need to be configured in order to forward the TFTP and DNS broadcast requests from the new access server to the host that is providing those services.

### Using a DOS-Based TFTP Server

Autoinstall over Frame Relay supports downloading configuration files from UNIX-based and DOS-based TFTP servers. AutoInstall over other WAN encapsulations and other booting mechanisms such as RARP and SLARP supports UNIX-based and DOS-based TFTP servers.

The DOS format of the UNIX _network-confg_ file must be eight characters or less, with a three-letter extension. Therefore, when an attempt to load network-confg fails, AutoInstall automatically attempts to download _cisconet.cfg_ from the TFTP server.

If _cisconet.cfg_ exists and a download succeeds, then the server is assumed to be a DOS machine. The AutoInstall program will then attempt to resolve the host name for the access server through host commands in cisconet.cfg.

If _cisconet.cfg_ does not exist or cannot be downloaded, or the program is unable to resolve a host name, DNS will attempt to resolve the host name of the access server. If it is unable to resolve the host name through DNS, the access server will attempt to download ciscortr.cfg. If host name is longer than eight characters, it will be truncated to eight characters. For example, an access server with a host name "australia" will be treated as "australi" and an attempt will be made to download _australi.cfg_.

The formats of the _cisconet.cfg_ and ciscortr.cfg files are to be the same as those described for _network-confg_ and _host name-confg_.

If neither _network-confg_ nor _cisconet.cfg_ exist and DNS is unable to resolve the host name, the program will attempt to load router-confg. If router-confg does not exist or cannot be downloaded, the program will attempt to load _ciscortr.cfg_. The cycle is repeated three times.

### How AutoInstall Works

Once the requirements for using AutoInstall are met, the dynamic configuration of the new access server occurs in the following order:

**1** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
The new access server acquires its IP address. Depending upon the interface connection between the two access servers, the new access server's IP address is dynamically resolved by either SLARP requests or BOOTP or RARP requests.

**2** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
The new access server resolves its name either through network-confg or through DNS.

**3** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
The new access server automatically requests and downloads its configuration file from a TFTP server.

**4** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
If a host name is not resolved, the _Newcommserver_ will attempt to load _commserver-confg_.

### Acquiring the New Access Server's IP Address

The new access server (_Newcommserver_) resolves the IP addresses of its interface by one of the following means:

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
If _Newcommserver_ is connected by an HDLC-encapsulated serial line to the existing access server (_Existing_), _Newcommserver_ sends an SLARP request to _Existing._

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
If _Newcommserver_ is connected by an Ethernet or Token Ring interface, it broadcasts BOOTP and RARP requests.

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
If _Newcommserver_ is connected by a Frame Relay-encapsulated serial interface, it first attempts the HDLC automatic installation process and then attempts the BOOTP/RARP over Ethernet or Token Ring. If both of these attempts fail, it attempts to automatically install over Frame Relay. In this case, a BOOTP request is sent over the lowest numbered serial or HSSI interface.

The _Existing_ access server responds in one of the following ways depending upon the request type:

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
In response to a SLARP request, _Existing_ sends a SLARP reply packet to _Newcommserver_. The reply packet contains the IP address and netmask of _Existing_. If the host portion of the IP address in the SLARP response is 1, _Newcommserver_ will configure its interface using the value 2 as the host portion of its IP address and vice versa. (See [](#wp13963).)

Figure 3-2 Using SLARP to Acquire the New Access Server's IP Address

![](https://www.cisco.com/en/US/docs/ios/11_0/access/configuration/guide/acsysimg_files/S3063.jpg)

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
In response to BOOTP or RARP requests, an IP address is sent from the BOOTP or RARP server to _Newcommserver_.

A BOOTP or RARP server must have already been set up to map _Newcommserver_'s MAC address to its IP address. If the BOOTP server does not reside on the directly attached network segment, access servers between _Newcommserver_ and the BOOTP server can be configured with the **ip helper-address** command to allow the request and response to be forwarded between segments, as shown in [](#wp13979).

AutoInstall over Frame Relay is a special case in which BOOTP is used, but the access server _Existing_ acts as a BOOTP server and responds to the incoming BOOTP request. Only a helper address and a Frame Relay map need to be set up. There is no need for a MAC-to-IP address map on the access server _Existing_.

Figure 3-3 Using BOOTP or RARP to Acquire the New Access Server's IP Address

![](https://www.cisco.com/en/US/docs/ios/11_0/access/configuration/guide/acsysimg_files/S3140.jpg)

As of Software Release 9.21, access servers can be configured to act as RARP servers.

As soon as one interface resolves its IP address, the access server attempts to resolve its host name. Therefore, only one IP address needs to be set up using either SLARP, BOOTP, or RARP.

### Resolving the IP Address to the Host Name

The new access server resolves its IP address-to-host name mapping by sending a TFTP broadcast requesting the _network-confg_ file, as shown in [](#wp14004).

The network-confg file is a configuration file generally shared by several access servers. In this case, it is used to map the IP address of the new access server just obtained dynamically to the name of the new access server. The _network-confg_ file must reside on a reachable TFTP server and must be globally readable.

The following is an example of a minimal _network-confg_ file that maps the IP address of the new access server (172.108.10.2) to the name _Newcommserver_. The address of the new access server was learned via SLARP and is based on _Existing_'s IP address of 172.108.10.1.

ip host newcommserver 172.108.10.2

If you are not using AutoInstall over Frame Relay, the host portion of the address must be 1 or 2. AutoInstall over Frame Relay does not have this addressing constraint.

If _Newcommserver_ does not receive a _network-confg_ file, or if the IP address-to-host name mapping does not match the newly acquired IP address, _Newcommserver_ sends a DNS broadcast. If DNS is configured and has an entry that maps _Newcommserver_'s SLARP, BOOTP, or RARP-acquired IP address to its name, _Newcommserver_ successfully resolves its name.

If DNS does not have an entry that maps _Newcommserver_'s SLARP, BOOTP, or RARP-acquired address to its name, the new access server cannot resolve its host name. The new access server attempts to download a default configuration file as described in the next section, and failing that, enters setup mode (except with AutoInstall over Frame Relay, in which case the access server enters user EXEC mode).

Figure 3-4 Dynamically Resolving the New Access Server's IP Address-to-Host Name Mapping

![](https://www.cisco.com/en/US/docs/ios/11_0/access/configuration/guide/acsysimg_files/S2388.jpg)

### Downloading the New Access Server's Host Configuration File

After the access server successfully resolves its host name, _Newcommserver_ sends a TFTP broadcast requesting the file newcommserver-confg. The name _newcommserver-confg_ must be in all lowercase, even if the true host name is not. If _Newcommserver_ cannot resolve its host name, it sends a TFTP broadcast requesting the default host configuration file commserver-confg. The file is downloaded to _Newcommserver_, where the configuration commands take effect immediately.

When using AutoInstall over Frame Relay, you are put into **setup** mode while the AutoInstall process is running. If the configuration file is successfully installed, the **setup** process is terminated. If you expect the AutoInstall process to be successful, do not respond to the **setup** prompts. If you do not expect the AutoInstall process to be successful, create a configuration file by responding the **setup** prompts. The AutoInstall process is terminated transparently.

If you expect AutoInstall to succeed, you can also respond to the prompts as follows:

Would you like to enter the initial configuration dialog? \[yes]: no 

Would you like to terminate autoinstall? \[yes]: no 

You will see the following display as the AutoInstall operation is completed:

Please Wait. AutoInstall being attempted!!!!!!!!!!!!!!!!!!!

If the host configuration file contains only the minimal information, you must connect into _Existing_ using Telnet then connect to _Newcommserver_, and then run the **setup** command to configure _Newcommserver_.

If the host configuration file is complete, _Newcommserver_ should be fully operational. You can enter the **enable** command (with the system administrator password) at the system prompt on _Newcommserver_, and then issue the **copy running-config startup-config** command to save the information in the recently obtained configuration file into NVRAM. If a reload occurs, _Newcommserver_ simply loads its configuration file from nonvolatile memory.

If the TFTP request fails, or if _Newcommserver_ still has not obtained the IP addresses of all its interfaces, and those addresses are not contained in the host configuration file, then _Newcommserver_ enters **setup** mode automatically. Setup mode prompts for manual configuration of the access server via the console. The new access server continues to issue broadcasts to attempt to learn its host name and obtain any unresolved interface addresses. The broadcast frequency will dwindle to every ten minutes after several attempts. Refer to the _Access and Communication Servers Getting Started Guide_ for details on the **setup** command.

### Perform the AutoInstall Procedure

To dynamically configure a new access server using AutoInstall, complete the following tasks. Steps 1, 2, and 3 are completed by the central administrator. Step 4 is completed by the person at the remote site.

* * *

**Step 1** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Modify the existing access server's configuration to support the AutoInstall procedure.

**Step 2** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Set up the TFTP server to support the AutoInstall procedure.

**Step 3** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Set up the BOOTP or RARP server if needed. A BOOTP or RARP server is required for AutoInstall using an Ethernet, Token Ring, or Frame Relay-encapsulated serial interface. With a Frame Relay-encapsulated serial interface, the existing access server acts as the BOOTP server. A BOOTP or RARP server is not required for AutoInstall using an HDLC-encapsulated serial interface.

**Step 4** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Connect the new access server to the network.

### Modifying the Existing Access Server's Configuration

You can use any of the following types of interface:

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
An HDLC-encapsulated serial line (the default configuration for a serial line)

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
An Ethernet or Token Ring interface

•![](https://www.cisco.com/en/US/i/templates/blank.gif)
A Frame Relay-encapsulated serial line

### Use an HDLC-Encapsulated Serial Interface Connection

To set up AutoInstall via a serial line with HDLC encapsulation (the default), you must configure the existing access server. Perform the following steps, beginning in global configuration mode:

\| 

Task

 \| 

Command

 \|
\| 

**Step 1** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Configure the serial interface that connects to the new access server with HDLC encapsulation (the default), and enter interface configuration mode.

 \| 

interface serial interface-number

 \|
\| 

**Step 2** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Enter an IP address for the interface. The host portion of the address must have a value of 1 or 2. (AutoInstall over Frame Relay does not have this address constraint.)

 \| 

ip address address mask

 \|
\| 

**Step 3** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Configure a helper address for the serial interface to forward broadcasts associated with the TFTP, BOOTP, and DNS requests.

 \| 

ip helper-address address

 \|
\| 

**Step 4** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Optionally, configure a DCE clock rate for the serial line, unless an external clock is being used. This step is needed only for DCE appliques.

 \| 

clock rate bps

 \|
\| 

**Step 5** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Exit configuration mode.

 \| 

Ctrl-Z

 \|
\| 

**Step 6** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Save the configuration changes to NVRAM.

 \| 

copy running-config startup-config

 \|

You must use a DTE interface on the new access server because there is no default clock rate for a DCE interface.

In the following example, the existing access server's configuration file contains the commands needed to configure the access server for AutoInstall on a serial line using HDLC encapsulation:

Router# configure terminal 

 ip address 172.31.10.1 255.255.255.0

 ip helper-address 172.31.20.5

Router(config)# copy running-config startup-config 

### Use an Ethernet or Token Ring Interface Connection

To set up AutoInstall using an Ethernet or Token Ring interface, you must modify the configuration of the existing access server. Perform the following steps, beginning in global configuration mode.

\| 

Task

 \| 

Command

 \|
\| 

**Step 1** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Configure a LAN interface and enter interface configuration mode.

 \| 

interface {ethernet | tokenring} interface-number

 \|
\| 

**Step 2** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Enter an IP address for the interface.

 \| 

ip address address mask

 \|
\| 

**Step 3** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Optionally, configure a helper address to forward broadcasts associated with the TFTP, BOOTP, and DNS requests.

 \| 

ip helper-address address

 \|
\| 

**Step 4** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Exit configuration mode.

 \| 

Ctrl-Z

 \|
\| 

**Step 5** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Save the configuration changes to NVRAM.

 \| 

copy running-config startup-config

 \|

Typically, the local-area network (LAN) interface and IP address are already configured on the existing access server. You might need to configure an IP helper address if the TFTP server is not on the same network as the new access server.

In the following example, the existing access server's configuration file contains the commands needed to configure the access server for AutoInstall on an Ethernet interface:

Router# configure terminal 

 ip address 172.31.10.1 255.255.255.0

 ip helper-address 172.31.20.5

Router(config)# copy running-config startup-config 

### Use a Frame Relay-Encapsulated Serial Interface Connection

To set up AutoInstall via a serial line with Frame Relay encapsulation, you must configure the existing access server. Perform the following tasks, beginning in global configuration mode:

\| 

Task

 \| 

Command

 \|
\| 

**Step 1** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Configure the serial interface that connects to the new access server and enter interface configuration mode.

 \| 

interface serial 0

 \|
\| 

**Step 2** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Configure Frame Relay encapsulation on the interface that connects to the new access server.

 \| 

encapsulation frame-relay

 \|
\| 

**Step 3** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Create a Frame Relay map pointing back to the new access server.

or

For point-to-point subinterfaces, assign a data link connection identifier (DLCI) to the interface that connects to the new access server, and provide the IP address of the serial port on the new access server.

 \| 

frame-relay map ip ip-address dlci

or

frame-relay interface-dlci dlci option \[protocol ip ip-address]

 \|
\| 

**Step 4** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Enter an IP address for the interface. This step sets the IP address of the existing access server.

 \| 

ip address address mask

 \|
\| 

**Step 5** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Configure a helper address for the TFTP server.

 \| 

ip helper-address address

 \|
\| 

**Step 6** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Optionally, configure a DCE clock rate for the serial line, unless an external clock is being used. This step is needed only for DCE appliques.

 \| 

clock rate bps

 \|
\| 

**Step 7** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Exit configuration mode.

 \| 

Ctrl-Z

 \|
\| 

**Step 8** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Save the configuration changes to nonvolatile memory.

 \| 

copy running-config startup-config

 \|

You must use a DTE interface on the new access server because the network will always provide the clock signal.

In the following example, the existing access server's configuration file contains the commands needed to configure the access server for Frame Relay AutoInstall on a serial line:

Router# configure terminal 

 ip address 172.31.20.20 255.255.255.0

 encapsulation frame-relay

 frame-relay map ip 172.31.10.1 255.255.255.0 48

 ip helper-address 172.31.20.5

### Set up the TFTP Server

For AutoInstall to work correctly, the new access server must be able to resolve its host name and then download a name-confg or _name_-cfg file from a TFTP server. The new access server can resolve its host name by using a _network-confg_ or _cisconet.cfg_ file downloaded from a TFTP server or by using the DNS.

To set up a TFTP server to support AutoInstall, complete the following tasks. Step 2 includes two ways to resolve the new access server's host name. Use the first method if you want to use a _network-config_ file to resolve the new access server's host name. Use the second method if you want to use the DNS to resolve the new access server's host name, starting in global configuration mode

\| 

Task

 \| 

Command

 \|
\| 

**Step 1** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Enable TFTP on a server.

 \| 

Consult your host vendor's TFTP server documentation and RFCs 906 and 783.

 \|
\| 

**Step 2** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
If you want to use a network-confg or cisconet.cfg file to resolve the new access server's name, create the file network-confg or cisconet.cfg containing an IP address-to-host name mapping for the new access server. Enter the ip host command into the TFTP config file, not into the access server. The IP address must match the IP address that is to be dynamically obtained by the new access server.

or

If you want to use the DNS to resolve the new access server's name, create an address-to-name mapping entry for the new access server in the DNS database. The IP address must match the IP address that is to be dynamically obtained by the new access server.

 \| 

ip host host name address

Contact the DNS administrator or refer to RFCs 1101 and 1183.

 \|
\| 

**Step 3** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Create the file name-confg, or name-cfg which should reside in the tftpboot directory on the TFTP server. The name part of name-confg must match the host name you assigned for the new access server in the previous step. Enter configuration commands for the new access server into this file.

 \| 

See the appropriate chapter in this guide for specific commands.

 \|

.

The name-confg or _name_-cfg file can contain either the new access server's full configuration or a minimal configuration.

The minimal configuration file consists of a virtual terminal password and an enable password. It allows an administrator to use Telnet to connect to the new access server to configure it. If you are using BOOTP or RARP to resolve the address of the new access server, the minimal configuration file must also include the IP address to be obtained dynamically using BOOTP or RARP.

You can use the **copy running-config tftp** command to help you generate the configuration file that you will download during the AutoInstall process.

![](https://www.cisco.com/en/US/i/templates/note.gif)

* * *

**Note**![](https://www.cisco.com/en/US/i/templates/blank.gif)
The existing access server might need to forward TFTP requests and response packets if the TFTP server is not on the same network segment as the new access server. When you modified the existing access server's configuration, you specified an IP helper address for this purpose.

* * *

You can save a minimal configuration under a generic newcommserver-confg file. Use the **ip host** command in the network-confg or cisconet.cfg file to specify newcommserver as the host name with the address you want to dynamically resolve. The new access server should then resolves its IP address, host name, and minimal configuration automatically. Use Telnet to connect to the new access server from the existing access server and use the **setup** facility to configure the rest of the interfaces.

For example, the line in the network-confg or cisconet.cfg file might be similar to the following:

ip host newcommserver 172.30.08.170.1

The following host configuration file contains the minimum set of commands needed for AutoInstall using SLARP or BOOTP:

enable-password letmein 
! 
line vty 0 
 password letmein 
!

The preceding example shows a minimal configuration for connecting from an access server one hop away. From this configuration, use the **setup** facility to configure the rest of the interfaces. If the access server is more than one hop away, you also must include routing information in the minimal configuration.

The following minimal network configuration file maps the new access server's IP address, 172.30.10.2, to the host name _Newcommserver_. The new access server's address was learned via SLARP and is based on the existing access server's IP address of 172.30.10.1.

ip host newcommserver 172.30.10.2 

### Set Up the BOOTP or RARP

If the new access server is connected to the existing access server using an Ethernet or Token Ring interface, you must configure a BOOTP or RARP server to map the new access server's MAC address to its IP address. If the new access server is connected to the existing access server using a serial line with HDLC encapsulation or if you are configuring AutoInstall over Frame Relay, the tasks in this section are not required.

To configure a BOOTP or RARP server, complete one of the following tasks:

\| 

Task

 \| 

Command

 \|
\| 

If BOOTP is to be used to resolve the new access server's IP address, configure your BOOTP server.

 \| 

Refer to your host vendor's manual pages and to RFCs 951 and 1395

 \|
\| 

If RARP is to be used to resolve the new access server's IP address, configure your RARP server.

 \| 

Refer to your host vendor's manual pages and to RFC 903

 \|

![](https://www.cisco.com/en/US/i/templates/note.gif)

* * *

**Note**![](https://www.cisco.com/en/US/i/templates/blank.gif)
If the RARP server is not on the same subnet as the new access server, use the **ip rarp-server** command to configure the existing access server to act as a RARP server. See the section "[Configure the Remote Username for rcp Requests](#wp13826)" in this chapter.

* * *

The following host configuration file contains the minimum set of commands needed for AutoInstall using RARP. It includes the IP address that will be obtained dynamically via BOOTP or RARP during the AutoInstall process. When RARP is used, this extra information is needed to specify the proper netmask for the interface.

interface ethernet 0 
 ip address 172.30.10.2 255.255.255.0 
 enable-password letmein 
! 
line vty 0 
 password letmein 
!

### Connect the New Access Server to the Network

Connect the new access server to the network using either an HDLC-encapsulated Frame Relay-encapsulated serial interface or an Ethernet or Token Ring interface. After the access server successfully resolves its host name, _Newcommserver_ sends a TFTP broadcast requesting the file _name-confg_ or _cisconet.cfg_. The access server name must be in all lowercase, even if the true host name is not. The file is downloaded to the new access server where the configuration commands take effect immediately. If the configuration file is complete, the new access server should be fully operational. To save the complete configuration to NVRAM, complete the following tasks in privileged EXEC mode:

\| 

Task

 \| 

Command

 \|
\| 

**Step 1** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Enter privileged mode at the system prompt on the new access server.

 \| 

enable[1](#wpxref14267) password

 \|
\| 

**Step 2** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Save the information from the name-config file into NVRAM.

 \| 

copy running-config startup-config

 \|

[1](#wpxref14268) This command is documented in the "User Interface Commands" chapter in the Access and Communication Servers Command Reference publication.

![](https://www.cisco.com/en/US/i/templates/caut.gif)

* * *

**Caution**![](https://www.cisco.com/en/US/i/templates/blank.gif)

![](https://www.cisco.com/en/US/docs/ios/11_0/access/configuration/guide/acsysimg_files/acsysimg-9.jpg)

Verify that the existing and new access servers are connected before entering the **write memory** EXEC command to save configuration changes. Use the **ping** EXEC command to verify connectivity. If an incorrect configuration file is downloaded, the new access server will load NVRAM configuration information before it can enter AutoInstall mode.

* * *

If the configuration file is a minimal configuration file, the new access server starts, but with only one interface operational. Complete the following steps to connect to the new access server and configure it, starting in EXEC mode:

\| 

Task

 \| 

Command

 \|
\| 

**Step 1** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Establish a Telnet connection to the existing access server.

 \| 

telnet Existing[1](#wpxref14288)

 \|
\| 

**Step 2** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
From the existing access server, establish a Telnet connection to the new access server.

 \| 

telnet Newcommserver1

 \|
\| 

**Step 3** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Enter privileged EXEC mode.

 \| 

enable password[2](#wpxref14299)

 \|
\| 

**Step 4** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Enter setup mode to configure the new access server.

 \| 

setup[3](#wpxref14306)

 \|

[1](#wpxref14289) These commands are documented in the "Terminal Service Connections" chapter in the Cisco Access Connection Guide.

[2](#wpxref14300) This command is documented in the "User Interface Commands" chapter in the Access and Communication Servers Command Reference.

[3](#wpxref14307) This command is documented in the Access and Communication Servers Getting Started Guide.

## Manually Load a System Image from ROM Monitor

If your access server does not find a valid system image, or if its configuration file is corrupted at startup and the configuration register is set to enter ROM monitor mode, the system might enter ROM monitor mode. From this mode, you can manually load a system image from Flash, from a network server file, or from ROM.

You can also enter ROM monitor mode by restarting the access server and then pressing the Break key during the first 60 seconds of startup.

### Manually Boot from Flash Memory

To manually boot from Flash memory, complete the following tasks:

\| 

Task

 \| 

Command

 \|
\| 

**Step 1** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Restart the access server.

 \| 

reload

 \|
\| 

**Step 2** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Press the Break key during the first 60 seconds while the system is starting up.

 \| 

Break

 \|
\| 

**Step 3** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Manually boot the access server.

 \| 

b flash \[filename]

 \|

In the following example, the access server is manually booted from Flash memory. Because no _filename_ is specified, the first file in Flash memory will be loaded.

Booting igs-bfpx.11.0 from Flash address space

F3: 3911536+96836+319604 at 0x3000060

Use, duplication, or disclosure by the Government is

subject to restrictions as set forth in subparagraph

(c) of the Commercial Computer Software - Restricted

Rights clause at FAR sec. 52.227-19 and subparagraph

(c) (1) (ii) of the Rights in Technical Data and Computer

Software clause at DFARS sec. 252.227-7013.

                 San Jose, California 95134-1706

3000 Software (IGS-BFPX), Version 10.2

Copyright (c) 1986-1995 by Cisco Systems, Inc.

Compiled Tue 05-Jul-94 16:14

% System running from device (System flash) being initialized.

   Setting System flash access to read-only.

SNMP Research SNMP Agent Resident Module Version 12.2.0.0

Copyright 1989, 1990, 1991, 1992, 1993, 1994 SNMP Research, Inc.

Cisco 2500 (68030) processor (revision A) with 1024K/1024K bytes of memory.

Processor board serial number 01244583

X.25 software, Version 2.0, NET2, BFE and GOSIP compliant.

SuperLAT software (copyright 1990 by Meridian Technology Corp).

Authorized for Enterprise software set.  (0x0)

1 Ethernet/IEEE 802.3 interface.

2 Serial network interfaces.

32K bytes of non-volatile configuration memory.

4096K bytes of processor board System flash. (Read only mode)

Press RETURN to get started!

In the following example, the **boot flash** command is used with the image filename  
_igs-bfpx.102.1_. That is the file that will be loaded.

Booting igs-bfpx.102.1 from Flash address space

F3: 3911536+96836+319604 at 0x3000060

Use, duplication, or disclosure by the Government is

subject to restrictions as set forth in subparagraph

(c) of the Commercial Computer Software - Restricted

Rights clause at FAR sec. 52.227-19 and subparagraph

(c) (1) (ii) of the Rights in Technical Data and Computer

Software clause at DFARS sec. 252.227-7013.

              San Jose, California 95134-1706

3000 Software (IGS-BFPX), Version 10.2

Copyright (c) 1986-1994 by Cisco Systems, Inc.

Compiled Tue 05-Jul-94 16:14

% System running from device (System flash) being initialized.

   Setting System flash access to read-only.

SNMP Research SNMP Agent Resident Module Version 12.2.0.0

Copyright 1989, 1990, 1991, 1992, 1993, 1994 SNMP Research, Inc.

Cisco 2500 (68030) processor (revision A) with 1024K/1024K bytes of memory.

Processor board serial number 01244583

X.25 software, Version 2.0, NET2, BFE and GOSIP compliant.

SuperLAT software (copyright 1990 by Meridian Technology Corp).

Authorized for Enterprise software set.  (0x0)

1 Ethernet/IEEE 802.3 interface.

2 Serial network interfaces.

32K bytes of non-volatile configuration memory.

4096K bytes of processor board System flash. (Read only mode)

Press RETURN to get started!

### Manually Boot from a Network File

To manually boot from a network file, complete the following tasks in EXEC mode:

\| 

Task

 \| 

Command

 \|
\| 

**Step 1** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Restart the access server.

 \| 

reload

 \|
\| 

**Step 2** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Press the Break key during the first 60 seconds while the system is starting up.

 \| 

Break

 \|
\| 

**Step 3** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Manually boot the access server.

 \| 

b filename \[ip-address]

 \|

In the following example, the access server is manually booted from the network file _network1_:

### Manually Boot from ROM

To manually boot the access server from ROM, complete the following steps in EXEC mode:

\| 

Task

 \| 

Command

 \|
\| 

**Step 1** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Restart the access server.

 \| 

reload

 \|
\| 

**Step 2** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Press the Break key during the first 60 seconds while the system is starting up.

 \| 

Break

 \|
\| 

**Step 3** ![](https://www.cisco.com/en/US/i/templates/blank.gif)
Manually boot the access server from ROM.

 \| 

b

 \|

In the following example, the access server is manually booted from ROM:

### Use the System Image Instead of Reloading

To return to EXEC mode from the ROM monitor to use the system image instead of reloading, perform the following task in ROM monitor mode:

\| 

Task

 \| 

Command

 \|
\| 

Return to EXEC mode to use the system image.

 \| 

continue

 \| 
 [https://www.cisco.com/en/US/docs/ios/11_0/access/configuration/guide/acsysimg.html#wp3634](https://www.cisco.com/en/US/docs/ios/11_0/access/configuration/guide/acsysimg.html#wp3634)

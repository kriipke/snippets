# Configure iSCSI connections with PowerShell | 4sysops
Windows offers the iSCSI initiator applet, which enables configuring iSCSI connections interactively. However, if you want to automate connecting and disconnecting iSCSI targets, you can use PowerShell. It covers the functions of the GUI tool with its iSCSI module.

-   [Author](#abh_about)
-   [Recent Posts](#abh_posts)

[![](https://4sysops.com/wp-content/uploads/avatars/9/37a6fefa42222eaa308e1e280814446f-bpfull.jpg)
](https://4sysops.com/members/wolfgang-sommergut/ "Wolfgang Sommergut")

Wolfgang Sommergut has over 20 years of experience in IT journalism. He has also worked as a system administrator and as a tech consultant. Today he runs the German publication [WindowsPro.de](http://www.windowspro.de/).

[![](https://4sysops.com/wp-content/uploads/avatars/9/37a6fefa42222eaa308e1e280814446f-bpfull.jpg)
](https://4sysops.com/members/wolfgang-sommergut/ "Wolfgang Sommergut")

Latest posts by Wolfgang Sommergut ([see all](https://4sysops.com/members/wolfgang-sommergut/))

When you start the applet for the iSCSI initiator on a computer on which a connection has never been configured, you will receive a message that the iSCSI service is not running. If you prefer to use PowerShell instead of the GUI tool, then you have to take care of the prerequisites yourself.  

## Checking the prerequisites [^](#Content-bal-title "Back to table of contents")

The first step is to query the status of the iSCSI service:

Get-Service -Name MSiSCSI

Get-Service -Name MSiSCSI

```
Get-Service -Name MSiSCSI

```

If it turns out that the service has to be started, you can do this with the following command:

Start-Service -Name MSiSCSI

Start-Service -Name MSiSCSI

```
Start-Service -Name MSiSCSI

```

Next, change the start type to _automatic_, so that it will be executed again every time the computer is rebooted.

Set-Service -Name MSiSCSI -StartupType Automatic

Set-Service -Name MSiSCSI -StartupType Automatic

```
Set-Service -Name MSiSCSI -StartupType Automatic

```

If you are not sure whether there are already connections to an iSCSI target, you can check this using:

```
Get-IscsiTarget

```

In the following figure, the command returns a blank result. Hence, no connections exist.

[![](https://4sysops.com/wp-content/uploads/2021/12/Verify-whether-the-MSiSCSI-service-is-running-and-targets-are-already-connected.png)
](https://4sysops.com/wp-content/uploads/2021/12/Verify-whether-the-MSiSCSI-service-is-running-and-targets-are-already-connected.png)

Verify whether the MSiSCSI service is running and targets are already connected

## Discovery of targets [^](#Content-bal-title "Back to table of contents")

To establish a new target, direct the initiator to the corresponding storage device. This is done by invoking:

New-IscsiTargetPortal -TargetPortalAddress "<IP or FQDN>"

New-IscsiTargetPortal -TargetPortalAddress "<IP or FQDN>"

```
New-IscsiTargetPortal -TargetPortalAddress "<IP or FQDN>"

```

This command uses the default initiator for the discovery process and default port 3260. Both can be specified using parameters, with _Target¬Portal¬Port¬Number_ responsible for specifying the port.

If the client has more than one initiator, you can display them using the command:

```
iscsicli listinitiators

```

You then pass the desired one to the _InitiatorInstanceName_ parameter:

New-IscsiTargetPortal -TargetPortalAddress "192.168.0.180" \\\`

\-InitiatorInstanceName "ROOT\\\\ISCSIPRT\\\\0000\\\_0"

New-IscsiTargetPortal -TargetPortalAddress "192.168.0.180" \\\` -InitiatorInstanceName "ROOT\\\\ISCSIPRT\\\\0000\\\_0"

```
New-IscsiTargetPortal -TargetPortalAddress "192.168.0.180" \\`
-InitiatorInstanceName "ROOT\\\ISCSIPRT\\\0000\\_0"

```

[![](https://4sysops.com/wp-content/uploads/2021/12/Invocation-of-New-IscsiTargetPortal-with-the-instance-name-of-the-initiator.png)
](https://4sysops.com/wp-content/uploads/2021/12/Invocation-of-New-IscsiTargetPortal-with-the-instance-name-of-the-initiator.png)

Invocation of New IscsiTargetPortal with the instance name of the initiator

The next call to _Get-IscsiTarget_ will provide the necessary information for the subsequent procedure. First and foremost, this includes the _NodeAddress_ of the targets found. This is needed to establish a connection with the target.

[![](https://4sysops.com/wp-content/uploads/2021/12/Connect-to-a-specific-target-by-specifying-a-NodeAddress.png)
](https://4sysops.com/wp-content/uploads/2021/12/Connect-to-a-specific-target-by-specifying-a-NodeAddress.png)

Connect to a specific target by specifying a NodeAddress

In our example, the target is provided by a Synology NAS:

Connect-IscsiTarget -NodeAddress "iqn.2000-01.com.synology:DS214.Target-1.0e1a3dc1d1"

Connect-IscsiTarget -NodeAddress "iqn.2000-01.com.synology:DS214.Target-1.0e1a3dc1d1"

```
Connect-IscsiTarget -NodeAddress "iqn.2000-01.com.synology:DS214.Target-1.0e1a3dc1d1"

```

This command can be varied as needed. For example, the above call will only create a connection until the next reboot. If you want to make it permanent, then add the parameter _-IsPersistent $true_.

If you want to connect all available targets, then the following command does the job:

Get-IscsiTarget | Connect-IscsiTarget

Get-IscsiTarget | Connect-IscsiTarget

```
Get-IscsiTarget | Connect-IscsiTarget

```

## Disconnect targets [^](#Content-bal-title "Back to table of contents")

The PowerShell module iSCSI also has cmdlets for the reverse operations. In our example, to terminate the connection to a target, you would issue the following command:

Disconnect-IscsiTarget -NodeAddress "iqn.2000-01.com.synology:DS214.Target-1.0e1a3dc1d1"

Disconnect-IscsiTarget -NodeAddress "iqn.2000-01.com.synology:DS214.Target-1.0e1a3dc1d1"

```
Disconnect-IscsiTarget -NodeAddress "iqn.2000-01.com.synology:DS214.Target-1.0e1a3dc1d1"

```

Finally, the target portal can be removed in this way:

## Subscribe to 4sysops newsletter!

Remove-IscsiTargetPortal -TargetPortalAddress "192.168.0.180"

Remove-IscsiTargetPortal -TargetPortalAddress "192.168.0.180"

```
Remove-IscsiTargetPortal -TargetPortalAddress "192.168.0.180"

```

Here, be sure to specify the parameters in the same way as before for _New- IscsiTargetPortal_. If you used an IP for the portal address, then you should not specify a DNS name here. The same applies to the _Initiator Instance Name_. If you have originally passed one to the cmdlet, you have to do the same when you remove it. 
 [https://4sysops.com/archives/configure-iscsi-connections-with-powershell/](https://4sysops.com/archives/configure-iscsi-connections-with-powershell/)

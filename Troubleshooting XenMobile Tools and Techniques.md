# Troubleshooting XenMobile: Tools and Techniques
### **XenMobile Analyzer**

Easy to miss, but by far the easiest to use and among the more powerful troubleshooting tools. XM analyzer ”simulates” enrolling a device, authentication to both XenMobile and the NetScaler Gateway, browsing to internal URLs with Secure web as well as ShareFile SSO and Secure Mail Auto Discovery. When I’m setting up a new environment or troubleshoot an existing one, I usually start over here since it gives me a nice breakdown of which step fails as well as suggestions for possible fixes. One nice thing is that you can actually schedule reports to run at regular intervals, provided of course you allow enrolls without PIN.  

[![](https://xenit.se/app/uploads/2017-12-20-13_59_10-XenMobile-Analyzer.png)
](/app/uploads/2017-12-20-13_59_10-XenMobile-Analyzer.png)

XenMobile Analyzer results

XenMobile Analyzer is available at [https://xmanalyzer.xm.citrix.com/](https://xmanalyzer.xm.citrix.com/) after logging in with your Citrix account.

### XenMobile Connectivity Check

Available in the admin console, under Troubleshooting and Support > Diagnostics > XenMobile Connectivity Checks is this test. It is basically trying to reach other servers necessary for XenMobile to work, and allows you to see whether it works or not. It is a simple way to check if your internal firewalls are blocking something important:  

[![](https://xenit.se/app/uploads/2017-12-20-14_09_27-XEN-DMZ2-01-Remote-Desktop-Connection.png)
](/app/uploads/2017-12-20-14_09_27-XEN-DMZ2-01-Remote-Desktop-Connection.png)

XenMobile Connectivity Checks

### Secure Mail Test Tool

If secure mail is what’s acting up for you, there is actually a dedicated test tool available to use. Much like the XenMobile Analyzer, it runs through a lot of tests and checks where it fails, but the Secure Mail Tool runs on your mobile device allowing you to see the other side of the equation. The only trouble with this tool is that you need to wrap it with the MDX tool in order to use it.  

[![](https://xenit.se/app/uploads/2017-12-20-14_23_00-secure-mail-test-tool-Sök-på-Google.png)
](/app/uploads/2017-12-20-14_23_00-secure-mail-test-tool-Sök-på-Google.png)

Secure Mail Test Tool

You can find more info about the test tool here: [https://support.citrix.com/article/CTX141685](https://support.citrix.com/article/CTX141685)

### XenMobile Logs

Logs, of course, should hardly need to be mentioned. There are several layers of logs: Debug and User (and admin audit) logs on the appliance, iOS/Android logs on the devices as well as logs from Secure Hub. Make sure to use these to your advantage, and remember that you can change the log levels both when reporting a problem in Secure Hub as well as under Log Settings in the admin UI.  
Browsing these logs, however, can be quite a daunting task. I usually start with only the appliance debug log, since you tend to get most of what you need from there. A good start with the debug log would be to check for exceptions, the first row usually gives you the information you need to fix it. Other than that you might need to search the logs for a username or something related to the problems (for example CSR if you’re having trouble enrolling client certificates). If you are able to reproduce the error, make sure you rotate the logs before you reproduce as that’ll leave you with a much shorter log to sift through.

### NetScaler AAA log

Most implementations of XenMobile use NetScaler for load balancing and MAM Gateway, and of course a lot of problems could arise here as well. The most basic way to check the NetScaler is to see if it actually accepts your authentication.  
To do this, logon to the NetScaler over CLI (PuTTY or other SSH client to the same hostname as the NetScalers, log on with your usual credentials) and run the commands below:

    shell
    cat /tmp/aaad.debug

Now you can see the log live, if it’s a well used NetScaler this might be rather busy so you will have to start the logging just before you log on and then stop it afterwards. When you’ve done a logon you’ll get something similar to the image below:  

[![](https://xenit.se/app/uploads/2017-12-20-14_41_43-XEN-DMZ2-01-Remote-Desktop-Connection.png)
](/app/uploads/2017-12-20-14_41_43-XEN-DMZ2-01-Remote-Desktop-Connection.png)

NetScaler aaad.debug log

Here, the top (blurred) rows lists which AD groups the user is in, this is useful to see if the user is actually in that group you connected your delivery policy to. What you want to see is something like the ”sending accept to kernel” lines you can see on the screenshot. This means the user has authenticated to the NetScaler properly. Likewise a ”sending reject” means the user login was not accepted. If you’re lucky, maybe the user was inputting the wrong password all those times?

### NetScaler Policy Hits

XenMobile depends heavily upon session policies for XenMobile to work properly, if these policies doesn’t ”hit” for the user you will most certainly see some problems. To check which policies hit when a user logs on, you can check the NetScaler CLI:

    shell
    nsconmsg -d current -g POL_HITS

Run the commands and then let a user log on and you will get something like the output below.  

[![](https://xenit.se/app/uploads/2017-12-20-14_56_01-XEN-DMZ2-01-Remote-Desktop-Connection.png)
](/app/uploads/2017-12-20-14_56_01-XEN-DMZ2-01-Remote-Desktop-Connection.png)

NetScaler Policy Hits

Do note that this isn’t user specific, all hits occurring during your logging time will be present in the output so you will need to know what you are looking for. If you used the XenMobile Wizard to set up the NetScaler integration, the most important policy you’ll be looking for is the ”PL_OS_YOUR_NSGW_IP”-one i highlighted above, as it is the default name for the policy to apply on a log on from a mobile device. You can also see which LDAP policies apply, and if your certificate policies apply as expected.  

Also, as a last, more specific tip. If your appliance is failing to boot, just looping on ”starting main app”, make sure that the database is available to the appliance. In my experience this is the problem almost every time.

Tags : 
 [https://xenit.se/blog/2017/12/20/troubleshooting-xenmobile/](https://xenit.se/blog/2017/12/20/troubleshooting-xenmobile/)

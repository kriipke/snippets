# CTX-CEM | Log files: DEBUG
# Systems Monitoring

July 7, 2020

Contributed by: 

K C

To ensure optimal uptime for app access and connectivity, you should monitor the following core components in the XenMobile environment.

## XenMobile server[](javascript:void(0))

XenMobile server generates and stores logs on local storage that you can also export to a systems log (syslogs) server. You can configure log settings to specify size constraints, log level, or you can create custom loggers to filter specific events. You can look at XenMobile server logs from the XenMobile console at any time. You can also export information in the logs via the syslog server to your production Splunk logging servers.

The following list describes the different types of log files available in XenMobile:

**Debug log file:** Contains debug level information about core web services of XenMobile, including error messages and server-related actions.

Message format:

`<date> <timestamp> <loglevel> <class name (including the package)> - <id> <log message>`

-   where `<id>` is a unique identifier like sessionID.
-   where `<log message>` is the message supplied by the application.

**Admin audit log file:** Contains audit information about activity on the XenMobile console.

> **Note:**
>
> The same format is used for both admin audit and user audit logs.

Message format:

With the exception of required Date and Timestamp values, all other attributes are optional. Optional fields are represented with “ “ in the message.

`<date> <timestamp> "<username/id>" "<sessionid>" "<deviceid>" "<clientip>" "<action>" "<status>" "<application name>" "<app user id>" "<user agent>" "<details>"`

The following table lists the available admin audit log events:

| **Admin Audit Log Messages for events** | **Status**      |
| --------------------------------------- | --------------- |
| Login                                   | success/failure |
| Logout                                  | success/failure |
| Get admin                               | success/failure |
| Update admin                            | success/failure |
| Get application                         | success/failure |
| Add application                         | success/failure |
| Update application                      | success/failure |
| Delete application                      | success/failure |
| Bind application                        | success/failure |
| Unbind application                      | success/failure |
| Disable application                     | success/failure |
| Enable application                      | success/failure |
| Get category                            | success/failure |
| Add category                            | success/failure |
| Update category                         | success/failure |
| Delete category                         | success/failure |
| Add certificate                         | success/failure |
| Delete certificate                      | success/failure |
| Active certificate                      | success/failure |
| CSR certificate                         | success/failure |
| Export certificate                      | success/failure |
| Delete certificate chain                | success/failure |
| Add certificate chain                   | success/failure |
| Get connector                           | success/failure |
| Add connector                           | success/failure |
| Delete connector                        | success/failure |
| Update connector                        | success/failure |
| Get device                              | success/failure |
| Lock device                             | success/failure |
| Unlock device                           | success/failure |
| Wipe device                             | success/failure |
| Unwipe device                           | success/failure |
| Delete device                           | success/failure |
| Get role                                | success/failure |
| Add role                                | success/failure |
| Update role                             | success/failure |
| Delete role                             | success/failure |
| Bind role                               | success/failure |
| Unbind role                             | success/failure |
| Update config settings                  | success/failure |
| Update workflow email                   | success/failure |
| Add workflow                            | success/failure |
| Delete workflow                         | success/failure |
| Add Active Directory                    | success/failure |
| Update Active Directory                 | success/failure |
| Add masteruserlist                      | success/failure |
| Update masteruserlist                   | success/failure |
| Update DNS                              | success/failure |
| Update Network                          | success/failure |
| Update log server                       | success/failure |
| Transfer log from log server            | success/failure |
| Update syslog                           | success/failure |
| Update receiver updates                 | success/failure |
| Update time server                      | success/failure |
| Update trust                            | success/failure |
| Add service record                      | success/failure |
| Update service record                   | success/failure |
| Update receiver email                   | success/failure |
| Upload patch                            | success/failure |
| Import snapshot                         | success/failure |
| Fetch app store app details             | success/failure |
| Update MDM                              | success/failure |
| Delete MDM                              | success/failure |
| Add HDX                                 | success/failure |
| Update HDX                              | success/failure |
| Delete HDX                              | success/failure |
| Add Branding                            | success/failure |
| Delete Branding                         | success/failure |
| Update SSL offload                      | success/failure |
| Add account property                    | success/failure |
| Delete account property                 | success/failure |
| Update account property                 | success/failure |
| Add beacon                              | success/failure |

**User audit log file:** Contains information related to the user activity from enrolled devices.

> **Note:**
>
> The same format is used for both user audit and admin audit logs.

Message format:

With the exception of required Date and Timestamp values, all other attributes are optional. Optional fields are represented with “ “ in the message. For example,

`<date> <timestamp> "<username/id>" "<sessionid>" "<deviceid>" "<clientip>" "<action>" "<status>" "<application name>" "<app user id>" "<user agent>" "<details>"`

The following table lists the available user audit log events:

| **User Audit Log Messages for events** | **Status**      |
| -------------------------------------- | --------------- |
| Login                                  | success/failure |
| Session time-out                       | success/failure |
| Subscribe                              | success/failure |
| Unsubscribe                            | success/failure |
| Pre-launch                             | success/failure |
| AGEE SSO                               | success/failure |
| SAML Token for Citrix Files            | success/failure |
| Device registration                    | success/failure |
| Device check                           | lock/wipe       |
| Device update                          | success/failure |
| Token refresh                          | success/failure |
| Secret saved                           | success/failure |
| Secret retrieved                       | success/failure |
| User initiated change password         | success/failure |
| Mobile client download                 | success/failure |
| Logout                                 | success/failure |
| Discovery Service                      | success/failure |
| Endpoint Service                       | success/failure |

| **MDM Functions**            | **Status**      |
| ---------------------------- | --------------- |
| REGHIVE                      | success/failure |
| Cab inventory                | success/failure |
| Cab                          | success/failure |
| Cab auto install             | success/failure |
| Cab shell install            | success/failure |
| Cab create folder            | success/failure |
| Cab file get                 | success/failure |
| File create folder           | success/failure |
| File get                     | success/failure |
| File sent                    | success/failure |
| Script create folder         | success/failure |
| Script get                   | success/failure |
| Script sent                  | success/failure |
| Script shell execution       | success/failure |
| Script auto execution        | success/failure |
| APK inventory                | success/failure |
| APK                          | success/failure |
| APK shell install            | success/failure |
| APK auto install             | success/failure |
| APK create folder            | success/failure |
| APK file get                 | success/failure |
| APK App                      | success/failure |
| EXT App                      | success/failure |
| List get                     | success/failure |
| List sent                    | success/failure |
| Locate device                | success/failure |
| CFG                          | success/failure |
| Unlock                       | success/failure |
| SharePoint wipe              | success/failure |
| SharePoint Configuration     | success/failure |
| Remove profile               | success/failure |
| Remove application           | success/failure |
| Remove unmanaged application | success/failure |
| Remove unmanaged profile     | success/failure |
| IPA App                      | success/failure |
| EXT App                      | success/failure |
| Apply redemption code        | success/failure |
| Apply settings               | success/failure |
| Enable tracking device       | success/failure |
| App management policy        | success/failure |
| SD card wipe                 | success/failure |
| Encrypted email attachment   | success/failure |
| Branding                     | success/failure |
| Secure browser               | success/failure |
| Container browser            | success/failure |
| Container unlock             | success/failure |
| Container password reset     | success/failure |
| AG client auth creds         | success/failure |

Citrix ADC also monitors the XenMobile web service state, which is configured with intelligent monitoring probes to simulate HTTP requests to each XenMobile server cluster node. The probes determine whether the service is online and then respond based on the response received. In the event that a node does not respond as expected, Citrix ADC marks the server as down. In addition, Citrix ADC takes the node out of the load-balancing pool and logs the event for use in generating alerts through the Citrix ADC monitoring solution.

You can also use standard hypervisor monitoring tools to monitor the XenMobile virtual machines and to provide relevant alerts regarding CPU, memory, and storage utilization metrics.

## SQL Server and database[](javascript:void(0))

SQL Server and database performance directly affects XenMobile services. The XenMobile instance requires access to the database at all times and goes offline (for example, stops responding) in the event of an outage to the SQL infrastructure. The XenMobile console may continue to function for a while following any disk space issues with SQL Server. To ensure maximum database uptime and adequate performance for the XenMobile workload, you should proactively monitor the state of your SQL Servers. For more information on monitoring your SQL Servers, see [Monitoring and Tuning for Performance Overview](https://docs.microsoft.com/en-us/previous-versions/sql/sql-server-2008-r2/bb500296(v=sql.105)). Additionally, you should adjust resource allocation for CPU, memory, and storage to guarantee service level agreements as your XenMobile environment continues to grow.

## Citrix ADC[](javascript:void(0))

Citrix ADC provides the ability to log metrics to internal storage or to send logs to an external logging server. You can configure the syslog server to export Citrix ADC logs to your production Splunk logging servers. The following logging levels are available in Citrix ADC:

-   Emergency
-   Alert
-   Critical
-   Error
-   Warning
-   Information

The log files are also stored in Citrix ADC storage in the /var/log/ns.log directory and named newnslog. Citrix ADC rolls over and compresses the files by using the GZIP algorithm. Log file names are newnslog.xx.gz, where xx represents a running number.

Citrix ADC also supports SNMP traps and alerts as a monitoring option. For a list of SNMP traps, see [SNMP monitoring](/en-us/xenmobile/server/monitor-support/snmp-monitoring.html). 
 [https://docs.citrix.com/en-us/xenmobile/server/advanced-concepts/xenmobile-deployment/systems-monitoring.html](https://docs.citrix.com/en-us/xenmobile/server/advanced-concepts/xenmobile-deployment/systems-monitoring.html)

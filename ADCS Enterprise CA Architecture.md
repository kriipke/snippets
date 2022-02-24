# ADCS | Enterprise CA Architecture
* * *

* * *

This is the second article of the Active Directory Certificate Service (ADCS) series. I published the first part on April 2018, and you can browse that article before you start reading the second part.

[_**Active Directory Certificate Services - Digital Certificate Overview**_](https://social.technet.microsoft.com/wiki/contents/articles/51429.active-directory-certificate-services-part-1-digital-certificate-overview.aspx)

In that article, we took a deep dive on the basic concepts of Cryptography and Digital Certificate. We discussed how a secure communication can be performed using Digital Certificate. Finally, we introduced Enterprise PKI and compared it with External PKI.

In this article, we will take a deep dive on ADCS , and how it provides a complete solution of the Enterprise PKI need. We will also discuss concepts and terminologies associated with ADCS.

* * *

When you are going to deploy an Internal CA server, you have the option of deploying Standalone CA or Enterprise CA.

Following are the key differences between Standalone CA and Enterprise CA.

-   For Enterprise CA, the Server must be domain joined at it leverages many features offered by ADCS. For Standalone CA, domain join is not mandatory as it does not use any feature offered by ADCS.

-   Enterprise CA offers features like Certificate Template, Certificate Auto enrollment and Key archival. Standalone CA does not offer these features.

-   **Enterprise CA is trusted by all domain joined clients.** For Standalone CA, some mechanism is required to establish that trust relationship, which includes adding the CA manually to the client Certificate Store, or using automation / group Policy to do that. 

**_  
So does it mean that we should not use Standalone CA in our PKI environment ?_**

The most common approach is to use a combination of both, which is as follows :

-   **Follow Standalone approach for the Root CA server.** This is because the Root CA is kept offline most of the time for security reason (we will discuss this later). So if the Root CA is a member of domain, there could be issues if the Root CA is offline for a long time and not logged into AD Domain for extensive period.

-   **Follow Enterprise approach for the  subordinate CA Servers.** This is because  the  subordinate CA servers should be trusted by all members of the AD Domain, and they need to use multiple features offered by ADCS which include Certificate Template, Certificate Auto enrollment and Key archival.

* * *

## Root CA

The Enterprise PKI hierarchy starts with the Root CA , also referred as the Trusted Root CA. Root CA is the first CA which needs to be deployed while designing a new PKI environment, and it is the top of the certification hierarchy.

Since Root CA is the top of the certification hierarchy, the certificate is issued to Root CA by the Root CA itself. In other words, **the certificate which is issued to the Root CA is a self sign certificate.**

**In a certificate hierarchy, Root CA Certificate is the only certificate which is self signed. All other Certificate must be issued either by Root CA or Subordinate CAs.**

A typical Enterprise PKI environment follows this approach :

-   Root CA is deployed in standalone mode (Not domain joined).
-   Root CA issues certificate to subordinate CAs.
-   Once all the work related to Root CA is done, it is generally taken offline in a highly secured area.
-     Root CA is not used for day to day purpose. Its only role is to issue certificates to its subordinate CAs.
-   Root CA is brought online again only when it is necessary, for example issuing / renewing certificate to subordinate CA Server or publishing the Root CRL (Certificate Revocation List).

The Root CA is kept offline for security reason. **Root CA is the most critical server of the CA hierarchy.** It is also one of the most critical server for the entire organization. This is because **if the private key of the Root CA is compromised by external attackers, the entire PKI and all certificates issued by that PKI would be compromised and would be invalid.**

In Windows certificate Store, the root certificate is stored under **"_Trusted Root Certification Authorities_"** Store.

## Subordinate CAs

**In a PKI environment, all CAs which are not Root CA are called subordinate CAs.**

The exact number and hierarchy of subordinate CAs depends on the PKI design. There can be only one layer of subordinate CAs which directly report to Root CA, or in a large and complex environment there can be multiple layers of subordinate CAs.  

In general, the subordinate CAs are deployed in two different layers , which are :

1.  Intermediate CAs and
2.  Issuing CAs.

### 1) Intermediate CAs

Intermediate CAs are subordinate CAs, which directly seats under the Root CA.

Intermediate CAs act as a layer between Trusted Root CA and Issuing CAs. As Root CA is extremely critical, it just issues certificate to Intermediate CAs , which in turn issue certificates to issuing CAs. This is called "Certificate Trust Chain" which we will discuss in the upcoming section.

Intermediate CAs are also used a Policy CAs, which dictates what kind of certificates can be requested from the Root CA.

In Windows certificate Store, the root certificate is stored under "_Intermediate Certification Authorities_" Store.

### 2) Issuing CAs 

These are the CAs which are deployed to issue certificates to end users , computers and applications.

**In a large PKI environment, there are more than one issuing CAs, each playing a specific role.** Example: Infrastructure CA , User CA and so on. While Infrastructure CA will issue Certificates to computers, devices and applications;  User CA would issue certificates to users.

You can further subdivide Infrastructure CA if you want to assign more granular roles, such as  Computer CA and Application CA.

**It is important to note that in the CA setup wizard, you will not get the options of Intermediate CA and Issuing CA, neither you will get the options for User CA or Computer CA. This is because all these CA types fall under the broader category called Subordinate CA. So while running the CA setup wizard (or commands), you will get only two options, to specify the current CA Server as Root CA or Subordinate CA.**

_Then how shall we design the hierarchy between Subordinate CA , such as Intermediate CA, User CA, Computer CA and so on ?_

**During the CA server setup, when you declare a CA as Subordinate CA, it will ask you the parent CA Server name**, **which will issue certificate to this CA Server.**

-   For Root CA Server, there will be no parent CA, and it will issue a self sign certificate to itself.
-   For Intermediate CA Server, the parent CA will be the Root CA.
-   For User and Computer CA servers, the parent CA will be the Intermediate CA Server.

Like this way, you will create you PKI hierarchy, and in the back end the Certificate Chain will be created.  

* * *

**The certificate chain, also known as the certification path, is a hierarchical list of certificates used to authenticate an entity.**

-   At the the bottom of the Certificate chain is the SSL certificate, which needs validation. 

-   At the top of the Certificate chain is the Root certificate, where the certificate chain terminates.

-   Any certificate between SSL certificate and root Certificate is called as Intermediate certificate, which is issued by intermediate CAs.

-   Each Certificate is digitally signed by the higher level certificate in the chain. The SSL certificate is signed by the intermediate certificate(s), which is in turn signed by Root certificate. The Root Certificate is signed by itself.

_**What is the practical usage of Certificate Chain ?**  
_

When a web browser or a computer uses a Certificate, it needs to ensure that the certificate is issued by a trusted source. In general, the Root CA is trusted by all the entities. For example, for ADCS based PKI, it is the Root CA certificate which is trusted by all the domain computers and users. So in this case, the domain computers, browsers and users do not trust issuing CAs or intermediate CAs, but they only trust the Root CA self sign certificate.

So the issuing CAs needs to "prove" , that they are trusted by intermediate CA, which in turn is trusted by the Root CA. So, the validation will follow this path until it reaches the Root Certificate, which is already added in the computers / users / browsers trusted certificate store. 

**This entire bundle of issuing certificate, intermediate certificate and the root certificate is collectively referred as "Certificate Chain".**

One important point to remember here is, while installing / importing the certificate, the entire chain needs to be installed so that the certificate could be authenticated. If Root and Intermediate Certificates are already installed in the respective stores, those need not to be imported.

**Recommended practice to ensure a valid certificate chain**  
**In a single / multi domain environment, use Group Policy to push Certificates to client computers and users across all domains.**

-   The Policy can be found at : Computer Configuration\\Policies\\Windows Settings\\Security Settings\\Public Key Policies

-   Push Root certificate in the "Trusted Root Certification Authorities" store.

-   Push Intermediate Certificates in "Intermediate Certification Authorities" store.

-   Link Group Policy to appropriate Site, Domain or OU.

-   Check whether client computers and users are having these certificates in respective certificate stores.

Once the Root and Intermediate certificates are there in the store, you only need to import the rest of the certificates in the chain, during certificate import.

* * *

For a CA Admin who is handling a large and complex PKI environment, the Enterprise PKI Tool (PKIVIEW.MSC) is one of the most useful tools.

Once run from any of the domain joined CA server, the PKIVIEW tool gives a comprehensive picture which is as follows :

-   **It discovers and captures the PKI structure of the entire AD Forest.** This includes the number of Root CA and all Subordinate CAs.

-   The CA hierarchy of all PKI environment within the forest.

-   **It validates the certificates and CRLs to ensure that they are working correctly.** If they are not working correctly or if they are about to fail, it provides a detailed warning or some error information.

The PKIVIEW tool gets all this information from Active Directory configuration partition.

**PKIVIEW is one of the most useful tools for a CA Admin, who gets a comprehensive picture and overall health status of the Enterprise PKI environment. The CA admin should run this tool on a regular basis and check the status. Any alert / warning on the PKIVIEW console must be investigated immediately without any delay.**

In the below image, we have have captured the PKI architecture of our lab forest corp.org, by running PKIVIEW in one of the CA Servers.

From the below diagram we can tell that our PKI infrastructure consists of :

1) Corp Root CA

2) Intermediate CA

3) Two Issuing CAs : a) Computer CA b) User CA

[![](https://social.technet.microsoft.com/wiki/resized-image.ashx/__size/550x0/__key/communityserver-wikis-components-files/00-00-00-00-05/6114.Img1.JPG)
](https://social.technet.microsoft.com/wiki/cfs-file.ashx/__key/communityserver-wikis-components-files/00-00-00-00-05/6114.Img1.JPG)

* * *

While designing and deploying a PKI environment with the help of ADCS, the CA admin should have a clear idea of the below points :

1) What is the boundary of this PKI environment ? In other words, which computers, users and application would trust this PKI environment ?

2) Does a single PKI environment work for multiple AD Domains, or we need separate Root CA and Subordinate CAs for each domain ?

3) Can we deploy multiple PKI environment under a single AD Domain / Forest ?

1. **The boundary of ADCS based PKI environment is Active Directory Forest.** In a multi domain forest, you can deploy the Root and Subordinate CAs to any of the domains, and it can issue certificate to any user and computer within that entire forest.

However, **in a multi domain environment, the recommended approach is to deploy the PKI infrastructure in the Forest Root Domain**.

2. **Most of the information related to PKI (such as CA hierarchy, certificate templates etc) are stored in the Configuration Partition of Active Directory, which is replicated to all domain controllers within entire forest.**

3. On the other hand, **you can deploy two (or more) independent PKI structures within a single domain or forest, each having its own Root CA and Subordinate CAs.** In such scenario, the PKIVIEW tool will display all independent structures along with corresponding Root CAs and Subordinate CAs.

However, **deploying more than one PKI structure within a single domain / forest will increase the complexity, and is not recommended unless there is specific business need.**

* * *

**One of the benefits of using ADCS is that it offers Certificate Template feature.**

As the name suggests, Certificate Template defines some predefined structure and policies associated with the certificate, some of which are as follows :

1) Format and Content of Certificate suitable for specific need. 

2) To specify which users and computers can request for what type of certificates though well defined permission settings.

3) To specify the enrollment process, manual or Auto enrollment.

4) To specify whether the private key would be exportable or not.

**The certificate templates and their permissions are defined in Active Directory,  and are valid within the forest. This is because Certificate Templates are stored in AD Configuration partition, which is replicated to the entire forest.**

**When a certificate template is created in one CA, it should be available to all other CAs within that forest, which is accomplished through AD Replication.**

Once you open the Certificate Authority Console in the CA server, you will see a section called “Certificate Template”. This section shows you all the templates which are enabled for this CA. Now, if you right click on “Certificate Template” and click “Manage”, you will see all the default templates. It is a good practice to copy the default template, and to customize the duplicate template rather than customizing the default template itself.

During duplication, you can configure multiple properties like “Make the Private Key Exportable”.

An important point to note here is, **although the certificate template is replicated to all CA servers within the forest, only the originating CA server can issue certificate using that template unless you activate the template in any other CA server.** This is to ensure that the request for a certain type of certificate would reach a certain CA server. If the certificate is issued randomly by any CA server, it would be difficult to track, segregate and control which CA is issuing what type of certificates.

**In case the originating CA server becomes unavailable and you need to issue certificate from that template, you can active that template in another CA server,which would act as a fault tolerance.**

It is also important to note that by default, **only below two entities can create and manage certificate templates :**

• **Domain Admin of the Forest Root Domain**

**This default behavior can be altered** by modifying security permission of below two containers under Configuration permission > Services > Public Key Services :

• Certificate Templates

• OID

Using ADSI edit, if you assign Full Control right to any user or group on these two containers, then that user / group will have access to create and manage Certificate templates on any CA within that AD Forest.

* * *

Certificate enrollment request to a CA server can be submitted manually, or we can leverage an ADCS feature called Auto-enrollment. In this section, we will discuss both the approaches.

## Manual Enrollment

Manual certificate enrollment request can be submitted in multiple ways :

**1) Certificates MMC**

This method can be used only from a Windows based System, which can be Server or Workstation. We have to open Certificates console, either through MMC or typing 'certmgr.msc' in Run.

We can request certificate for User, Computer or Service Accounts.

**2) Web Enrollment**

The web enrollment method allows a web page, through which we can interact with the CA Server. 

The web page would give us multiple options like :

• To request a new certificate from CA

• To request CAs own certificate (Ex: Root or Intermediate CAs)

• To submit a certificate request

• To retrieve Certificate Revocation List (CRL)

The URL format for web enrollment page is: https&#x3A;//<servername>/certsrv

Some of the advantages of web enrollment method over other methods are :

• The request can be generated from any Operating System, such as Windows or Linux.

• We can request a certificate to a Standalone CA, which is not part of AD Domain.

**3) IIS Console**

If we are requesting web site certificate from an IIS Server, we can also generate the request through the IIS console.

## Auto Enrollment

In a large organization, manually enrolling, retrieving and renewing certificates would be a cumbersome task. To make these regular operation easy, ADCS offers a feature called Auto enrollment.

Using Auto enrollment feature, users and computers can perform below operations without any manual intervention :

1) Certificate Enrollment

2) Certificate Retrieval

3) Certificate Renew

**To leverage Auto enrollment feature, user or computer account must be domain joined.** Standalone systems or non-domain users cannot use auto enrollment.

Auto enrollment needs to be configured in two different areas :

**1) Enable Auto Enrollment through Group Policy :**  
If we want that auto enrollment should be enabled for all domain computers and users, the best way to configure is to edit Default Domain Policy. Otherwise. we have to link the Group Policy to the specific Organizational Unit.

For Computes : Computer Configuration > Windows Settings > Security Settings > Public Key Policies > Certificate Service Client – Auto-enrollment

For Users : User Configuration > Windows Settings > Security Settings > Public Key Policies > Certificate Service Client – Auto-enrollment

**2) Grant Auto Enrollment permission in certificate template :**  
We need to ensure that the user or computer account has Auto Enrollment permission in the specific certificate template. For that, edit the security settings of the certificate template and grant 'Autoenroll' permission to specific group, user or computer account.

If the template is a Computer Certificate template, grant appropriate computer accounts, group or Computer Security Principle.

If the template is a User Certificate template, grant appropriate user accounts, group or User Security Principle.

**Auto enrollment will work when both the above conditions are met.**

If you want detailed steps for setting up Auto enrollment, please visit [**this link.**](https://www.vkernel.ro/blog/set-up-automatic-certificate-enrollment-autoenroll)

* * *

This is the second article in this series. In this article, we have explored the architecture of Enterprise CA and components associated with it. 

The areas which we have covered in this article are :

-   CA Architecture and Hierarchy
-   Certificate Chain
-   How to view Enterprise PKI architecture and overall health
-   How an Enterprise CA integrates with AD Domain
-   Certificate Template
-   Certificate Enrollment

* * *

[](https://social.technet.microsoft.com/wiki/contents/articles/51429.active-directory-certificate-services-part-1-digital-certificate-overview.aspx)\*   **\_\[**Active Directory Certificate Services - Digital Certificate Overview**]([https://social.technet.microsoft.com/wiki/contents/articles/51429.active-directory-certificate-services-part-1-digital-certificate-overview.aspx](https://social.technet.microsoft.com/wiki/contents/articles/51429.active-directory-certificate-services-part-1-digital-certificate-overview.aspx))\_**

-   _**\[**Active Directory Certificate Services - AIA , CRL and OCSP**]([https://social.technet.microsoft.com/wiki/contents/articles/53271.active-directory-certificate-services-aia-crl-and-ocsp.aspx](https://social.technet.microsoft.com/wiki/contents/articles/53271.active-directory-certificate-services-aia-crl-and-ocsp.aspx))**_

If you find this article useful, please visit my other articles [**here**](https://social.technet.microsoft.com/wiki/contents/articles/52534.user-page-subhro-majumder.aspx).  

* * *

 [https://social.technet.microsoft.com/wiki/contents/articles/53249.active-directory-certificate-services-enterprise-ca-architecture.aspx](https://social.technet.microsoft.com/wiki/contents/articles/53249.active-directory-certificate-services-enterprise-ca-architecture.aspx)

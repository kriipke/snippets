# Active Directory Certificate Services - AIA , CRL and OCSP - TechNet Articles - United States (English) - TechNet Wiki
* * *

This is the third article of the ADCS series. 

Before reading this article, please go through the previous two articles in this series which are :

-   [**Active Directory Certificate Services - Digital Certificate Overview**](https://social.technet.microsoft.com/wiki/contents/articles/51429.active-directory-certificate-services-digital-certificate-overview-part-1.aspx)

-   **\[**Active Directory Certificate Services - Enterprise CA Architecture**]([https://social.technet.microsoft.com/wiki/contents/articles/53249.active-directory-certificate-services-part-2-enterprise-ca-architecture.aspx](https://social.technet.microsoft.com/wiki/contents/articles/53249.active-directory-certificate-services-part-2-enterprise-ca-architecture.aspx))**

**In this article, we will discuss few important concepts related to Certificate : 1) AIA 2) CRL 3) OCSP**

* * *

Let’s assume a SSL / TLS client (Ex: Web Browser) receives  a digital certificate from a web server. The certificate is issued to the web server by the User CA.

In order to ensure that the certificate is issued by a trusted entity , the SSL client needs to validate the entire chain. The validation will continue until it will reach to the Root CA. The validation process will stop once it will reach to the Root CA, because Root CA is already trusted by the web browser and does not need further validation.

**AIA (Authority Information Access) is useful during this validation process. The AIA field captures the location of the issuer certificate, and client can download a copy of the issuer certificate during each stage of validation.**

•  In this case, the web site certificate is issued by ComputerCA, so the AIA field shows the location of ComputerCA.

•  Similarly, the AIA filed of Computer CA points to the location of its issuing CA, which is Intermediate CA.

•  The AIA filed of Intermediate CA points to the location of its issuing CA, which is Root CA.

•  AIA is not applicable for Root CA, as it is using self signed certificate. Moreover, Root CA is the Trust Anchor which should already be trusted by the web browser.

So AIA is mostly instrumental to furnish a copy of the intermediate CAs to the SSL client, during the validation process. 

**Another key function of AIA is to support Microsoft Online Certificate Status Protocol (OCSP) Responder.** **We will discuss OCSP later, but the location of OSCP Responder needs to be added in AIA.**

The OCSP responder server FQDN must be specified in the AIA field, following below format :

http&#x3A;// <FQDN of OSCP responder server>/ocsp

**AIA repositories support HTTP and LDAP protocol.**

* * *

**Certificate Revocation List (CRL) contains the list of non-expired revoked certificates. It does not contain the revoked certificate itself, but the serial number of the revoked certificate.**

**CRL Distribution Point (CDP) is the repository where CRL can be found and downloaded.**

**Validating CRL is one of the most important part of certificate validation,** as the client wants to ensure that the certificate is not revoked by the issuer. 

• If the certificate serial number is not found in the CRL, that means the certificate is not revoked. 

• If the certificate serial number matches with any serial number within CRL which is signed by the issuer, that means the certificate has been revoked and no longer valid. Therefore, most of the SSL based applications do not work until they can access a valid CRL which is mentioned in the certificate.

Like Certificates, each CRL is digitally signed by the CA which has published the CRL. Also, each CRL has a validity period beyond which it is no longer valid.

## Base and Delta CRL

There are two types of CRL :

a) **Base CRL :** This contains the list of all Revoked certificate. This can be a long list in a large environment.

b) **Delta CRL :** This contains the list of all revoked certificates since the last base CRL is published.

CRL is generally cached to the client. If the base CRL is already cached, client has to download only the delta CRL.

**We do not recommend to enable Delta CRL in Root and Intermediate CAs.** The reason is, Root CA issues certificate only to Intermediate CAs, and Intermediate CAs issue certificate only to issuing CAs. These certificates are hardly revoked unless there is a security compromise. So for these CAs, Base CRL is enough and there is no need to configure Delta CRL.

**Delta CRL is mainly useful for Issuing CAs,** which issue (and probably revoke) a large number of certificates and where the Base CRL is too large to be downloaded every time. For example, when a user leaves the organization, the user certificate is generally revoked from the issuing CA so that it cannot be misused. As the number of revoked certificate grows, the base CRL becomes larger, and Delta CRL becomes useful.

**While Delta CRL is useful in the situations where the Base CRLs become too large over time, Delta CRL has its own disadvantages.**

•  Delta CRL adds some complexity in CRL design. In addition to the Base CRL,  CA administrator needs to take care of Delta CRL validation, publication interval etc.

•  While downloading the CRL, client first has to download the Base CRL if it is not already downloaded or Base CRL has expired. After that, client needs to download the latest Delta CRL. This adds some layer of complexity from the client side.

•  Even if the Delta CRL is enabled, still client needs to download the Base CRL time to time, once the Base CRL validity is expired. However, if we keep the Base CRL validity higher, then the frequency of downloading Base CRL is less, and client only needs to download Delta CRL once the existing Delta CRL is expired and new Delta CRL is published.

## CDP Protocols

**You can publish CDP using LDAP or HTTP protocol. Do not use HTTPS**, as client cannot access CRLs using HTTPS due to authentication issue.

**In a Windows based domain environment, publishing CDPs using LDAP has some advantages.**

• It offers fault tolerance, through Active Directory replication.

• With the help of AD Sites, client can download CRL from the nearest Domain Controller.

However, non-windows and Windows workgroup clients cannot leverage LDAP to download CRL. Also, to access the CDP over LDAP, various firewall ports need to be opened.

**However, in general, publishing CDP using HTTP has some advantages over LDAP.** First of all, any client can access an HTTP based CDP irrespective of OS and Domain join status. Secondly, It does not require complex firewall rules to be implemented. 

However, **unlike LDAP, HTTP does not offer built in fault tolerance and AD Site advantage.**

• More than one CDP can be included in the CDP Extension.

• While checking a CRL, the client access the list of CDPs one by one, as they are mentioned in the CDP extension, until it finds a valid CRL file.

• If the client does not find a valid CRL find within a certain amount of time, then there will be time out. Therefore, the CA admin needs to plan the CDP in such a way that the CRL can be accessed by all clients before it’s timed out.

• In an environment where most of the clients are domain joined Windows system, you can publish two CDP, the first one using LDAP and the second one using HTTP. That way, most of the domain joined system can use the first option, whereas non windows and workgroup client can use the second option.

## CRL Validity and Publication Interval

There are two important parameters which are associates with CRL :  

• **CRL Validity period** : Like Certificate, each CRL has a validity period. Beyond the validity period the CRL would expire and would no longer be valid.

• **CRL publishing interval** : The CRL publishing interval determines how often new CRL would be published.

**These two parameters are applicable for both types of CRLs : Base CRL and Delta CRL.**

While configuring these two parameters for Base and delta CRLs, we should consider below points :

• If the validity period is too long, it means client will not look for a new CRL till that time. This has some advantages as well as disadvantages. 

• The advantage of having a higher validity CRL is, as long as the CRL is valid, client does not look for a new CRL. This gives us more time during a CA recovery / restore.

• However, this approach also has its own disadvantage. In case we have revoked a certificate, client will not download it unless the existing CRLs validity would expire.

• Generally, the Base CRL validity of Root CA and Intermediate CAs should be kept higher (Ex: 6 months to 1 year) as they issue limited number of certificates. Also, in most of the organizations Root & Intermediate CAs are kept offline, so it is better to keep the CRL validity period higher for these CAs.

• Validity of Delta CRL is not applicable here, as we do not recommend enabling Delta CRL for Root and Intermediate CAs.

• For Issuing CAs (Ex: Computer CAs, User CAs), you can enable Delta CRL if the Base CRL is too large. If you enable Delta CRL, you can keep the validity of Base CRL higher and publish Delta CRL more frequently. For example, you can keep the validity of Base CRL as 2 weeks, while validity of Delta CRL can be 3 days. However, it  depends on some factors, like how frequently these CAs revoke certificates.

For more details regarding CRL publication strategies, please refer [**this article**](https://blogs.technet.microsoft.com/xdot509/2012/11/26/pki-design-considerations-certificate-revocation-and-crl-publishing-strategies/) . 

* * *

Another factor which needs to be considered is the High Availability of CDP and AIA.  

**Please note that in order to make the revocation checking successful, client must access both Base CRL and Delta CRL (if enabled). So a CA administrator must plan to ensure that CRLs of all CAs are accessible and there is some fault tolerance.**  

**Similarly, in order to validate the issuer’s certificate and (if enabled) to access OSCP, the client must access AIA .**  

• When CDPs and AIAs are published through LDAP, the High Availability is taken care by Active Directory, through AD replication. However, non-Windows clients and Workgroup clients cannot access CRLs and AIA which are published through LDAP.  

• HTTP is the preferred method over LDAP for publishing CDP and AIA, where non-windows and workgroup clients are concerned. However, one major drawback of HTTP approach is, unlike Active Directory, it does not offer built in fault tolerance. We need to deploy more than one web servers behind a Load Balancer, to achieve Fault Tolerance.  

**One important point to note here is, CDP and AIA can be configured in different servers, other than CA servers.**  

• When we use LDAP, both will be stored in Domain Controllers within Configuration partition.  

• For HTTP, we can use a separate web server (IIS/Apache or any other) where CDPs and AIA of all CAs are configured. As mentioned earlier, we can deploy multiple web servers behind a Load Balancer to achieve Fault Tolerance.   

If you want to publish CRL and AIA in a separate web server, please refer to **[this article](https://www.vkernel.ro/blog/how-to-publish-the-crl-and-aia-on-a-separate-web-server) .**

* * *

 In the previous section, we have seen that **the CRL approach has some drawbacks.**

First of all, **there are certain management overhead involved in order to configure CDP and manage it**. Also, **the client needs to download these CRLs and go through each serial number to confirm that the specific certificate is not revoked**.

Starting from Windows Server 2008, Microsoft launched a feature called Online Certificate Status Protocol, or in short OCSP.

The OCSP approach is little different than the CRL approach. In the CRL approach, the client goes through a given list (or lists) to ensure that a specific serial number is not there. 

**OCSP has two major components: 1) Online Responder 2) OCSP Client**

Online Responder (Or OSCP Responder) is the server component, which accepts requests from OCSP client to check the revocation status of a certificate. Before making the request, client uses AIA extension to check whether OSCP is configured, and if yes what is the OSCP responder location.

The OCSP responder server FQDN must be specified in the AIA field, following below format :  
http&#x3A;// <FQDN of OSCP responder server>/ocsp

Windows Vista or above can act as an OCSP client.

For more details regarding OSCP, please refer [**this series**](http://blogs.technet.com/askds/archive/2009/06/24/implementing-an-ocsp-responder-part-i-introducing-ocsp.aspx) .

* * *

This is the third article of the ADCS series. In this article, we have covered some of the important areas of Certificate Services 1) AIA 2) CRL & CDP 3) OCSP.

* * *

-   [**Active Directory Certificate Services: Digital Certificate Overview**](https://social.technet.microsoft.com/wiki/contents/articles/51429.active-directory-certificate-services-digital-certificate-overview.aspx)

-   [**Active Directory Certificate Services: Enterprise CA Architecture**](https://social.technet.microsoft.com/wiki/contents/articles/53249.active-directory-certificate-services-enterprise-ca-architecture.aspx)

* * *

 [https://social.technet.microsoft.com/wiki/contents/articles/53271.active-directory-certificate-services-aia-crl-and-ocsp.aspx](https://social.technet.microsoft.com/wiki/contents/articles/53271.active-directory-certificate-services-aia-crl-and-ocsp.aspx) 
 [https://social.technet.microsoft.com/wiki/contents/articles/53271.active-directory-certificate-services-aia-crl-and-ocsp.aspx](https://social.technet.microsoft.com/wiki/contents/articles/53271.active-directory-certificate-services-aia-crl-and-ocsp.aspx)

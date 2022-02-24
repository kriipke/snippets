# ADCS | Digital Certificate 
* * *

* * *

Before we start our discussion on AD CS, we need to understand how Digital Certificate works. Once we discuss and understand all the relevant terminologies and concepts related to Digital Certificate, our discussion on AD CS would be fruitful.

This article is dedicated to the understanding of the Digital Certificate and associated terminologies.

It is important to remember that Microsoft AD CS is mainly for Enterprise, and unlike the Global Certificate Authorities (CA), the validity of Microsoft CA is valid within a well defined boundary, which is typically within the organisation which is deploying AD CS. We will discuss Global CA and will compare that with Active Directory based Enterprise CA. For a Certificate Administrator, it is important to understand the difference between a Global CA and an Enterprise CA. Most of the large organisations typically use both external and internal CAs for different purposes.

* * *

Let's start our discussion from the very basic of Cryptography, which is Encryption.

Encryption is a process, by which information can be transformed (encoded) in such a format, that only authorized parties can read (decode) the information, and unauthorized parties cannot. The purpose of encrypting information is to protect the sensitive data from unauthorized use.

Generally, encryption uses complex mathematical algorithms to encrypt and decrypt information. 

## Encryption and Decryption Keys

An encryption key is a random string of bits used by the encryption algorithm to encrypt the information. 

From encryption standpoint, a message which is not encrypted is called Clear Text. The encrypted message is commonly referred as Cipher Text.

During a secure communication, this encrypted information (Cipher Text) is being sent from sender to the recipient.

In order to decrypt the Cipher Text, the recipient should have the following information:

1.  The algorithm used during Encryption. The same algorithm needs to be used during decryption.
2.   The recipient must have the decryption key.

Using the decryption key along with the decryption algorithm, the encrypted information can be decoded back to Clear Text.

[![](https://social.technet.microsoft.com/wiki/resized-image.ashx/__size/550x0/__key/communityserver-wikis-components-files/00-00-00-00-05/8103.Encryption.png)
](http://social.technet.microsoft.com/wiki/cfs-file.ashx/__key/communityserver-wikis-components-files/00-00-00-00-05/8103.Encryption.png)

## Symmetric and Asymmetric Encryption

In a symmetric encryption algorithm, both the sender and the recipient use the same key to encrypt and decrypt the message.

Examples of the Symmetric Algorithms are:

-    DES (Data Encryption Standard)  3DES (Triple Data Encryption Standard)  AES (Advanced Encryption Standard)

In an Asymmetric encryption algorithm, sender and the recipient use different keys to encrypt and decrypt the message.

These two keys, or key pairs are called Public Key and Private Key, which we will cover in the next section.

Two most popular Asymmetric Algorithms include:

-   RSA (Rivest Shamir Adelman)
-   PGP (Pretty Good Privacy)

## Private and Public Keys

In an Asymmetric encryption system, each participant needs to have a key pair, a private key and corresponding public key.

Private Key , as the name suggests, is a key that is known only to the sender who possess it, and not known to anybody else. For a computer, the Private key is stored locally within that computer which is not accessible from any other computer.

Public Key, on the other hand, can be shared to anyone.

It is important to note that, any data which is encrypted using the private Key can be decrypted by corresponding Public Key (not by any other Public Key). Similarly, any data which is encrypted using the Public Key can be decrypted by corresponding Private Key (not by any other Private Key).

Also, a data which is encrypted by a Private Key, cannot be decrypted by the same Private Key. Similarly, a data which is encrypted by a Public Key, cannot be decrypted by the same Public Key.

It is very important to keep the private key secret. In case the private key is compromised, the entire encryption system using that Private-Public key pair becomes insecure. In such a scenario, it is better to generate a new key pair and reconfigure the encryption system.

## How an Asymmetric Encryption Works

Let’s assume a scenario, where Computer A wants to share some information to Computer B, and the information would be exchanged over Asymmetric Encryption.

Here are the high level steps:

-    Computer B shares its Public key to computer A.

    Computer A encrypts the data using the Public Key of Computer B. Now, this becomes a Cipher text which can be opened only using Computer B’s Private Key.
-    Computer A sends the encrypted data to Computer B over the network. Even if this data is captured by unwanted parties, it cannot be decrypted as nobody (except Computer B) has the Private Key of Computer B. Once the information is received by Computer B, it decrypts the data using its Private Key.

In this way, information is shared securely using Asymmetric Key.

## Session Key

A session key is a single-use symmetric key used for encrypting messages in one communication session.

As the names suggests, a session key is valid for only a single session or transaction. This makes the hackers difficult to break the key, because for every session (even between the same sender and receiver) the session key gets changed.

Please note that encryption using the Session Key is a Symmetric type of Encryption. This means that unlike Private-Public Key Pair, the information is encrypted and decrypted using the same key, the Session Key.

_Why do we need session key, when we are already using Asymmetric Key?_

This is because although Asymmetric Encryption is secure, it is complex, slow and consumes more computer resource. So typically, the Asymmetric Encryption is used only at the beginning of the session, just to share the Session Key in a secure manner. Once the Session Key is shared securely, then rest of the communication generally happens using the Session Key.

## How a secure communication works using a combination of Asymmetric Key and Symmetric Key

Let’s consider a scenario, where Computer A wants to have some secured Transaction through Computer B. Here are the high level steps followed:

-    Computer A already has the Public Key of Computer B.
-    Computer A randomly generates a Session Key, and encrypts the Session Key using the Public Key of Computer B. 
-    Computer A sends the encrypted Session Key to Computer B over the network. Even if this Session Key is captured by unwanted parties, it cannot be decrypted as nobody (except Computer B) has the Private Key of Computer B.
-    Once the Session Key is received by Computer B, it decrypts the Session Key using its Private Key. 
-    Now that both the Computer has the Session Key, the rest of the transaction would take place using that Session Key. 
-    Once the transaction is over and session is terminated (Ex: Logging of from the Application/ Browser) , the Session Key becomes invalid. 

* * *

A Digital Certificate is a digital form of identification, like a passport or an Identity Card. A digital certificate certifies information about the identity of an entity. That entity can be an individual, an organization or a computer.

In real world, the identity cards or passports are issued by a recognized trusted party like a Government Organization. Similarly, Digital certificates are issued by recognized trusted parties known as Certificate Authority (CA).

There are some Certificate Authorities which are globally recognized and trusted, like VeriSign. A Certificate issued by VeriSign is trusted by most of the Internet Browsers in the world. Therefore, if an organization or an individual can obtain a certificate issued by VeriSign, that certificate would be trusted by almost all the browsers.

Apart from the globally recognized CAs, there are some CAs which are trusted within a certain boundary. The most common example of such CA is the Active Directory based Enterprise CA, which is trusted by all computers which are part of that AD Forest. However, certificate issued by that Enterprise CA will not be trusted globally, as computers outside that forest does not trust that Certificate Authority.

Like the real world, when an entity requests for a Certificate to a CA, the credentials of the entity is verified offline by the CA. Once the verification is successful, the CA issues a Digital certificate to that entity.

## Certificate Signing Request (CSR)

A CSR is an encoded text file that is given to a Certificate Authority when applying for an SSL Certificate.

While generating the CSR, the Private- Public key pair is generated automatically. The Private Key is securely stored in a location within the computer, from where the CSR wizard has been executed. The Public Key is attached with the CSR file. 

Apart from the Public Key, the CSR contains other information including:

Name of the entity to which certificate will be issued

Location, City and Country of the Entity.

When the CA issues the Digital Certificate, it attaches the Public Key with the Certificate along with below information:

-   Name of the entity to which the Certificate has been issued.
-   The CA (Certificate Authority) name, who issued the Digital Certificate.
-   The Certificate Validity Period (From and to dates)
-   Public Key of the Entity, which was shared by the entity while making Certificate Request.
-   Certificate type, purpose , encryption algorithm.
-   Various other information.

Once the requesting entity receives the certificate from the CA, it imports the certificate to its keystore. Keystore is a location where all the digital certificates are stored securely. 

While importing the certificate in the keystore, the handshaking between the Public Key (encoded in the certificate) and corresponding private key (stored within the computer) happens. For this, the certificate needs to be imported in the same computer from where the CSR was generated, because that is the only computer which has the corresponding Private Key. 

If the Certificate needs to be imported to some other computer or device, then the private key needs to be exported to that computer’s or device’s keystore.

## Hash Algorithm

Hash Algorithms are one way algorithms, which are used to verify the integrity of an encrypted message.

It is important to understand the difference between an Encryption Algorithm and a Hash Algorithm. Encryption Algorithms are used to encrypt a message which can be decrypted by using a decryption key, so it is a two way algorithm. Hash Algorithm, on the other hand, is a one way algorithm.

When Hash Algorithm is applied to a particular message, it produces a Digest. Since it is a one way function, it is technically impossible to determine (re generate) the original content from the Digest. Hash Algorithm is used to verify the integrity of a message, whereas Encryption Algorithm is used to encrypt the content of the message.

Popular Hash Algorithms are SHA-1 and SHA-2. Please note that Microsoft (and many other vendors) no longer considers SHA-1 as secured Hash Algorithm, and recommends using SHA-2. Also, the SHA-2 family consists of six hash functions with digests that are 224, 256, 384 or 512 bits: SHA-224, SHA-256, SHA-384, SHA-512, SHA-512/224, SHA-512/256.

It is highly unlikely that two different sources will produce same Hash value. If that ever happens, we call it a collision. Improvements with the SHA-2 algorithm greatly decrease the likelihood of ever producing a collision.

Also, please remember that Hash Value is not secret; anyone _cannot_ obtain the original content (which is encrypted by signature algorithm) by knowing the Hash value.

We will discuss in the upcoming section, how Hash Algorithm is used to ensure the integrity of a Digital certificate.   

* * *

Now that we are familiar with all the basic terminologies, let’s discuss how a digital certificate works. We will assume a real life scenario, where a company will obtain a SSL certificate for its Web Site example.com, to ensure secure transaction with its clients. We also assume that the Certificate Authority (CA) that they are going to use is VeriSign CA.  

## Step 1: Generate the CSR

Example.com will generate a Certificate Signing Request (CSR) for VeriSign from the example.com Web Server. 

The exact steps of generating the CSR will vary from OS to OS, and from application to application. If the Web Server is Windows Server, then CSR can be generated using Certificate Management Console and also from the IIS. For UNIX or LINUX based systems, there are different tools available like Keytool.

While generating the CSR, the Public-Private key pair will be generated. The Private Key will be automatically stored in some secure location within the web server (Keystore), while the public key will be part of the CSR.

In addition to the Public Key, the CSR will contain below important information:

1.  Common Name (The name of the web site, to which the certificate would be issued).
2.  Organization Name
3.  Country, State, City
4.  Signature Algorithm
5.  Certificate Validity (Example: 1 Year, 2 years etc.).

There may be additional information which will be part of the CSR, but the one we mentioned here are most common.

## Step 2: Certificate Validation and Issuance

The CSR will then be shared with the Certificate Authority, VeriSign in this case. Typically, this is done by logging into VeriSign account and uploading the CSR. Upon receiving the CSR, VeriSign will validate the organization. This validation can be offline or online.

Generally, big organizations already have a subscription with an external CA (like VeriSign or DigiCert), as they have to purchase certificates frequently for multiple purpose. For those organizations, the Organization name and Domain name are already validated, to avoid the validation process every time they require a certificate. This makes the Certificate issue process quicker. In such cases, requestor himself can login to the CA console, upload the CSR and generate the certificate immediately, provided Certificate Units are already purchased from the CA. No further validation is required in this case.

If the organization name or domain name are not already validated, then it will take some time for the CA to complete all the required validations, as it may require some off line paperwork between the organization and the CA.

Once all the validations are successful, CA will issue a Digital Certificate to example.com

### Contents of Digital Certificate

The Digital Certificate issued by CA will contain below information :

-   Issued By (CA Name – VeriSign in this case)
-   Issued To (example.com in this case)
-   Certificate Validity (From and To dates)
-   Public Key (obtained from the CSR)
-   Organization Name
-   State, Country, City
-   Signature Algorithm used (Ex: RSA)
-   Digest (Hash) of the Certificate, which is encrypted with Private Key of the issuing CA. This is called the Thumbprint.
-   Hash Algorithm used (Ex: SHA1, SHA 256 etc.).

[![](https://social.technet.microsoft.com/wiki/resized-image.ashx/__size/550x0/__key/communityserver-wikis-components-files/00-00-00-00-05/1854.Certificate.JPG)
](http://social.technet.microsoft.com/wiki/cfs-file.ashx/__key/communityserver-wikis-components-files/00-00-00-00-05/1854.Certificate.JPG)

One important point to note here is, the digest is encrypted with the Private Key of the CA (VeriSign). This means, if it can be decrypted by the Public Key of the VeriSign (which is available by default in any standard browser) then it is ensured that this certificate is issued by none other than VeriSign.

After that, if the digest value is re calculated with the same Hash algorithm on the digital certificate, and the same digest value is obtained which is attached with the CA, then it is ensued that the contents of the Certificate has not been tampered during transit, otherwise Digest value would not match.

In this way, the authenticity and integrity of the certificate will be verified.

## Step 3: Certificate Import

Once the Certificate has been issued, it will be imported to the Web Server, from where we generated the CSR. For Windows Server, The Certificate will be imported using the Certificate Management console, and will be imported to the keystore. 

During Import, the handshaking between the public key (embedded with the Certificate) and the private key (stored within the web server) will be done.

From Windows, post import there will be a statement within the certificate showing “You have a Private Key corresponding to this certificate” which will ensure that key pairs are synced properly. If the statement will say that “You do not have a Private Key corresponding to this certificate”, that will indicate that the private and public keys are not synced properly, and we have to investigate what went wrong.

## Step 4: Binding of Certificate with Web Site

After importing the certificate to the web server, the certificate also need to bind to the web site. This can be done through IIS or Apache console. Post binding, the “HTTP” option needs to be changed to “HTTPS” for that site, to enabled secure communication.

* * *

Now that example.com has incorporated a Digital Certificate from a globally trusted CA (VeriSign), it is ready for a secure communication with its web clients over the Internet.

Step 1: The web client will connect to [https://example.com](https://example.com) through the Internet (or Intranet).

Step 2: Since this is a SSL connection, the Digital Certificate of example.com web site will be sent to the web client.

Step 3: The web client will check the “Issued By” field and will understand that this certificate has been issued by VeriSign Inc. However, the web client first needs to verify that the certificate has been actually issued by VeriSign. For that, the web client will try to decrypt the encrypted digest (thumbprint) with VeriSign’s public key. 

As mentioned before, public keys of well-known CAs are generally present in most of the browsers. This is because well-known CAs are by default “Trusted” by most of the browsers. So probably the web client already trusts VeriSign CA and already having its public key. If not, it has to be imported separately.

If the digest can be decrypted successfully using VeriSign’s public key, that will ensure that it has been encrypted with VeriSign’s private key. Since only VeriSign has access to its private key, this will ensure that this certificate has been issued by none other than VeriSign.

Step 4: The web client will then check the integrity of the Certificate. For that, it will re-calculate the digest value using the same Hash Algorithm (SHA1 or SHA2) which is mentioned in the certificate, and will compare it with the digest value which it has just decrypted (which is attached with the certificate). If both the value matches, then the integrity of the certificate will be ensured.

Step 5: Now that the Certificate authenticity and Integrity is verified, the web client will begin a secure communication with the web server of example.com. For that, it will generate a Session Key (Symmetric Key) and encrypt the session key with the example.com web server’s public key. Now, this session can be decrypted only using the example.com web server’s private key.

The web client will then send that encrypted message to example.com web server.

Step 6: When the Web Server will receive the encrypted message, it will decrypt the message and will obtain the session key. In this way, the session key would be exchanged securely between web client and web server.

Step 7: The rest of the communication (transaction) between the web server and web client will happen using the session key as the encryption key. The same session key will be used by both sides to encrypt and decrypt the messages. This session key will only be valid in the current session. The moment the session will be terminated, the session key will become invalid and cannot be re used. 

In this way, a secure communication will be performed by using a Digital Certificate.

* * *

Generally, critical web sites have more than one web server, behind a Load Balancer. Lets assume that our example.com web site is hosted on five Web Servers, and all these web servers are behind a Load Balancer. The Load Balancer could be Microsoft NLB or any other Load Balancer like F5.

Here are the high level steps which need to be followed to incorporate the same Digital Certificate to each web server of example.com :

1.  Generate CSR from any one of the five Web Server, say Web Server 1.
2.  Get a Digital Certificate from an external CA based on that CSR.
3.  Import that Certificate to Web Server 1 and bind with the web site example.com.
4.  From web server 1, export the certificate along with its Private Key. For Windows OS, if we export a certificate along with Private Key , the exported file format would be PFX (Personal Information Exchange) . Please note that this file will contain both Private and Public keys, so this file needs to be handled very carefully.
5.  Copy the exported certificate file to other four web servers and import the certificate to each server. 
6.  In some cases, the Load Balancer also decrypts and encrypts digital certificate, in order to offload web servers of the processing burden of encryption and decryption. This feature is known as SSL Offloading. In such cases, the same certificate also needs to be imported to the Load Balancer and to be binded with the example.com web site. We may need to contact Network team or Security Team who supports the Load Balancer, and work with them to import the Certificate to Load Balancer .
7.  Once all the above mentioned steps are done, please do not forget to delete the certificate export file from all the servers, which will be a PFX file in case of Windows. Please note that PFX file contains the Certificate along with the key pairs (Private and Public keys), so if the file goes to a wrong hand , it can be misused and then the communication using that Digital Certificate will no longer be secure.

* * *

In the above example, we have discussed how a web site can establish a secure communication with web clients using the Internet. 

Since the encrypted communication was happening over the Internet, the web site opted for a Digital certificate which was issued by an External CA, VeriSign in this case. This is because most of the popular External CAs are already "Trusted" by web browsers like Internet Explorer, Firefox or Google Chrome.

When we say the CA VeriSign is "Trusted" by a web browser or an operating system, we mean that the root certificate , or the subordinate certificate of VeriSign Inc is already present in the web client keystore, along with VeriSign public key. The OS manufacturers (like Microsoft) include these well known CA certificates and their public keys during the OS shipping, so that users do not need to import those certificates manually. As a result, most of the web browsers in the world trust all certificates issued by these well known External CAs, so we can call these CAs as globally trusted.

Internal CAs, on the other hand, are not trusted globally. This is because internal CAs are generally custom built CAs, which is unique for every organisation. Typically, certificates issued by these CAs are trusted within a certain organisational boundary. For example, an Active Directory based CA is by default trusted by all computers, which are part of that AD Forest, where the CA is deployed.

* * *

We can now start our discussion on AD CS. Microsoft offers to build a complete Enterprise PKI (Public Key Infrastructure) solution through Active Directory, which is extremely popular among large and medium sized organisations.

There are some advantages and disadvantages of using an Internal PKI solution, we will discuss both.

## Advantages of Internal PKI

-   In an Internal PKI environment, there is no cost associated per certificate. An organization typically uses thousands of certificates for its internal web servers, application servers, mail servers, users and so on. Purchasing units for each of these certificates from external CA and periodically renewing those certificates would be a costly affair. By implementing an enterprise PKI solution, organisation reduces this huge recurring cost.

-   Organizations can have more granular control over the PKI implementation. For example, they can build customize certificate templates for each purpose and also control which groups of users/ computers can use which template. Such kind of control is not possible for external PKI.

-   AD CS supports a feature called Autoenrollment. Using this feature, a certificate can be automatically approved by the CA as soon as request is generated. This does not require manual intervention for every certificate issuance. Similarly, the Auto Renewal feature will automatically renew a certificate before it expires. All these features can be incorporated to certificate template, and access can be controlled at granular level. 

-   Since AD CS is integrated with Active Directory, the overall management of the PKI environment would be simpler with more control. For example. we can use Group Policies to control many operations of AD CS. This includes adding a CA in the trusted certificate store, configuration of autoenrollment, controlling certificate templates and so on. 

Overall, Internal PKI is a perfect solution to use within organization or within a certain boundary, where the Certificate need not to be globally recognized.

## Disadvantages of Internal PKI

-   The CA servers within Internal PKI are not globally trusted. For this reason, any certificate issued by an Internal CA would not be trusted globally.

-   When an organization deploys Enterprise PKI, the responsibility of maintaining that PKI falls under the organization. For that, it requires few skillful CA admins and proper training. 

-   The initial deployment and training cost is higher for Enterprise PKI, However, in order to achieve a long term goal, this investment is worth.

-   The organization needs to ensure that the CA servers are secured, and their keys are not compromised. They should also have a recovery plan, in case the key of the CA servers would be compromised. For external PKI, all these overheads would be taken care by third parties.

* * *

Its time to conclude this discussion.

As we mentioned in the beginning, this is the first article of the AD CS series. In this article, we have taken a deep dive on the basic concepts of Cryptography and Digital Certificate. We have discussed how a secure communication can be performed using Digital Certificate. Finally, we have introduced Enterprise PKI and compared it with External PKI.

* * *

1)[ Active Directory Certificate Services: Enterprise CA Architecture](https://social.technet.microsoft.com/wiki/contents/articles/53249.active-directory-certificate-services-enterprise-ca-architecture.aspx)

2) [Active Directory Certificate Services - AIA , CRL and OCSP](https://social.technet.microsoft.com/wiki/contents/articles/53271.active-directory-certificate-services-aia-crl-and-ocsp.aspx)

* * *

 [https://social.technet.microsoft.com/wiki/contents/articles/51429.active-directory-certificate-services-digital-certificate-overview.aspx](https://social.technet.microsoft.com/wiki/contents/articles/51429.active-directory-certificate-services-digital-certificate-overview.aspx)

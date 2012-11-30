# <a name="SecurityModel">Security Model</a>
Security concerns about the public cloud are one of the main obstacles for enterprises when building cloud applications. It is also one of the biggest red herrings, as there are few fundamental differences between cloud security and traditional security. After all, traditionally deployed applications still have vulnerable, public endpoints.

While never an excuse for lax security, whether that is leaving a flash drive on a train or having a public website vulnerable to SQL injection attacks, public cloud projects are subject to extra scrutiny, and security breaches are less acceptable. Most cloud projects are new, paving the way for further applications that can be deployed to the cloud, and the first application, as a flagship test case, should not have security problems. If the first application is subject to an attack, it will be jumped on by the sceptics and derail any future endeavours.

The opinion that 'the public cloud is insecure' is generally unfounded, and even the position that 'the public cloud is more insecure than on-premise' is also probably false in most cases. The security of individual cloud platforms is well implemented, understood, and documented. The same cannot be said for most private datacentres (not all, of course, some private data centres are really good at security). It's more likely that a hard drive that holds important data would be ripped out of a server in a private datacentre than from the Windows Azure datacentre. Apart from physical access, where would you even start looking for a specific hard drive in a Microsoft datacentre? Windows Azure applications are less vulnerable to flaws for which Microsoft has issued a patch, as the underlying operating system is always patched for you by Microsoft. The same cannot be said for the millions of un-patched servers in private hands.

We can go back and forth about which is more secure. Arguments about wanting 'to keep my data in my datacentre' get complicated and are fruitless. The one big difference with public cloud applications is that the attack surface is bigger. There is little in the way of perimeter security, and poor security practices cannot be ignored based on the assumption that a firewall is going to restrict public access to a service that is full of holes. Everything on the public cloud is accessible, even Windows Azure SQL Database. The moment you allow Windows Azure connections to Windows Azure SQL Database anybody can fire up a virtual machine and try to connect to it. All (most) ports are open across all regions, for any amount of data. On the public cloud you need to make sure that good security practices that apply to all applications are applied consistently and rigorously.

The development of the security model within the context of designing for the cloud is about:

* Understanding the differences between cloud-based security and on-premise security.
* Asserting the need for good security practices.
* Developing a plan for building a secure application.
* Inviting early security assessments and audits.
* Understanding the specifics on how Windows Azure supports the development of secure applications.

## Developing the Security Model

### Understand cloud security differences
A web role running in Windows Azure is pretty much like any other ASP.NET application running on any other hosted or on-premise platform, and subject to the same security concerns and solutions. There are still some differences to be considered when developing the security model.

#### Attack surface
As mentioned above, the attack surface of an application hosted on Windows Azure is higher than a traditional application. In multi-tier applications, it is common to put the web server outside the firewall and the application and database server inside the firewall, thus restricting access to traffic from the web servers. Windows Azure applications are not architected in the same way (the multi-tier pattern is seldom used) and all services are generally publicly accessible. If the consumer portion of the application needs to communicate with an 'internal' WCF service, even if only intended for use by the application, that service is publicly accessible and needs to be secured. 

#### Enterprise infrastructure
Experienced enterprise developers take a lot of things for granted; firewalls, active directory, secure storage, certificates, DNS, and many more. Some of these are specifically unavailable on Windows Azure (such as firewalls), and some are provided as part of enterprise IT, where developers don't have to think about them too much. In enterprise IT, developers fill in a requisition for a certificate to be installed by IT. As well as architecting and building for the lack of physical infrastructure, Windows Azure implementation teams also find that they need to do a lot of things by themselves. 

#### Governance
Enterprise IT generally has well-established governance processes to ensure that applications deployed within their data centre meet certain requirements. These processes can range from extensive security audits, to making sure that the SQL login passwords are strong enough. Public cloud implementation teams tend to work outside the existing IT processes (at least initially), and may not be aware of the importance of governance, however irritating and draconian they may seem. Windows Azure implementation teams may need to develop their own governance processes, or at least find a way to leverage existing enterprise governance. 

#### Identity
Identity is becoming more complex in modern applications, as attacks become more sophisticated and frequent. It is no longer sufficient to have simple username/password pairs or AD trust. Users want to be able to authenticate using other credentials such as Facebook. REST based services are becoming widely accepted, particularly on mobile platforms, so the issue and acceptance of security tokens becomes necessary. All of this needs to developed within a set of standards (such as SAML) where authentication and authorisation between different security domains and different providers, need to be implemented.

This is not specific to applications deployed in Windows Azure. The types of applications currently being developed need to handle identity properly, and an application being built for Windows Azure may be the first time that an implementation team has run into these issues.

#### Windows Azure SQL Database
On premise SQL Server has, over the last few years, added extensive security features, such as data encryption. Windows Azure SQL Database does not support the same features (although it can encrypt connection data), and needs to be dealt with differently from on-premise SQL Server. For example, some data may need to be encrypted by the application before being stored in the database.

#### Storage Keys
Windows Azure Table, Queue, and Blob storage are publicly accessible REST services that use a key for authorisation. There are only two master keys, and keys cannot be generated for general use. This means that access to Azure storage is limited because keys cannot be given to the client application/browser (Blobs have shared access signatures which differs slightly). It also means that if these keys are accessible, or sent down to the browser, all storage can be accessed. Azure storage keys need to be aggressively protected.

#### Compliance and Regulations
Although Windows Azure supports some compliance standards (ISO/IEC 27001:2005 certification, SSAE 16/ISAE 3402 Attestation and HIPAA BAA), it does not support PCI-DSS or any number of varied and international compliance standards. As the acceptance of the cloud develops, so too do regulations and laws. Microsoft does not explicitly conform to all regulations, even though it may be possible to build a compliant application on top of Windows Azure. In some cases, like PCI-DSS, it is impossible to comply without some part of the application running on-premise. It is up to the implementation team to understand the regulations, and ensure that their application can support those regulations.

### Adopt a security assurance process
Due to the importance of security on Windows Azure, and the need to ensure that the number and severity of vulnerabilities in the application is as low as possible, a security assurance process must be adopted for the project. These processes are rigorous and take effort to implement, but in the long-run reduce development and maintenance cost by the early implementation good quality secure code.

The [Microsoft Security Development Lifecycle](http://www.microsoft.com/security/sdl/default.aspx) is a process used by Microsoft to ensure security within applications. The process and supporting tools are publicly accessible and available for use. Other methodologies exist, and it is up to the implementation team to use the most appropriate one (perhaps there is an existing internal process).

Project Managers need to ensure that sufficient budget and capacity is available for implementing a security process, and should embed security documentation, quality gates and other aspects into their software development project plan.

### Plan for specific security implementations
A security assurance process encourages the practice of implementing secure applications but it doesn't actually describe the platform and application-specific details required. Security needs to be implemented throughout development and cannot be added just before delivery. It makes sense to get started early by addressing practical, security implementation during the design stage. 

The security model should indicate at a high level how the application is going to be secured. This ensures developers can be made aware of development tasks related to security, so that they can be included in estimates and planning. Typical areas include:

* Identity - Using ACS and federated identity.
* Authorisation - Claims based identity.
* Securing ASP.NET - [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page), [OWASP Top 10 for .NET developers](http://asafaweb.com/OWASP%20Top%2010%20for%20.NET%20developers.pdf).
* Database security.  
* Securing Services - e.g. [Securing REST Services](http://msdn.microsoft.com/en-us/library/hh446531.aspx).

### Engage with audit and governance
One of the advantages of using a cloud platform is the ability to bring an application to market quickly without having to wait for the procurement of infrastructure. With all that speed of delivery, it would be frustrating if an internal audit group stalled the project for a few months while they conduct a thorough investigation.

Depending on the level of executive support for the cloud project within the organisation, the stage where internal governance and audit processes get involved can be tricky to determine; too early and they have a panic attack and derail the project. Too late and it takes so long for them to understand that they panic anyway and stall delivery. One thing is for certain, governance and audit processes fulfil an important role and should not be ignored. They need to be engaged on the project as early as possible so that you can sail through any governance gates further down the road.

### Understand Windows Azure specifics
Due to security being a major concern with cloud platforms, there is a wealth of information on security within Windows Azure; provided, sponsored, and supported by Microsoft (probably more than any other part of Windows Azure). With so much available, there is little point in covering the detail in this book. As part of the design process and establishment of the security model, it is imperative that at least one member of the team is assigned the subject matter expert on Windows Azure and reads all necessary documentation.

Below are *some* resources for Windows Azure Security specifics:

* [Windows Azure Trust Centre](https://www.windowsazure.com/en-us/support/trust-center/) - The Windows Azure Trust Centre contains the latest details about security, compliance and privacy on Windows Azure. It includes details of all certifications and links to the latest downloads on security documents specific to Windows Azure.
* [Microsoft Global Foundation Services - Securing Microsoft's Cloud Infrastructure](http://cdn.globalfoundationservices.com/documents/SecuringtheMSCloudMay09.pdf) - A paper introducing how Microsoft manages security within its data centres.
* [Windows Azure Security Overview](http://go.microsoft.com/?linkid=9740388).
* [Windows Azure Security Notes](http://go.microsoft.com/?linkid=9741707) - at 121 pages it is more than 'notes'.
* [Security Best Practices For Developing Windows Azure Applications](http://download.microsoft.com/download/7/3/E/73E4EE93-559F-4D0F-A6FC-7FEC5F1542D1/SecurityBestPracticesWindowsAzureApps.docx).
* See MSDN and the Windows Azure SDK for information on:
	* Access Control Service.
  	* Windows Azure Active Directory.
 	* Windows Azure Connect.
 	* Windows Identity Foundation and Security Token Services.
 	* Installing and using certificates on Windows Azure.
 	* Securing Windows Azure storage.
 	* Windows Azure SQL Database logins, certificates and firewall.
	* Windows Azure Virtual Network.

## Summary
The security of cloud applications is of critical importance and is echoed in the vast amount of material available, references, and specialisation on cloud security in general, as well as Windows Azure specific information. Due to the availability of security information and processes, and the rate at which security has to change, CALM is not prescriptive about the security model. CALM recommends that a security process and methodology be adopted that is specific to the application, and is complemented by Microsoft's security endeavours on Windows Azure.

### Steps

1. Understand cloud security differences.
2. Adopt a security assurance process.
3. Plan for specific security implementations.
4. Engage with audit and governance.
5. Understand Windows Azure specifics.


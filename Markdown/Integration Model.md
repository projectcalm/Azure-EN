# <a name="IntegrationModel">Integration Model</a>

Applications do not exist in a vacuum, and almost every application needs to communicate at some point with another. This communication between completely separate applications is more complicated than starting a conversation and requires integration; protocols need to be established, languages, agreed formats, schedules and even what to do when the other party doesn't respond. Integration is absolutely critical for any application but, as people who have worked in IT environments will know, can be very difficult, time consuming and expensive.

In developing a model for the integration of applications, it is important to frame what we are referencing. Integration is a huge business, with specialist tools, vendors, patterns, frameworks and just about every technology that has ever been used in IT. To produce an integration model that encompasses every type, variation, load, and complexity of integration is virtually impossible. After all, integration is about the complex interdependencies between systems with all of their nuances, edge cases, defects, failures, and oddities exposed. This is why integration is a specialist area with wildly varying approaches depending on the market, need, technologies and budgets. Within CALM, we have no intention of solving every integration problem you may encounter, and if your integration environment is complex then we recommend that you build your approach based on a model capable of handling your particular needs.

Anecdotal evidence suggests that while cloud computing applications have some integration, it is seen as a secondary feature. This is probably because:

* Integration-heavy, core enterprise systems are not being moved to the cloud due to risk aversion.
* ERP and other large vendors are still shaping their cloud services. These applications are heavy consumers of integration and their absence on the cloud is reflected in an absence of extensive integration in the cloud.
* Enterprise middleware (MQ Series, Oracle Fusion, TIBCO, etc.) is taking time to be established on public cloud platforms, probably as a result of customers not asking for it yet.
* Integration tools, such as SQL Server Integration Services, that run millions of daily integration jobs worldwide are not yet built for cloud environments.
* Traditional tried-and-tested methods for integrating, such as using text files on FTP, are not seen as cloud friendly, due to their vulnerabilities and difficulties running in a load-balanced environment.

As a result, the integration that a cloud application will contain is likely to be fairly simple, is not a core feature and does not take much effort or consume much of the overall project budget. This basic integration requirement is a result of the type of applications that run in the cloud, combined with the immaturity, or at least lack of coverage, of established integration technologies.

The aversion to integration-biased projects in the cloud might suggest that the tools and technologies available are fairly basic. This may be valid when assessing tools across the board, but Windows Azure does have specific integration solutions that are advanced and solve particular integration problems very well (such as Windows Azure Active Directory and Windows Azure Service Bus). Cloud platforms, specifically Windows Azure, may not have a breadth of integration technologies, but do have depth where it is required. For example, while most applications won't need the support of a mainframe based TP monitor, they may need to integrate authentication using [Facebook as an identity provider](http://msdn.microsoft.com/en-us/library/windowsazure/gg185919.aspx), which is surprisingly simple to do on Windows Azure.

When researching integration options for your application be aware that cloud applications generally have a low integration requirement and cloud platforms, that are PaaS biased including Windows Azure, are not architecturally aligned to traditional integration. This influences the extent of the integration in the project and the choice of technologies, and this in turn, is reflected in the integration model.

## Challenges of integration in the cloud
Before examining how integration is tackled in the cloud, it is useful to highlight why integration is difficult in the cloud. This should help clarify why there is tool immaturity, few implementations, and provide clues as to possible problems that may be encountered when integrating applications with an application being developed for the cloud.

### Integration is already difficult and risky
Integration is always harder than expected, despite vendor promises or project delivery pressures. Cloud projects are under scrutiny as they chart new waters, and there is little point in making them harder than necessary by adding complex integration work. While the cloud specific part of the project may be running well, the integration part can drag the project down and if it fails it will go on record as a failed cloud project, not a failed integration project. This alone should initially qualify out integration-heavy applications and create a preference for lower hanging fruit that is better suited to the cloud.

### Lack of control
The compute/role PaaS model specifically limits what can be run on Windows Azure, and these limits can be a problem for integration approaches. Even in the IaaS model with the existence of persistent VMs on cloud platforms, some control is surrendered; be it networking, disk I/O, OS versions and even physical controls. This lack of control may be problematic, but it may not be immediately obvious. For example, a service that needs integration may want communication over a specific port or protocol (say UDP) that is just not possible with the available networking infrastructure. Another example is email, which is a shockingly common protocol for integration. Sending and receiving of emails is not straightforward on Windows Azure or any other cloud platform, due to the risk of clouds being blacklisted as sources of spam.

### Custom configuration
Integration often requires custom configuration of the server that hosts the technologies or acts as an integration endpoint. Whilst persistent VMs make it easier to have a hand-configured virtual server, this goes against cloud architectural principles, and the configuration should at least be scripted so that it can be recovered in a different location if a failure occurs. This scripting may simply not be possible with the particular integration tool in use, which may require an operator to step through installation while setting up the server.

### Authentication
There are many different authentication mechanisms used in integration, some powerful and sophisticated, and others less so. While Windows Azure supports complex federated identity mechanisms, processes running in Windows Azure won't have things that may normally be taken for granted, such as being on a domain, fixed IP addresses, hardware dongles (yes, they are still used), and other parts of a physical infrastructure that may not normally be a problem.

### Licensing
Many integration tools and services have licensing conditions that don't translate well into a cloud computing environment. Even if they can contractually accommodate the deployment of the client service across multiple machines, they can fall flat during implementation because they require that a key be generated for each specific machine. Generating keys for specific virtual machines, which may not exist if they are rebooted, is not easy to do or even desired in cloud architectures.

Data providers (systems that need to be integrated with) and tool vendors (of the tools and libraries necessary for integration) aggressively protect their domain. They are likely to have a combination of licensing terms or technologies that mean that they either cannot be used in a cloud environment, or are very expensive to license.

### Long-running batches
One of the fundamental architectural principles of cloud applications is the concept of 'design for failure' on the back of a commodity infrastructure. This is where the availability of the service, not an individual machine, is of concern. This means that applications are built to expect failure at any point in time and generally have very simple stateless processes that, if unexpectedly terminated, have little impact because they start again somewhere else (either explicitly developed for, or because the user has clicked again). This is in contrast to integration applications that frequently have long running processes with transactions containing multiple operations that cannot handle the fallout, if they terminate unexpectedly. Consider, for example, running SQL Server Integration Services on a VM in Windows Azure. The Windows Azure SLA and operating environment means that, at any time, the Fabric Controller can bounce your VM for any reason it sees as valid. So, if you have an SSIS task that runs for four hours you will need to build in recoverability for cases where the underlying VM is shut down. At the time of writing the SLAs for Windows Azure Virtual Machines have not been announced, but it is likely to be lower than a high-end, on-premise database cluster.

Many integration approaches require that a single process runs either as a listener, polling for changes, or taking a long time to run jobs. These are not particularly suitable in a cloud environment because single-instance processes are difficult to build in dynamic environments. 

### Scheduling
Scheduling of (cron) jobs is surprisingly difficult on Windows Azure. This is not necessarily a platform limitation, but the result of the fault-tolerant environment; where any number of processes need to be able to kick-off a task at a particular time, and then need to discuss it amongst themselves to make sure that only one of them is executing it. This is in contrast to environments where stable machines wait patiently for a particular time of day to kick-off a job. Integration often requires scheduling, be that specific times of day (often after hours), or according to a predetermined frequency. Sophisticated job scheduling tools are an important aspect of integration that simply does not exist on cloud computing platforms.

### Orchestration
Orchestration creates a layer of abstraction over services that need to be integrated. It separates the process logic from back-end services and provides an environment to redefine business processes on the fly, without needing to alter any of the services. In many ways, it shares some of the cloud computing application architectural principles of loose coupling, asynchronous processing, and fault tolerance. It would seem, at least in theory, that orchestration should be relatively easy on the cloud. Unfortunately, the technologies and products to do orchestration are complex, expensive, resource intensive, and difficult to use. Orchestration never comes packaged as something simple and is included as part of ESB (Enterprise Service Bus) technologies, such as IBM's WebSphere and Oracle's ESB (as part of Oracle SOA Suite). Products of this scope and pedigree require infrastructure that, in most cases, will not be satisfied by the commodity infrastructure offered by public cloud providers. Even Microsoft's own product for orchestration, BizTalk Server, has struggled to be ported to Windows Azure and only went to CTP (preview) on a VM in August 2012, illustrating how complex it is to get orchestration technologies running in the cloud.

Implementing fully-fledged orchestration can be done using tools from major vendors and run on Windows Azure VMs, but the cost, effort, and energy required is not conducive to a simple integration task. An application that requires such extensive integration should be qualified out as an unsuitable candidate for Windows Azure.

### Outdated integration approaches
As much as sophisticated EAI (Enterprise Application Integration) tools and platforms exist, a lot of integration is implemented using outdated approaches. Integrating complex applications is frequently undertaken with CSV files that are shared using an FTP server, or emailed back and forth. These approaches result in integration that is unmanaged, brittle, error-prone, and accomplished in so many different ways across the organisation that it is difficult to keep track of what is going on, or whether or not it is working.

The problem with these outdated approaches is that they are difficult to get working within a cloud environment. They require too much manual configuration, operator input, and their reliability can be a problem for applications with a high availability requirement. As there is little standardisation across the enterprise, it is neither easy nor worthwhile to try and convert them to modern approaches.

Since many of these approaches were developed before security became a major concern, they are fairly unsophisticated in this respect. In the case of protocols such as FTP, simple username/password keys are sent in plain text, making them vulnerable to hacking. Many other outdated integration approaches create vulnerable, insecure endpoints. As a result, network security is used to enhance integration security by limiting access to machines with fixed IP addresses.

### Data gravity
Large enterprises with large databases tend to keep integration close to their centralised data. Applications in need of data have a tendency to stay in the same datacentres as the source data. They share the same infrastructure (including database, networking, and backup), and are safe, from a networking point of view, behind the same perimeter security. Integration that requires large volumes of data to move from the primary datacentre and out of the control of the organisation will be questioned, and may even be impractical from a bandwidth, encryption, or other points of view.

Cloud based applications, where a lot of data is moved from centralised control, have to escape the data gravity well. This may include technical solutions, as well as the need for significant executive-level support.

### Networking
Although cloud based applications can be configured to be more private and static, by their very nature they are dynamic and publicly addressable. A cloud service should be referenced by a DNS name, not a static IP address. Many enterprise integration approaches demand the opposite, and expect services to have static IP addresses in order to create a firewall rule that allows a single machine to have specific access. Although cloud applications can be configured to use static IP addresses and VPNs, it is too close to having the architecture dictated by infrastructure. While this kind of configuration may be possible with IaaS, the further an application moves from the infrastructure, to PaaS and SaaS, the more difficult it becomes to configure the application to work with enterprise security practices.

## Opportunities of cloud-based integration
The infrastructure available in the cloud allows for interesting possibilities for the provision of datasets that need to be integrated with, and the availability of compute resources to process data at a rate that would be impossible or impractical in on-premise datacentres. As larger workloads and applications are moved to the cloud, the need for integration increases. People who develop cloud applications are aware that their applications do not run in an isolated environment, and extensively integrate with other services, such as third party identity providers. Applications architected for the cloud have accessible, well-described endpoints, can scale to handle load, communicate securely, use message based asynchronous protocols, and are generally open to necessary integration.

Unfortunately, approaches to integration are different to existing on-premise integration approaches and in many cases, such as the use of network perimeter security, are in conflict. The solution sits somewhere in the middle, where cloud based architectures adjust to conform to on-premise practices (such as the use of VPNs), and on-premise integration approaches adopt more of the cloud-oriented approaches (such as SAML authentication tokens).

### Enterprise focus
The enterprise is a big market that public cloud vendors are trying to penetrate, whether that is Amazon on the back of its public cloud brand, or Microsoft leveraging existing enterprise relationships. Due to the amount of integration in enterprise applications, cloud vendors need to add more extensive integration-specific features and services. This can be seen with Microsoft's addition of IaaS features that allow the use of enterprise integration approaches, and include:

* Persistent IP addresses (through infinite DHCP leases) — Allow firewalls to allow access from cloud based services.
* Persistent VMs — Allow the installation of specialised integration technologies that have not been engineered for the cloud.
* Virtual Networking — Allow cloud based virtual machines to connect to the enterprise network via a VPN.

Over time, more and more features will be added to Windows Azure that facilitate integration with enterprise applications.

### Cloud specific integration technologies
As market demands better technologies on the cloud to integrate applications, more will become available. Technologies that are cloud specific and deal primarily with integration are still emerging, and are in the early phases of the adoption cycle. Cloud specific integration will be split into the following areas:

* PaaS message-oriented platforms — Most cloud providers offer some sort of message-oriented infrastructure that, while useful within the application, is also useful for integration. Some features, such as the Service Bus Relay in Windows Azure Service Bus, are important for enterprise integration because of their ability to work around network security issues.
* SaaS integration platforms — Integration should be offered as a service, where the familiar approaches to integration using integration-specific tools are adopted for the cloud. Examples include [Bableway](http://www.babelway.com/) and [Boomi](http://www.boomi.com/), and many more are sure to emerge in the future. Microsoft does not offer any such integration platform.
* On-premise tools — It is possible to run an integration tool on-premise to the cloud (such as using SQL Server Integration Services), but problems still exist because of the assumptions that these products make about infrastructure. Vendors will need to adapt these on-premise tools to become more cloud-aware, even though they remain firmly rooted on-premise. An example of this approach is IBM's [WebSphere Cast Iron](http://www-01.ibm.com/software/integration/cast-iron-cloud-integration/). 

### Increasing use of standards
Applications that make use of standards improve interoperability and the ease at which they can integrate. These standards are not specific to cloud applications, and are shared by modern applications built on the web to web standards. Examples include:

* Web services — Applications that expose web endpoints and allow communication over http (and https). This has been enhanced by a wider adoption of SSL for encryption and REST patterns.
* Data interchange — Modern applications are getting better at sharing data in agreeable and understandable formats. The use of browser-based JavaScript has increased the use of JSON for data exchange. ATOM and RSS are still good standards for exchanging data, but are falling out of practice. [OData](http://www.odata.org/) is Microsoft's open protocol for querying and updating data but hasn't gained sufficient traction amongst non-Microsoft developers.
* Identity — Web applications have a great need for identity services to authenticate and authorise the use of resources. Standards include [OpenID](http://openid.net/), SAML (Security Assertion Markup Language), and OAuth.

### Increasing use of services
Cloud architectural patterns promote the use of discrete, loosely coupled services with publicly addressable endpoints (generally over http). Decomposing application functionality as services, and adding authentication and authorisation to access those services, creates an environment that can make integration easier. For example, if a service exposes some JSON data to the web browser as part of normal functionality, it is not too difficult to expose further data via the same endpoint and protocol. After all, to a web server, the authenticated browser consumer is indistinguishable from another authenticated application, accessing the data over http.

This allows many integration problems to be solved using existing approaches, where an application simply consumes data from another service, without having to worry about formats, location of files, schedules, and networking problems. Likewise, an application that is a source of data for integration simply needs to expose the data, and have little concern for how it is consumed. This does not solve all integration problems and more sophisticated protocols may be needed that require, for example, confirmation that data has been correctly received.

### Virtual Machines and Virtual Networking
The IaaS cloud features of Virtual Machines and Virtual Networking ease integration on the cloud by allowing cloud applications to behave more like their on-premise counterparts. The primary uses of Virtual Machines and Virtual Networking are:

* Installation of specialised tools — In some cases, there may be a specific need to integrate using a tool that is not able to run on a PaaS platform. These tools might offer a specific set of features that may be required, or there could already be a significant investment in integration scripts or models and in-house skills. In these cases, installing the tools on a virtual machine may be the only way to integrate the application.
* Integration with on-premise networks — Networking specialists on existing datacentres are nervous about exposing endpoints to services outside the datacentre network perimeter. In the context of some of the methods of integration, such as the use of FTP, these concerns are valid. Virtual networking provides a relatively simple method to get around security concerns, by making the cloud-based application part of the on-premise network through the use of VPNs. Most hybrid cloud solutions have some sort of virtual networking as their architectural base.

## Developing the integration model
Against the backdrop of the complexity of integrating applications, developing the integration model is difficult particularly when at least one of the integration points is within enterprise datacentre. Readers with extensive experience and existing organisational processes for integration are encouraged to make use of them where possible. As expressed in the beginning of the chapter, this book does not attempt to provide a complete solution to the highly specialised integration business. Readers may look at other integration methodologies and guidance, and use that as a basis for their own version of the integration model, using this as a checklist to ensure that all areas are covered.

Regardless of the coverage of integration in CALM, the need for sufficient and clear documentation of the tools, technologies and approaches to integration, are required for the application. The use of the CALM integration model should drive sufficient questions about integration in the application, and highlight solutions to satisfy those needs. 

### Understand the applicable scenarios
The first step to understanding integration within the application is to understand how complex the integration is going to be, and potentially how much effort is required. Integration can be simple or highly complex, and it is the complex integration scenarios that should be carefully considered or avoided altogether.

Consider each type of integration against the following scenarios:

* Client or server — Simple integration where a client requires some data. This can be a JSON document that has some data that needs to be used on the client.
* Services Integration — A service endpoint provides an interface for integration using a well-described contract, and communicates over a standard protocol (such as http). The service may allow for pagination, updates and other more complex data operations.
* Application Integration — Integration between entire application stacks, rather than simple client/server or services. This may involve large datasets that are integrated using ETL techniques (Extract, Transform, Load). It can often involve scheduled integration, where jobs are started at particular times of day.
* Workload Integration — Where small data is transferred, using messages, from one service or application to another, in order to continue the logical workflow of data. Workload integration can be performed intra-application (as is a common cloud pattern) or between different applications on different networks (such as order placement integrating with order fulfilment).
* Orchestration Integration — Where business rules are implemented on the fly and data is routed to the correct destination based on the executing rule, specific data within the message, and reference data that may only be relevant at the time. For example, routing of financial transactions based on the time of day, order size, and the spot price of a financial instrument.

The scenarios can be fed back to project management to assess the risk of the integration work to the project, and the effort involved. If the application absolutely requires orchestration, or has a significant number of application integration scenarios, then it may be better to qualify out the application as an unsuitable candidate for the cloud.

### Determine the scope of integration
Since integration is not a strength of cloud-based applications, it's logical that an application that is primarily about integration is going to be difficult to execute and won't show off the best features of the cloud. On the other hand, all applications require integration. The amount of integration, counted in both the number of integration points and the integration scenarios, needs to be carefully considered and weighed up in terms of viability.

![Integration scope](./images/Integration-002.png)

As illustrated in the diagram above, using the cloud as part of a broader enterprise application, where most of the application is hosted on-premise, is a viable solution. Certain features can run on the cloud, and if those features are consumer facing, possibly free, and subject to variable demands, the cloud can be the best option. In such cases, the integration between the on-premise application and the cloud is one of the most important features. Without integration, the on-premise application is not publicly exposed, and the cloud application will have so little data that it will be worthless. Yet the integration points need not be complex. The cloud application has a subset of requirements many of which require less data consistency, can cope with slight data inaccuracies, may not send all data back to the on-premise application, and similar aspects that mean that the integration can be simplified.

### Detail each integration point
Across all projects, not only cloud projects, integration detail is left very late. This may not be much of a problem for traditional applications hosted on-premise, where skills exist, and tried-and-tested approaches can simply be rolled out. On a cloud project, failing to consider integration early can derail the project. For example, an assumption can be made about how integration is going to work, only to find when everything is tested on production, that on-premise networking requires a fixed IP address on a purely PaaS based solution. This would require some rework of the architecture, and possibly some cobbling together of solutions such as Virtual Machines and Virtual Networks.

The integration model requires that integration be documented in sufficient detail to make necessary adjustments to the architecture, so that specific integration issues can be accommodated.

#### Cover all the integration points
Make sure that **all** of the integration points are covered in sufficient detail. Don't just cherry-pick the easy and obvious ones. It may be that the apparently simple requirement of 'FTP us a CSV file', is the one that is the most difficult to deal with. Often it is the smaller integration points that create problems because the project cannot influence any change in the 'corporate standard' of how applications integrate with that specific application.

#### Determine the integration class
The integration class encapsulates the technologies used, approaches, people involved and the complexity of the application. Capturing the class of each integration point is a good way to get a high level view of what is involved. The integration classes are detailed below:

![Integration class](./images/Integration-003.png)

##### Identity
Used for making use of an identity service that is not part of the application itself. The identity service will provide authentication and authorisation at a minimum. Some may provide additional identity attributes, tokens, delegation and other features.

Technologies used in identity integration include:

* Windows Azure Active Directory. 
* Integration with Azure hosted identities. 
* Federation with other identity providers, such as Windows Live ID and Facebook. 
* Federated identity with on-premise Active Directory.

##### Data
Data integration is the most common integration in enterprises, where large datasets are exchanged, or databases are synchronised. Data integration is complex because the differences between the source and target schemas, representing differing business semantics, need to be resolved. At a basic level, field mapping needs to be completed but other complexities, as a result of (mostly) undocumented business rules, can take a long time to resolve. Data integration is covered in further detail within the data model under [Shared data](#SharedData).

Technologies used in application integration include:

* ETL using staging databases or flat files.
* Replication.
* Master Data Management.
* Data synchronisation (SQL Data Sync).

##### Application
Application integration requires that contracts be established at the interfaces between applications. Application integration can take many forms, including a type of data integration, but is mostly associated with single service calls that pass a relatively small piece of processed data. Where possible, and especially within cloud architectures, applications should integrate asynchronously using persisted messaging technologies. Application integration can become difficult because both parties in the integration may have to write custom code to perform the necessary actions on the data, and there may be a lot of variations of data and many methods needing to be developed.

* RPC style service calls.
* RESTful service calls over http(s).
* POX (Plain Old XML) over http(s).
* SOAP.
* WCF (Windows Communication Foundation).
* Messaging (Windows Azure Queues, Windows Azure Service Bus).

##### Infrastructure
Most infrastructure integration with cloud applications comes down to the configuration of VPNs so that the cloud application is part of the on-premise network topology. Other infrastructure issues, such as punching holes in firewalls, become part of the specific integration being dealt with.

* VPN (Virtual Private Network).
* IPSec tunnels. 
* Shared storage.
* Cron job scheduling.
* Programmable load balancers (ADC - Application Delivery Controller).
* Operations integration (System Center).

#### Understand the other party
Integration, by definition, involves disparate systems. These systems have different technologies and different approaches. Most importantly, they have different teams or organisations that the project team needs to work with. Many problems exist when working across technical teams. Perhaps the other party has integration options that are not in the available documentation. Almost certainly, the other party has different pressures and project schedules, so will probably not be able to bend to the will of your project.

Make sure that the integration model includes as much information about the other party as possible, including:

* Technical information — As much information as possible should be collected about the other system. Include all available documentation such as formats, protocols, schedules, and integration workflow.
* Project team — Document as much as possible about the other party's team. Include project managers, technical contact points, developers, testers, operations, and anyone else that may be an asset when trying to bring the integration to a conclusion.

##### Networking in the middle
Despite the desire of primary integrating parties to work well together, the third party is always networking and security operations. Networking, for valid reasons, has a tendency to lock things down to such an extent that the planned integration will simply not be possible within the deployed environment. When working with other parties make sure that networking and security operations are included in the review of each integration point. Getting guidance, approval, and signoff by networking and security operations as early as possible in the development process is a large risk that is easily managed.

#### Detail the integration requirement
The integration model needs to contain the detail of the requirement for each integration point. As the integration model, like all CALM models, is intended to evolve over time, the degree of detail up-front is low. Describing the full requirement to implement the integration can be a time-consuming exercise, with participation from many stakeholders, as all of the integration business rules and edge cases are hammered out.

The simpler and quicker requirements to document include:

* A basic description of the data.
* A sample of existing data (either in the documentation, or an example used for another application).
* A list of preferred protocols for integration (web, FTP, etc.).
* The data format that will be used (JSON, XML, CSV, etc.).
* Approaches to authentication and authorisation (username/password, active directory, etc.).
* The general networking requirements (ports, static IP address requirements, etc.).
* The schedule or trigger for integration (daily schedule, on-demand, per transaction, etc.).

Further requirements that take longer to document include:

* Fields and field mappings.
* Validation of incoming data (datatypes, required fields, domain lookup, etc.).
* Business rules for transformation from source to target schemas.
* Handling of errors and failed integration (halt import, rollback, compensating transactions).
* SLAs (Service Level Agreements) between the parties.
* Health monitoring and operational procedures.
* Testing of the integration (test data, test platforms, etc.).
* Detailed network requirements (specific ports, logins, IP addresses, firewall rules, etc.).
* Detail of security risks and how they are addressed (authentication, authorisation, encryption, non-repudiation, etc.).

#### Proposition of alternatives
The reason why we have such a proliferation of outdated integration approaches in our systems is because we let it happen. When an architect or application designer is told that data will be provided, for example, by placing a CSV file on an FTP server, it is accepted as being feasible and, to a degree, trivial. Because some integration problems are simple to solve, and we are under pressure to deliver features quickly, the de facto integration approach is accepted as 'good enough'.

Once some detail of the requirement and the other party is known, see if you can push the technical solution to something that is more cloud-friendly. Perhaps the other party will welcome the opportunity to integrate 'the right way' instead of persisting with an approach that is outdated. For example, if the stated requirement is to send data using a daily CSV file, enquire whether or not sending the data per transaction is better (this is easier to implement in your application). If this is acceptable, see if it is feasible to put the data on a message queue (such as a Windows Azure Queue) that the receiving application can pick up.

You may find that suggestions of alternative approaches are welcomed, and an integration point that initially seemed clunky, prone to failure, and tricky to implement, all of a sudden becomes message based, loosely coupled, and well aligned to your own application architecture.

#### Schedule the integration effort
Each integration point needs to be worked into the project schedule. The integration model is not a substitute for project management but should be used as a source for the project schedule. Integration takes a lot longer than expected, not just because of increased effort, but also because of the elapsed time waiting for other parties to do what they need to. Milestones include:

* Date specification complete. This will depend on the integration point, as some interfaces will be product-based and be better described at the beginning. Others may take longer as both sides need to be developed.
* Date development starts.
* Date that the other party is ready for testing.
* Date of initial test data and test runs.
* Date of full integration test start.

### Assess integration technologies
Integration is assisted by various technologies. The technologies range from simplistic FTP servers, to Windows Azure services, to sophisticated integration tools. The integration technologies chosen are going to be part of the application architecture for a long time, as changing them requires another organisation to make changes too. Because of their architectural longevity, the technologies should be chosen very carefully and, in addition to their technical suitability, must be considered against:

* Their suitability for cloud oriented architectures. This needs to consider the use of non-persistent virtual machines, as used by web and worker roles, load balancers, and other concerns.
* Cost considerations as per the cost model, which tends to be based on consumption, rather than licensed per node.
* The effort for the other party to make use of a particular technology. For example, some parties may not be able or willing to read a message off Windows Azure Service Bus.
* Development effort to implement the technology. Minor integration points may not be worth the effort to implement on top of sophisticated tools.

Technologies considered can be from third parties or Windows Azure. Third party software can be of the following types:

* Virtual Machine installed tools — A specific tool, technology, or service can be licensed and installed on a VM. For example, SQL Server Integration Services (SSIS) can be installed on a VM and integrate with many databases (including SQL Azure).
* On-premise tools — Tools can be used on the on-premise network, and used to integrate with the application through the firewall or across a VPN. Again, SSIS would fit this model, as will any number of tools that IT has skills and licenses for.
* Hosted or cloud integration services — Something as simple as a managed FTP service, or more sophisticated integration services that are cloud-based or more traditional.

Windows Azure also offers many technologies useful in integration, and these need to be understood and considered.

#### Windows Azure integration technologies
Windows Azure offers many features that are applicable to integration, both with on-premise applications as well as other cloud-based applications. There is a lot of material available from Microsoft and third parties on how these technologies can be used for integration, and it is not covered in detail in this book. Architects and application designers are encouraged to investigate them in sufficient detail to determine their fit.

##### Windows Azure Service Bus
[Windows Azure Service Bus](https://www.windowsazure.com/en-us/home/features/messaging/) is a powerful set of services that has been specifically developed for application integration. The approaches of Windows Azure Service Bus are more cloud-centric than on-premise, but do offer very useful features for enterprise integration, such as the [Windows Azure Service Bus relay](https://www.windowsazure.com/en-us/develop/net/how-to-guides/service-bus-relay/) that allows services to be exposed without the need to open up firewall connections, or changing of enterprise network infrastructure.

The Windows Azure Service bus team is continuously evolving the product to deal specifically with integration issues. The TechEd 2012 presentation, [Achieving Enterprise Integration Patterns with Windows Azure Service Bus](http://channel9.msdn.com/Events/TechEd/NorthAmerica/2012/AZR317), by Clemens Vasters and Abhishek Lal, specifically works through examples that reference [Gregor Hohpe's Enterprise Integration Patterns](http://www.eaipatterns.com/). The Windows Azure Service Bus team is also beginning to add more orchestration-like features to their product (although these are not yet production-ready) using the [Windows Azure Service Bus EAI & EDI - April 2012 Release](http://msdn.microsoft.com/en-us/library/windowsazure/hh689864.aspx).

##### Virtual Machines
[Windows Azure Virtual Machines](https://www.windowsazure.com/en-us/home/features/virtual-machines/) are important for integration as they allow existing technologies that have not been developed for the cloud to be used within a cloud environment. This is demonstrated by Microsoft's addition of pre-built VM images for [SQL Server 2012](https://www.windowsazure.com/en-us/manage/windows/common-tasks/install-sql-server/) and [BizTalk Server 2010 R2](http://blogs.msdn.com/b/biztalk_server_team_blog/archive/2012/08/29/getting-started-with-biztalk-server-2010-r2-ctp-in-windows-azure-virtual-machines.aspx) to the Virtual Machine gallery.

##### Windows Azure networking
Windows Azure offers two features to help with integration, namely [Virtual Network and Windows Azure Connect](https://www.windowsazure.com/en-us/home/features/networking/). Virtual Network allows the creation of VPNs (Virtual Private Networks) in Windows Azure that extend the on-premise network using DNS configuration and IP address ranges. Windows Azure Connect is agent-based and works by being installed on an on-premise server, and setting up an IPSec tunnel between the on-premise server and Windows Azure.

##### SQL Data Sync
Data can be synchronised between on-premise SQL Server and Windows Azure SQL Database using [SQL Data Sync](http://msdn.microsoft.com/en-us/library/windowsazure/hh456371.aspx). SQL Data Sync differs from SQL Server integration techniques (using replication, SQL Server Integration Services, or custom tools), by being entirely service-based, negating the need to setup an additional server. SQL Data Sync is also simpler, both in features and user interface, and does not require custom code to be written.

##### Windows Azure Active Directory
[Windows Azure Active Directory](https://www.windowsazure.com/en-us/home/features/identity/) provides identity management and access control features that integrate with other identity providers. Where this plays a role in identity is two-fold. Firstly, having an existing identity platform that has support for so many identity providers is a type of integration service itself. Secondly, the ability to use on-premise Active Directory is useful for integration with on-premise applications or user credentials.

### Foundations
Integration is often seen as something that is tacked-on to an existing application and is subject to lower levels of quality. Treat integration points as first-class features that require the same development and architectural rigour as any other parts of the application.

#### Ensure that standards are followed
Make sure that the integration isn't hacked together with little concern for standards. Follow the established standards within the project environment, enterprise, and broader industry.

* Engineering standards that are developed by the team should be followed, including technical standards, such as unit testing, and process standards, such as peer review.
* Follow standard approaches for sharing data across the cloud, web, and enterprise. This may include web data interchange standards (such as JSON) and security standards.
* Treat incoming data as if it were your own, by making sure that it is stored properly and responsibly. This includes correct data models, encryption, backup procedures and controlling data access.
* Test integration points as thoroughly as any other parts of the application, or even with more attention.

#### Develop the security model for integration
Many of the fears and concerns about the cloud surround security, and integration requires additional attention to allay security fears. Because your application is hosted on the cloud, it may be perceived as being insecure, and any data stored in it considered at risk. For each integration point, for each party, security needs to be adequately addressed. The security of the integration falls within the scope of the [security model](#SecurityModel), but also needs to be highlighted within the integration model. Aspects of security that should be addressed include:

* Perimeter security — Where the system that needs to be integrated with is inside the enterprise network, open ports, permitted source machines and other firewall rules need to be carefully considered. There may also be similar firewalls used in public cloud endpoints too.
* Authentication and authorisation — Ensuring that the identity of the calling service is confirmed, and permitted to perform the operation. This includes the expiry of identities and permissions.
* Channel encryption — Making sure that the data cannot be read (sniffed or man-in-the-middle attacked) while in transit by using SSL or another encryption mechanism.
* Data security — Where data is moved from an on-premise application to a cloud based application, how the data is stored, where it is stored, and how it can be accessed needs to be addressed. This may require the involvement of compliance experts and auditors.
* Data retention — To reduce the risk of data being compromised, try to keep only as much data as is needed for the current operation and discard the rest, either immediately, or over time. The data should be owned, controlled and managed by the source system where possible.
* Operational security — Address the security of data that is stored in database backups, as well as the access that operators have to any data received via integration.

#### Develop an approach to availability
Points of integration are a problem when building available applications. Integration points are prone to failure due to the complexity of handling all edge cases, network issues and availability of the other service. There is little that can be done to increase the availability of services on which the application depends, as it is owned and operated by an organisation that is outside your control.

The integration model needs to be aware of the approaches defined in the [availability model](#AvailabilityModel) and each integration point should have a strategy for dealing with the failure or diminished health of the other service. This may mean implementing approaches such as asynchronous message orientation, feature shaping (service degradation), retries, alternative data stores, and so on. The integration model should be specific about how availability is going to be dealt with for each integration point, so that developers can estimate and plan accordingly.

### Estimate the integration effort
Integration effort is generally severely underestimated. This is due to:

* Business understanding of integration — Non-technical people involved with projects have a good idea of specific data that is required for integration. They are involved with understanding what data they need, and what is available, so believe that they understand how the data is integrated. They have little understanding of the complexities involved as there is a big difference between a sample spread sheet of data and fully integrated, reliable applications. This misunderstanding results in estimates that are either set or influenced by the business, and not technical members of the team.
* Involvement of many people — Integration of applications requires the integration of people across teams. Developers of one application do not know the detail of the other, so must work together. Business representatives need to get involved, from both parties, to describe the rules for data, field mappings, and edge cases. Testers need to run tests across multiple systems. Operators and network security staff exist within both parties, and need to be involved. The participation of so many people, across many teams and organisations, is a big project management task, and the time taken from start to finish of integration can be very long indeed.
* Implementation of business rules — Whether it is in workflow, or data transformation, integration requires the translation and mapping of business rules and semantics from one system to another. This can be quite difficult, as the systems satisfy different needs and customers with differing data schemas and processes. There are always edge cases, odd hard-coded rules, default and required data issues, cardinality differences, and even seemingly simple data such as times can cause problems if the source and target systems are in different time zones. Failure to consider the complex business rules can make estimates far lower than they should be.
* Painstaking development processes — There is no easy way to write integration code. Unit testing adds minimal value and development is done largely by trial and error, where undocumented behaviour of source systems has to be slowly and methodically picked through. Developers with little integration experience have a tendency to greatly underestimate the effort involved.
* Testing is difficult — When testing a fully integrated system, it would be useful if a fully representative test platform were available. This may be difficult to achieve, particularly if a full test requires that all of the integration points are available in a test mode, but running at production capacity. Finding errors using 'live' data and production systems is often the only way to fully test the application, and this happens very late in the project. Failure to estimate and consider how much effort remains once the integration is considered 'code complete', can severely impact delivery very late in the project.

The integration model requires enough (realistic) estimation to cover the effort required to implement all integration points. It is important that all integration is considered, as the points that may seem small and insignificant can often be the ones that create the most problems.

### Remove unnecessary integration points
If integration does not show the best of a cloud application and exposes the application to significant risk, it makes sense to minimise the number of integration points. Go through each integration point and classify it in terms of the adopted project classification of features ('Critical', 'Nice to have', etc.). Together with the estimate of effort and the technical complexity, use this classification to prioritise and aggressively remove unnecessary integration points. This may require that an entire feature be cut from the application and moved to a later phase. You can also consider the interim use of manual integration, such as the ad-hoc, manual importing of data into the database.

The integration model should clearly reflect the team's choices on which are the priority integration points and the motivation for including them. The amount of development effort spent on integration should be minimal, say less than 5% of the total effort for the application.

### Review the project qualification
If an application has too many integration points, the effort to implement integration is too high, or the application functionality suffers greatly from the removal of integration features, consider the viability of the application as a good candidate for the cloud. Feed this back into the [qualify](#Qualify) process, and consider selecting a better suited candidate project that requires less integration. If the project absolutely has to proceed, make sure that the delivery of features is staged in such a way as to defer the implementation of integration features into later phases (post-live). Allow the application to mature and demonstrate its viability on the cloud before allowing it to get bogged down by integration effort and brittleness.

## Summary
Few applications exist in isolation, and the ability of an application to integrate with others is an important part of being able to deliver value. Indeed, much of the web is built on applications that have little of their own core functionality, but are able to present data in innovative and useful ways. Even Google search is built on integration with millions of web servers around the world. Cloud applications are well positioned to integrate successfully with other applications by being on the public internet with few firewall or bandwidth problems to deal with, and are good candidates to show the value of integration done well.

However integration is difficult, requiring significant effort to get right and often more effort than is initially estimated. 
Integration has to deal with a variety of source systems, from those that are architecturally similar and based on the same standards, to those that use outdated protocols and proprietary interfaces. Some source systems sit on similar, publicly accessible platforms while others hide deep behind enterprise firewalls. Some source systems are fast and reliable, while others slow and prone to failure. Some source systems have well matched schemas and implement similar business rules, while others require significant effort to transform data from their unfamiliar models. The breadth and variety of applications is what makes integration difficult, time-consuming and full of surprises and also introduces significant risk to the project.

The integration model encourages the collection of detail about all of the interfaces and the devising of technical, business and project managed solutions. Effort put into the integration model at the right time during the development process, will pay dividends by aggressively managing risk early in the application lifecycle.

![Integration summary](./images/Integration-001.png)

### Steps
1. Understand the applicable scenarios.
2. Determine the scope of integration.
3. Detail each integration point.
4. Develop the security model for integration.
5. Develop an approach to availability.
6. Estimate the integration effort.
7. Remove unnecessary integration points.
8. Review the project qualification.


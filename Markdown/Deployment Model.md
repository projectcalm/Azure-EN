# <a name="DeploymentModel">Deployment Model</a>

It seems odd that deployment needs to be considered so early in the design stage. We are used to building applications with a vague idea of how it will be physically deployed and, as the release date approaches, throwing it over the wall to IT for deployment (assuming the hardware is available).

With cloud computing applications, the deployment needs to be considered much earlier in the development cycle. Not only does the unfamiliarity of cloud deployments require that more time is spent understanding it, but also the deployment has an impact on the overall application architecture. Early consideration and planning of deployment is an important aspect of cloud computing that should not be glossed over.  

## Differences with cloud deployments
Arguably every deployment is different and, particularly with high load systems, the deployment is non-trivial, requiring highly skilled hardware, networking and other engineering expertise. Cloud application deployment is, at least in some ways, far easier (there is, for example, no great big specialised database cluster with complex underlying storage), but it is significantly different. This difference is reflected in both the physical deployment and the impact it has on how the application is built. It is that departure from what is known, understood, and for which there are skills, that makes cloud deployments more difficult.

Some of the differences between cloud deployments and traditional application deployments are described below.

### Unfamiliarity
Windows Azure deployments are unfamiliar to most people. There are no physical servers, power, networking equipment, or other traditional reference points. Even for teams that are accustomed to managing virtual infrastructure, things look different. There are no servers hosting virtual machines, no SANs or virtual network cards. The management tools are unfamiliar, with a web based management portal as the interface to the deployment with hosted services, storage accounts, affinity groups, and other unfamiliar services and terminology. That unfamiliarity also extends to the development team. Some developers may have been on an Azure project, or played with it in their spare time, but for others, even if they understand what a web role is, they are unfamiliar with how the applications are to be deployed.

The architectural approach may also be unfamiliar, with loosely coupled services, multiple instances, asynchronous processing, and a data model that extends beyond SQL Server. There are no clear lines to draw between *this* server and *that* server when services are only tenuously coupled with messages in a queue. There are also no existing mental models on how things should work, so developers struggle to depict (and even keep track of in their minds) the complex flows of data between services, when all failure and edge cases are considered.

Testers, who are accustomed to running a specific test and checking that the data is indeed over *there* and in the correct format, find themselves struggling to write test harnesses that make allowances that the result will get *there* eventually.

Even project managers, already sufficiently distanced from the technology, can find grey areas of deployment difficult to come to terms with. Determining completeness can become tricky when an application is deployed to test and production on the same platform, at the same time, with different services stopped for one reason or another. 

### Deployment of services versus hardware and software
The concept of deployment of services to the cloud versus hardware and software in a traditional datacentre is one of the most unfamiliar elements. The traditional process of installing hardware, then operating systems, then third party software, then the application, with configuration steps along the way is no longer the practice. Applications are deployed as services that run on top of existing services (the 'S' in PaaS) that are already available, configured, running and just need to be consumed. The custom application itself is not even 'installed' or 'configured' but rather has its source and configuration deployed to the service, ready to be automatically installed on an instance as and when it is needed. This conceptual difference is fundamental in the day-to-day design, development and operation of cloud based applications.

### Influence of deployment on availability
Modern applications that take advantage of cloud computing platforms generally have higher availability requirements. There may be little or no downtime window that allows for major releases, and application features may, by design, go through rapid release cycles in response to customer demand. This requires a different view on deployment, where parts of the application can be deployed and tested in a production environment without negatively affecting the overall availability of the application. This is accomplished using a combination of application design (asynchronous processing, feature shaping etc.), features built into the Windows Azure platform (staging, virtual IP address swaps, etc.), and product development (Test In Production). Deployment becomes more of an operational extension of the implementation team than something that happens in the early hours of Sunday morning.

### Cost model
Although we try not to prematurely optimise the cost during design, the deployment model and the cost model do work hand-in-hand during design. For example, the size of Windows Azure SQL Database databases chosen, and approaches to partitioning based on the cost of the databases, can influence design choices. Logically separate workloads may be combined because the workload on one is so low that it doesn't cost justify a deployment that mirrors the separation. For example, a single worker role processing messages from multiple queues, or a web application and WCF interface being in the same web role.

### Rapid provisioning
If one of the benefits of choosing a cloud platform is the ability to bring an application to market quickly, then the ability to deploy quickly should match the expectation. Leaving deployment considerations too late in the application lifecycle could mean that when it comes to launch, a host of poor assumptions and unfamiliarity with the process come to light. Since the 'production' platform is available, deployment to 'production' whether it is publicly accessible or not, should be part of the POC (Proof of Concept) and development processes.

### Billing
Billing is a difficult point throughout development and into production. One of the first problems that cloud implementations encounter is the question about whose credit card is used to buy the service during the POC. Pay-per-use services such as Windows Azure don't lend themselves to normal business accounts payable and procurement processes, and the financial impact of deployments, particularly for development and testing, often create problems. The person responsible for deployment is often the first person to run into questions about billing.

### Internal IT
Enterprises with good perimeter security and other IT practices may create problems for cloud deployments. Firewalls often limit access to outgoing SQL ports, have bandwidth restrictions, or other rules that get in the way of delivering cloud applications. Considering deployment during design, and involving internal IT, helps to create an understanding of what needs to happen on the internal infrastructure in order to support the implementation and delivery of the application.

## Principles of the deployment model

### First Principle - Provide an early view of deployment
Due to the differences in deployment on the cloud versus traditional deployment, it is important to publish a view on your approach as early as possible. This is why we encourage the development of a deployment model during the design stage. In publishing a view of the deployment early:

* Developers unfamiliar with Windows Azure begin to understand where their code fits into the overall architecture.
* Testers have a reference for developing the complex test scripts that are required in asynchronous environments.
* Testers have a view on what can be scaled up early on in the project for performance testing.
* Operators (or devops during development) have a document that they can take to internal networking, or existing operations, in order to facilitate the required cooperation.
* The cost model is dependent on the deployment model, and there is an obvious need to have an idea of platform costs early in the project.
* Project managers have a reference that has many uses, such as completeness, costs, and dependencies

### Second Principle - Let the deployment process influence the application architecture
The ability of all or part of an application to be deployed (which includes scaling) in a production environment, without negatively affecting performance and availability, is a primary architectural goal of cloud applications. It is important that when design choices are made that they are checked against deployment considerations and reworked if necessary. Constantly questioning the ability to deploy, and the deployment processes that surround a particular design decision, influences service decoupling, asynchronous processing, and the adoption across the application of similar design patterns.

## Developing the deployment model
The deployment model consists of two distinct parts. The layout view describes, in as much detail as is available and necessary, what the services and related configurations are. The process view describes what is deployed, how and at what stages, in an operational context, in order to maintain adherence to the application SLAs.

![Deployment Model](./images/Deployment-001.png)

### Develop the Layout View 
The layout view contains as much detail as is known about what will be deployed, where it will be deployed, and configuration specifics. Although some diagrams may be useful or required, the goal of the layout view is to provide as much detail as possible to various interested parties.

#### Documentation and depiction of the layout view
Views of deployment of hardware and software in a datacentre have traditionally been diagram biased with representations of servers, network connections and even user workstations on a single diagram. Cloud applications tend to be more difficult to depict diagrammatically due to their dynamic nature and loose coupling between services. How do you draw a line representing data flow or dependency between two services when there may be hundreds of geographically dispersed instances, eventually consistent data flow and a tenuous, dynamic coupling between the services?

While there may be pressure to create a single, representative diagram of an application deployment, don't let it divert attention from the content. It may turn out that the deployment is too complex to represent in a diagram, and trying to visualise it will either over simplify the deployment model, or reduce the likelihood that the diagram will ever be delivered or understood. Focus on providing the content over pretty pictures.

#### Deployable units
A Windows Azure application will have many deployments; from the initial deployment during development, to the final go-live deployment and frequent (perhaps daily) deployments as defects are fixed, configuration updated, and features added.

The deployment layout view has to be aware of the deployment process and the impact of deployments on the availability of the running application. This means that deployable units need to be grouped together in a way that makes both architectural and operational sense. For example, it does not make sense to package a service's database scripts together with the application, as the database will be deployed once and the service frequently. The first consideration when grouping together artefacts into deployable units is:

* Frequency of deployment — Some parts of the application will need to be deployed frequently. This will mostly be application code that is packaged for a service. When critical defects are found and changes made, it should be possible to deploy the unit to production (via test) within a matter of hours.
* Impact of deployment — Parts of the application on which there is a high dependency can have a significant impact on the application availability when they are deployed. Shared databases are the obvious example, but databases are seldom deployed from scratch anyway because ALTER scripts are deployed to live databases. Services on which there is a high dependency, and little consideration for availability built in to the consuming application, can have a significant impact. Shared identity services are an example of a service that can have a significant impact when it is (re)deployed.

When determining the deployable units, and considering the frequency and impact, the following deployable units will tend to emerge:

* Infrastructure — Usually a one-off deployment that includes virtual networking, storage, and subscriptions.
* Shared data — Shared database resources, such as a Windows Azure SQL Database that is shared by many services, also tend to be one-off deployments and, in this respect, behave a lot like infrastructure. Database changes (either DDL or DML) need to be carefully scripted and tested in order to reduce the impact of the deployment.
* Services — Services are the most common deployment unit, where everything for a particular service is packaged together. This could include the application code, as well as other services, such as a cache and database services, and configuration that comprise the unit.
* Configuration — Configuration can be deployed as individual units, particularly when used to store infrequently changing configurations, such as integration endpoints.

#### Scale Units
The [capacity model](#CapacityModel) defines the scale units for a service. These scale units specify all of the infrastructure services with their sizes (compute, database, cache, etc.), as is needed for application scalability. Because scalability should largely be automated, a scale unit should be configured as an individual deployment unit and managed accordingly. Changes to the size of services within a scale unit (such as instance size) would be part of the configuration and should deploy fairly easily.

#### Azure service configuration
When describing the configuration required for Windows Azure, step through the available options in the Windows Azure management portal, and document the relevant configuration. It doesn't matter if some detail is not available, but it is important to use it as a reference to ask questions. There is a lot to cover in the service configuration, and it depends on the Windows Azure features that are used.

The deployment model should detail all of the service configurations that need to be kept track of. This includes subscriptions, storage account keys, affinity groups, virtual networks, certificates, and many more.

#### On-premise infrastructure, services and integration
Enterprises are understandably nervous about allowing public access into their perimeter, and Windows Azure is, at least from a networking perspective, indistinct from any other public IP address (hence the 'cloud' notation of networking diagrams). If the application deployed on Windows Azure has a dependency on on-premise infrastructure, then it is imperative that this is described up front. This allows the necessary design decisions to be made, and the all-important interaction with enterprise IT to take place (particularly perimeter security). 

Discussion about the deployment may influence either the architecture, or the existing on-premise network. For example, say the application needs access to an on-premise SQL database it is unlikely to be accessible over port 1433. The solution is either to move the SQL database outside the perimeter (which may be a bad idea), to synchronise data up to Windows Azure SQL Database (which may be unworkable due to data volumes), or to develop an on-premise service that resides outside the perimeter and exposes the necessary data. All of these are significantly different technical and architectural solutions to a problem that may seem, at first, to be relatively simple. Finding out which solution works for everybody, and finding it out as soon as possible, is facilitated by ensuring that on-premise infrastructure requirements are detailed in the deployment model.

The deployment model should cover:

* Networking — any assumptions about ports that need to be opened for incoming traffic (from Windows Azure to on-premise).
* Active Directory — that may be used for federated identity.
* SQL Databases — for database integration that may be required, either to read from or post data to, and involves the sharing of logins (and perhaps certificates). It may also involve punching a hole in the firewall when SQL Data Sync is not used.
* Other internal services may also be used, such as web services, email infrastructure, or even file shares.

Much of the detail of the infrastructure used will be contained within the [Integration Model](#IntegrationModel), but this should not be skimmed over in the deployment model, as the deployment (or the inability to deploy) influences the integration architecture and model.

#### External service and integration dependencies
External services on which the application depends tend to be more publicly accessible than on-premise infrastructure, but still require documentation in the deployment model. External services may require specialised identity mechanisms, or even machine based licensing that must be addressed and dealt with. The detail of these services will help to determine the location of configuration information to be administered by operations.

As with on-premise integration, the consumption of external services, as far as the actual implementation and use, is covered in detail within the [Integration Model](#IntegrationModel).

#### Mapping of features and components to roles and virtual machines
The traditional developer view on application deployments is both quite simple and, from the developer perspective, somebody else's problem. It used to be that a web application or IIS hosted WCF service would map to a virtual directory on IIS. The administration of the virtual directory would be left to IT/operations, or simply created on an existing server.

With Windows Azure, different components can be scattered across multiple roles, or combined into a single role if individual usage is low. Consider the example of a web application that is accompanied by a service API. Within Visual Studio, these would be built as separate projects in a single solution and deployed independently (either to the same or separate physical services). In an Azure project, these components may be deployed within the same role, requiring some [manual configuration of endpoints](http://msdn.microsoft.com/en-us/library/windowsazure/gg433110.aspx) which, at the very least, creates developer confusion when developing and debugging; as the developer tools only support one application within a role.

This gets more complicated when working with worker roles that read messages off queues. In a traditional environment, queue readers could be implemented as Windows Services, and deployed on the same machine or multiple machines as required. The combination of multiple readers of queue messages within a single worker role is a design-time decision that cannot be easily undone. If, for some reason, one queue needs to be moved off to a separate role, it may require a significant code change (new project, moving of libraries).

The deployment model should attempt to map these individual applications/components/websites into specific named roles. While the *Single Responsibility Principle* (that good developers embrace) would favour the separation of functionality into roles, the costs of having a multi-instance role hanging around to perform some occasional low load function may be too high. Since the deployment model is one of the primary inputs into the cost model, these costs will only become clear if the individual roles are teased out. Try and create (and maintain) a table that maps between Visual Studio projects and roles, the example below (which uses fictional project and role names) will help during discussions with the development team.

[table][Example: Mapping solution projects to Azure roles]
| Role/VM 	| Application/Project 	|
| ------	| ---------------------	|
| WebAppRole 	| WebAppMVC, WebAppAPI, AdminWebAppMVC	|
| SmallTasksWorkerRole 	| BackgroundQueueReader (inject emailProcessor, lostPasswordProcessor, startImportProcessor)	|
| OrderfulfilmentWorkerRole 	| Orderfulfilment 	|
| IntegrationWebRole 	| IntegrationWCF 	|
| IntegrationServer | SSIS Packages |
[Example: Mapping solution projects to Azure roles]

### Develop the Process View
The deployment layout view facilitates the architectural and project management processes, but represents a 'to be' static representation of how the application is deployed. The process view of the deployment model represents how deployments are undertaken and getting this sorted out is crucial. "How to deploy an application" is likely to fall through the cracks if not specifically addressed. Developers don't care much, and operators are more concerned with post-live maintenance, so there is generally nobody concerned enough about the bit in the middle.

This middle space is starting to be filled by 'devops', which is a role that emerged out of the continuous integration and continuous build environments. In a nutshell, devops within the development team perform functions that seem operational in nature. Devops don't work on application features, but rather with infrastructure and deployment features. Defining the devops role and ensuring you have someone in that role, is part of the process view of the deployment model.

#### The architectural influence of the deployment process
When an application needs to be highly available, scalable, fault tolerant, and other attributes common to cloud computing applications, the deployment process influences the architecture. There would be no way, for example, to meet high availability targets if the application required downtime for an hour every time a new version was deployed. Applications can indeed be architected to handle frequent deployments and the process needs to be understood in order to accommodate this within the design.

Questions that might be asked with respect to the deployment process include:

* How can we deploy updates to the application without impacting availability?
* How can failed deployments be quickly identified and rolled back?

#### Typical cloud deployment activities
The following activities illustrate the differences in cloud deployments and what they mean to the deployment process or application design.

##### Redeployment of the entire application
The physical Windows Azure deployment of services, and the configuration of various services (as detailed in the deployment layout view), is often performed during development as requirements become clear, detail of interfaces becomes available, and finished components are deployed for testing. This *organic* deployment is common in traditional datacentres where it is assumed that the infrastructure will exist until it is decommissioned. It is tempting (or overlooked) to follow a similar approach on Windows Azure and not to script out the ability to rapidly redeploy the entire application, with all of its configuration. What if the subscription was cancelled for some reasonable administrative reason? What if the entire application needed to be deployed to a different datacentre in the event of an extended outage or insufficient capacity in the current datacentre? To manually redeploy the entire application and its configuration, with no documentation of the configuration and the sequence, would be a time consuming and risky task.

The correct approach is to script out the deployment (using the [Windows Azure Management API](http://msdn.microsoft.com/en-us/library/windowsazure/ee460799.aspx)) as a development activity. Ensure that the scripts are checked into source control, and are subject to the same disciplines and rigour of application code. These scripts should include scripts (or clear documentation for manual configuration) of external dependencies (such as on-premise firewall configuration).

##### Configuration updates
Configuration data can be stored in many different places, and the handling of updates by the application needs to be understood and considered. Some places where configuration data is stored include:

* Service definition — changes to the service configuration generally require a restart of the roles that are impacted by the configuration. 
* Service configuration — the service configuration file is a good way to get the Azure fabric controller to ensure that configuration changes are deployed to running roles. However, in order to prevent unwanted restarts, the RoleEnvironmentChanging event is triggered and code needs to be written in order to determine if the configuration update requires a restart. 
* web.config — the traditional place for configuration data is the web.config file. This should not be used for configuration information in Windows Azure, and the service configuration should be used instead.
* SQL — configuration data is frequently stored in the database. If it is frequently accessed, it also means that it is probably cached, and the invalidation of the cache (or recycling of the roles) is needed when the configuration is updated.
* Cache — storing data in a distributed cache is a good technique to ensure that data is both available and up-to-date.

##### Deployment of updates to Windows Azure hosted services and roles
Windows Azure provides extensive support of updates to services and roles. Upgrade domains, virtual IP address swaps, and production versus staging environments provide many options to deploy updates without impacting availability. Which of these features are used, and in what cases, needs to be properly understood and assessed by deployment teams. The features enable:

* A 'rolling thunder' update of roles where not all instances are updated at once, allowing running roles (whether on the old or new version) to handle the load while others are being updated.
* The ability to deploy to a staging environment and swap the staging and production environments quickly without having to start up new instances.

While these features provide critical functionality that ease deployment, some resulting issues need to be dealt with, such as multiple versions of a single role running at the same time and the ability to 'point' services to staging addresses if necessary.

##### Deployment rollbacks
Updates to applications can, and do, go wrong for any number of reasons. Whenever an application is deployed, the question always needs to be asked "What if this doesn't work?", and plans put in place 'just in case'. This may require careful consideration in the application, particularly if contracts (message schemas or other fixed schemas) change. A worker role, for example, may need to be able to handle message payloads from previous versions just in case the message originator has to rollback to an earlier version.

##### Database snapshots
Performing database snapshots (database copies in Windows Azure SQL Database) is frequently used as a technique in deployments where bulk database updates or schema changes are implemented in order to have a database (or data) to rollback to. When using database snapshots the deployers need to be considerate of inconsistent data. The application may need to support the temporary suspension of database operations while a snapshot is taken and the deployment rolled out, or allow for updates to multiple databases at once (almost like having an equivalent of a deployment 'staging' database). This becomes more difficult, and even impractical, at a scale where there are many databases.

#### Design considerations for deployment process view
Techniques that need to be built into the application to handle deployments are similar to the techniques used for availability (as covered in more detail in the [Availability Model](#AvailabilityModel)) — after all, the ability of an application to handle the failure of a component due to unplanned failure is indistinguishable from a component that is unavailable because it is being deployed. Indeed, the ability to handle deployments gracefully can come about as a side effect of good design and architecture to accommodate any type of failure. In this way, the 'design for failure' mantra becomes 'design for deployment' as deployments are more frequent than failures.

Availability techniques that are particularly relevant for deployments include:

* Stateless services — when services (particularly websites) are stateless, the updating of a service will be transparent to the user. Before a role is shutdown, the current request will be properly processed, and the next request will pass to an updated instance. This can cause problems if there are significant changes that prevent a single session being served by different service versions. In such cases, the session may need to be abruptly terminated and the user redirected 'home'.
* Stable interfaces and contracts — if services are loosely coupled and not dependent on a particular contract or schema, the calling service simply interacts with the available service, and doesn't care whether or not it has been updated. This requires that the contracts do not change, otherwise the services have to handle multiple versions.
* Asynchronous processing — if extensive use is made of asynchronous processing using a persistent message store (such as Windows Azure Queues), the updating of the worker role that processes messages is completely independent of the application that puts messages on the queue. Provided that there is enough storage (at 100TB for a Windows Azure Queue and 5GB for a service bus queue) and that the worker role can be updated quickly enough in order for the processing to proceed, there will be no impact on availability during the deployment.
* Retries — while not the best fall-back position for deployments, retry logic that can handle a brief unavailability of a service could be applied for some updates that result in a brief downtime (such as changing the number or type of endpoints for a service).
* Alternative data store — applications that can cope with the failure of a database can handle deployments where the database is taken offline. This may involve insertion of data into another database (or other storage), or the reading of data from an alternative, possibly out of date, data store.

#### Development approaches that influence deployment
Deploying an application to production is only one part of building and operating applications, and other processes influence the overall deployment process. The deployment process of a well-tested application will obviously differ from one that is quickly patched and moved to production with insufficient testing.

##### Testing
By far the biggest influence on the confidence of deploying an application update is the confidence in how well the fix has been tested. Where testing is weak, deployments are delayed or batched, as nobody wants to take responsibility for pulling the trigger (and rightly so). The testing extends beyond simple button pushing UI testing, and should include performance and load testing. Unfortunately, not every tiny little update can demand a full set of regressive UI, performance and load tests, which leads to the need for other quality processes, to avoid a reliance on testing alone.

##### Test driven development and continuous integration
TDD, software craftsmanship and other practices and processes are concerned with ensuring that the maintainability of software is high. With these processes, a change can confidently be made to a class knowing full well that it will be contained with a set of unit tests to ensure that the change behaves as expected. This is very important with deployments, as it ensures that developers can confidently make changes without having to attempt to understand the ramifications of their change. That confidence is transferred to deployers who have to make the call on quality before deploying.

##### Continuous delivery
An extension to continuous integration is the concept of continuous delivery. Continuous delivery extends continuous build by being able to automate the build, test and deployment on a continuous basis, even to production if necessary. While a high level of maturity (and a limited type of application) is required for continuous delivery, many of the practices can be individually applied. Some of those include:

* Automating deployment by selecting a candidate build from the build server (as used in continuous integration).
* Creating and running integration tests using unit test frameworks.
* Automating performance tests, including the automated instantiation of test agents.
* Continuously running tests on a production using test frameworks and feeding this in to health monitoring

##### Test in production (TiP)
TiP is the idea that at scale not all errors can be picked up in testing due to unpredictable load and edge cases. In TiP, the application may be deployed with updates that are only available to certain users, such as employees or beta testers. In such cases, the application may need to be developed to connect to services at different addresses based on a user profile or originating IP address.

#### Scheduling deployments
While small, urgent deployments could be made at any time, deployments generally need to be planned to take place at specific times or intervals. Cloud computing deployments need to be considerate of platform/SDK updates and the lifecycle model.

##### Lifecycle model
The [Lifecycle Model](#LifecycleModel) heavily influences many aspects of the design, and its value in the deployment model is high. Deployments need to be planned (or at least scheduled) for when there is going to be minimal impact on user satisfaction, this is because:

* Deployments often result in reduced capacity (as roles are taken offline).
* The time taken for a deployment may cause a backlog that needs to be cleared (such as a backlog of messages on a queue).
* The deployment may need to be rolled back or result in extensive downtime, in which case it would be preferable if as few users as possible are trying to use the application.

The lifecycle model should provide all of the data necessary in order to determine the optimal time for deployments. Things to look for include:

* Daily peak load times — deployments should be scheduled when load is off-peak, and when sufficient capacity is available. Remember to consider availability of developer, test and operational resources for deployment. Even though 2am may seem to be the best time for a deployment, if anything goes wrong there is nobody available to fix it.
* Planned high load events — the lifecycle model should contain data that represents when an increase in load is expected due to marketing or a release of new features. Scheduling a deployment to occur far enough in advance, or after the event, is advisable (in order to iron out bugs).

##### Platform and SDK updates
Although Windows Azure allows applications to run different host OS versions and different versions of the SDK, running an application on an outdated version immediately incurs technical debt. Delaying deployments because the application is lagging the platform can cause problems. The schedule of updates to the Windows Azure platform needs to be understood, communicated to the development team, and allowed for in the deployments.

In addition to well-publicised platform updates, occasional tweaks are made to the platform (particularly incremental updates to Windows Azure SQL Database), which need to be considered in the deployment model. Also, CTP services, for which there is no official support, and the expected release date of official versions need to be described within the deployment model.

## Summary
An application developed without consideration for deployment will not run optimally on the cloud. The deployment processes whereby live services are updated, tested, or even rolled back, without affecting application availability, are not simple. It requires an application architecture that supports partial availability, multiple versions, and live updates. It requires developers, testers, operators, and even the business to work well together, according to a clear view of the layout and the processes. The deployment model creates these layout and process views, making it invaluable for a running application and providing useful input into application design and development.

### Steps

1. Develop the layout view.
2. Develop the process view.


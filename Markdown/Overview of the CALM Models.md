# <a name="ModelOverview">Overview of the CALM Models</a>

## Qualify
Not all applications fit well in the cloud, and some applications are better positioned to take advantage of the cloud to seize business opportunities or resolve existing challenges.

The qualify process is the first step on embarking on a cloud project, and the model helps ensure that the best candidate application is selected to run on the cloud. It considers the business value, risk, and cost factors across a number of projects in order to select those that are viable. Together with some indicators of what types of applications to avoid, and what types of applications to look for, working through the qualify process should result in a strong business case for an application that is to be developed on the cloud.

## Prove
One of the primary reasons for following the CALM guidance is to reduce the project risks as early as possible. Early in the project lifecyle, the most important method to remove risk is to prove that both business and technical assumptions made about the application and its architecture are valid and that the application is still a good candidate for the cloud.

The prove process pays particular attention to the technical proof of concept (POC). Running code on a production Windows Azure platform developed to prove the specific design decisions made in the application, is the only way to sufficiently prove that the application will work.

## Workload Model
Cloud implementations favour the development of loosely coupled services that perform specific functions. The removal of infrastructure influences on the cloud allows the services to run in the most appropriate fashion. Determining what is appropriate is achieved by first of all defining workloads that are either logically separate or separated by differing attributes and behaviours across other models.

The workload model requires that fundamental architectural decisions as to how functionality is decomposed into workloads is made by a capable architect early on in the project lifecycle. The influence of the output of the workload model on other models is profound as they form the basis for the architectural separation and physical isolation of implemented services.

## Lifecycle Model
Cloud applications are undertaken on the promise that they are able to handle spiky traffic. The ability to scale-out based on demand is, as cloud vendors highlight, based on the capacity available in the platform, but it is also something that needs to be engineered into the application. An application that has been built without any idea of what load it is going to be subject to, will either fail to perform to expectations or be over-engineered to handle traffic that never arrives.

Creating a common understanding within the team as to what is meant by 'load' or 'traffic' is critical to architecting and implementing the application correctly. Getting business to think hard about the load, and the cycles over which it is expected, is the first step in building a scalable cloud architecture that is fit for purpose. The lifecycle model is about documenting the relevant workloads through various lifecycles, be that hourly, daily, weekly, monthly or any other relevant interval.

One of the most valuable aspects of the cloud is the ability to handle irregular workloads, thus optimising the consumption of resources in order to keep costs under control. Making sure that this irregularity is modelled and understood is critical to building applications that unlock the value of utility computing. The lifecycle model describes and communicates that understanding so that everyone on the team is clear on what they are trying to build. 

## Health Model
Cloud applications need to be able to respond rapidly to changes in load, and they need to do this on a platform that is built on commodity infrastructure with variable performance and reliability. The application has to run in an environment where the heartbeat can continuously be monitored, so that responses to diminished health can be fast and efficient. This requires that the business is clear on what constitutes good or diminished health, that developers implement mechanisms to collect health data, and that operators are able to see, at a glance, what needs to be done.

The health model provides a framework for defining the healthy states of the application in SLAs, the methods for collecting health and viewing monitoring data, and the processes needed to restore the application to a healthy state.

## Cost Model
The costs of cloud computing are frequently used as the justification for moving applications to the cloud. There is definite value in consuming compute resources as needed and paying for them monthly, instead of large infrastructure investments. Unfortunately for many projects, the costs are quickly calculated when the business case is being drawn up, and promptly forgotten about once the project gets the go-ahead. In order to deliver on the cost saving promises of the cloud, costs need to be understood, planned, and kept under control.

The cost model creates a better understanding of the operating costs of the target application, and requires that a custom model be developed with sufficient detail and is constantly maintained. The cost model encourages the understanding and sharing of cost considerations amongst all members of the team, so that they can understand their individual influence on the running costs of what they are building.

## Security Model
Security is one of the major concerns about the public cloud. Whilst this concern may be unwarranted, in comparison with any other infrastructure, security still requires careful consideration. Whilst there are specific security, compliance, and regulatory aspects to understand about Windows Azure, applications that run on the cloud do not need to be treated differently to any other application. Full attention should be given to security across all levels of the application and its operation. CALM is not prescriptive about security model specifics and, in a highly specialised and fast-changing industry, encourages the adoption of a recognised security assurance process or methodology.

The security model sets the groundwork for understanding some of the cloud security concerns, and points the implementation team in the direction of credible security processes to be adopted.

## Availability Model
Modern applications have high availability expectations. The adoption of the cloud, where a small application can have massive adoption without the need to build expensive and reliable datacentres, puts pressure on applications to be highly available. Cloud computing patterns reflect modern application architectures by building applications that are designed for failure. This is implemented through various technical approaches, such as redundancy, resiliency, scalability, fault isolation, and many others.

The availability model starts by establishing a common understanding of availability, which is then used to encourage the business to describe their requirements in terms of availability expressions that are contained within the SLAs. Before delving into implementation, the costs of availability and the business rationale that supports those costs, is considered within the availability model. Finally, the model helps the team to select and describe the approaches to availability that will be implemented within the application.

## Data Model
Modern applications have drastically changed the data model. No longer are applications built on monolithic SQL databases, but make use of multiple data stores with specific strengths for specific purposes. Cloud applications are architected to run as isolated services that store their data in the best data store for the particular service; from SQL, to key-value stores, to persistent messaging. Applications can choose from a vast array of available technologies including services offered by cloud providers, such as Windows Azure Storage.

The data model describes the need for multiple data stores in terms of specific functionality and the needs of cloud applications. The data model then outlines an approach to resolving the data model across all workloads and their data schemas, including all of the aspects to be documented. The choice of databases and approaches to data is one of the most important aspects of cloud computing. A cloud application that has a data model that does not scale cannot take advantage of the scalability benefits of the cloud platform, and may fail to satisfy its availability requirements.

## Capacity Model
Due to the 'infinite capacity' offered by cloud platforms, it would seem that capacity planning for the cloud is unnecessary. Capacity planning for the cloud is completely different from on-premise capacity planning; there is no need to estimate future resource requirements in order to procure the necessary hardware. Capacity planning on the cloud is about making optimal use of capacity, and making sure that the application can take advantage of the available capacity.

The capacity model is workload-based and introduces scale units for individual workloads. Scale units allow the capacity needs of workloads to be determined and rationalised. Once scale units have been determined, and the application engineered to work with any number of scale units, the provisioning (and de-provisioning) of capacity to handle load is as simple as an operator spinning up additional units.

## Deployment Model
Cloud applications are not deployed as single applications during downtime, but as changes are made and within a production environment that is serving live requests. This is made possible by an architecture that is loosely coupled, designed for failure, isolated, and other patterns. It is assisted by the cloud platform itself, which provides the available capacity and the cloud 'operating system' to enable services to be easily swapped in and out of production. This type of deployment may be unfamiliar to most members of the team including developers who are not used to developing for such principles and operators who are used to deploying to a fairly static infrastructure.

The deployment model ensures that deployment is considered early during the development process and that any requirements positively influence the application architecture. The deployment model requires that the specifics of what needs to be deployed are described in the layout view. It also requires that the process view is described, so that this is considered within the application and the operating environment.

## Integration Model
Application integration is difficult and complex regardless of the platform that it runs on. Whilst cloud applications have the advantage of being accessible and having sufficient bandwidth, they are more difficult to integrate with on-premise applications. On-premise applications implement integration interfaces using outdated protocols, and hide behind robust firewalls that refuse to let in anything without a static IP address. But applications do not exist in a vacuum and cloud applications will require some integration work to be done.

The integration model describes some of the differences with cloud integration, and highlights potential problem areas. The integration model requires that integration points be worked through in detail, early in the project to minimise risks, and describes the detail that may be required. It also discusses some of the technical options that are available, specifically on Windows Azure, which can be used to assist with integration.

## Operational Model
Operations in cloud environments will be unfamiliar to operators. The ability of an application to respond to load and meet its availability targets requires that the application is designed for operations and used by operators with the correct skills and knowledge.

The operational model takes into consideration the CALM models, deployment, health monitoring and release management processes. These are used to determine the required proactive and reactive operational activities. In an environment that needs to operate at scale, with multiple services running, and which needs to respond quickly to load variations and failures, the degree of automated response must be high. The operational model provides a context for increasing the operational maturity of the application and operations team in order to meet these complex operational demands.

## Test Model
Cloud computing platforms offer production capacity and configuration for testers from the day that the project starts. This is a massive opportunity for testers, and allows them to run tests with production simulations as soon as the first build is checked in. Testers have other considerations when testing for the cloud, such as the new technology, the operating environment, the complexity of testing loosely coupled services, and the need to simulate tests of scalable applications.

The test model highlights the opportunities and problems associated with cloud testing and the important role that testing plays versus traditional environments. The test model then describes the types of tests to be included in the test plan, and flags those which may be new to testers, such as the scale test and operational test.

## Future Models
Cloud computing is an emerging market and application architectures are bound to evolve, as are the platforms on which they run. This first release of CALM is focused more on the design aspects of application lifecycle management. The authors are aware that not all aspects of ALM have been covered, such as developer centric tools and processes. We feel that this release is in line with the current maturity of most cloud development teams and covers the parts that are important in ALM at present. Over time the models will be adjusted, and others added, based on our experiences in the field, feedback and demand.

Future releases of CALM are planned to contain the following models:

* Tenancy Model — This model deals specifically with the needs of developers building multi-tenant and SaaS applications.
* Development Model — This model deals with different development approaches for the cloud. It includes both technical approaches as well as processes that are specifically relevant for developers.
* Migration Model — This model deals with the need to migrate applications from on-premise to the cloud, and considers hybrid applications and IaaS-based applications on their way to becoming well architected PaaS-based solutions.
* Scalability Model — In this release, CALM scalability is part of the availability model. The scalability model deals with specific detail on scalability concerns and patterns as a solution to availability.


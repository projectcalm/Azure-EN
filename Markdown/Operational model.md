# <a name="OperationalModel">Operational Model</a>

Applications designed specifically for a cloud computing environment are different from traditional designs, and have familiar attributes to modern application design and architectural approaches. The common theme is the demand for high scalability and availability, on top of commodity infrastructure, for a low operating cost. Traditional data centres with specialised high-end servers struggle to come to terms with this new approach, which has changed how IT infrastructure is owned, procured and managed. Similar changes are being felt by those who are expected to support and operate the applications deployed in cloud environments.

In many ways, cloud computing applications are quite complex: 

* Applications are composed of various, loosely coupled and isolated services that require an understanding of how they fit together.
* Data is stored in many different places, not in a single SQL database, and is not quite consistent or easy to backup, manage and control.
* Some parts of the applications will work when one part has failed or is failing.
* Applications are updated constantly and out of sequence, meaning that a deployment downtime window is a thing of the past.
* Applications run on a publicly accessible, multi-tenant infrastructure that creates security risks and odd performance characteristics.

These complexities mean that the operation of the applications changes.  Operators need to be able to understand more about how an application hangs together and what to do when things go wrong, or how to prevent them from going wrong in the first place.

Marketing hype would have us believe that applications in the cloud magically run by themselves, with availability and scalability built into the platform and little need for skilled operators to ensure smooth operation. While that may be true in some cases, the demand on operators in the cloud is arguably higher than ever. Operators no longer have roles where they monitor infrastructure, back up the database, reboot suspect servers and call the developers in all other cases. Operators now need more familiarity with the application because the developers don't (and shouldn't) understand the nuances of running the application on a public cloud. The rise of the 'Devops' movement is an indicator of a general demand, including in non-cloud environments, for operators with skills that span both development and traditional operations. Only with these skills can operators develop the automated environments, handle deployments on live applications, determine application bottlenecks and make the necessary changes to the cloud platform in order to resolve issues. 

## Principles for cloud enabled operations
In order to facilitate the operation of cloud applications, there are some basic principles that should be top of mind.

### First principle - Build the application for operations
Just as security is difficult to retrofit into applications, so too is the serviceability of the application. It is important to develop the application in such a way as to:

* Collect the necessary data and metrics for health monitoring.
* Eliminate dependency on manually installed or configured components (as many legacy applications require).
* Implement retry and failover within the application, in order to handle failure or diminished health in services on which the application depends.
* Build services that can be independently tested, deployed and rolled back.
* Provide administrative service APIs so that operators can build scripts to automate operation.

These application features need to be planned, designed and budgeted for implementation as part of the initial application development. Serviceability aspects of the application need to be considered first class features, just like end user functionality, otherwise it will be difficult to operate and many of the benefits of cloud computing will be lost.

### Second principle - Eliminate reactive manual operations
Applications taking advantage of cloud computing tend to exhibit unpredictable behaviour. They no longer have flat 9–5 lifecycles but rather have to respond to high load spikes, huge amounts of data storage and processing, and rapid releases of fixes and new features based on user demand. 

When applications need to scale and updates are being released daily, things can break. The platform may not perform as it did in testing, multiple versions may be running at once and operational teams (and their supporting development and test teams) may have immature processes or simply crack under the pressure. Using a skilled operator to sit and manually fix problems in a production environment where things are falling to pieces, users are frustrated, and the business is prowling for scapegoats, is not an ideal situation for any operational team.

Therefore operations must mature to a state where most of the work that they do is either:

* Proactive — where tasks are undertaken before things become a problem, or
* Automated — where diminished health requires none or little human intervention. 

### Evolve operational maturity
Blowing half of the development budget on an awesome operational dashboard is going to be a difficult sell. Part of the problem with the unpredictability is you may not know up front the hot points that need extra instrumentation or automation. Ensure that the basics are in place and add operational functionality over time. This evolution needs to be planned and considered up front, and there are things that can be done in order to ensure that it receives focus.

* Add operational 'features' as first class features to the product backlog in an agile environment.
* Encourage that operational features are implemented properly, preferably with APIs, so that they can be automated.
* Communicate and provide feedback between operations, testing and development teams in order to ensure that the right features are implemented.

## Developing the operational model
The first step in developing the operational model is to consider what aspects of the application and infrastructure the operations team should focus on. This is achieved by understanding the architectural models of the application, being familiar with the actual deployment of the application, and working with any available health monitoring.

![Operational model - existing models and artefacts](./images/Operational-002.png)

### Architectural Models
Most of the architectural models are relevant to operations as they influence how the application is built for, and deployed in, a cloud environment. Some models require extra attention and understanding within an operational environment. The following models are pivotal when developing the operational model.

#### Workloads
Operations need to understand the workloads defined within the application. It is likely that what would have traditionally been a single workload would be split into separate workloads with queue based messaging in-between.  So operations would need to understand the decomposed workloads, the messaging subsystem and how the diminished health of one affects the other.

#### Lifecycle Model
The most important model for operations to understand is the lifecycle model. This provides the information to be able to plan capacity and respond to increased or decreased load. For example, the lifecycle model would indicate, in a retail 'Black Friday' lifecycle, the events (catalogue update, sale begins) and the phases (initial sale rush, order fulfilment, stock availability updates) that require attention from operations. The Black Friday example may seem obvious, but business reality and diversity means that there are many lifecycles that may be obvious to business insiders but unknown to operational staff. The lifecycle model provides information about what is expected to happen to a running application which is absolutely vital to operations.

#### Availability Model
Operations are measured on the availability of an application and this is not simple to define or achieve. The availability model, which describes differing availability requirements, is important for operations to be able to perform proactive tasks and respond to any events that affect availability.

#### Data Model
The data model needs to be singled out for operations, mainly because in a cloud environment the data model is more complex than a traditional single SQL database. When the application is under load or when health is diminishing, operations need a clear understanding of where data is, and where it is supposed to be, in order to react.

#### Capacity Model
The capacity model is operations' blueprint on how to plan capacity and scale accordingly in order to respond to load. By being familiar with the scale units defined in the capacity model, operations can easily ensure that availability SLAs are met and automate the rapid provisioning of extra capacity as needed.

### Deployment
When an application first goes live, the deployment of the application will be familiar to everyone on the team, including developers and testers. As time progresses the application stabilises, growth becomes more constant, the initial deployment plan and the actual deployment in production diverge. It is the responsibility of operations to own the deployment as it is operations that made the decisions to change the deployment, add additional capacity, and make the necessary tweaks to get things working properly.

The additional complexity of cloud based applications means they may be built with loosely coupled services, heterogeneous data stores, varying configurations handling different workloads, and multiple versions that may be in production at once. Keeping a handle on the deployed application is an important and difficult task. While the deployment may match an understood and documented pattern, the details are relevant and will differ. Deployments will be in specific accounts (possibly more than one), multiple databases, have different instance counts for different roles, have specific certificates for different roles, different storage endpoints and any number of important specific aspects. Operations need to maintain an accurate picture of these details.

The tools for obtaining the data on the current landscape will vary from one implementation to the next and will be based on a combination of different tools. Tools that come as part of the platform are useful, as are self-assembled dashboards that use the platform APIs (although there will be a development cost). Health monitoring data may also be used to keep track of the underlying deployment, but some of the metrics may be missing and not all will be applicable.

While the application and its data are the most important parts of the deployment for operations to understand, the lower levels of the stack require special attention. It would seem counterintuitive that a PaaS platform such as Windows Azure would require lower level operational knowledge. After all, Windows Azure is supposed to abstract the underlying platform and remove the need to understand or control it. While this may be true for application developers, there is still a need for operations to understand the Windows Azure platform in some detail, including:

* How the Fabric Controller works and how it automatically allocates resource.
* The role statuses and how they relate to an application being deployed.
* The Guest OS and how applications that target a specific OS behave.
* How Windows Azure fault and upgrade domains work.
* How load balancing works and the capabilities of the load balancer and traffic manager.

### Health Monitoring
As per the health model, all data from the application and Windows Azure platform needs to be collected and presented to operations. This data is used for:

* Real time monitoring of key metrics, particularly during the beginning of a phase (as expected by the lifecycle model) that results in extra load. Real time monitoring should be on minimal data points that can be presented on a single screen, as too much data can be difficult to action.
* Historical analysis of health data in order to determine the health trend of the application and to establish whether the application behaved as expected after changes were applied (particularly those by operations).

It is important to note that health monitoring forms the interface between operational activities to be actioned (either proactively or reactively) and the deployed application.

More detail on the health monitoring aspects of the operational model can be found in the Health Model.

### Release management
As part of the application lifecycle, release management comes well after design.  It is not expected that the operational model, which is developed early on in the design phase, will have much detail on exactly how releases will work. 

It is worth asking questions about releases during design so that everyone on the team is considerate of the role of operations in releases. Some aspects that should be highlighted at this stage are:

#### Out-of-band and frequent releases
Because cloud applications are composed of a set of loosely coupled services, it is easier to make changes to one part of the application without affecting the other. For example, a worker role that processes messages from a queue can be updated without having to update the web role. Indeed, because of the queues, there will be no need to stop the web role while the worker role is updated.

This means that there is no single version of the application running and no batching of updates for a single release of the entire application, during a planned release window. Small parts of the application will be released and even during a peak period if necessary. This reflects the increasing adoption of 'continuous integration' where there are sufficient, automated tests of the application to maintain high quality. It also ensures that quality control is part of the development and continuous build process, rather than being addressed after development is complete.

#### Availability of infrastructure for testing
Because cloud computing platforms provide infrastructure on demand, it is possible to spin-up a massive deployment just to test an application. When preparing for a release it may be desirable to deploy to a test environment that may represent a peak capacity deployment, and is therefore larger than the current production deployment. Additionally, the cloud can be used to run simulations from instances deployed all over the world and possibly on a completely different cloud platform. In such cases, the management and operation of the test platform may be more complex than most operational teams are used to — and all of that effort to run some tests for an hour or two before it is all dismantled and reassembled for a release the next day.

#### Ability to roll back releases
Windows Azure has a staging environment where deployed applications have virtual IP addresses and can be swapped between production and staging. This allows applications to be tested in staging, moved to production and, if problems are detected in production, quickly swapped back. The virtual IP address swap enables this to happen quickly as no roles need to be started or shut down so there are effectively two versions of the application running at once.

This adds to the ability of operations to deploy releases but it can become confusing, particularly when releases are out-of-band and services are kept in staging for long periods 'just in case'.

#### Multiple versions in production
One of the problems with out-of-band releases is that changes to services can have breaking changes, where there is a dependency on a specific version of the output or interface. Because of the loose coupling of services it is perfectly reasonable to have multiple versions running in production, where the dependency between versions is set in the configuration of a particular part of an application. 

For example, a web application may have an endpoint defined for a search service. When something changes, multiple versions of the web application may exist with multiple versions of the search service running at once. Only after the eventual shutdown of all instances of the old version of the web application can the old version of the search service be shut down. 

This is also seen with asynchronous processing using queues. A worker role processing messages off a queue may be version specific. When something changes, the queue name changes as an endpoint configured in the application, and the new version of the worker role processes messages off the new queue. Only when the old queue is empty can the old worker role be shutdown. This requires operations to be familiar with how things hang together and the ability to monitor queues to recognise if they are empty or not.

### Operational Activities
The operational model needs to have a view on the activities that operations will perform.

* Focus attention on proactive automated support. Trying to detail all reactive support activities goes against this principle and may be futile as events, triggers and responses will be less fixed. If they were fixed, they would be automated.
* Remember to evolve the operational maturity. Use the process of identifying activities, to build the operational model as an opportunity to create a list of operational features for the product backlog.

![Operational activities](./images/Operational-003.png)

#### Proactive Activities
Proactive operational activities are defined as those which:

* Occur during normal working hours (operational working hours may differ from user working hours e.g. it may be performed by a night shift).
* Are performed in a consistent and calm manner.
* Are planned to take place.
* Are well documented.
* Provide feedback of their success or failure.

##### Typical proactive operational activities
Proactive operational activities are generally those that either attempt to maintain a stable state of the application, or prepare for an upcoming event. General areas of proactive operational management are:

* Risk management — activities are not required if all goes well, but since things seldom do, should be done anyway. Examples include making database backups, changing or expiring authentication tokens, and testing availability and recoverability.
* Preventative maintenance — ensures that parts of the system are well oiled and in a state where they will function normally when the application is under load. Examples include ensuring that sufficient storage is available, clearing out unnecessary log files/data and may even extend to a 'rolling thunder' style of restarting of application pools or roles. 
* Preparation for load (capacity provisioning) — while we should be able to scale up applications immediately, or even automatically, in some cases it may not be the safest option. Prior to an event (from the lifecycle model) occurring, operations can pre-provision, and even test, additional capacity so that when the rush starts, sufficient capacity is immediately available. Other load preparation activities may include warming up the cache and rebuilding indexes. 
* Preparation for release — releases can cause major disruption if things go wrong and planning for a release helps manage the risk. Include proactive operational activities that can be performed before release in order to reduce the risk of the release failing and make it easier to roll back a release if necessary. Activities include backup of databases, termination of excess capacity and the graceful termination of non-critical processes.

##### When to perform proactive activities
Proactive operational activities are, by their nature, activities that can be planned and scheduled in advance.

* Scheduled activities — these activities run on a specific day at a specific time. For example, database backups may run daily, weekly and monthly.
* Time triggered activities — are a special case of activity where the time that they run is of importance. For example, activities may be timed to run before a data export is due to kick off, or immediately after the market closes.
* On demand activities — will make up most of the activities that result from the lifecycle model and health monitoring. They are the result of an event but are not reactive; they are not trying to restore health, but rather proactively prevent diminished health from occurring. For example, capacity may be provisioned based on the lifecycle model, caches may be flushed or reconfigured based on the number of cache misses, and additional database shards may be added as data volumes grow.

During the process of defining the operational model, a table of scheduled and time triggered activities can be drawn up as per the example below.

[table][Example: List of proactive operational activities]
| What 	| When 	| How 	|
| ------	| ------	| -----	|
| Full database backup 	| Weekly, Sun 02h00 	| Automated database backup 	|
| Differential database backup 	| Daily 02h00 	| Automated database backup 	|
| Rebuild full text indexes 	| Weekly, Sat 02h00 	| Run rebuild_index.ps 	|
| Purge old Perfmon data 	| Monthly, 1st, 04h00 	| Run purge_perfmon.ps 	|
| Close stale connections 	| Daily, market close 	| Run stale_con.ps 	|
[Example: List of proactive operational activities]

#### Reactive Activities
Reactive operational activities span from simple task changing, the instance count for a role, to an all-hands panic with lots of meetings and finger pointing when a major outing occurs. Reactive activities are always needed, but it is the level of skill, preparation and maturity that makes the difference between a quickly restored application health and those that go into a spiral of failure, bad decisions and problematic fixes.

It is beyond the scope of the operational model to describe all of the practices, procedures and management of an operational centre, as reactive operational activities vary greatly depending on the environment and team.

What the operational model should begin to define is listing out possible events and triggers, with responses for those events. In many cases, health monitoring will pick up a problem and the defined responses make up a subset of the overall reactive operational responses i.e. recovery from a diminished health to good health. See the [table in the Health Model](#HealthModelDiminishedHealth) for an example.

* Triggers — are things that happen, usually faults, which set off a chain of failures. Although there is a tendency to brush off many triggers as unlikely (such as an aeroplane crashing into the data centre) variations of the big ones need to be listed. What if there is a complete Windows Azure outage, can you redirect to another address? What if the database becomes corrupt, can you restore the database and keep some functionality working? When working through the triggers, it is probably better to start off big and unrealistic to set the mood and get them out of the way, before going into detail on smaller, more likely triggers.
* Events — are things that happen, generally an external influence, that cause diminished health to occur and require reaction in order to restore the application health. Examples include increased load due to a marketing campaign, increased size of data that needs to be processed and the addition of a feature that requires increased capacity. While events can be prepped by proactive activities (such as knowing that the sale is going to start), in many cases it may be appropriate to wait until a health indicator shows diminished health before responding.
* Responses — to the events and triggers and where the interesting stuff happens in reactive operations. The operational model needs to identity some of the actions and the detail should be filled in over time by the operational team. The responses can include standard fault detection workflows, escalation procedures, responsibility assignment, case management, diagnosis and prepared planned responses. Responses are also the result of diminished health and are covered within the [Health Model](#HealthModel). 

### <a name="OperationalMaturityLevels">Operational maturity levels</a>
As per the principles for cloud enabled operations, reactive, manual operational activities should be avoided. Reactive operations happen too late and manual operations are error prone. It may be preferable to have everything automated, but generally that will not be the case. Operations should still strive to increase their level of maturity, across the entire application, to a point where reactive, manual activities are rare.

![Operational maturity](./images/Operational-004.png)

#### Level 3 - No reaction needed
The holy grail of cloud operations is that there is no need to do anything; the system takes care of things by itself. From the perspective of a Windows Azure consumer, a lot of this is done for you. If a Windows Azure SQL Database falls over, it is taken out of commission and another takes its place without the application even noticing. Likewise with roles that are always kept at the same instance count by the  Fabric Controller. 

The application needs to do a few things in order to take care of things by itself. The easiest is to ensure that retries are properly implemented, in which case a minor timeout or throttling will not result in an error, and may not be noticed. Others may be more complex, such as taking data from an alternative source, or temporarily and automatically shaping features while load is high.

#### Level 2 - Automated
In an automated operational environment, an operational system (or more than one) monitors the health of the application and automatically takes corrective action. The most common is services (possibly third party services) that monitor load on web servers and increase (or decrease) the instance count in order to take the load. Applications can also manage a degree of automated response by, for example, increasing the number of threads for processing messages in a queue by monitoring the length of the queue.

Automated operations can be built up from electronic operations by taking scripts that are produced over time and hardening them for an automated environment.

#### Level 1 - Electronic
Electronic maturity refers to an operator performing an operational activity (either proactive or reactive) and using a script (such as a PowerShell script). These scripts may take input parameters and perform complex tasks but require that an operator explicitly executes them while monitoring for errors. There is a degree of risk associated with running scripts in this manner. The incorrect script may be run, the parameters may be wrong and errors may not be detected.

#### Level 0 - Manual 
Manual maturity is the lowest level and often results in operational environments where there is continuous panic and lack of predictability in response to diminished health. In this environment the operator, on detecting a problem, starts manually 'pulling levers' to try and get the application working again. Manual operations are error prone and rely on a skilled operator that may not always be available.

The existence of a manual operational environment can be tolerated when the application first goes live, as the operational team may not yet be familiar with the application. 

Over time, manual tasks must move to a higher maturity level.
The table below summarises the maturity levels and shows the only benefit of lower levels of maturity is the reduced initial cost.

[table][Maturity level quick reference]
| 	| Cost to develop 	| Speed of recovery 	| Risk of mistakes 	| Headcount required 	|
| -	| -----------------	| -------------------	| ------------------	| --------------------	|
| No reaction 	| High 	| n/a - no failure 	| None 	| None 	|
| Automated 	| High 	| Quick 	| Low 	| Low 	|
| Electronic 	| Low 	| Medium 	| High 	| High 	|
| Manual 	| None 	| Slow 	| High 	| High - may include dev team 	|
[Maturity level quick reference]

## Summary
The cloud computing benefits of elastic resources, scalability, automatic recovery, and the associated cost reductions can only be realised with a mature operating environment. Not only is the operations of cloud computing applications different from traditional applications, but applications need to be engineered with operations in mind.

Developing the operational model brings operational concerns to the fore early enough in the application lifecycle for them to be actioned. Operators need to familiarise themselves with how to keep cloud computing applications running long before the application goes live. Architects and application designers need to ensure that features are implemented with a view to being operable. The importance of operations in cloud computing applications and the general disregard of operational issues by developers makes the operational model one of the most important CALM models. 

![Operational model summary](./images/Operational-001.png)

### Summary of steps

1. Discuss the principles of enabling cloud operations and create an initial list of features that operations require.
2. Extract elements from other architectural models that are of relevance to operations.
3. Develop the initial view of the application deployment, including details such as accounts, that are important to operations.
4. Identify the health monitoring data and instrumentation needed.
5. Develop an approach for involvement of operations in release management.
6. List proactive operational activities.
7. List possible events, triggers and reactive responses.
8. Create a plan for migrating to a higher level of operational maturity.


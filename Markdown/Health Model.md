# <a name="HealthModel">Health Model</a>
Cloud applications are not monolithic applications that can be considered healthy when you are only monitoring a single metric. Unfortunately cloud platforms are not silver bullets either and cannot automatically adjust to stay in perfect working order. Cloud applications are composed of many loosely coupled and distributed application components and services that, while collectively contribute to the overall health of the application, have individual health behaviours that need to be understood and managed. Well-architected cloud applications are designed for failure and, to a degree, self-recovery or at least tolerance of failure, which masks underlying health issues.

Whilst monitoring every possible metric is useful in debugging and understanding the root cause of problems, health monitoring needs to be more targeted. For example, if a web role puts messages on a queue to be processed by a worker role, the performance counters for both roles may indicate adequate health. But it's the number of messages on the queue that is the best indicator of health. The worker role may be the malfunctioning component, but the immediate recovery solution may be to increase the number of instances in order to get the application to a healthy state as soon as possible.

## Health monitoring
The objective of the health model is to provide a platform for operational monitoring of the health of the application. Referring to the diagram below:

![Health monitoring](./images/Health-001.png)

* The application needs to provide data for instrumentation of health metrics that can be monitored.
* The operational processes (manual or automated) use that instrumentation to continuously monitor the health of the application. 
* Under normal circumstances the output from the monitoring is compared against the SLAs in order to:
	* determine if the application is in a state of diminished health or, 
	* establish any proactive maintenance tasks that can be performed, or
	* provide feedback for application developers on improvements that can be made.
* In the event of an unhealthy application, response and recovery procedures can be followed in order to restore the application to a healthy state.

## Developing the health model
To operate a healthy cloud application, a health model needs to be developed during design that will facilitate the necessary business involvement, developer responsibilities and operational needs. The health model should:

* Define the health states of an application.
* Describe how health will be measured and monitored within the application.
* Provide procedures for the recovery of the application to a healthy state.
* Allow for the use of health data within the entire application lifecycle.

### Defining the health states of an application
In order to monitor the health of the application, we need to define what is meant by healthy, unhealthy, and diminished health of the application.

#### Health Indicators
Knowing that a server is up and running is not a good indication of the health of an application. Some of the metrics being monitored need to make sense to the business and this can be achieved by defining health indicators to be measured. These indicators should be set against the decomposed workloads, so that it can make sense within the application architecture and operational model.

List each workload, and at least one operation against each workload. For each operation establish:

* Health indicators for healthy, diminished health, and unhealthy behaviour.
* A possible technical method to be used to measure health. This helps to establish what it is feasible to measure for the business.

The table below illustrates an example of health indicators captured against workloads.

[table][Example: Health indicators against workloads]
| Workload 	 	| Health Indicator 	||| Technical Measure 	| 
| 	 	| Healthy 	| Diminished Health 	| Unhealthy 	|	|
| :-------- 	 	| :------- 	| :---------------- 	| :----------	|
| Search - Basic	 	| < 3 sec response 	| 3-5 secs response 	| > 5 secs response or error 	| Page load time 	|
| Search - Full Text	 	| < 5 sec response 	| 5-8 secs response 	| > 8 secs response or error 	| Page load time 	|
| Order confirmation	| < 30 secs	| 30 secs-2 mins	| > 2 mins or none sent 	| Approx messages in queue	|
| Sales report	 	| < 1 min	| 1-5 mins	| > 5 mins or timeout 	| sys.dm_exec_query_stats	|
| Load catalogue	 	| 30 items/sec	| 30-80 items/sec	| > 80 items/sec or failure 	| custom perfmon (items/sec)	|
[Example: Health indicators against workloads]

##### Mapping health indicators to the Lifecycle Model
When establishing the health indicators, it is important to establish whether it applies during peak periods or not. Normally one would expect that the application would be designed to be healthy during peak periods, but what about if the load is in excess of what was expected? In some cases, such as batch jobs, peak load may not be relevant, and in other cases, slower processing times (such as order confirmation from the table above) may not be cause for concern. 

It is good to discuss the health indicators in the context of the lifecycle model for the particular workload, and perhaps set varying health indicators based on the expected (or unexpected) workload. The (exaggerated) chart below illustrates how a higher load can result in a higher number of requests being considered unhealthy, which for cost/benefit reasons the business may deem to be 'good enough'.

![Health Lifecycles](./images/Health-002.png)

##### Statistical interpretation of measured data
The SLAs need to consider how the measured data is going to be interpreted. For example, on a web server, recording the measurement of **any** request taking longer than it should is significantly different from the average over a period. While it may seem reasonable to take the worst-case request in a period as a measurement, it does not reflect the behaviour of the system for most of the users. When specifying the health indicators, it is good practice to state the interpretation method. The best place to start would be 90 percentile (90% of the observations must be within the value). 

It is worthwhile mentioning that aggregating the data too much can also lead to meaningless results. For example, aggregating across all instances of a web role may not show an underlying problem with a particular instance.

#### Monitoring the health of dependencies
Most applications have dependencies on systems that are beyond their control, and these need to be monitored as much as the application itself. The monitoring of dependencies is a big part of proactive operational management, as the time needed to restore an external dependency is quite high and detecting diminished health before it impacts customer satisfaction may buy precious recovery minutes.

##### Windows Azure
A Windows Azure application is dependent on the Windows Azure platform. There are a surprising number of individual (and interdependent) services that a typical application is dependent on, and they all need to be monitored, as a degrading or unavailable Windows Azure service will have an impact on the application. Windows Azure has a publicly accessible [service dashboard](http://www.windowsazure.com/en-us/support/service-dashboard/), with service history, to monitor the various services across regions. The [deployment model](#DeploymentModel) should describe which services are used by the application, and need to be monitored.

##### Third-party services
Many applications depend on other services in order to function. Examples include identity providers (used by the access control service), analytics, and content delivery. While in many cases, it may be difficult to monitor thirty-party services directly. A process to diagnose their role in health degradation, and support processes for recovery, needs to be developed.

##### On-premise infrastructure
Enterprise applications that are deployed in Windows Azure may have a dependency on on-premise infrastructure, such as database servers, active directory servers, and even the networking infrastructure used to access on-premise infrastructure.

##### On-premise applications and services
Applications that have a lot of enterprise integration may have a high dependency on on-premise applications and services. These might include on-premise ERP or CRM applications. Those that are accessed using RPC style calls are particularly prone to impacting the health of an application deployed in the cloud.

#### Unhealthy cost
One of the benefits of cloud applications is their ability to scale out and add additional capacity on demand. But what if the demand is the result of an application fault; will additional capacity be wastefully allocated? An application may appear to be in good health against health indicators but is running far too many compute instances, sending too many messages, or using up more storage or bandwidth than initially planned.

The [cost model](#CostModel) should contain details of expected costs against workloads under various load scenarios. The health monitoring should take this into account, and flag the application as unhealthy if it exceeds the expected cost. However, it may be difficult to extract cost data (particularly at a sufficiently granular level), so other mechanisms of estimating actual cost for a workload need to be developed.

At the very least, check the monthly bill as part of the health monitoring process rather than just being part of a financial check.

#### Encapsulation within the SLAs
Once the health indicators, dependencies, and cost measures have been defined they should be put into the SLAs for the application. It is only by capturing the health monitoring metrics in the SLAs that they can be used to actively encourage better operations and changes within the application. It should be possible to copy the table of health indicators and the list of monitored dependencies, with few changes, into the SLAs.

### Collecting monitoring data
Once the health states of the application have been defined, it is up to the development team to implement mechanisms for gathering the required data as input to the instrumentation tools.

#### Differences of cloud applications
Gathering data to use in health monitoring is a well-established practice. Effective techniques that work for the application type (e.g. ASP.NET MVC, web services), the .NET environment and the operating system (Windows Server) should be applied; particularly those metrics that the development or operational teams are familiar with. Developing a new way of monitoring, which introduces risk, should only be considered after careful deliberation. When adapting existing practices to Windows Azure, there are some key differences that should be considered:

* Infrastructure is not static — The latency and throughput between various parts of the underlying infrastructure (e.g. between compute and Windows Azure SQL Database) is variable due to the multi-tenant nature of the platform. For example, where existing data collection strategies may have largely ignored a constant latency, some additional data collection in this area is worthwhile.
* Applications are loosely coupled and asynchronous — Windows Azure encourages functionality to be spread across multiple services that are loosely coupled and interact with asynchronous messaging. This means that there is much more to monitor (multiple services) including the points of interaction (e.g. queues).
* Ephemeral instances mean that log files need to be centralised because individual instances can come and go. Log files that are collected on a machine (such as IIS logs) are expected to disappear with no warning. Local machine logging needs to be avoided and integrated into a mechanism that ships local log files to a central storage location.
* Low level monitoring is not available as the base infrastructure (disk, network, etc.) is invisible to operators and administrators of applications. Teams need to wean themselves off a reliance on low level monitoring and choose higher level monitoring metrics.
* Throttling and retries — Windows Azure places limits on the rate at which services can be called (for example, throttling on Windows Azure SQL Database and Table Storage). Knowing when throttling is taking place is an important addition to health monitoring metrics.
* The API for collecting data has changed — although not a fundamental change (Windows Azure still relies on base trace logging), the addition of the Windows Azure diagnostics API requires learning new features and differences.
* DBAs have less information — Windows Azure SQL Database, as a multi tenant database platform, removes a lot of the monitoring that DBAs are used to on an on-premise database. There is no access to the low level metrics of the database server itself (such as disk subsystem performance counters), SQL Trace cannot be used, and many management views are unavailable.

#### Cost of data collection development
The engineering effort involved in collecting data for health monitoring is often overlooked in task estimates and project plans. Development teams embarking on their first cloud project are advised to make explicit allowances for additional effort to implement health monitoring. For example, while in an on-premise application the IIS log files are not the concern of the development team, some development effort is required to make sure that they are stored in table storage. This is an additional cost to be accommodated.

### Viewing monitoring data
The dashboard for viewing health monitoring data is part of the [operational model](#OperationalModel), but is worth cross-referencing. By being considerate of the operational use of health data, developers can provide more appropriate data collection mechanisms.

#### Types of monitoring use
How data is collected, stored, and the tools used to view it, is dependent on the type of data being collected and the value it has at a point in time. The health model should describe how the data is going to be used in order to meet the health monitoring objectives. The following categories of data monitoring need to be considered:

* Real-time availability — A simple 'traffic light' status of services in order to gauge, at a glance, the general health of those services. While services that are part of the application will often have more detailed views, the monitoring of external dependencies can generally only be viewed as on or off.
* Real-time capacity — A view of the available capacity for a given service. For compute, this may not be relevant as the number of instances available is variable, as with table storage capacity. For other services, this may need monitoring and can include available capacity for transaction throughput and remaining database space.
* Real-time performance counters — Custom performance counters are relatively easy to put into an application, and there are many performance counters available for Windows Server and IIS (web/worker roles). However, the collection, storage and use of performance counters does require some work before it can be used in a monitoring tool.
* Alerts — Alerts are a side effect of real-time monitoring. The availability of alerts is an important consideration because (automated) alerts work better when they have more (structured) data about the event in order to perform a meaningful action.
* Log file analysis — The eventual analysis, aggregation, and storage of log files is one of the most important uses of health data. Often the amount of data produced in real-time is too much to consume and glean any useful information from. Looking for patterns and trends in log files is necessary to understand murky health issues.

#### Monitoring tools and platforms
A tool is required to view collected data and present it to operators.

##### Windows Azure Management Portal
The Windows Azure Management Portal is the primary tool for monitoring the health of applications. Although the detail may not be application specific or fine-grained enough for all health monitoring purposes, it is the only place where information is presented on the exact state of individual instances.

In addition, the management portal also has live graphs on various metrics that can be viewed to monitor the health of an application. While useful, the graphs are displayed per service, and not able to display a higher-level overview, but they are individually customisable, so specific problematic services can be more closely monitored. The addition of live health graphs (viewable on any browser), was a major feature addition of the 'Spring 2012' Windows Azure release, and is expected to increase their functionality over time.

##### Microsoft SCOM
The Microsoft standard for presenting, managing and responding to health information is [Microsoft System Center](http://www.microsoft.com/en-us/server-cloud/system-center/default.aspx) (SCOM). As Microsoft moves Azure into more enterprise environments, and consolidates its private and public cloud operational environment, the product story and roadmap of System Center will continue to improve and evolve. With [System Center 2012 Service Pack 1 CTP2](http://www.microsoft.com/en-us/download/details.aspx?id=30133), 
System Center is able to collect data (performance counters, event logs, IIS logs) and store, manage, and present them as if they were any Windows based service. System Center has the advantage of being able to present cloud based applications within the same operational environment as other on-premise applications within the enterprise.

##### Third-party tools and platforms
Although the Windows Azure Management Portal and Microsoft System Center are the official tools for analysing data for health purposes, other third-party tools exist.

[Cerebrata Azure Diagnostics Manager](http://www.cerebrata.com/products/AzureDiagnosticsManager/) — Coming out of the RedGate stable, the Cerebrata Azure Diagnostics Manager offers an affordable mechanism to import information logged in Windows Azure. It presents it in a rich client that allows event logs, IIS logs, performance counters, and other useful information in an archived or real-time fashion.

[New Relic](http://newrelic.com/) - While not a tool specifically for Windows Azure (or Windows based applications), New Relic offers a low effort option that works on Azure applications (implemented as a profiler that sends data to a service). New Relic is particularly good for web applications, and also offers support for Ruby, PHP and other platforms that may be used within Windows Azure.

### Recovery of unhealthy applications
Applications in a diminished state of health may initially be the responsibility of the operational team, but the sustained health of an application is ultimately the responsibility of the entire team including business, developers and testers. The nature of applications deployed in the cloud is such that the business plan is often based on unpredictable, aggressive load that pushes the application beyond what development, operations and maintenance staff are familiar with. This requires far more inter-team collaboration for recovery and each member has a role to play in the detection of, response to, and recovery from diminished health.

#### Detection
The first point of detection of diminished health should not be the users, as is often the case. It should be the operational staff that are monitoring the application. Although detection is a somewhat passive activity, operators should be armed with the lifecycle models, and have other information on hand (such as notification of a marketing campaign). This allows them to proactively monitor specific parts of the application or determine if apparent diminished health is the result of expected high load.

#### Response
The response to an unhealthy system should not be panic, as that is when mistakes are made. The first question about the response is the impact on the users (or processes underway). If users are seemingly unaffected (such as an indicator that the database will run out of space shortly), then the response will be different to one where there is a complete failure across all workloads.

Although operators want a complete document of what to do in all scenarios, it is virtually impossible to put one in place up-front. However, it is recommended that a placeholder document be created which can be expanded in the future. This will create a knowledge base of valid responses to certain health indicators.

Responses for diminished health across the workloads can be documented in an indicative manner using a similar table to that created for the health indicators. For example:

[table][Example: Impact and responses to diminished health]
| Workload 	| Unhealthy Indicator 	| Impact 	| Response 	|
| ----------	| -----------------------	| --------	| ----------	|
| Search - Basic 	| > 5 secs response or error 	| Serious, catalogue not working. Lost sales	| Raise level 1 incident 	|
| Order confirmation 	| > 2 mins or none sent 	| High, potential lost orders 	| Check order log. If orders lost, raise level 1 otherwise level 2 	|
| Load catalogue 	| > 80 items/sec or failure 	| Low. Out of date catalogue is still valid	| Investigate root cause 	|
[<a name="HealthModelDiminishedHealth">Example: Impact and responses to diminished health</a>]

This can be done across all workloads as well as during different times of the day (or availability of second line support). Note that few health problems detected need to have "Panic and get developers out of bed" as a response.

#### Recovery
If poor health is sustained, where it doesn't stop after automatic recovery or reduced load, or it is recurring, the system needs to be restored to a healthy state. Recovery is not necessarily just an operational function, it may require more effort from other parts of the team (including developers and testers), and take a while to recover.

##### Automated Recovery
The Windows Azure Fabric Controller takes care of a lot of automated responses without us being aware. Suspect instances may be shut down, databases will fail-over, and many other services will take care of themselves.

Under the banner of 'design for failure', there are many patterns which provide a degree of self-recovery within an application, particularly when the poor health is due to a brief load spike. Implementing such self-recovery does have an engineering cost and may not represent value for money, or even be necessary, up-front. Examples include:

* Feature shaping (service degradation) — where certain features are unavailable, or operate in a less resource intensive manner when the application is in poor health.
* Data stubbing — where personalised data (or other specific data) is replaced with a default.
* Eventual consistency — where data can be persisted elsewhere, such as a queue, and processed once the problems are resolved.

##### Immediate Recovery
Some recovery may be quite simple, and be part of the standard operational practice for a large number of poor health indicators. There are a few levers that can be pulled, and it is worthwhile spending some time discussing and documenting some of the 'best guesses' for immediate recovery. Examples include:

* Increasing the number of Azure instances for a role.
* Changing the instance size for a role.
* Changing configuration data (that may turn features off).
* Increasing the size of the cache.

##### Planned Recovery
By far, most of the recovery is not as simple as flicking a switch or waiting for something to magically recover itself. If all goes well, an operational person can quickly run a script or perform a simple action, but often recovery that is not automated or immediate requires input from developers, testers and even business. Actions performed in a planned recovery include:

* Assignment of responsibility for the recovery of the application to good health. This may be the business/product owner not operational staff.
* Assessment of the business impact of declining health in order to understand the urgency of the recovery.
* Diagnosis of the underlying causes for diminished health.
* Formulation and review of a recovery plan for restoring application health. This may include some testing of approaches, scripts and deployments, in order to determine the feasibility of the approach.
* Implementation of an application change, in the case of a defect or a problem that can be addressed in the application.
* Testing of changes.
* Scheduling of changes. Updates may be deployed during off-peak periods to reduce the number of users impacted if the update fails.
* Deployment of updates.
* Testing of deployed changes on the production environment.
* Monitoring of the success of the deployment.
* Rollback of changes in the case of a failed deployment.

The above list requires the involvement of a lot of people over an extended period of time. The recovery of an application to good health is not the sole responsibility of any group, such as operations or developers.

### Health review
Monitoring of health is not just about keeping an application healthy at any given moment; it is also a mechanism to improve the application over time by providing raw data for business, development, and operations. The health model needs to encapsulate how health monitoring data will be used to improve the entire application lifecycle.

#### Measure health against the SLAs
The health indicators are encapsulated within the SLAs and monitored health should be measured and recorded against the SLAs. While there may be a fear of the SLAs being used as a stick to beat operational and development teams with, the flip side is that failure to meet SLAs may not be a quality issue, but indicative of a lack of resources (for development and operations), or usage behaviour beyond what was expected or planned.

#### Review recovery processes
When deploying a new application, operational staff will probably start with a blank sheet of paper in terms of how to respond to poor health. While panicked responses are inevitable, especially at the beginning, it is important that a knowledge base of recoveries (both successful and unsuccessful) be built up. This helps to ensure that future recoveries are faster and cause less disruption.

#### Review health behaviour in order to improve the application
In most cases, the development of a cloud based application will be new to the development team and there is sure to be room for improvement. "It is running slow, make it faster" is a common, but unhelpful, request. The provision of detailed health monitoring information is important for improving the application and provides insight into architectural bottlenecks and underperforming parts of the application, as well as into which workloads require attention (health against business value).

## Summary
The decomposition of modern applications into services, and the infrastructure on which they run, makes monitoring the health of the application complex. The health model addresses this by establishing the SLAs as the primary understanding of what constitutes diminished health. It also encourages the collection of health monitoring data that is used in operational instrumentation. The health model is crucial to building applications that can deliver on the application availability, scalability, and performance promises of the cloud.

### Steps
1. Define the health states of the application using the SLAs.
2. Implement collection of health data in the application and platform.
3. Provide a mechanism to view collected health data.
4. Develop approaches to handle the recovery of a poor health application to a healthy state.
5. Review application health in order to provide feedback into development and operations.


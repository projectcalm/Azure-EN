# <a name="CapacityModel">Capacity Model</a>

One of the most widely understood aspects of cloud computing is the concept of 'infinite capacity', where capacity can be acquired on demand, used as needed, and disposed of when no longer in use. The idea is that consumed capacity runs optimally, without straining available resources or having expensive servers sitting around idle.

The concept of on-demand capacity only works when applications have been specifically architected to take advantage of the available capacity. This is covered extensively in all CALM models. Taking advantage of available capacity requires more than just spinning up a machine; some capacity planning needs to be done. The capacity model addresses this need for planning and architecture related to capacity, in order to be able to make use of this very important feature of the cloud.  

## Capacity planning on the cloud
Traditional on-premise application development requires capacity planning to ensure that the physical infrastructure is available when the application goes live. Of course, since the capacity has to be determined early in the development cycle, it is often a best guess, based on experience and generally overestimated, rather than being an accurate analysis. Traditional applications, where capacity takes a long time to provision, also need to plan for a peak load at some time in the future (6–12 months out, depending on procurement processes and budget cycles). The style of capacity planning needed for traditional applications is completely different to that of cloud applications, and very few of the practices found in on-premise datacentres are transferrable.

Cloud computing applications have the advantage that capacity can be provisioned on demand. This means that estimating medium-term physical infrastructure needs of an application is unnecessary. Indeed, the provisioning of capacity becomes more of an operating cost issue (as covered in the [cost model](#CostModel)), and an operational problem (as a response to diminished health, covered in the [health](#HealthModel) and [operational](#OperationalModel) models).

Capacity planning on the cloud then becomes more of a modelling exercise, where it is not the availability of capacity that needs to be planned, but rather:

* The architectural approaches to consuming that capacity.
* The configuration of services to make optimal use of consumed capacity.
* Determining when capacity is needed.
* The operational triggers and processes required to provision the capacity.
* The de-provisioning of capacity when it is no longer needed. 

Just because capacity can be provisioned on demand, it doesn't mean that an application can use it easily. You can spin up as many instances as you like, but if you are stuck with a struggling, single shared database, no extra capacity will help. On-demand capacity doesn't automatically translate to on-demand availability or scalability. This can be an issue in the following instances:

* Unavailable capacity — while infrequent, there may be times when capacity is simply unavailable. Despite cloud providers' promises of infinite capacity, it is obviously not infinite. Maybe the specific datacentre is running out of capacity itself, or there is a partial outage that prevents the spinning up of new instances. Also, your demand for capacity will frequently be placed on a single datacentre, and whilst there may be capacity elsewhere on the network, it may not be available where you need it.
* Bottlenecks — The provisioning of new compute or virtual machines is relatively easy and not prone to failure. The provisioning of storage, network, database, and other throughput is more problematic. Windows Azure Storage, for example, has limits on the number of transactions that can be processed per second. Yes, you can provision more storage, but how is the application going to work across multiple storage accounts? What if hundreds of storage accounts are provisioned? Compute has the advantage that capacity is provisioned behind a load balancer, but with storage it is not so simple. There is no such thing as a load balancer for SQL databases. The provisioning of capacity needs to take these bottlenecks into consideration.
* Health monitoring — When there are hundreds of instances running, it can be difficult to keep track of what is running well and what isn't. Some instances will be running hot, while others are barely ticking over. Some databases may be timing out frequently, while others are snappy. Where do you start with adding or removing capacity when so much is running, and there are so many interdependencies?
* Costs — The decision to add capacity may be easy in the moment when customers are flooding the site, and the argument for low costs during the 'difficult period' is easy to swallow. It becomes surprisingly difficult to remove capacity, and make the decision to actively slow the application down. There may be so much interdependency between services that removing some capacity has a drastic effect on the availability of some services. Those initial, easy decisions on costs become monthly bills that keep mounting.

## Influence of capacity planning on application development
Because of the promise of on-demand capacity, implementation teams are tempted to leave capacity planning until late in the development cycle, if they get around to it at all. Attention should be placed on capacity planning early on in the development process in order for it to exert the right kind of influence on the architecture and application design.

Capacity planning early in the development lifecycle influences:

* Application architecture — the inability of a component to handle the expected load needs to be identified early. The answer to the question *"Will the addition of capacity solve this performance problem?"*, will influence the architecture. If the answer is "No", then the approach needs to be reworked until the answer is "Yes". This is an important role of the capacity model. In order to solve scaling issues, capacity should be added without additional rework required. For example, with an implementation that uses Windows Azure Queues, if all messages are sent to one queue, then the addition of capacity may not solve scaling problems, because the queue becomes the bottleneck. In this case, the application needs to be able to handle multiple queues, where additional capacity contains a new queue in the scale unit.
* Cost model — the predicted operational costs of the application are related to the capacity provisioned. The outputs from the cost model may be fed back into the architecture when the cost exceeds the value provided by the component. Perhaps the component needs to be optimised for cost, as opposed to throughput or performance.
* Scalability — it may be impossible to determine an application's ability to handle target loads, as simulation may be impossible. The capacity model provides units of capacity that are used to predict the behaviour (performance, cost, etc.) of the application under peak load.
* Workload scheduling and optimisation — the capacity model combines the capacity requirements across all relevant workloads and lifecycles, providing an insight to which workloads should run when, and with how much capacity, in order to optimise cost, operations, or cross-workload contagion.

## Developing the capacity model
The capacity model provides a bridge between application design and application operations. This is what makes development of the model difficult, as application developers are seldom concerned with the operation of applications. Most of the time developers hand over source code and an indication of the target configuration, leaving the operators to figure out the rest. The capacity model brings the operational needs to development, and requires that developers consider how their applications will run and respond to load.

### <a name="ServiceWorkloadIsolation">Isolation of services and workloads</a>
Cloud computing architectural approaches encourage the use of discrete, independent, decoupled and isolated services. This principle is especially relevant in the [availability model](#AvailabilityModel), where isolated services improve availability because they allow for fault isolation, feature shaping, asynchronous processing, and other architectural approaches for availability. One of those approaches is the ability to scale by adding capacity, which is where it touches the capacity model.

Although the use of isolated services is encouraged, it doesn't always work out that way. The most common example is where worker roles are used to process messages from a queue. The number of messages on a queue for particular workloads may be very low. It doesn't make sense to have a worker role (with at least two instances for availability) running constantly in order to process ten messages a day. Application designers then decide to combine a number of workloads into a single worker role, and end up with a 'back-end' worker role, or something similar. This doesn't make sense in the context of isolated services, because a poison message from one workload can bring down another. But it does make sense from a cost optimisation point of view. Things get more complicated where the same code is run on a number of roles, and the messages to be processed are set in the configuration. Similar examples exist with other types of services, such as combining an occasionally used web API (e.g. WCF) with an MVC web application.

This approach of combining workloads and services for practical reasons can complicate the capacity model. Capacity cannot be easily planned where differing workloads, with differing load, scale and availability requirements are combined in the same executable. Unfortunately the situation is largely unavoidable. Even good architecture that allows code to be easily moved around, such as by deploy-time dependency injection, creates planning problems because there is no clearly defined relationship between functions and Windows Azure roles (or virtual machines).

The process of capacity modelling can draw attention to these issues and force architectural decisions about service isolation to be made early in the development process.

### Understanding the role of a scale unit
As per the analysis undertaken in the lifecycle model, we understand that the demand placed on a workload is going to vary over time. At its most basic, we also understand that in order to handle additional demand we need to scale. The preference, particularly in the context of cloud computing, is to scale out rather than up, as attempting to scale up introduces limits as to how far we can scale. We are also generally familiar with scaling out certain parts of the application, particularly web servers, and it is common practice to add a number of web servers behind a load balancer. The big question is, once we have scaled out one component (say the web server) what else needs to be scaled out? What about cache, storage, access control, or any other components that are assembled to handle the workload?

The role of the scale unit is important in being able to group a set of components that, in response to demand, can be treated as a group and scaled together. This allows for improved planning, provisioning and releasing of the resources required to handle the demand.

The example below shows two workloads with distinct, and different, components (Windows Azure services or features). These components are grouped together to form a 'scale unit'.

![Capacity model workloads](./images/Capacity-001.png)

Based on output from the [lifecycle model](#LifecycleModel), you should be able to determine how many of each of the scale units are required for each workload. In this example, 'Workload B' is not very spiky, or under much load, and remains constant at a single unit throughout the day. 'Workload A', on the other hand, is subject to daily load fluctuations and additional scale units are required to handle the load. Those workloads can then be summed, in terms of capacity, for a particular lifecycle. Bear in mind that this example shows only one lifecycle, and there are others which need to be considered, such as the monthly lifecycle.

![Capacity model lifecycles](./images/Capacity-002.png)

Once all workload capacities are overlaid, a clear picture of overall capacity requirements can be painted for all lifecycles. In the example below, further workloads (in grey), are depicted on the same lifecycle graph, giving a total view of consumed capacity.

![Capacity model - all workloads](./images/Capacity-003.png)

### Determine the load and spikiness of the workload
Extensive capacity modelling is only required on workloads that are subject to high loads and/or spiky traffic. For example, an application that has an administrative website accessed occasionally by a handful of people should probably be skipped in the capacity model. Such a workload, that is satisfied by a single web role running at low utilisation, with no chance of needing to scale, can be left as is.

Workloads that are very spiky need careful capacity modelling. A workload that is subject to massive load over regular cycles needs attention, as the ability to add and remove capacity to match the load is important. For example, an application that is for millions of consumers in a single region will need a lot capacity during the day, and virtually none at night. This requires special attention as the application availability is important, but so is the optimisation of costs.

### Determine the platform requirements for each workload
In order to determine the scale unit, the platform requirements for each workload need to be determined. These include the components, resources, consumption, and anything else that is needed in order to be more specific about workload requirements. These requirements should be directly translatable into consumable features on Windows Azure. For example, the instance size needs to be determined as accurately as possible, as it has a direct impact on the cost model and the deployment in terms of configuration (in terms of the configuration of what needs to be deployed).

#### Identify platform components
An initial design needs to be completed on each workload to determine the platform components required to implement the workload. These components will include roles (web, worker, VM etc.), storage, queues, cache, and service bus endpoints; any underlying component on the Windows Azure platform. At this stage, the individual components do not have to be named, designed or described in much detail. Some components may be shared by different workloads, so be careful not to double-count (see [Isolation of services and workloads](#ServiceWorkloadIsolation) above).

#### Estimate the consumption of resources
The size of the components required to satisfy the demand of the workload must be estimated.

##### Instance size
Whilst we endeavour to scale out as much as possible, there is a minimum component size to be considered. For example, whilst a simple web application may be able to run on an 'Extra Small' instance, if there is a need for resource intensive image processing, then a larger instance with more memory may be required.

In the context of Windows Azure, it is the compute instance size that needs to be estimated. Most other Windows Azure components do not have an instance size, and are only sized in terms of consumption. Windows Azure SQL Database has an instance size, but should be considered in the data model, not in the capacity model. See [Considering shared state in the capacity model](#ConsideringSharedState) below.

##### Metered consumption
The consumption of most resources in Windows Azure is on a per unit basis. These can be general, such as bandwidth, or specific to the component, such as Windows Azure Storage transactions. It is important to understand the constituents of metered consumption otherwise there is the risk that consumption will be undercounted. This can be in the context of:

* Side effects — for example, processing a message from the queue will probably read from a queue and then write to another place (say storage) after completion, so there is more than one storage transaction
* Counting all the underlying transactions — On Windows Azure, every single rest call is counted as a transaction, and it may not be clear from the API how many underlying rest calls are being made. For example, reading a message from a queue may result in two transactions (one to read the message and another to delete it).

In many cases, more than one consumption item needs to be considered. For example, writing to table storage will have a storage transaction, and a persistence cost. These can also vary depending on whether or not the data is being consumed inside the Windows Azure datacentre. For example, reading from table storage from a web role will have no cost, but reading the same data from an on-premise application will have an outbound data cost.

##### Counting redundancy
The extra capacity required for redundancy (in order to achieve some fault tolerance) needs to be considered when planning the capacity. Windows Azure roles, for example, may have an instance count of two or higher (required as part of Windows Azure SLA) in order to satisfy availability objectives, even if they both only run at 50% of capacity. In more complex redundancy scenarios, additional components may be added, such as would be required for geographic or off-site redundancy. Depending on the workload, redundancy may not be required at all e.g. a queue reader that has a high latency tolerance and low availability requirements, where a single instance will satisfy the requirements.

##### Component specific consumption
Some components may require that we estimate consumption specific to the component. These should be chosen carefully and only considered if they are seen as architecturally significant. Examples include:

* Cache hits and misses — the number of cache misses may be relevant in estimating the size of a cache.
* Storage partitions — the number of partitions may need to be estimated as they can affect performance.

##### <a name="ConsideringSharedState">Considering shared state in the capacity model</a>
Shared state components should be included in the scale unit for a workload if there is little performance, data consistency or other impacts on, or by, other workloads. Something like messages on a queue should be added to the capacity model or table storage used exclusively for the workload.

A monolithic shared Windows Azure SQL Database should not be included in the scale units of the capacity model, as it does not fit into any particular workload, and the capacity model is workload oriented. Even in the case where a SQL database is for a single workload, it can't scale out and cannot be included in a scale unit.

A distributed, eventually consistent database, such as Cassandra, on the other hand, may be included in the capacity model. When using a distributed database, the database nodes are able to handle a specific load and can simply be added as demand increases. For example, if one database node is required for every ten web roles, both the web roles and the database node can be contained in the scale unit.

The scalability of databases is covered in more detail in the [data model](#DataModel).

### Rationalise to the minimum scale unit for the workload
Scale units should be as small as possible, and should only be able to handle the absolute minimum load. Only by having small enough scale units can the capacity be provisioned and decommissioned depending on the demand.

This rationalisation should consider components that can be scaled out, as well as those that will be single (larger) instances.

### Determine the demand capacity of the scale unit
Once you have small enough scale units, we need to estimate the demand that they can handle, as well as the consumption of resources when they are under load.

The primary input should be a single event that is specific to the workload and make sense in a business context. Examples include:

* For web role, a user performing a general set of tasks (pages).
* For worker role, a message being read from a queue.
* For a service web role, processing particular incoming data.

This single event should then be counted in terms of the number of times it occurs within a given period, preferably the shortest possible period (an hour is the smallest billing period offered by Windows Azure). The period chosen should not be based on expected load during any particular hour, but the load that can be handled by the scale unit.

The load that can be handled by a single scale unit can only be determined by [quantitative performance testing](#QuantitativePerformanceTest), as covered in the test model. It may take a few cycles of development and testing to accurately estimate the capability of a scale unit.

## When to develop the capacity model
The capacity model should be started in the early stages of design. As the design evolves, architectural variations, the accompanying estimates of capacity needs and the components that make up a scale unit will be assessed and documented in the capacity model. Towards the end of development, when more features are complete and performance testing gets underway, the detail in the capacity model will increase and be more accurate. 

## Summary
The result of capacity planning is a set of small, well described and understood units for planning, provisioning and decommissioning capacity. They play an important role in determining the application architecture, and become useful when used within other models, such as the cost model and the deployment model.

The ability to efficiently operate the application is highly dependent on the quality of the capacity model. An incomplete capacity model will make deployment and scaling of the application by operators in a production environment very difficult.

### Steps
1. Determine the load and spikiness of the workload.
2. Determine the platform requirements for each workload.
	1. Identify platform components.
	2. Estimate the consumption of resources.
3. Rationalise to the minimum scale unit for the workload.
4. Determine the demand capacity of the scale unit.


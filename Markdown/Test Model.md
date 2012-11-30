# <a name="TestModel">Test Model</a>
In traditional applications, testing has become a secondary activity that attracts little attention, budgets and skills. This has a lot to do with the maturity of current stacks, where defects are mostly relegated to the web front-end, and caught by 'button pushing testing' rather than rigorous test strategies, plans and execution. Applications have, over time, become more robust as frameworks take care of problem areas and developers embrace test driven development, where quality becomes a developer focus rather than something addressed later in the process. Testing skills reflect this reactive and distanced role, so testers tend to be less technically capable. Many testers are only able to use testing tools, are unable to write code or scripts, and have a tenuous grasp of the technical detail of the applications being tested.

The new approaches, architectures, and implementation of cloud applications create a demand for high quality testing. The blasé approach to testing has no place in a cloud computing project, and of all the teams involved in building the application the testing team may be the one that needs the most attention. Not only does testing have an important role to play in ensuring the quality of the application is high enough, but should also become a core part of the development and operations. Testers will cement their role by finding defects specific to the differences of the cloud, and providing input and data about application behaviour that cannot otherwise be collected.

The process of testing highlights cloud application problems that need to be addressed, as well as presenting opportunities to add value to the application lifecycle that are not apparent in traditional environments. These are discussed below.

## Testing Opportunities

### Testing Early
In traditional environments, there is an intention to test as early as possible on a production environment, but the reality is that simulating production loads on a production platform often gets left too late to be of much value. Testers are left with a development or test platform that is not configured as it would be in production. This creates problems with configuration management, and the platform is not up to the required specification, making some of the tests pointless. The cloud provides the opportunity to turn this around, and enable testing early in the project. The ability to test early can benefit the quality, simply because it enables defects to be detected much earlier. The cloud also offers specific benefits related to having production capacity and configuration in order to simulate exactly how the application is supposed to run.

#### Platform configuration
Since the platform is not configured by hand and needs to be automated (as Windows Azure does for every instance with the Fabric Controller), so the configuration used for test environments will (mostly) be exactly what is intended for production. The OS version will be the same, the database version will be the same, and all libraries that are used in development and testing are the same as would be used in production. Each instance is configured as would be expected, as are the network, storage and other infrastructure components. The network is the same public network as production, so there will be no nasty firewall surprises. Storage and databases will also perform the same way as they would in production, allowing realistic benchmarking to take place.

#### Availability of production resources
A simulated production platform can be spun up as if it were live, immediately, and for very little cost. An application that will have a peak load on launch of, say, a million page views per day, can easily be tested for two million per day without the need to go and buy resources that will sit around idle after the test. Indeed, the platform can be provisioned for as little as an hour, allowing potentially thousands of instances to be spun up, tested and shut down for minimal cost. This is important for testing functionality, scalability, availability, and other aspects of testing that require production platforms.

### Confirmation of architectural assumptions
Because cloud computing is new, a lot of assumptions about how the platform will behave are made during design. In many cases, these assumptions are not even identified or made explicit, but rather the result of an understanding of traditional infrastructure and transferring that knowledge to the cloud. For example, the throughput and latency between an application and its database is seldom considered (provided it is on the local network), and those assumptions about throughput and latency are brought into a Windows Azure environment. Unfortunately, the behaviour of the application in response to database latency and throughput may not be as expected, and this only comes out when doing load tests on a production platform.

Stating the 'confirmation of assumptions' is the positive spin on the function, but the important aspect is the ability to reduce the risk on the project by uncovering unexpected bottlenecks as a result of the platform not behaving as expected, marketed, or documented.

The importance and value of this cannot be understated, as unlike traditional on-premise applications there is little that can be done to improve performance without a lot of rework. On-premise, for example, an underperforming database can have its hardware upgraded (processor, memory, disk I/O, or network) fairly late in the process without having to rework the application. On the cloud it is simply not possible. If an application has a dependency on, say, database throughput being higher than is provided by the underlying platform, there is nothing that can be done to the infrastructure to change it. In which case, the application itself will have to be reworked, sometimes at great cost. Testing and the ability to test on a 'production' platform on the cloud early in the development process help to uncover these potential problems before the cost to rework the application becomes prohibitive.

### Capacity and cost planning
As discussed in the [cost model](#CostModel), costs are estimated up-front, but need to be continuously reassessed during the application lifecycle, both as part of development and post-live operational procedures. Testing plays an important role, as it allows the application to be simulated in a production environment so that the costs can be better understood. This includes:

* Choosing correct instance size — One of the biggest negative influences on costs is developers choosing instance sizes that are bigger than they need to be. Testers can simulate different configurations of instance sizes and the number of instances. These simulations can be run under varying loads (as described in the [lifecycle model](#LifecycleModel)) to determine the optimal configuration. Bear in mind that the effort to perform the simulation may be more than the cost savings, so be careful not to over-test for optimal instance sizes.
* Understanding data costs — Windows Azure can throw up nasty cost surprises for the data egress from the data centre, a number of transactions performed against Azure storage, and other areas that are not traditionally considered. Tests accompanied by the billing details for the subscription over the test period, will go a long way to feeding data and insight into the cost model. This is also a hint to be able to run tests in separate subscriptions so that the costs are better understood and not cluttered by other activities.
* Extrapolation for capacity planning — Tests should be conducted on a scale unit (as defined in the [capacity model](#CapacityModel)), so that it can handle can be determined. This can then be fed into the capacity model to determine how many 'scale units' will be required to handle the loads expected by the [lifecycle model](#LifecycleModel).

## Testing problems
While testing on the cloud does provide opportunities for greater participation and value in the application lifecycle, there are also complexities for testers to deal with that are specifically found in cloud applications. The reasons are similar to those for architects, developers and operators, where different patterns, architectures, development approaches and hosting platforms require that testing approaches need to be adapted to reflect the new way that the applications work.

### New technologies
Any new technology implementation introduces the need to learn, not only how to use the technology, but also how it needs to be tested. Cloud applications throw a bunch of new technologies at testers that they will need to understand in terms of:

* How the technology fits into the overall application architecture.
* The tools required to use the technology, such as different tools to view the output.
* The expected behaviours and possible failure points of the technology.
* The deployment of the technology into various environments.

While the development team will generally assert that the applications developed for Windows Azure are familiar .NET applications, the platform requires that some new technologies and frameworks will be introduced. Specific examples within Windows Azure include:

* Different storage locations for configuration data — no longer in web.config, but at least in the service configuration.
* Different logging frameworks — application logging data is fundamental to understanding the behaviour of applications, and Windows Azure has a specific approach to logging that will be unfamiliar to testers.
* Different data models — where testers may have been used to just looking in SQL Server for data, Windows Azure applications make use of other data storage technologies (in addition to Windows Azure SQL Database) such as table and blob storage. The tools to access these data stores are not as familiar, or user friendly, as SQL. For example, writing queries may be more difficult.

### Complexity
Well-architected cloud applications are more complicated to test. The adoption of asynchronous processing, using message queues, the loose coupling of services, the stateless nature of services, and other architectural principles make things more difficult for testers to understand.  

For example, in traditional applications a tester can fill in some data in the UI, and check the database directly after pressing the button. But, asynchronous applications make this more complicated. Data, in this example, *eventually* gets stored correctly and when this happens is subject to many external influences.

Just looking at asynchronous processes, testers now need to understand:

* The flows of data between services.
* The delay in processing that it is subject to (perhaps even days). 
* The confirmations and changes in state of long-running transactions.
* Where data is stored before it is committed (such as in the queue).
* What happens if there is resource contention?
What happens if data is processed multiple times (idempotence)?
* Whether or not the feature can tolerate dirty reads (as can happen with eventual consistency).
* How errors are handled and communicated back to the user.

The above list is just for one aspect of the increased complexity of cloud applications, and there are many more. Testers were traditionally able to get away with a much simpler understanding of how the application works, but this is not the case for cloud applications. While architects and developers can, and should, be able to describe how the various bits work, and what to look for, it is only with a clear understanding that testers are able to do their job properly. This heightened understanding is particularly required if they are expected to spot potential problems that the developers have not thought of, or catered for, in their code.

### Multiple concurrent versions
Because cloud applications don't run on dedicated hardware, the opportunity exists to run multiple versions of the application in production concurrently. Perhaps they share the same underlying database and route requests to different services depending on the type of user? Maybe the traffic manager is used to route to different regions depending on the source of the request? At the very least, some parts of the application will be deployed in a staging environment, where they can be thoroughly tested, even by end users, before being 'swapped' to production. This can last for minutes or days, depending on the risks involved.

The traditional model of deploying from dev, to test, to UAT, and finally to production (after much paperwork), should not exist in a cloud environment. It may do, at least initially, but as the application, and the operation of the application matures, it will tend to result in a more continuous delivery model. This trend towards rapid release cycles does not mean that testers are able to do less work; after all, the quality still needs to be maintained. Testers are left with more to do; more automation of testing, including spinning up of test platforms, more understanding of deployment processes, including feature shaping (where parts of the application are shut down while a deployment is in progress), and more understanding of the Windows Azure deployment model, including how upgrade domains work.

### Out-of-band releases
Related to the running of multiple concurrent versions, is the idea that when building applications with loosely coupled services any service can be changed at any time (provided the contract remains the same). Windows Azure applications will typically be built out of many different services, and these services will need to be updated and deployed, as demand requires (bug fixing and enhancements). They will typically be deployed in a cycle that is out-of-band with any other services or parts of the application. This means that, at a point in time, multiple services can be in various states of development, deployment and testing. While this is primarily the concern of operators that need to deploy the bits and pieces, testers need to participate in the process. A typical test team may be performing a number of different tests, on different parts of the application, in different environments, all at the same time. There is no consistency in the test cycles, where all testers are performing similar tasks for a set duration where they can report definitively on the overall quality of a release.

### Availability testing
Part of the attraction of cloud platforms is the cost-effective availability. Applications that would normally not be concerned with availability are now considered to have an availability requirement, and where previously an application would not be undertaken due to availability constraints, it can now be developed. This means that many implementation teams will be exposed to availability development and testing for the first time. Two primary questions need to be asked by testers:

#### How do you develop for availability? 
It is important for testers to understand what they are looking for when things go wrong, or cases that developers haven't taken care of. 

For example, if a worker role that is processing messages fails, the message is returned to the queue (with Windows Azure queues; other queues may differ slightly) and is processed again. What happens if a message was partially processed? Was the data committed to the database, or rolled back? If the message gets processed again, will the data be duplicated? 

Other examples relate to the timeout or unavailability of services. Using the 'design for failure' approach, if a dependent service is unavailable, the application should not fail and should gracefully continue. But this will vary on a case by case basis and, in some instances, the service may be so important and reliable that if it doesn't respond the application collapses in a heap, thereby breaking the 'design for failure rule'. So, if a service does not respond, does the application retry immediately, retry after a delay (if possible), try an alternate address, attempt an alternate service, or store the data somewhere so that it can be sorted out later? These are the sorts of questions that testers need to ask developers so that they can develop their test plans. 

Testers should not have to do all of this work themselves and must be involved in the development of the [availability model](#AvailabilityModel), using it as a reference for the test model and test plans.

#### How do you test for availability?
Availability is more than just outright failure of services and includes the responsiveness of the application — which is generally related to its ability to scale (see availability [outcomes](#AvailabilityOutcomes) and [influencers](#AvailabilityInfluencers) in the availability model). So the primary ways to test for availability are to shut down or disconnect services, and to put the application under extreme load to see if it scales well.

##### <a name="ChaosMonkey">Chaos monkey</a>
The shutting down of services is often referred to using the term 'Chaos Monkey', which refers to a [technique developed by Netflix](http://techblog.netflix.com/2011/07/netflix-simian-army.html) to test availability. They developed 'Chaos Monkey' scripts that will **randomly** and indiscriminately **disable** **production** instances. While haphazardly disabling production instances should not be at the forefront of any test plan, the principle remains valid. One of the best ways to test availability is to shut things down and see what happens. This may be easier said than done as it's not that easy to shut down external services (but may be possible to change the password), but it should be relatively easy to find a way to break something on purpose.

##### Testing response to increasing load
Windows Azure applications should be designed to scale, but not necessarily to auto-scale. Testing the application under load should uncover when capacity negatively influences availability (load is too high for current capacity and the responsiveness suffers) as well as the triggers necessary for operations to respond to degraded health (refer to the [operational maturity levels](#OperationalMaturityLevels) in the operational model). As most experienced testers will be aware, testing under high load uncovers bottlenecks in unexpected places. With cloud platforms, those unexpected places are even more remote or obscure.

### Testing at scale
As with availability, cloud platforms also offer implementation teams the ability to handle massive scale that would previously have been ignored. Adding social media and free options to applications suddenly exposes the application to an unprecedented load. How do you test an application that is intended to handle hundreds of thousands of page impressions *per second*? Well understood testing tools cannot cope with these volumes, as they are not able to scale themselves, and the reporting of test results becomes a problem at scale as the amount of data generated becomes a bottleneck itself.

As much as we would like to think that the solution to testing at scale is to use the cloud to perform the testing, by throwing unlimited capacity at it, it is simply not possible to test at scale without the tools, experience and methodologies. In most cases, the solution is to test _scale units_ as a basis for understanding the behaviour with fixed capacities, and outsource a large part of the testing to organisations with the capabilities. This is discussed in more detail in [scale test](#ScaleTest) below.

### Multi-tenant variability
When an application runs on the cloud, it is not alone. It sits on shared servers and communicates across a network shared by thousands of other applications. While Windows Azure does its best to ensure that the multiple tenants running on its platform play nicely together, occasionally there may be 'noisy neighbours'. It could happen that one of the physical machines on which an application is running is subject to degraded performance because other applications on the particular physical machine happen to be under load simultaneously. While this shouldn't affect available memory, and may not affect the processor (as there is a dedicated core), it could degrade the network throughput if all of the roles are trying to communicate at the same time. The Fabric Controller will do its best to ensure that this doesn't happen, by letting noisy neighbours run on their own, but it can, and does, occur. It is particularly common with Windows Azure SQL Database, where performance can vary wildly depending on what other databases on the same machine are up to.

While this is an architectural problem that needs to be catered for, the problem presented to testers is variations of performance with no apparent pattern. Having a successful performance test may not be good enough, because it just happens to be done at a time where other tenants on Windows Azure are not under load. Tests need to be conducted at different times in order to allow for variation and particularly when all other applications may be under load, such as during month ends or early mornings.

### Guest OS Upgrades
Windows Azure roles, which run on top of Windows Server, are upgraded from time to time (at least monthly) and, if so configured, automatically. Although it can be limited, it is good practice to ensure that the applications are always running on the latest version of the guest OS. It is more of a concern to operations when upgrades are due (or occur), but can become something that testers become involved in if the application behaves erratically after a guest OS upgrade. If operators are risk averse and choose to manually upgrade the guest OS, they will probably want testers to perform tests before the upgrade takes place. See [Managing Upgrades to the Windows Azure Guest OS](http://msdn.microsoft.com/en-us/library/windowsazure/ff729422.aspx) for more information.

### Lack of developer skills
As can be expected with cloud development being different, developers may not have much experience with the tools, approaches, and patterns that they are using. While it is the responsibility of team leads and senior developers to ensure that a lack of skills doesn't mean an increase in mistakes, there will be examples that either senior people themselves are not aware of, or cases that simply fall through peer review cracks. It then becomes the responsibility of testers to pick up any poor implementation due to lack of skills on behalf of developers.

In many cases, the problems will occur in places that testers have previously glossed over because they are in areas they are not used to seeing faults in. It then becomes difficult to create tests for application areas that, traditionally, have been considered unnecessary to test. For example, saving data to a SQL database may be something that is so familiar to developers that they never make mistakes and as a result over time testers pay less and less attention because faults are never found. If the developers are introduced to new data storage technology, such as Windows Azure Table Storage, new problems could start to appear in previously reliable operations. Testers need to have enough tests to handle this apparently regressive behaviour.

Testers should work with senior developers and architects to identify any new technology being used, and in areas that developers may be unfamiliar with, so that sufficient test coverage can be implemented.

### Tester skills
In many cases, testers lack the basic skills to be able to test cloud applications fully. Traditional testing approaches have focussed on 'button pushing' testing using automated browser-oriented test tools, and possibly some SQL querying. The nature of cloud platforms is that they are fundamentally self-service, and this extends to testers who need to provision capacity, configure and deploy for testing purposes. It is no longer good enough to wait for a physical test platform to be setup by the datacentre operations team, as testers need to be able to provision and deploy themselves. In order to do this, as well as analyse logs and data (which may not be conveniently in a SQL database), testers need to develop some basic script programming skills.

Scripts are required to automate provisioning, deployment, configuration, and possibly analysis. It virtually doesn't matter which language is used. While PowerShell may be dominant in a Windows Azure environment, the SDKs and REST interfaces can be developed against using PHP or even Ruby. Testers without some script writing skills will struggle in a cloud environment.

## Test plan content
Cloud computing obviously does not negate the need for proper testing, and while there are differences tester should take their existing skills, approaches and experiences, and adjust them for a cloud application.

The points discussed below as content for the test plan should not be taken as the complete and final list for what is a specialised and complex discipline. They are simply to highlight areas that are of specific relevance to cloud applications.

Tests should continue as per their normal processes to ensure application quality and produce familiar documentation. This includes test plans, test strategies, test cases, test script or any other relevant artefacts. They then need to consider the adaptation of existing approaches with items highlighted below, in order to enhance their processes to make them cloud capable.  

### Role of testing and testers
As discussed above, the role of testers within the application lifecycle can, and should, change. Testers can add value much earlier during the design process, and are crucial for effective operations once an application is in production. The first step in extracting value from testers is to discuss and understand their role within the team and how it relates to the cloud application.

Items for discussion and inclusion include:

* Will testers help architects and application designers test the platform by running extensive tests on technical spikes?
* Will the testers run full batches of performance and load tests on the application at the end of every sprint?
* Are testers expected to deploy to Windows Azure themselves, out of the build environment, or are they expected to develop their own deployment scripts?
* How much will testers be included in senior developer design discussions, so that they can understand the complexities of what is being implemented?
* How involved will testers be in developing the health monitoring metrics destined for use by operations?
* How are testers involved in feeding data back to the cost model?

### Test schedule
Fundamental to every test plan is the test schedule which should reflect existing approaches that the test team is familiar with, particularly for functional tests. In addition, the schedule should highlight tests that are either unique to cloud applications, or are scheduled differently with cloud applications, such as:

* Performance and load tests on the 'production' platform early in the development cycle.
* Tests on staging platform, when in-place upgrade deployments are done.
* Tests that need to be conducted at varying times, in order to remove the influences of multi-tenancy.
* Automated tests that run for long periods (to pick up memory leaks or similar) due to the availability of capacity.

### Functional Tests
Applications that run in the cloud are no different in terms of functionality to traditional applications. There may be opportunities for different functionality as advantages are eked out of the platform, and there may be subtle differences that suit the architecture, such as feature shaping (the turning off of features subject to load). But functionality is determined by the requirements and the need, not the cloud platform. So functional tests developed for cloud applications are no different to functional tests for a traditional application.

### Qualitative Performance Test
Before subjecting an application to a high load test, it is necessary to determine if the performance of features is adequate when isolated from performance degradation caused by contention for resources.

The qualitative performance test ensures that individual features perform adequately on the target platform when the application is not under load. Qualitative tests are, therefore, run in a single user mode.

#### Objectives of the qualitative performance test

##### Identify initial bottlenecks
Provided sufficient logging exists, or a detailed logging framework that is able to extract interfaces is in place, the qualitative performance test will be able to pick up performance bottlenecks. Testers should have a view on all of the operations in the workload, particularly those that are subject to network latency, network throughput and (virtual) disk I/O. This will allow them to understand if the underperformance is infrastructure related, in which case it may need to be worked around, or is a more fundamental defect in the application that is causing the bottleneck.

##### Diagnose issues
By knowing the detail of the tests, the precise deployment and configuration used, and the initial dataset, testers are in the position to diagnose the root cause of performance issues. They can also change configurations quickly on the 'production' environment, to confirm or rule out suspicions.

##### Identify performance related defects
Some performance issues are clear defects that need to be addressed. For example, testing may uncover that for a particular operation a cache is not used and that the database is put under unnecessary high load (a frequent culprit). Testers should use the qualitative performance test to identify such defects, and are in the position to correlate expected load against underperforming processes, in order to focus on those areas where the defects have the most significant impact.

##### Provide input for optimisation
The optimisation of performance issues can only be planned if sufficient data exists on the gains that will be achieved, against the cost of coding the optimisations. Data gathered by testers during the qualitative performance tests forms the basis for planning of performance optimisations. Developers may be keen to cherry pick and optimise a particular part of the application that interests them, but it needs to be supported by test data as being relevant.

#### Performing qualitative tests on Windows Azure
Some points to bear in mind when performing the qualitative performance test on Windows Azure are detailed below.

##### Deployment
No dedicated test platform exists in cloud projects, and testers can deploy however much capacity they need, whenever they want to. With this power comes the responsibility to make efficient use of the platform, and the responsibility to take care of things by themselves. Testers should not rely on operations or developers, and perform tasks on their own, such as: 

* Testers need to be familiar with the Azure management portal, configuration and deployment of roles, and possibly be able to deploy out of the build environment.
* Testers may need to create their own deployment scripts.
* Testing may need to run in a separate subscription (account), so that billing data can be meaningfully extracted.
* Testers need to make sure that roles are shut down, databases and other objects deleted once the tests are complete; otherwise the costs may be significant. 

##### Multiple workloads as part of a single feature
Architects of cloud applications will endeavour to decompose workloads as much as possible (see [Workload Model](#WorkloadModel)), and the result is that a feature may be split across multiple workloads. For example, the 'Submit' of a form may not save data to a database straight away, but will add the data to a queue, and have a separate workload that saves it to the database. This means that a functional test that describes, for example, 'Save data to database on submit', spans multiple workloads, and is virtually impossible to reliably test as a single test case. It is particularly difficult to understand the performance issues and bottlenecks when multiple workloads are used, as the application, by design, introduces a processing delay, which is not necessarily a performance problem. On the other hand, the testers should be familiar with the requirement and test in the context of the requirement. Just because developers have chosen to split functionality across workloads, it doesn't mean that it is adequate to satisfy the requirements, and the processing delay may turn out to be unacceptable.

##### Logging
Logging is very important in a cloud application, and forms the basis for health monitoring (see the [Health Model](#HealthModel), so should be included by default for testers to use. Particularly when performing the qualitative performance tests, testers need to make use of the logging infrastructure, as direct access to the underlying infrastructure doesn't really exist and logs need to be used. Some aspects to consider include:

* Turning on logging — Logging can be very resource intensive, and full diagnostic trace logging will almost surely be turned off for production use. Testers need to understand the difference, and use of, perpetual logs versus diagnostic logging. Diagnostic logging can be used in cases where the root cause of defects needs to be determined.
* What to log - Logging for health monitoring and other operational needs may differ from logging that is useful in testing. Testers need to be clear on what they need to log, such as latency for service calls, and influence developers to put logging in at the correct places in the code to facilitate testing.
* Viewing logs - Logs are not stored on individual instances (as the machines may cease to exist, taking their logs with them), but rather in Azure Table storage, or some other data store (depending on the logging framework). Tools that testers may be familiar with for viewing logs will probably not work, and they will have to pick up or develop alternatives.

### <a name="QuantitativePerformanceTest">Quantitative Performance Test</a>
The idea of the quantitative performance test is to quantify the load that can be handled by a specific scale unit (as defined in the [Capacity Model](#CapacityModel)). When building scalable architectures, it is necessary to define a scale unit that describes the roles, instance sizes, data storage, and other aspects that and the demand that a scale unit can handle.

For example, say the architect has initially defined a scale unit to consist of web roles to handle web traffic, worker roles to process the data, and queues to handle the data between the two. There will be an optimal ratio of web roles to worker roles in order to handle the demand, and also at some point the queue is going to be throttled as the amount of data processed increases. Testers need to assist in establishing the load that can be handled by this scale unit (in terms of page impressions, number of concurrent users, or some other metric), the number of web and worker roles, as well as the instance sizes.

Testers may think it unnecessary to spend time establishing the capacity of scale unit in such detail, after all, with unlimited capacity, it makes sense you would just throw what you need at it. But this becomes critical as part of the overall test strategy, particularly when performance and scale tests are conducted. Testers need to be able to communicate using a base metric, otherwise there will always be arguments about the particular deployment and configurations used to render particular test results.

Testers should:

1. Assist in baselining the scale units for particular workloads or features.
2. Develop deployment scripts that deploy the exact scale unit for testing.
3. Perform a load test against the scale unit to baseline the capacity that a scale unit can handle.
4. Tweak configurations, ratios of roles, and instance sizes to help find the optimal configurations of scale units.
5. Report **all** test results in the context of the scale unit used for the test.

### <a name="ScaleTest">Scale Test</a>
How do you test for massive scalability? Even if you can provision the resources for the application, how do you build out a test network of millions of machines around the world? There are two ways to test scalability; use scale units and extrapolate the ability of the application to scale and/or use a third party testing organisation that has the capacity to perform the tests.

The scale test is the ultimate performance test for an application and, thanks to the cloud platform, can be performed a lot easier than with traditional applications. The scale tests intention is to simulate production load across workloads, features, time of day, durations and geographies. The scale test should be performed on the production platform, which should be relatively easy on the cloud, with production configurations, instances, instance sizes, data storage, health monitoring, and so on.

Building applications that need to scale massively, shows up any weaknesses because they are amplified at scale. The weaknesses extend beyond the application to the people and processes within the implementation teams due to their inability to cope with the stress, the lack of time to prep large applications for load, and the time taken to conduct root cause analysis when thousands of machines are running. Testers are not excluded from the pressure and are encouraged, if scale tests are necessary, to understand in detail how they are going to test at scale, develop their processes, test their approaches, and generally make sure that they know exactly what needs to be done when the time for scale tests starts.

While scale tests are important for systems that need to scale, it is necessary to understand how imperative they are and whether or not the implementation team has the skills and capacity to perform scale tests themselves. Applications that need to scale because the business founders think that their idea is cool and will go viral, differ greatly from an established brand that is going to be under massive load from launch. Scale tests need to be planned and conducted according to the realistic business need. In some cases, it may not be feasible to do too much scale testing (rather spend the money on features to attract users). In other cases, it may be preferable to outsource scale testing, at least for the front-end simulation, to a third party.

Additional considerations when testing at scale, specifically on a cloud platform, include:

* The data logging needs to be carefully considered. The amount of data that can be generated by millions of users can both clog up the logging systems (degrading performance), and be so vast that any meaningful analysis will be virtually impossible.
* The chosen datacentre may not have sufficient capacity for massive scale tests, and someone from Microsoft may call up to ask what on earth you are doing. It may be useful to engage with Microsoft before running massive scale tests.
* The scripting of provisioning and, importantly, deprovisioning needs to be properly figured out before scale tests begin. If deprovisioning is not done, you may find that thousands of instances are running and costing money, whilst waiting for a test to run that isn't ready yet.
* At scale things take time. Initialising a database with terabytes of data may take a long time to upload. Similarly with bringing test results and logs down for analysis. Even the spinning up of thousands of instances may take longer than expected.
* Scale tests will not happen often, and it is a good idea to have the application run by operations as a practice run for their processes; letting them try and respond to increased demand as they would in a live scenario.

### Availability Test
The availability [outcomes](#AvailabilityOutcomes) and [influencers](#AvailabilityInfluencers) in the availability model infer that, from a testing perspective, most availability is tested by performing a good functional test, performance soak and other tests. It is the overall quality and responsiveness of the application under varying loads that ultimately determines its availability.

Tests for availability don't need to be as broad as one would think because other tests take care of it to a large degree. Remaining tests will target unplanned partial or complete failure, and the adherence to the 'design for failure' principles that are implemented by developers. This means that the availability tests are largely about shutting down services (breaking something) to see what else would break.

When developing availability tests:

* Go back to the requirements and design, to ensure that the failure of a service has been catered for. In some cases, architectural assumptions are made about the availability of a service, such as Windows Azure Storage, that is sufficiently high that there is little point taking on the cost of developing fault tolerance for the service. In other cases, there may be such a high dependency on a service, such as a credit card payment gateway, that there is simply no workaround other than a polite error message.
* Try and find ways to terminate services randomly. Terminating services is actually quite difficult on Windows Azure, as the Fabric Controller will do its best to try and keep things running.
* Try options of expired logins for services, as this can be a common fault that affects availability.
* Timeouts and latency can be the Achilles heel of distributed computing, so any tests that simulate timeouts and long delays will show up availability problems.
* Recoverability is part of availability and can be tested in conjunction with operational tests. What would happen, for example, if data storage was completely unavailable in a particular region? Is there a plan to get a basic site running, or stand up alternative storage? What would operations do in these cases?

### Soak Test
A soak test is a long-running test that is used to establish if there are any defects that are only presented when an application has been running (preferably under load) for an extended period of time. Soak tests are frequently used to detect defects such as memory leaks and with cloud applications can also be useful to collect data about the underlying platform over an extended period of time (as it may vary due to multi-tenancy). The cloud makes it easy to run soak tests, since they can run on isolated and available capacity for long periods, even months.

### Operational Test
There is a data centre adage that says something like "A backup is not a backup until it has been restored", illustrating the need to test basic operations. No test strategy or plan would be complete without ensuring that operational tests are performed. Again, there are some specific focus areas for cloud applications when performing operational tests:

* Health monitoring needs to be tested. The mechanisms that collect, aggregate, and view health data need to be included in test coverage. The accuracy, timing and usability of health metrics need to be given the same attention as any other feature as much of it will be new, unfamiliar, and hand-rolled.
* Operational processes need to be tested. The response of operations needs to be tested, particularly those operations that relate to availability. Can operations detect diminished health and respond accordingly in either a manual or automated manner?
* Storage, archiving, and restore of backups needs to be tested. Operations unfamiliar with the cloud will be used to accessing extensive and fast local storage for backups. Backups may be more difficult to manage on the cloud because they are not on the local network, or they may take too long to upload to the Azure datacentre.

### Penetration and security tests
There is valid concern around cloud security, not just because of the increased attack surface, but because the security models may be unfamiliar to the implementation team. Testers have a responsibility to ensure that the security related tests are rigorous and beyond reproach. Specific aspects that need to be considered include:

* Security of storage keys, passwords, and other logins that are stored as part of the configuration.
* Penetration testing across the entire application extending beyond the obvious endpoints (such as web and service endpoints), and including the less obvious, Azure specific ones, such storage (which has a public endpoint), messaging subsystems, and logs.
* Testing of encryption and access to data, including cloud based backups.
* Testing of exposure of on-premise endpoints that have been opened up to integrate with cloud based services.

The testing team and the security team need to work closely together and establish who is responsible for what. The security team may be responsible for threat modelling, security practices and governance, while testers are responsible for making sure that it has been successfully implemented, by developing tests that specifically try to penetrate the defences.

## Summary
Cloud applications offer opportunities for implementation teams to re-engage with testers and create a methodology that makes maximum use of testing skills, processes and disciplines. This is important to the entire team, not just because the cloud has risks that need to be managed through testing, but because the cloud offers the ability to test very early in the project on a production platform. This opportunity to test early, and with high coverage, should not be squandered.

### Steps

1. Familiarise testers with cloud principles and how they apply to testing.
2. Include cloud specific aspects in the test plan
	1. Test schedule.
	2. Qualitative Performance Test.
	3. Quantitative Performance Test.
	4. Scale Test.
	5. Availability Test.
	6. Soak Test.
	7. Operational Test.
	8. Penetration and security tests.


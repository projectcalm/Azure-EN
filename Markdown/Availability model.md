# <a name="AvailabilityModel">Availability Model</a>
The demand for high availability is a driving force behind the architecture for modern applications. Users and consumers are always connected using a multitude of devices and expect all applications to behave the same way as the large scale social media applications they consume on a minute by minute basis. Increased connectivity and discoverability of applications means that demand can spike unexpectedly, but has to be handled. With the income per user so low (or completely non-existent), architects cannot throw money at the problem and need to implement innovative ways to achieve high levels of availability on the cheap.

Cloud computing is a popular base solution for high availability for most applications. While the top 1% of applications may build their own datacentres economically, the cloud provides a platform for availability that is more accessible and as affordable as the rest. Cloud computing is no availability silver bullet and there are two catches to getting the most out of availability in the cloud. Firstly, cloud platforms are based on commodity infrastructure and availability is provided by architecture in the application. This means that application architects and developers need a better understanding of how to implement availability, as getting the infrastructure to go faster is not an option. Secondly, most organisations have little experience in availability and skills, neither amongst their own staff nor in the market. This extends from planning, to development, to testing, to operations — where the business need for availability cannot be easily matched by delivery capability.

The simple requirement for an application to work when needed and perform adequately makes availability the most important non-functional requirement. This, in turn, means that availability needs a lot of attention and, when cloud computing is chosen because of its perceived availability characteristics, requires that specific attention be given to the availability model. The availability model process is actually fairly simple; develop the SLAs, establish the business rationale for spending on availability, and choose the appropriate technical approaches. Yet these simple steps mask a depth of necessary understanding. What is needed is a clear brief from the business detailing their realistic requirements against the implementation costs, a well-defined understanding of the required effort, other related costs, and the risk of technical approaches to availability.

As a result, developing the availability model is an extensive process that requires a degree of depth that can be glossed over in some of the other CALM models.

## The cloud computing availability myth
When cloud computing platforms are discussed, availability is one of the hot topics. On the one hand, there are those that see cloud computing as a mechanism to solve availability issues by handing responsibility to the cloud providers with the scale, skills and funds to do it well. On the other hand, there are those that point to frequent and significant cloud outages (whether they are more frequent or significant than non-cloud outages is a different matter) to illustrate that cloud platforms are not the availability panacea that is promised. Wrapped up in this are the businesses that are building applications on the cloud that, by the very nature of being modern applications, must deliver the rapid scale that is promised by cloud computing.

Despite outages, the myth persists that cloud applications have higher availability than traditional applications. There is, of course, an element of truth to this perception. Looking at Windows Azure compute, for $30 per month you can have two extra small instances (two are required for the availability SLA) that run with an availability guarantee of 99.95%. There is no way to self-host and get that kind of availability for that price. Likewise with databases ($5 per month for 99.9%), storage and any other services. Despite this guarantee, it is both possible and highly probable that a developer will build an application so badly that it is always falling over, crashing because of errors, deleting data by mistake and is generally not as available as the underlying infrastructure.

The most important and generally overlooked aspect of building highly available applications on cloud platforms is that the availability guarantee is relevant only to the infrastructure. The availability of the application is, from the perspective of the cloud provider, someone else's problem. That problem then becomes yours, where you are provided with a reasonably highly available platform and challenged to build an application on top of it that satisfies the availability requirements of the business.

![Availability of the entire stack](./images/Availability-001.png)

## Availability empowerment of cloud platforms
It may be disheartening to discover that cloud platforms only provide availability for some part of the application. It is also difficult to break the news softly to a business that has been convinced by the (narrow) promise of availability. But it is not all bad news; the availability of infrastructure offered by cloud platforms can be significant in terms of the overall application lifecycle and costs to the business.

The degree of significance depends on the amount of availability needed down the stack. For example, an application that relies on the database for availability would get a big chunk of availability, for little cost and effort from Windows Azure SQL Database. Applications that have a high dependency on external services would not benefit much from the availability offered by a cloud platform.

> When comparing PaaS cloud platforms with IaaS platforms, the benefits offered by having the availability at the platform level of the stack, are worth considering. Windows Azure Compute availability for the platform includes the infrastructure below it, and is why the availability assumes more than one instance is running. Amazon Web Services EC2 availability is for the infrastructure (underlying VM, OS and networking) and does not cover any part of the application that runs on it (Elastic Beanstalk, the AWS application platform, doesn't seem to have a very clear SLA).

Ultimately the benefit of the availability of the underlying platform is the ability to use this as an affordable building block for the rest of the application. While the overall application availability is still the responsibility of the development team, a large part of it is provided for 'free' (in a sense that there is no development cost or other capital expenditure cost associated with it). This means that an application of reasonable availability can be implemented a lot cheaper than a self-hosted solution. At some point this cost advantage is lost up to the point where the availability exceeds the 99.95% guarantee, where the only option on Windows Azure is a geo-distributed architecture with additional cost considerations.

The diagram below illustrates that self-hosted applications can provide higher availability than public cloud platforms, but only at the high end of availability, where costs are prohibitive, whereas Windows Azure provides higher availability for a lower cost at the lower end of the availability scale.

![Cost of providing availability](./images/Availability-002.png)

How these costs are incurred also differs on cloud platforms and again depends on the services used, how much availability has to be built into the application layer of the stack, and how much load the application will be subject to. Although, as described earlier, it is possible to achieve relatively high availability for extremely low cost on very small applications, for larger applications the underlying nature of the platform may mean that more is spent on development for availability than traditional applications. 

With a database, for example, if we disregard the cost of the actual database itself (hardware, licenses and storage) it is cheaper to develop for an on-premise database than a cloud one, as certain assumptions can be made. The on-premise database may have high availability, sufficient bandwidth and be reasonably fast, where simple database code can be written against it. On the same sort of database in the cloud, more code has to be written to handle throttling, latency and other issues endemic to a cloud database. 

While for a particular application (or part of an application) the development cost for availability may be higher, the on-going operational costs may be lower. A platform that has a lower on-going maintenance cost (OS patching, backup, configuration) or has a better model for performing tests, may prove to have a better cost benefit over the full application lifecycle. If the on-going costs associated with availability are largely in the platform and infrastructure layers of the stack, the cloud platform may work out more cost effective over the long run because those costs are largely taken care of.

The charts below illustrate that on the cloud the development costs consume most of the budget, as the upfront platform costs are minimal. However, the pay-per-use model of cloud computing means that running applications will spend more on the platform than the maintenance of the application.

![Availability costs over full application lifecycle](./images/Availability-003.png)

## What is availability?
Availability is a term that is so widely used in different contexts that it is very difficult to define in a way that satisfies all audiences. At its most basic, availability is the ability of a system to provide the expected functionality to users. This means that the application needs to be responsive (not frustrating users by taking too long to respond), and reliably able to perform those functions. But that is not enough to understand the full story about availability.

Availability is simplistically viewed as binary; the application is either available at a point in time, or it is not. This leads to a misunderstanding of availability targets (the 'nines of availability'), the approaches to improving availability and the ability of salespeople to sell availability snake oil off-the-shelf (see 100% availability offered by Rackspace). 
Application availability is **influenced** by something and has a visible **outcome** for the consumer, as discussed below.

### <a name="AvailabilityOutcomes">Availability outcomes</a>
The outcome, or end result, of availability is more than just 'the site is down'. What does 'down' mean? Is it really 'down' or is that just the (possibly valid) opinion of a frustrated user? A user that, for example, is trying to capture a claim after arriving at work late because they crashed their car, is bound to be frustrated enough to deem a slow web application to be 'down'. The outcomes of availability are those behaviours that are perceived by the end users, as described below.

#### Failure
The obvious visible indication of an unavailable application is one that informs the end user that something has failed, and no amount of retrying will make it work. The phrase 'is down' is commonly used to describe this situation, which is an obvious statement about the user's perception and understanding of the term 'down', rather than a reasonable indication of failure. The types of failure include:

* Errors — where the application consistently gives errors. This is often seen on web applications where the chrome works but the content has an error, or garbage.
* Timeouts — an application that takes too long to respond may be seen as being 'down' by the user or even the browser or service calling it.
* Missing resources — a '404 - Not found' response code can have devastating effects on applications. Beyond missing image placeholders, missing scripts or style sheets can 'down' an application.
* Not addressable — a DNS lookup error, a 'destination host unreachable' error and other network errors can create the perception that an application is unavailable regardless of its addressability from other points. This is particularly common for applications that don't use http ports and network traffic is refused by firewalls.

#### Responsiveness
While it may be easy to determine that a switched off application is unavailable, what about one that performs badly? If, for example, a user executes a search and it takes a minute to respond, would the user consider the application to be available? Would the operators share the same view? Apdex ([Application Performance Index](http://apdex.org/))  incorporates this concept and has an index that classifies application responsiveness into three categories, namely: Satisfied, Tolerating, and Frustrated. This can form a basis for developing a performance metric that can be understood, and also to acknowledge that in some cases we will experience degraded performance, but should not have too many frustrated users for long or critical periods.

#### Reliability
In addition to features being snappy and responsive, users also expect that features can be used when they are needed and perform the actions that they expect. If, for example, an update on a social media platform posts immediately (it is responsive), but is not available for friends to see within a reasonable time, it may be considered unreliable.

### <a name="AvailabilityInfluencers">Availability influencers</a>
While the availability outcomes receive attention, simply focussing on these by saying "Don't let the application go down", fails to direct effort and energy to the parts of the application that ultimately influence availability. Some of these availability influencers are discussed below.

#### Quality
The most important, and often unconsidered, influence on availability is the quality of the underlying components of the system. Beyond buggy (or not) code, there is the quality of the network (including the user's own device), the quality of the architecture, testing, development and operational processes, data and many others. Applications that have a high level of quality, across all aspects of the system, will have higher availability without it being specifically addressed. An application hosted in a cheap datacentre, with a jumble of cheap hardware, running a website off a single php script, thrown together by copying and pasting from forums, by a part time student developer, will guarantee low availability.

#### Fault tolerance
Considering that any system is going to have failures at some point, the degree to which an application can handle faults determines its availability. For example, an application that handles database faults by failing over to another data source and retrying will be more available than one that reports an error.

#### Scalability
If a frustratingly slow and unresponsive application can be considered to be unavailable (not responsive or reliable) and this responsiveness is due to high load on the application, then the ability to scale is an important part of keeping an application available. For example, a web server that is under such high load that it takes 20 seconds to return a result (unavailable) may be easily addressed by adding a bunch of web servers.

#### Maintainability
If a fault occurs and an application needs to be fixed, the time to recovery is an important part of availability. The maintainability of the application, primarily the code base, is a big part of the time that it takes to find, fix, test and redeploy a fixed defect. For example, applications that have no unit tests and large chunks of code that have not been touched in years wouldn't be able to fix a problem quickly. This is because a large code base needs to be understood, impacts need to be assessed and regression tests performed, thus turning a single line code change into days of delays in getting an important fix deployed.

#### Serviceability
Modern web based applications don't have the luxury of downtime windows for planned maintenance that exist in internal enterprise applications (where planned maintenance frequently occurs on weekends). The ability of an application to have updates and enhancements deployed while the application is live and under load is an important aspect of availability. A robust and high quality application will have low availability if the entire system needs to be brought down for a few hours in order to roll out updates.

#### Recoverability
Assuming that things break, the speed at which they can be fixed is a key influencer of availability. The degree of recoverability in an application is largely up to the operational team, including support/maintenance developers and testers, to get things done. The ability to diagnose the root cause of a problem in a panic free environment and to take appropriate and effective corrective action is a sign of a high level of operational maturity and hence recoverability.

#### Detectability
If availability is measured in seconds of permissible downtime, only knowing that the application is unavailable because a user has complained, takes valuable chunks out of the availability targets. There is the need not only for immediate detection of critical errors, but for the proactive monitoring of health in order to take corrective action before a potential problem takes down the application.

### Build a consistent understanding of availability
Whether you use this definition of availability, or source and adapt one that suits your environment, it is important to build a consistent understanding of availability across the team. Too often we state that something needs to be highly available without considering the different interpretations of "highly available". The web developer's idea will be completely different to the telecommunications engineer and they will go their separate ways and build completely different solutions.

While a common understanding of what availability means in general discussions is important, it is not enough to assume that it is sufficient to build an application. The availability understanding needs to be translated into something that can be understood by the entire team, from technical to business bias. This translation and understanding needs to be captured in the availability SLAs.

## Developing the SLAs
In most cases, availability is expressed as a percentage of service uptime, and even Windows Azure expresses availability this way. There are benefits of having a simple percentage statement of availability, but it is generally difficult to measure and meaningless in a business context. Often these percentages are expressed for a specific component and the entire user experience is neglected. Also, the time period that they apply to is disingenuous to the need for an application to be available during critical periods. Indeed, some providers make a mockery of service availability by promising an unachievable 100% availability and relying on SLA credits if the target is not met, leaving the customer to measure and argue the downtime.

In order to architect, build and operate a cloud application properly, it is important to express the SLAs in a manner which makes sense to the business, can be reasonably measured, and is attainable within the budget.

### Heterogeneous availability across workloads
Not all workloads are equally important to the business. If increased availability comes at an increased cost, it makes business sense that the less important workloads have less money spent on their availability and therefore a lower availability target as a requirement.

Examples are easy to uncover, such as the primary web application, where an hour of downtime is headline news, versus the monthly reporting workload which, if it sent out reports an hour late, nobody would even notice. Others are less obvious and in some cases the differing availability requirements may be irrelevant because the rest of the application already provides higher availability than is required.

Places to look for differing availability requirements include:

* Significant lifecycle models or phases. The lifecycle model may uncover times when availability is high — for example, a retail site may have a high availability requirement for the start of Boxing Day or Black Friday sales.
* Back office processes, such as order fulfilment indicate that a particular workload's unavailability is offset by the time that it takes to process transactions manually. 
* Third party integration. Some third party services may have a lower SLA and the availability of our application cannot be higher than that of the third party service.
* Administrative features. An application will always have features available to the administrative users of the application and generally these users have a lower availability requirement, where they can do something else if the application is down. For example, an application may have a high availability requirement for the reading of content on the public pages, but the content management functions will be lower.

#### Availability influence on workload definition
Part of the reason for putting effort into decomposing workloads is because different workloads have different availability, operational, data and other requirements. But workloads may be defined in a specific way because features that have distinctly different availability requirements in a single workload, may be decomposed into different workloads in order to take advantage of the reduced availability requirement for a particular feature. This is often seen where asynchronous messaging via queues is used to isolate a single feature into different workloads. For example, the emailing of an order confirmation may be seen as part of the ordering process, but the lower availability requirement (email confirmations don't have to go out immediately) results in the architecture being changed to put email confirmations in a different workload.

Part of the design process of cloud applications is to be able to revisit existing workload decompositions as the design evolves. Architects may find that the development of the availability model causes their workload decompositions to change slightly.

### Expression of Availability in SLAs
The traditional means of describing availability using the "nines" (99.99% = four "nines") is open to interpretation and doesn't adequately communicate the availability requirements of the business. An availability expression of 99.9% allows for 43 minutes of downtime per month, and if those 43 minutes take place one minute at a time at 3am, then it is probably good enough availability. But if they happen all at once on Black Friday, it may not be good enough. Availability expressed over a long period such as a year or month, as the "nines" generally are, creates an unintended bias towards applications that are highly available when not needed and do not allow for reduced availability to perform maintenance tasks, or free up resources for other workloads that may be more important at the time.

It is better to express availability in a business context that can also be understood by the development team. These statements about availability are expressed in the following formats:

* When *\<event\>* the *\<workload\>* should handle *\<load\>*
* During *\<phase>* the *\<workload\>* should always respond within *\<response\>* for *\<percentile\>* users
* The *\<workload\>* can be unavailable available during *\<phase\>* for up to *\<time\>*

These formats suggest that the lifecycle model and workload models are required in order to sufficiently express the availability.

For example, consider the following lifecycle models and their corresponding availability statements.

![Workload variation availability example](./images/Availability-004.png)

The above e-commerce example illustrates heterogeneous workloads that have different availability requirements at different stages of the lifecycle. Product search is used consistently throughout the day with additional load during the day. The catalogue update is done at the end of the day, but early enough for administrators to address problems. The availability can be stated as (this is not a complete list):

* When the catalogue update starts at 4:30 pm, it needs to complete by 5:00 pm.
* The catalogue update can be offline at any time except between 4:00 to 6:00 pm.
* During the day, the product search should respond within 0.5s for 90% of users.

![Brief frequent important events availability example](./images/Availability-005.png)

The above example shows a live sports event where the primary feed is only used during live matches. Example availability statements include:

* The live feed has to be available for up to 100,000 concurrent users for 30 minutes before the match and until 30 minutes after the match.
* When under load, the frame rate can drop by 10% and latency by 2 seconds.
* When live matches are not being played, the workload is not used at all.

![Predictable customer behaviour availability example](./images/Availability-006.png)

The above example shows a workload that has a predictable seasonal pattern and illustrates how reduced availability can be planned based on business knowledge. The availability requirements can be stated as:

* From October to February, when people are booking holidays, three times the annual average load needs to be handled.
* The application can have reduced performance during any period except for daytime during the normal sales, late bookings and special offer periods.

![Planned event availability example](./images/Availability-007.png)

This example illustrates statements about availability in the scenario where an application needs to be prepped for a specific event, such as a launch.

* Anonymous users need a page response time of <0.5s for 80% of requests during normal periods where there will be 200 requests per second.
* When a product is launched and for five days after the launch, the application needs to handle 1000 requests per second with a response time of <0.5s for 70% of requests.

#### Meaningful availability expressions
As can be seen from these examples, it is easy to create expressions of availability. However you should consider that too many availability expressions in the SLA can become problematic:

* Too many different availability expressions may conflict with one another, making it impossible for all of them to be achieved.
* Members of the team need clear goals and too many availability expressions can dilute the value of having goals to work towards.
* If the availability SLAs are not met then corrective actions need to be developed to resolve the underlying problems. Attempting to unravel too many expressions can complicate the process.
* Too many expressions about availability could add unnecessarily to the costs as every single one of them, including the less important ones, is attempted to be successfully met.

When developing the availability expressions:

* Keep the number of expressions to a minimum. Two or three for each primary workload and/or lifecycle. Obviously for very large and complex projects, there may be many parts and accompanying expressions, but for a project to be developed by a small team in a few months there should be less than ten.
* Make sure that availability expressions can be measured. Using phrases like "up most of the time" cannot be measured and will create confusion.
* Develop availability expressions that lead to practical architectural discussions and decisions. Do this by having something that can be tested (e.g. simulating a load), technologies chosen (e.g. a latency expression leading to a decision about the cache), or developed against (e.g. expressing that a message needs to be delivered within a certain time).
* Make the availability expressions accessible to the entire team. Put them up on the wall if it helps and constantly ask individuals, when they are making a decision, how it impacts the availability targets. SLAs that are hidden away in the contract are only of use to lawyers.

## <a name="CostOfAvailability">Cost of availability</a>
High availability costs money and time, and it is the main factor in all architectural discussions relating to availability. It is important that this simple fact is understood by both business and the technical team. Business needs to be careful about asking for too much availability, as the costs will increase. Architects need to be careful about committing to too much availability without asking for extra budget.

Availability costs can be demonstrated with a simple non-IT example. 

Consider a simple requirement of lighting an area. The cheapest solution that satisfies the lighting requirement may not satisfy the availability requirement. A simple solution has a single light wired to the mains and will give sufficient light once switched on. But the light bulb itself may need to be of high quality so that it burns out less frequently, and putting in a higher quality bulb costs more money. Obtaining high availability with a single bulb will be difficult, as a failed bulb would need to be detected quickly and replaced. So it would be better to have two bulbs, either sharing the load, or with electronics to switch the spare on when one goes off. This adds cost in both the extra bulb and the additional electronics. Since the source power can fluctuate, we don't want spikes in external power to burn the bulb prematurely, so we add the cost of additional electrical components to smooth the power. Relying completely on a single external power source may not be good enough so we add the cost of building in batteries so that the light is on when the power fails. We can go on adding costs to something that has a simple functional requirement but a high availability requirement. The more available it has to be, the more we have to spend on taking care of possible reasons for failure. That is why there are completely different approaches to having a light in an attic versus an emergency exit light on an oil rig.

The same process of adding costs in order to achieve higher availability exists with applications. The increase in costs is not necessarily simple or linear. Using the lighting example, you can buy a cheap emergency lighting unit with LEDs, electronics and batteries that you just plug into the wall and get high availability for a low cost. Likewise, applications have similar affordable off-the-shelf high availability solutions, such as the platform provided by Windows Azure. But, to refer again to the lighting problem, such an affordable, high availability solution may not exist at your local hardware store if you want to light an airport runway.

The answer as to why availability costs more is a complex one, but has to be provided in the specific cases of your proposed architecture. The articulation and investigation of costs and options is fundamental to the availability model. Some of the factors that contribute to the increase in costs are discussed in more detail below.

### Skills
People need to know how to build available systems or at least have the ability and capacity to learn them on the job. These skills are expensive if they are acquired through consulting or recruitment. It is also costly and risky to develop these skills on the job, as a lot of new technologies and techniques need to be learned. The skills required broadly fall into the following three categories:

* Architectural skills — the knowledge as to how to approach solving a problem that implements a scalability pattern. For example, when to separate processes with a message queue. Included in these skills is the ability to rapidly evaluate the suitability of a particular technology or pattern to solve problems in the way that is expected, without negative side effects.
* Specific technology skills — availability is delivered using specific technologies, be that a message queue or NoSQL data store. These technologies, many of which will be new to the team, from installation and maintenance to learning a new API and discovering hidden problems, can only be learned through experience.
* Implementation skills — many developers are unfamiliar with the basic implementation skills necessary to actually code available systems. For example, developers tend to save data to a database without thinking about timeouts and retries. While web developers may be more used to building stateless code, other developers have implementation skills that make assumptions about the state of the host process.
* Operational skills — as discussed in the [operational model](#OperationalModel), operators need to learn a whole new way of working with applications that have high availability. Particularly if those applications involve complex data models or various out-of-phase deployments taking place and all while monitoring that the application is still available.
* Planning and estimating skills — with a lack of architectural, technology, implementation and operational skills on the team, the ability to plan the project suffers. Without having the basis and experience, it is difficult to apply planning and estimating techniques that people are familiar with. Skills need to be developed to plan and estimate unknown technical challenges.

### Engineering effort
Building available systems takes more engineering effort from the developers who write the code. The additional technologies, complexities of distributed architectures, the general slower pace of writing quality code (with all of its supporting processes of reviewing and refactoring) and many other day-to-day tasks required to build features, takes more time and effort when availability is important. The actual difference in effort depends on the approach taken to availability and the difference between 'normal' and 'high availability' in the code quality and patterns that the development team is comfortable with. The section below on [development approaches for availability](#DevelopmentApproachesForAvailability) indicates some of the engineering effort involved with each approach.

### Testing effort
A fundamental principle of cloud oriented architectures is that of 'design for failure', where the application should be able to cope with, and recover from, failure. There is an accompanying incorrect assumption that testing has a decreased role because the application can cope with failures, so testing all possible failures is not important. The effort required in testing cloud applications is extremely high, and availability places extra demands on testing. Looking at the [availability influencers](#AvailabilityInfluencers) above, each of the influencers (Quality, Fault tolerance, Scalability, Maintainability, Serviceability, Recoverability and Detectability) needs to be explicitly and exhaustively tested. Without adequate testing, there is no way to determine if availability targets can be met or to provide feedback to the team in order to make necessary adjustments.

### Operations effort
The operation of a highly available application takes a lot more effort than an application that is backed up weekly and left to itself the rest of the time. Operators need to develop processes and tools that allow them to keep the application running within the agreed SLAs. With cloud applications, this is further complicated by unfamiliar architectures and relationships with cloud suppliers (such as Microsoft) that work differently, thus requiring extra effort to establish operational processes.

### Resource consumption
Building higher availability applications usually involves building a distributed application with some redundancy. This means that more processes run that are spread across more machines, connected by a network that consumes more bandwidth. Simply put, high availability systems need more resources. Those resource can include more machines, more bandwidth and more storage.

### Complexity
Building applications that are highly available generally increases the complexity. Availability patterns such as message orientation and caching, add to the overall complexity. Complexity increases the cost; particularly if that complexity results in reduced failure (usually increased complexity increases the likelihood of failure because there are more places for defects to develop).

### Non-linear cost increases
Building an application that is 50% available is fairly easy and cheap. Building one that is 80% available is not that much more expensive. Even building one that is 99.9% available is not significantly more expensive than the 80% one, especially when using a cloud platform. But building an application that is 99.9999% available is significantly more expensive than the 99.9% one. The increase in costs is non-linear and sometimes to gain a tiny bit of availability you need to spend a large chunk on technologies and code to support it. 

For example, consider needing to display the current user profile on a page. Let's say that direct database access takes 0.25s when there are 1000 concurrent users and 1s when there are more than 1000 users. Let's also assume that the rest of the page render time is 0.25s. Consider now two availability expressions:

* The page must display the user profile within 0.5s for 80% of the users during normal load (when there are 1000 concurrent users).
* The page must display the user profile within 0.75s for 70% of the users during the peak lunchtime hour (when there are 1500 concurrent users).

Based on the constraints of the database it may be decided that the only way to meet the second availability expression is to add caching as increasing capacity will not solve the database bottleneck. Adding of caching is not a simple task and significantly adds to the cost. In this example, increasing the load by 50% for a brief period (1 hour), while also compromising on performance expectations (which is a fair and seemly minor adjustment to availability) still has a major impact to the overall development cost.

It is good to train your eye, when looking over availability expressions, for those which trigger a solution that increases cost, so that they can be discussed and ruled out if possible. It is also important that capacity and availability be tested early on in the development and decision process, so that you can feedback to the business with reliable cost implications.

## Availability business rationale
Knowing that increased availability increases cost or time, the availability model seeks a balance of what costs are acceptable for what availability. The project sponsor is ultimately responsible for finding the balance and making the necessary trade-offs, but has to be assisted by the architect and development team.

Before engaging with the technical team for detailed solutions, it is important that the business is clear about how much they are willing to spend, or how much time they are willing to take (cost and reduced time to market) in order to achieve availability goals.

### Assessing business value of availability
Having a highly available application has to have some benefit to the business, otherwise there is little point in building an available application. This may sound obvious, but many implementations are done in the name of availability but are actually done because a technical person thought it was 'cool' and wanted to pad their résumé with the latest shiny new technology.

The business benefits can be varied, and are not always clear in terms of the ability to handle normal business load and transactions. Some of these possible benefits are highlighted below:

* Customer experience — even if customers are not buying products using the application, they still expect an experience that works, is snappy and usable. For example, a major bricks-and-mortar retailer may still want a web application that enables customers to find their nearest store quickly and easily.
* Sales and revenue throughput — applications that generate revenue have a clear need for availability. After all, if no sales take place when and outage occurs, then it is obvious that outages need to be reduced.
* Paid for service — while people may tolerate sluggish web applications when they are free, if they are paying for the service they expect it to work a lot better  than the rest of the free applications that they may use. This may be unfair as some of the free applications that we all use are also some of the most sophisticated and available. Apart from SaaS type applications that are paid for, there are others, such as the schedule or live news and streaming for a cable sports channel that require subscription.
* Response to unprecedented demand — no application should be over-engineered on the basis of wishful thinking and hoping that the product will go viral. But, if there is a valid business reason and expectation that the application needs to handle unprecedented load, then it may justify spend on availability in order to cope with the load. Cloud computing platforms, by allowing the rapid provisioning of capacity, make this a significantly smaller investment than traditional hosting platforms.
* Reputational risk management — an established brand that may not use a web application for any transactions may still want to ensure that their application is always available, so as not to reflect poorly on the brand.
* Development of skills — the implementation of costly availability approaches for no other reason than to give developers a chance to play with something new, sounds nonsensical at first. If the application being developed has been chosen because it is low risk, and will be used as a test of Windows Azure before embarking on bigger projects, then putting in engineering that is unnecessary for the current requirement in order to get a feel for the technologies and develop skills, is a valid longer-term business benefit.

### Special considerations when assessing the need for availability

#### Variation of availability requirements across workloads
Stating availability needs that broadly apply to the whole application, will result in additional costs being applied to parts of the application where it is unnecessary. For example, a customer facing workload such as placing an order may have a high availability requirement The same application may have a separate order fulfilment workload that has a significantly lower availability requirement (fulfilment of orders can easily be delayed by a few minutes). This should become clear when the detail of the availability expressions are worked out for the SLA, but it is worth considering when communicating the business rationale. This consideration allows for budget to be allocated for high availability of specific parts of the application, without significantly adding to the overall cost.

Architectural input is required because workloads share platforms, technologies, tools and even skills. Although the availability targets may differ across workloads, the end result will tend towards the highest availability common denominator because of the integrated nature of the application and teams. Using the order placement and fulfilment example from above, although the database availability for fulfilment can be low, when implemented, where the order placed is stored in a shared database, the fulfilment workload will benefit from the availability requirement of the order placement workload.

#### The option of decreasing availability
Most availability discussions are about increasing availability, and responsible architects make sure that questions about high availability are addressed. But the option of decreasing availability can also be investigated. We don't necessarily want to discourage availability as it can be interpreted as an acceptance of low quality, yet in some cases reducing availability can result in significant savings.

For example, FTP is commonly used as an integration tool, and it is surprisingly difficult to build a highly available FTP endpoint. It is worthwhile questioning whether or not a highly available FTP server is required as it may be possible that the client service that uses the integration point has sufficient capabilities for retries and resends, negating the perceived need of availability.

#### Disregarding baseline availability during assessment
Assuming some basics are in place, a degree of availability can come for 'free', where the request for availability comes at no additional cost. A stable platform such as Windows Azure, has established development practices that have a positive influence on quality, and the right mix of skills. When assessing the business support of availability objectives, this degree of 'built in' availability can be disregarded once the baseline has been established. It doesn't decrease the importance of the availability required or delivered, but allows conversations to focus on specific key aspects of availability.

It is often specific workloads where the baseline is simply 'good enough', such as administrative or back office workloads. In these cases, specific components of the application, such as the availability of web roles, can largely be disregarded.

### Assessing the business impact of reduced availability
Before being able to determine how much business is willing to pay for availability, the risk of reduced availability needs to be determined. This is potentially quite a complex subject and is, particularly within enterprises, a highly specialised function that falls under a broader banner of business risks and contingencies where availability of specific IT components is only a small part of the overall picture. If your environment has an approachable and helpful risk management practice, you are encouraged to make use of it where practical and possible.

#### The likelihood of reduced availability
Determining the likelihood of outages occurring is an industry in itself and assessing the risk, with any authority, shouldn't be undertaken by amateurs. Experienced technical people can make observations about what is more or less likely to go wrong, and these can be fed to the business to include in their overall risk management plan. It is worthwhile to document some of these in the availability model.

The table below provides examples of causes of outages simply classified as to whether or not they are more or less likely.

| More likely 	| Less Likely 	|
| -------------	| -------------	|
| Failure of application due to edge-case bug 	| Complete network infrastructure failure 	|
| Degraded performance due to memory leaks 	| Complete disk failure 	|
| Database throttling/performance issues 	| datacentre off line 	|
| Things deleted 'by mistake' 	| Unable to provision new instances 	|
| Degraded performance due to unexpected load 	| Loss of long-term storage (backups)	|
| Network throughput problems 	| 	|
| Network latency problems 	| 	|
| Single region partial Azure outage 	| 	|
| External service unresponsive 	| 	|

Without getting into the detail of how frequently these reductions of availability may occur, it encourages focus on causes that are more likely. These examples coincide with some of the fundamental principles and practices in cloud application architectures. For example, we will put effort into designing an architecture that allows for poor database performance because it may occur frequently and is reasonably easy to design for. But we won't do the same for complete network loss, which is both highly unlikely and difficult to work around. The 'less likely' outages are also specific to cloud providers than they might be with private datacentre. Windows Azure storage, for example, has fairly sophisticated and trustworthy geo-replicated mechanisms as well as the engineers in place to ensure that the likelihood of data being lost is close to zero. This cannot be assumed by storage within a private datacentre which may not be highly available in every case,

> The high dependence on base cloud infrastructure is of valid concern to some people. If most applications assume that the network works, then the failure of the network, due to hacking and other sinister means, can cause massive economic and other damage. So while we make assumptions about infrastructure, don't forget that there is a risk, and be thankful that there are networking, security and other specialists continuously working to keep our infrastructure available.

#### The business impact of reduced availability
Knowing the likelihood of reduced availability is one aspect of managing the risk; the other being the impact on the business when this occurs. For example, a web server falling over may happen quite frequently, but having stateless web servers behind a load balancer will mean that the impact to the business in negligible. However, a system with a single database that is unavailable for days can have a massive impact on the business. As with assessing the likelihood of failures, you should make use of existing risk management practices if they are available in your business.

Impacts on the business can be loosely grouped according to those that have a direct financial impact and those that impact the business reputation or customer perception.

##### Direct financial impact
Financial impacts are generally the easiest to measure, but the reality of actual losses incurred may not match what the business believes the losses will be. How much this is reconciled or adjusted may be worth discussing in particular situations. Often it is the perception of loss that is the greater driver for availability than actual losses, which may influence a business to spend more on availability than they need to. The most common financial losses incurred are:

* Lost revenue — Applications that generate revenue per transaction, such as e-commerce websites, an outage can mean that customers don't make the purchases. Depending on the type of product and the value of the brand, the calculation of losses is not as simple as average revenue per minute multiplied by the number of minutes of downtime. Not only does it depend on when the system is available (at what point in the lifecycle), but in some cases customers will return (as Apple customers do when the store is offline for the latest product release), where in others the customers may go elsewhere to make the purchase immediately (such as airline reservations).
* Penalties - some applications, particularly those that provide a service for other businesses, have SLAs with penalty clauses. In some environments, regulatory or government penalties may also apply.
* Refunds - if an application has paid for customers, such as subscribers to live news or sports, downtime may contractually necessitate some sort of refund. Even Windows Azure, as a supplier of application services, is subject to customer refunds if it is not available.

##### Reputational damage
Large, established brands that have web applications for lead generation to traditional channels (brochureware) or investor relations, can suffer serious reputational damage if the application is unavailable. Nobody really cares if coca-cola.com is available, as consumers don't turn to the web when buying coke or make purchasing decisions based on the companies' home page. But a sustained lack of availability could become a mainstream or investor news item that can tarnish the brand's reputation.

##### Lost loyalty
Loyal customers are not easily swayed, but frequent lack of availability of their favourite services can turn the most loyal customers over to the competition. Not only are loyal customers lucrative, they are expensive to develop and can take others with them when they leave. A good example is the sustained outage of Blackberry messaging services in 2011 that was severely damaging to the brand. Loyal customers who had kept with the less trendy handsets vowed to leave in droves when their upgrade was due.

#### The cost of mitigating reduced availability
The [cost of availability is discussed above](#CostOfAvailability) and ultimately that cost needs to be weighed against the impacts to the business of reduced availability. The availability model should contain the business rationale behind expenditure on availability mandated approaches, technologies, infrastructure and skills. Apart from direct costs to implement a specific approach, the availability model needs to present detail on the following:

* Costs for the entire application lifecycle — The cost of availability needs to be considered in the broader context of the application lifecycle which includes development, testing, operations, maintenance, and decommissioning.
* Capital costs and operating costs — Rarely are costs incurred in a single budget, and the application is likely to have costs associated over a number of years. Upfront development costs may be taken off a CAPEX budget, reducing the on-going costs if availability is addressed later. Conversely, reducing development costs may allow the business that sponsors the application to generate revenue, which can be used to fund future availability endeavours.
* Costs of maintenance — Too often we focus on the direct costs to implement a specific technology and disregard on-going maintenance costs. Enterprises are familiar with the problem of dealing with high maintenance costs, mostly due to the brittle nature of applications that have been built. A focus on availability, with the associated development approaches, can have a positive effect on reducing maintenance costs. This, of course, is only relevant in cases where the application is intended to have a long life.
* Costs of skill retention — Once the development is complete, the skilled individuals that put together the solution may move onto something else or leave the organisation altogether, which could introduce significant risk. Business needs to consider the cost of retaining key individuals, or having access to a skills base, as part of the long-term costs of implementing availability.
* Sharing costs with other projects — In larger organisations it should be possible to share skills across projects or business units. These skills can be used during initial development and on-going maintenance. This is an important aspect of increasing the relevance of internal IT in an environment where applications are moving away from the on-premise datacentre and onto the cloud.
* Use of third-party consulting services — Some of the costs of providing higher availability can be reduced by making use of third-party consulting services. They can provide resources and patterns that would otherwise take a lot of time and trial and error if left up to a development team unfamiliar with aspects of high availability on cloud platforms. Consulting services can also be retained during on-going maintenance, further reducing long-term costs associated with high availability.
* Long-term predictability of availability requirements — availability requirements for launch may be different to ongoing future requirements. They could be higher, if the application is a success, or lower if the adoption is not as high as planned. Business needs to make an informed decision on availability for the longer term. Putting in high availability after development is complete may be cost prohibitive and maintaining a high availability application that doesn't need to be could cost a lot over time.

### Assessing the business impact of increased time to market
Much of the focus on availability cost relates to direct costs of people, man hours, resources and so on. One of the biggest costs of building high availability is the impact that it places on the project schedule. The scarcity and specialisation of skills required mean that implementing high availability cannot necessarily be implemented faster by increasing the size of the development team, and it is likely that a high availability application will take longer, in terms of calendar time, to implement than a low availability counterpart. One of the benefits of cloud computing is the ability to bring an application to market quicker, because capacity is instantly available and the provisioning of hardware is not on the critical path. It is a benefit that business places a high value on and it would be a wasted opportunity to squander that benefit by taking too long in development.

The availability model needs to assess and communicate the impact on the project duration that availability introduces, in addition to the direct cost. Business may opt for lower availability in exchange for shipping the application. This also requires some architectural and design input into the viability of adding availability later. 

### Investigating the staged implementation of availability
If high availability is not necessary at product launch, can it be implemented later, saving upfront costs and time? There is no simple answer. The degree to which you can defer engineering for availability depends on:

* Engineering practices and maturity in the team.
* Different approaches taken to building for availability.
* Specific technology choices that force the application down a particular architectural path.

For example, redundancy is generally easy to add later in the development cycle or during application maintenance. The idea being that adding another instance is simply a configuration task. This is only true for specific parts of the application. Adding redundant databases can prove tricky, whereas adding redundant web servers is fairly simple. But this, in turn, is only true if the developers have followed the practice of implementing stateless web servers.

At the other extreme, fixing scalability of databases late in the application lifecycle is virtually impossible. The data model and approaches to data have to be engineered to cope, at the application level, with availability. This is particularly relevant in the cloud. On-premise database performance is frequently addressed by adding expensive hardware to the database server, and this option is not available on the cloud. If SQL causes a performance bottleneck for your application, there is nothing that can be done to the infrastructure that underlies Windows Azure SQL Database, and no quick fixes to increase the application availability.

If application architects and designers insist that all the future availability is engineered upfront, it is likely that the project will fail; if not because the proposal is undercut or shelved because it doesn't match the business case, then it will fail because of the time taken to deliver or the risk associated with building complexity into the first release with an inexperienced team. A balance is required between implementing enough availability upfront to reduce further cost and risk, versus scrimping initially with the intention to increase availability later. Architects need to take a look at their longer-term 'to be' architecture and implement the foundations. The process will be along the lines of the following:

1. Select the appropriate availability approaches for the long-term architecture.
2. Define ways that the implementation can be skipped in the current release. This can include creating necessary abstractions or stubs of availability functionality but not actually implementing it, selecting a less available version of a specific technology or implementing a different technical solution to be swapped out later
3. For each approach perform an assessment of the implementation costs in the current release versus later releases.
4. For each approach determine the risks involved with implementing the approach later, particularly as it applies to extensive rework or instability.
5. Determine the 'have to have' approaches based on the cost and risk assessment.
6. Establish 'have to have' principles and approaches that are absolutely required, regardless of perceived cost savings. Examples include code quality, retries and exception handling. These are more programming principles which, if not implemented from the beginning, greatly devalue the application and make general operation, never mind high availability, a difficult task.

The end of this process should result in a workable plan that is architecturally sound and can be used by project management to plan execution, delivery and budgets.

### Obtaining accurate availability expectations from suppliers
It is often overlooked that 'advertised' availability from suppliers may not match up to what is realistic or even possible. Internal IT frequently makes statements about availability that is anecdotal, not audited, and difficult to measure. So in building a cloud application that is compared with on-premise applications be wary of availability statements that are apocryphal. In many cases on-premise IT states their availability targets or SLAs, which are often not met but not called out because it is not noticed or measured. Similarly, cloud providers can make availability claims that are blatantly untrue. Rackspace has a well-known "100% uptime guarantee" which is impossible. What they do offer is a discount if the service is unavailable but a 5% discount on a few minutes of hosting is not going to make any difference if the financial impacts of an application being down are large.

It is important, when working through the business rationale, to remember that openness and honesty about availability, risk, cost and time may not be reciprocated by others. You will ultimately land up with a better solution but may feel that you are not doing it as well, easily or cheaply as others say they do.

## <a name="DevelopmentApproachesForAvailability">Development approaches for availability</a>
The following approaches for availability summarise the most common patterns used in cloud computing applications and more specifically in Windows Azure. It is not, by any means, a complete list of all approaches. Availability is influenced by so many aspects of the technology, team and processes, and they can't all be discussed. If availability is so dependent on code quality, then the pursuit of high quality code, with all of its variations, disciplines and processes, is an important approach to availability. Similarly, all the solutions and approaches to performance, as a key part of availability, are not discussed here either. Read and understand these approaches in the context of being complementary to availability that your environment already adopts.

### Trusting the availability of infrastructure
Developers of applications on Windows Azure (or any other cloud platform) need to place a certain level of trust in the underlying infrastructure. Although low level hardware engineers need to make allowance for power or cooling failures in order to build available systems, developers of applications do not. Many availability decisions require some level of trust or assumption about availability, otherwise solutions can become unnecessarily complex or costly.

For example, when using persistent messaging as an availability approach, a degree of trust is placed in the persistence store. On Windows Azure you would place a message on a Windows Azure Queue, which you know is persisted, and assume that it is delivered. Distrusting the availability, and more importantly, the durability, of the message queue would mean that you would need to build a solution with an alternative persistence mechanism, and preferably one that is at least as reliable. Most architects would, quite rightly, scoff at the idea of having to make provision for an alternative for Windows Azure queues, but this is an obvious example; others are less so.

### Failure and bottlenecks
Available systems need to cater for throughput (maintaining availability even when the application is under load) and failures (maintaining availability even when something fails in the application). Approaches to availability generally favour one at the expense of the other. For example, modern databases like Windows Azure SQL Database seldom fail but are the source of availability problems because of their inability to handle the load, where it is the performance of the database that is the bottleneck. The solution to database performance is to put something in-between to buffer the bottleneck, such as a cache for reads or messaging for writes. This results in more moving parts that introduce more defects and more components that can fail; generally reducing the high availability offered by the database in exchange for the ability to handle the required throughput. Conversely, in-memory cache can handle a high throughput, which may be critical in rendering web pages quickly enough to achieve availability SLAs, but is unreliable and prone to failure (cache misses, inconsistent data, termination of process, long warm-up).

Most availability approaches deal with failure rather than throughput. It is important to remember that performance will be prominent in availability expressions and should be given considered attention. It is also necessary to understand the performance trade-offs whenever an availability approach is chosen that favours reliability.

### Failure points
Logic bugs do occur, but they affect the behaviour and output of an application and are seldom the cause of reduced availability because of failure. Most failures occur, not at arbitrary points in the application logic, but at failure points. Part of the engineering process of developing for availability is to try and identify failure points within the overall architecture and specific points in the application.

Failure points are typically design elements that are subject to external change or are dependent on data from external sources. Identifying these elements is an effective way of identifying failure points. Examples include database connections, web site or service connections, configuration files and registry keys.

There are common areas where failure points can be identified, typically where processing crosses a physical boundary or an application or system domain. These areas include ACLs (Access Control Lists) and identity services, database access, external web site or service access, configuration, and network capacity and latency.

The identification of failure points is critical in decisions relating to the implementation of other availability approaches, such as resiliency.

### Failure modes
In systems engineering, the observing and understanding of the root cause of a failure is known as a failure mode. A failure point is _where_ the failure occurs, and the failure mode is _why_ it occurs. Failure modes are not:

* Product defects — bugs in the code are more specific and dealt with more directly.
* Symptoms — failure modes are about understanding the root cause of failures.
* Informational — failure modes have to result in an actual failure, not the inability to handle warnings or other information data.

The table below illustrates some examples of common failure points and their matching failure modes.

| Failure Point 	| Failure Mode 	|
| ---------------	| --------------	|
| Database Access 	| Excessive writes to the database 	|
| Configuration not found 	| Configuration file not in the default place 	|
| Slow page render time 	| Too much traffic on web servers 	|
| Storage inaccessible 	| Storage keys changed 	|

You can see from the above examples that the root cause of the failure is important to resolving the problem and preventing future failures.

Understanding failure modes is a vital part of designing for failure and if you don't get a handle on likely root causes, then it is unlikely that you will be able to implement the design artefacts for coping with failure.

#### Assessing failure modes
Determine common failure modes that apply to the particular type of application and technologies used. For each failure mode assess the likelihood of this failure mode occurring in a live application. If the likelihood is high enough to have a significantly negative impact on the ability to meet availability SLAs, mark it down as a failure mode that needs to be specifically addressed. Identified failure modes can then be addressed through the availability approaches such as resiliency, isolation etc.

#### Determine the appropriate actions
Once failure modes have been assessed, the actions to handle the failure modes need to be established. Different failure modes will have differing actions, and with many failure modes it becomes clear that some approaches are not practical or feasible. For example, the failure mode of invalid credentials is unlikely to be resolved by retrying (in fact it may make it worse), whereas the failure mode of throttling is likely to be resolved by a retry.

#### Testing
Testers need input from architects and application designers in order to develop tests for availability. The identification of failure modes and the actions employed to deal with them is vital for testers to develop plans. These plans may include simulation, load testing, and the actual replication of a failure mode (such as the forced changing of credentials). Testers need to provide feedback so that the application responds or recovers as expected when a failure mode occurs.

### Stable storage
One of the fundamental building blocks of any availability solution is stable storage. Stable storage should be available (there when it is needed) and durable (never loses data). Many availability approaches involve the handoff of data between processes (e.g. asynchronous processing), or at least keep modified data in memory as briefly as possible. This needs to be underpinned by stable storage in order for them to achieve their availability objectives.

Storage on cloud platforms works a bit differently to traditional datacentres. Local machine storage is considered ephemeral and not stable at all. Apart from the inability to share local resources, any data stored on the local disk on a Windows Azure Role will be deleted when the role recycles. While there may be marginal performance benefits of using the local disk (which is, after all, not physically on the machine and subject to network latency), the base stable storage on Windows Azure is Windows Azure Storage (which includes tables, queues and blobs). Windows Azure SQL Database is for relational data and doesn't qualify as being a stable storage building block because it doesn't support any binary or file operations. The performance of Windows Azure storage in practice is better than the two-second response time commitment made in the [SLA](http://www.microsoft.com/en-us/download/details.aspx?displaylang=en&id=6656). The [performance latency](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/05/10/windows-azure-storage-abstractions-and-their-scalability-targets.aspx) of Windows Azure Storage is around 100ms for small objects. The real advantage is the durability as there are [six copies of each object](http://blogs.msdn.com/b/avkashchauhan/archive/2012/02/09/how-many-copies-of-your-blob-is-stored-in-windows-azure-blob-storage.aspx), across three fault domains in two datacentres.

Using Windows Azure Storage requires that developers become familiar with a [new API](http://msdn.microsoft.com/en-us/library/windowsazure/dd179355.aspx) which is encapsulated in .NET within the [StorageClient](http://msdn.microsoft.com/en-us/library/windowsazure/microsoft.windowsazure.storageclient.aspx) library. This means that developers need to become familiar with I/O operations which do not behave like the file system, adding some cost and risk. It doesn't take much for developers to master, and applications will have minimal need for persistence that is not to a database or queue.

### Design for failure
The 'design for failure' mantra has been borrowed from the design philosophies of distributed computing and has become popular in designing applications for cloud computing platforms. Central to the idea of 'design for failure' is to expect that everything is going to fail at some point and ask questions and develop approaches accordingly. The key point is that *everything* can fail, and the [chaos monkey](#ChaosMonkey) testing technique is about randomly and arbitrarily turning things off to test this principle.

'Design for failure' is good to use as a core philosophy in development teams but is no substitute for solid availability design and engineering. It does not address non-failure availability problems, such as the inability to handle load or code quality. Neither does it address the problem of whether or not designing for failure in each particular case is practical, valid and supported by the business rationale.

### Redundancy
The simplest approach for building availability is to have two or more mostly identical components running where, if one fails, the other is able to take the load. Using redundant components is a popular availability technique because it is fairly simple to configure (as the redundant components are simple and the complexity is in the load balancer). Also, redundant components can simultaneously be used for load balancing and all resources are utilised.

The aim of redundancy is to avoid failure of a single component by having more than one available. By having identical copies of the component the load can be taken up by one if the other fails. Redundant components are generally put in place across all levels of the application stack, from individual hardware components such as power supplies, to physical infrastructure units such as servers (instances), to application infrastructure such as IIS Application pools.

The problem with redundancy as an approach to availability is that while it supports the failure of individual components, it won't prevent failure due to defects. Because all components are identical, a failure due to a defect can result in failure of all components because they are exactly the same and share the same defect. This is particularly relevant in redundant software components where a buggy code, such as a leap year bug, is shared and executed simultaneously. This means that while there will be fewer failures, because a failure of an individual component does not affect availability, when failures of redundancy occur they have a much greater impact. Most of the well-publicised cloud outages have been a result of software bugs in the redundancy systems. Redundancy is also problematic where the components are widely distributed (across datacentres), as the network latency and cost of data transfer, influence whether or not it is practical. This is covered in more detail in [multiple locations](#MultipleLocations) below.

Windows Azure has multiple levels of redundancy built in to the hardware platform and different Azure services support redundancy in different ways. Windows Azure Compute, for example, allows for redundant roles that are monitored by the Fabric Controller, whereas Windows Azure SQL Database replicates data to redundant databases. The Windows Azure development approach, and indeed the SLA, encourages building applications in a redundant manner. The approach of using stateless (redundant) roles in Windows Azure Compute is the most common method of implementing redundancy that Windows Azure developers need to consider.

The success of redundancy implementation can be measured by recording the mean time between failures of components. In a stable redundancy solution, failures should be infrequent as any particular failure should go unnoticed.

### Resiliency
Assuming that failures do occur, a system needs to be able to recover back to a healthy state as soon as possible. Resiliency is about the ability of the application to 'spring back' after a failure in order to ensure that the service is available. The important aspect of resiliency is the automated ability to detect failures and respond to those failures. Examples include:

* Detection of failed redundant components and the automated recovery of those components while others are taking the load. For example, in Windows Azure Compute, the roles are resilient in that when a single instance fails, the Fabric Controller detects the failure and recycles the role. During recycling the application may be in a degraded health state (as fewer instances are handling the same load), but the recovery of the failed instance is fairly quick.
* Detection of failure and fail-over to an alternative as a response to the failure. This is common with ACID databases where two redundant servers cannot run simultaneously. Once a failure has been detected an alternative database is brought online to serve data. The monitoring and fail-over is non-trivial and even when automated, needs to take care of data consistency. This is why fail-overs on database clusters typically take minutes, rather than seconds, to restore availability.
* Detection of failure and queuing up of processing until the failure has been restored. Message oriented architectures rely on this solution, where the failure of a component is not an immediate problem and processing simply backs up until the failed component has been restored.

Windows Azure has a lot of resiliency built into the platform, such as with Windows Azure compute and Windows Azure SQL Database. Application developers can use other Windows Azure services, such as Windows Azure Queues to implement other types of resiliency.

Resilient systems assume that failure will occur and can respond accordingly. As a result, failures may be more frequent, but the impact is low. There is frequently a cost benefit of building resiliency rather than stability, and this is common on cloud platforms where commodity equipment is used. Although it may be more prone to failure than high-grade equipment, the overall resiliency of the platform makes it an acceptable compromise.

The measure of a resilient application is the mean time to restore i.e. how quickly it can 'bounce back' from failure.

### Scalability
As much as there is significant focus on scalability when discussing cloud computing, this release CALM doesn't have a separate model for scalability. Scalability is part of the lifecycle model (the demand for scaling), the operational model (the ability to respond to scale events), the cost model (the cost benefits of scaling, rather than having under-utilised resources), and the availability model. Scalability, in the context of availability, is the ability of an application to respond to load and maintain availability expectations, in particular performance expectations.

Scalability is an approach whereby certain parts of the application can simply be 'scaled out' as a response to load in order to maintain availability. Not all parts of the application can be scaled-out and databases are particularly problematic, but where it is possible it can be a highly effective and affordable approach to availability.

The success of a highly scalable application is measured by the ability to rapidly respond to increased (or decreased) load, using the minimum amount of resources, for the minimum about of time.

Future releases of CALM will bring together other models (availability, deployment, operations, and others) for a clearer view on how scalability is specifically addressed within the application.

### <a name="ReduceDependency">Reduce dependency</a>
Dependency of components on others is the most common cause of failures and dependency should be avoided where possible. This applies to components within a single application and those that are external. In the application layer, a process that calls another process, through a service API or similar, and has to wait for a result, is prone to the failure of the external process it is dependent on. This process could be on another machine, across the world, or owned and operated by a completely different organisation, so is susceptible to network problems and general availability, in addition to defects that may cause it to fail.

The most common approach to reduce dependency is to interface asynchronously using shared durable state. In this case, one process persists the data that needs to be worked on, and sends a message to the other process to deal with. Once the data is saved, and the message sent, the process has nothing further to do and continues. The second process is free to receive the message and process the data, and if it fails at some point, the first process is not impacted.

The Windows Azure practice strongly supports the reduction of dependencies by using messaging and provides highly available underlying services that make it simple within the Windows Azure environment. The pattern, using a typical e-commerce scenario, works as follows:

* The web role during the checkout process saves the incomplete order to Windows Azure SQL Database.
* The web role, on receiving the 'place order' request, adds a message containing the unique order number to the 'new order' (Windows Azure Storage queue) and returns an 'Order successfully placed' page.
* The 'New orders' worker role that is polling the 'new order' queue receives a message of a new order.
* The worker role uses the order number to retrieve the order from Windows Azure SQL Database.
* The worker role performs necessary operations on the order, such as validate, send email confirmation, and add to fulfilment queue.
* The worker role deletes the message from the new order queue once it has completed.

In the above example, the web role is isolated from any failures that can occur on the 'New orders' worker role.  Any that exist can be many, including failures of services that need to be called (such as shipping), email service failures and others. The persistent message queue is also an important part of the fault tolerance, by allowing the message to be reprocessed if the worker role fails to process it correctly.

Windows Azure supports this approach by:

* Providing a clear distinction between web roles and worker roles and encouraging the use of both.
* Providing a number of shared state options to work with data across processes, including queues, blobs, table storage and SQL.
* Providing a reliable messaging service in Windows Azure Queues which means that as soon as a message is added to the queue it is guaranteed to be processed at least once.
* Providing various APIs that are REST based with .NET wrappers that allow easier implementation.

The problems with this approach to dependency reduction are:

* Process decomposition - in order for dependencies to be reduced, processes need to be decomposed as much as possible. The above 'order' example may be fairly easy to decompose because the end user is not concerned about the back-end processes. But a complex process that is highly dependent on back-end processes, such as payment authorisation, may be more difficult to decompose.
* Notification of completeness — the problem with asynchronous processing is notifying the calling process that work is complete so that it can continue with other processes. Generally we try to avoid waiting for this completeness notification and asynchronous callbacks, as it defeats the idea of reduced dependency. Asynchronous processing that uses callbacks is good for reducing the *appearance* of waiting, but doesn't actually reduce dependency. It is possible to marshal completeness checks on the front-end with technologies like [SignalR](https://github.com/SignalR/SignalR), and is emerging with more sophisticated front-end applications (within the browser). 
* No distributed transactions — Distributed transactions, two phase commits, and sophisticated mechanisms in traditional TP monitors are generally not used in the cloud and not supported by Windows Azure. The Windows Azure Queues (and Service Bus to a lesser degree) are fairly simple messaging systems. While this has the disadvantage of having to hand code compensating transactions, distributed transactions rarely work with the type of applications developed in the cloud, due to the underlying technologies that don't support rollbacks.

### Asynchronous processing
Rapid scaling where resources are added when demand increases, and removed when demand drops, is an important aspect of availability. The ability to respond is necessary to ensure that the application performs regardless of the load that it may be under at a point in time. Windows Azure supports the ability to scale rapidly by allowing the required instance count to be changed and the Fabric Controller to automatically spin up an instance and slot it in to the load balancer. Rapid scaling is not without its problems:

* The time from detection of diminished health (as a result of the load) to provisioning of resources is measured in minutes, so there may be a period of reduced availability.
* Very spiky traffic, where the high load interval is less than the granularity of billing (one hour in the case of Windows Azure) removes some of the benefit of scaling.
* Monitoring the need for scaling is not simple — it may not just be average CPU or another metric that determines the need to scale.
* Some parts of the application may not be able to scale. Scaling out front-end web servers, for example, may put excessive load on the database, which cannot scale, and availability is impacted.

The implementation of asynchronous processing allows the load that various components are under to be smoothed out. Using the order processing example from [reducing dependency](#ReduceDependency) as described above, the front-end order web role can be scaled up and down as needed, but the back-end 'New order' worker role can be left to run at its maximum throughput at a lower scale. When the front-end is under load, messages on the queue simply wait a bit longer before they are processed. This allows the maximum (and therefore optimal) use of the available resources on the worker role, while at the same time reducing the load on downstream components, such as the database or external services. The diagram below illustrates how the back-end process picks up the load and 'flat-lines' while the front-end process responds to the variable load.

![Smoothing out load using asynchronous processing](./images/Availability-008.png)

On Windows Azure, the implementation of asynchronous processing (across different machines) is the same as the implementation of reduced dependency using Windows Azure Queues. This is a valuable double use of queues to resolve availability and one of the reasons why the use of Windows Azure Queues is such a fundamental paradigm within Windows Azure.

The diagram below illustrates asynchronous processing using persistent messaging. The separation of work into separate, independent processes allows the data to be persisted (in messages) as it flows through the processes. A failure of any of the processes should not result in data loss as it has been safely saved to a queue.

![Decomposition with persistent messaging](./images/Availability-009.png)

#### Deferred processing
Asynchronous processing also supports the approach to handling failures of deferred processing. Deferred processing is a retry that allows for long periods between the initial failure and the retry. Asynchronous processing using message queues is well suited to deferred processing, as all of the design artefacts needed for deferred processing exist and may be well used in the application already. In deferred processing using queues, if a failure occurs, a message is added to a queue for later processing. The later processing can perform the same action or, based on information about the data, perform a slightly different action. Unfortunately, Windows Azure Queues do not support delayed delivery of messages so it is possible that the message will be delivered too quickly for the failure mode to be resolved.

### Fault tolerance
Failures are bound to occur — what is important is what the application does when failures occur. The first step in the context of availability is to at least make it look like failures are either not occurring or not causing significant problems. This can be done by:

* Ensuring that it is only the failed process, thread, or whatever is the smallest unit, which is impacted. Failures should not bring down the entire application by corrupting data, typing up the request pipeline, or overloading external services.
* Having alternatives when failures do occur, such as retrieving from the database when the cache fails, retrieving static or inconsistent data when live data requests fail, or redirecting to a stable service.
* Storing the input data in an alternative store, such as a queue, for later processing when the dependant service is available.
* Providing alternatives to the user in the event of a failure that cannot be resolved, such as a telephone number to assist with urgent transactions.
* Hiding error messages. These are largely useless to the user, cause panic and are a potential security risk.

#### Fail early
Fault tolerance is not the same as fault recovery. Developers believe that it is possible to write code to recover from errors, but beyond a few simple options (such as reading static data when a database error occurs) it is seldom successful. A fundamental principle of building high availability systems is to fail as early as possible and not try to recover. Recovery can become complex, result in corruption and inconsistencies and seldom addresses the underlying problem, so it is likely that failure will occur on the next part of the operation anyway. It is also impossible to specify all failure scenarios, so building recovery for 'unknown unknowns' is virtually impossible. Note that this advice doesn't apply to applications that are responsible for life support, Mars rovers, weapons or other applications where more recovery rigour is mandated.

### Retrying on transient faults
Sometimes processes cannot be decomposed into isolated services separated by asynchronous processing. A web page, for example, may need consistent data immediately in order to render a page correctly and has a synchronous dependency on services. In other cases, it may be decided that the increased effort or complexity of decomposing a process is too high, especially when the required service is deemed to be fairly reliable. In these cases, part of fault tolerance is the ability to retry an operation.

Retry is not an attempt at active recovery, but rather waiting a bit before trying the same operation again on the assumption that the fault is transient. Transient faults are faults that are likely to last for a short time and are generally the result of timeout or capacity related failures. Retries should only be attempted on transient faults where there is an expectation that a delay will change behaviour. For example, there is no point retrying a database operation for a key constraint error, but it may be worthwhile if the error is because the database has been actively throttled (as with Windows Azure SQL Database). There are cases where availability may be easily restored by retrying an operation. It is worthwhile retrying at least once if it is not an expensive operation and does not make matters worse. 

Scenarios where retries are applicable include:

* Failure from short-lived outages.
* Cases where a service has failed and is in the process of recovering (fail-over).
* Failure due to unreliable or congested networks — this may be a common occurrence when integration with on-premise services.
* Throttling that has been enforced by the Windows Azure platform. [Windows Azure SQL Database throttles](http://msdn.microsoft.com/library/ff394106#throttling) to prevent the overuse of resources on a machine by a single tenant. Similarly, Windows Azure Storage also [imposes limits on throughput](http://blogs.msdn.com/b/windowsazure/archive/2012/11/02/windows-azure-s-flat-network-storage-and-2012-scalability-targets.aspx) for partitions, accounts, queues, etc.
* Calls to services that do not give error codes that indicate a transactional fault.

Retries are a programming technique that is generally applicable and only explicitly supported by Windows Azure through meaningful error codes, such as the throttling specific error codes returned by Windows Azure SQL Database. Separate from the Windows Azure SDK, the Microsoft Patterns and Practices group has produced a library called the [Microsoft Enterprise Library Transient Fault Handling Application Block](http://msdn.microsoft.com/en-us/library/hh680934(v=pandp.50).aspx) that allows the developer to select an incremental, fixed interval or exponential back-off retry strategy when accessing Windows Azure SQL Database, Windows Azure Service Bus, Windows Azure Storage or the Windows Azure Caching Service.

Here are some implementation points to bear in mind when retrying transient faults:

* Retry a finite number of times — sticking with the availability principle of failing fast, do not keep retrying indefinitely as the fault may not be a transient one and the application should move to a failed state quickly.
* Make use of back-off strategies (such as exponential back-off). There may be no point in retrying an operation ten times with a 100 ms delay if the failure lasts for two seconds.
* Include logging and instrumentation in record retries so that adjustments can be made, either to the called service or the component that always has to retry.
* Make sure that operations are idempotent. In some cases, the failure is because the called service is running slowly, so it takes a long time to return a result, but the operation is performed. In this case, the retry will perform the same operation, and idempotent data is needed to ensure that the operation does not result in multiple transactions or duplicate data.
* Ensure that if all of the retries fail, the failure is handled correctly by making sure that the data is in a consistent state and that errors are properly handled. Don't let retries run in the background assuming that they will never fail.

#### Fail Whale — failing with style
During the massive growth of Twitter in the early years, failures were common. The 'fail whale' page that was displayed when Twitter was unavailable became a pop culture icon. The fail whale is well-designed and somehow allowed dedicated fans to develop an affinity for a service that was struggling. Twitter managed to resolve their availability issues, users seemed to stay, and the fail whale was influential in developing that loyalty. This success has been replicated in different forms as humorous, well designed and apologetic error pages that users are more tolerant of.

Spending time and effort on failure error messages that graciously acknowledge a fault, has proven to be worthwhile and can go a long way to retaining users who would otherwise become frustrated and click away.

### Isolation
Isolation is a key principle in available systems — whether that is isolated storage, network infrastructure, processes or services. Isolation works for components that fail, by making sure that they don't bring down other healthy components. It also works for healthy components, ensuring that other failing components do not affect their health. Isolation goes hand in hand with 'autonomy', where components exist in their own place and are blissfully unaware and unaffected by rogue components around them.

Modern applications have the additional problem of frequent, out-of-band releases of some parts of the application, resulting in potential problems when one component is being shut down for release while others are under load. This is not technically 'failure', but can result in the unavailability of stable components from being knocked out because of the deployment of another. Cloud applications that have components in various stages of deployment, staging and testing, are particularly susceptible if they are not sufficiently isolated.

When developing applications on Windows Azure, isolation is primarily implemented by building distinct services that have very little interaction with one another. Isolated services only interact asynchronously, where the success or failure of the interaction is irrelevant, or by using stable and reliable shared state, such as Windows Azure SQL Database.

Windows Azure supports the paradigm of isolation by encouraging the use of distinct services. The web and worker roles are the obvious examples, but the Windows Azure model goes further by supporting the implementation of many distinct services due to deployment and cost models. For example, a web application may be comprised of a number of different services, deployed across many roles of varying sizes and varying instance counts depending on the need. It is no longer necessary to be concerned with the physical machine resources available, when architecting the granularity of services in the application.

### <a name="FeatureShaping">Feature Shaping (Service Degradation)</a>
One of the key concepts in scalability is the ability to allow for graceful service degradation when an application is under load. But service degradation can be difficult to explain  and 'degrade' has negative connotations.

The networking people overcame the bad press of degradation by calling it '[traffic shaping](http://en.wikipedia.org/wiki/Traffic_shaping)' or 'packet shaping'. Traffic shaping, as we see it on the edge of the network on our home broadband connections, allows some data packets to be of a lower priority (such online gaming) than others (such as web browsing). The idea is that a saturated network can handle the load by changing the profile or *shape* of priority traffic. Key to traffic shaping is that most users don't notice that it is happening.

Feature shaping is the ability for an application under load to shape the profile of features that get priority, or to shape the result to be one that is less costly (in terms of resources) to produce. This is best explained by examples:

* Farmville degrades services when under load by dropping some of the in-game features that require a lot of back-end processing, thus shaping the richness of in-game functionality.
* Email confirmations can be delayed to reduce load. The deferred load is either by the generation of the email itself, or the transmission of the email.
* Encoding of videos on Facebook is not immediate and is shaped by the available capacity for encoding. During peak usage, the feature will take longer and the difference in time taken is generally not noticed by users.
* A different search index that produces less accurate results, but for a lower cost, may be used during heavy load, thus shaping the search result.
* Real-time analytics for personalised in-page advertising can be switched off when under load, shaping the adverts to those that are more general.

Feature shaping can be defined as:

* Feature shaping allows some parts of an application to degrade the normal performance or accuracy service levels in response to load.
* Feature shaping is not fault tolerance — it is not a mechanism to cope when all hell breaks loose.
* Feature shaping is for exceptional behaviour and features should not be shaped under normal conditions
* Shaped features will be generally unnoticeable to most users. The application _seems_ to behave as expected.
* Feature shaping can be automated or manual.
* Feature shaping can be applied differently to different sets of users at the same time (e.g. registered users don't get features shaped).

Like with performance requirements it is best to extract the feature shaping requirements as simple statements, for example:

1. Page render time can drop by 100% (2 seconds to 4 seconds)
2. The most popular 10% of items can be cached without being refreshed for 5 minutes.
3. Personalised content can be unavailable
4. Processing of external feeds can be suspended
5. Administrative functions should still be performant and accessible. 

Unlike performance requirements, where it is easier to deal with as few points as possible, when degrading due to load it is probably better to list as many functional parts of the system as possible. Business needs to be given the opportunity to communicate the core features, or minimum viable product, of their application. By listing each of the major features, and how they can degrade under extreme load, architects will build accordingly.

### <a name="MultipleLocations">Multiple locations</a>
Distributing an application in multiple locations is a common approach to availability. The main reasons this approach is chosen are either to reduce the latency to end users, or to provide an alternative datacentre in the event of a major outage.

Where an application has users around the world, particularly if they are outside the US or Europe, the response times of the application can have a severe impact on availability as users perceive the performance of the application to be poor. The most common solutions to this problem include the replication of the entire application and its data to a more localised datacentre, but it does have drawbacks associated with having shared data across multiple datacentres (as discussed below). Simpler options include the use of CDNs (Content Delivery Networks), such as the Windows Azure CDN, which facilitate the placement of static data at edge locations, where latency to end users is lower. Strategies of moving particular workloads to other datacentres, while keeping core data centralised, can also be employed and facilitated by using technologies available in Windows Azure such as Windows Azure Queues. 

In cases where applications have regional variations, such as language, an entirely different product catalogue or regional fulfilment provider, it may be preferable to build, operate and deploy the application as a completely separate one. Code can be shared across the two projects, and shared data can be handled as integration between two disparate services, rather than a common data source. It may also be that the regional variation has a different lifecycle model, different availability requirements and, because it is owned and operated by a different organisation, has a unique profile that bears little resemblance to any other region.

Major cloud computing outages are generally restricted to a single datacentre. Not because of regional disaster that brings down communication or power, but because of the management, operations and deployments specific to that datacentre. In the event of a single datacentre outage, the only way to ensure availability is to ensure the entire application can run in a separate datacentre. While the risk of a datacentre outage exists, and it is technically possible to address the problem, the cost of geographic redundancy needs to be well understood. Windows Azure helps by providing rapid scalability, where the fail-over deployment does not have to continuously run at full capacity, but there are still on-going costs. Identical replication of data will incur data egress costs from the primary datacentre, and with large datasets this may not be trivial. Costs will also be incurred by the overhead of having to deploy and monitor a second deployment.

Replication of data across multiple datacentres is challenging. These challenges include:

* Data volume — the volume of data could saturate the network, or at least carry a significant cost.
* Latency for non-replicated data — it may be determined that some data cannot be replicated for regulatory or data consistency reasons. Applications that are deployed in different datacentres may have performance problems at key points where centralised data is queried.
* Data consistency — a requirement for high data consistency may not be able to be satisfied with replicated data, as the time taken to replicate the data may result in inconsistent queries.
* Operations — replication of data carries an extra operational overhead. Not only must both systems be highly available, schema changes need to be synchronised and the recovery of an identical dataset in the event of failures can be a challenging task requiring manual intervention.

If it is determined that data does need to be shared across datacentres, consider the following approaches:

* High read, low write workloads — identify workloads that have many reads and few writes. These are better candidate workloads to run out of distributed datacentres.
* Keep writes centralised — if only one database contains 'the truth' and distributed databases contain read-only data, it is easier to maintain the replication. Consider using Windows Azure Queues and worker roles for writes (such as using a CQRS type pattern)
* Allow for restoration of snapshots — sometimes databases can become so out of sync that it is difficult to reconcile, such as when a slave datacentre has a long outage. It may be easier to restore a snapshot of the central database rather than resolving synchronisation idiosyncrasies by hand.

#### On-premise data redundancy
One of the myths of cloud computing is the issue around key business data being stored on a platform that is not owned by the business. Business and regulatory reasoning aside, it may be technically simple, and create peace of mind, to replicate data from the cloud to an on-premise datacentre. This provides a sense of control of the data and can offer tangible benefits if BI and other tools are on-premise, where it may be better to have the application data on the local network where it can be used further. If data is stored in Windows Azure Storage, an agent will have to be developed to transform the data to suit the on-premise storage architecture. Data stored in Windows Azure SQL Database cannot be replicated using the built-in SQL Server replication, but Windows Azure does offer the Windows Azure SQL Database Data Sync service to replicate data to an on-premise SQL Server database.

### Data availability 
The availability of data is one of the most important aspects of availability. This applies to both short-term availability, where the data needs to be available when it needs to be processed, as well as long-term availability, where data needs to be safely stored for long-term use.

The implementation of data availability is contained in, and influenced by, the [data model](#DataModel) which deals with the performant and reliable persistence and retrieval of data. The broad categories of data availability to be contained within the availability model, as references to the detail within the data model, include:

* Data services — when a service that provides data cannot be connected to, either a service that is part of the application or a third party service, the application availability is affected. Strategies need to be developed for alternative sources of data, such as reading from a cache or another service.
* Data performance — applications that wait for data, whether that is from the primary database, the cache or another service, can perform badly and have their availability targets severely impacted. Database performance is probably the single, biggest cause of poor application performance and therefore needs considerable attention when architecting.
* Data consistency — applications have different requirements for having access to data that is correct and consistent. The reliability [availability outcome](#AvailabilityOutcomes), where the measure of availability is the accuracy or relevance of the data, may be negatively impacted by inconsistent results. Applications that require high consistency (and there are very few of these) place higher demands on data availability.
* Data longevity — often overlooked by application developers and even business, data that is lost forever can have a significant impact on the business. Ensuring that data can be reliably backed up, restored and accessible, is part of the overall requirement for data availability. Windows Azure Storage, for example, is reasonably durable, unless a defective application deletes it 'by mistake', in which case it will be lost forever unless a backup was made. Most losses of long-term data are not related to any platform failures, but rather defects, operational process problems (like forgetting to configure backups) or administrative issues (like termination of an account).

#### Multiple copies and multiple paths
One of the core principles of availability is to have multiple copies of data and multiple paths to getting data. For example, instead of just having data in a SQL database (single store) accessible by a web page (single path), have the data in an additional data store (say table storage) that can be accessed by an independent service. This principle can be applied to storing the data in a single format in multiple locations (geo-redundancy), or multiple formats in a single location (such as having data in a search index like SOLR in addition to the primary database). It can also extend to accessing or storing data from applications and data stores that are located on-premise. Obviously, the implementation of this principle can significantly add to costs in terms of data storage, consumed bandwidth, development of alternative access paths, and operational effort. Availability of critical data, such as financial transactions, may warrant the extra cost, whereas less important data such as traffic logs may not, so the principle can be inconsistently applied depending on the requirement.

### Skeleton applications
In some cases, failure can be so catastrophic that the application is down for days. This can be the result of a major cloud outage or database corruption that takes days to recover. One of the considerations for availability is to have functionality available in order to satisfy the needs of critical business functions. For public facing applications, this may be the ability to view the catalogue or access account information. For internally focussed applications, this may be limited to capturing important transactions or customer lookup and forgoing analysis and reporting features.

Skeleton applications are those that provide the minimum functionality in the event of a major outage in order to provide some level of service. Skeleton applications should be considered separate (as per the principle of multiple paths) and should be able to run on a completely independent platform (such as on-premise). How these applications are developed and how much is spent on them depends on the business rationale and the impact on the business during a sustained outage. Care needs to be taken to ensure that they are not full featured and expensive to maintain. They could even be simple database enquires using Excel.

### The role of UX
The discipline of User Experience (UX) is aimed at providing the best interaction with the application for the users, which can have both a positive and negative effect on application availability. For example, a product catalogue search result may provide stock availability information to the user, but this may require queries against a stock database that is already under load. A sudden increase in searches (even by anonymous users) could bring down the stock database and ultimately affect the availability of the application.

Many examples of application of UX, in terms of availability, are more subtle and could require a significant programme of user testing, analytics from live systems and behavioural analysis to find out if the impact on resources is justified. Some reining in on UX may be quite simple. For example, minimising the number of results returned by a search may have a significant impact on the availability of the search engine and be irrelevant to UX, making it an easy decision to make. In other cases, consulting with UX experts may help solve an availability problem. For example, a resource intensive feature may be inadvertently accessed because users try to 'discover' what it does and a small UX tweak, to help the user understand if they need to access the feature at all, may be all that is needed to discourage pointless use.

Most UX practitioners are unaware of the performance and availability impacts of UX decisions, and while they don't try and clutter the user interface, they do try and increase information density, which can be costly to produce. Architects and application designers need to work closely with UX designers during the early stages of the design of the front-end. Even though the application architecture may not be defined, experienced architects and developers will be able to provide input into what parts of the front-end are resource intensive. This should provide an indication as to whether or not particular UX elements are required. For example, a web page with a search box may require an incremental search (where the results are displayed as the user types) and an architect could point out that every keypress is a separate search. If the average search term is ten characters, satisfying such a search requirement would require ten times the resources as a non-incremental one (this is not quite true as search engines have indexing features to facilitate incremental search, but there are definitely more resources consumed). This could result in a discussion between developers, UX and business as to the value of the incremental search feature and business could decide to either commit the extra resources, or decide that the feature is not important enough. Working together early on also provides input to the application design process, where features that are required by UX have architectural considerations that need to be made as early as possible.

It is obvious that features impact performance and availability, yet reducing features in the interests of availability can be futile because reducing features can reduce user satisfaction and adoption. The role of UX is not necessarily the reduction of features, but the presentation, richness and depth of those features. Well engineered UX can find the crucial balance between them.

#### Air France — Extreme UX for availability
When the Air France Flight 447 disappeared over the Atlantic Ocean on 1 June 2009 the airfrance.com site was put under excessive load due to people wanting more information. Air France changed their home page to provide current information on the crash, in plain text, with a link to the normal booking website. The Air France response is a sombre example of a UX tweak as a response to load. Features were not shaped as they were still accessible (just an extra click away). The placement of a fairly static page as the 'home page' had obvious and necessary non-technical reasons, yet the impact on system scalability, and resultant availability, was high. The static page, which most of the users wanted to see, was lightweight, unauthenticated, directly served (no database connection), cacheable (even across a CDN) and able to be massively scaled out.

### Azure specific features for availability
Windows Azure has significant features that help with availability and are built into the platform, so architects, designers and developers can get on with writing application code and worry less about application availability. These features take care of some availability concerns but still need to be understood by architects; so that what they offer is clear within the context availability and their use can be maximised. 

#### Fault domains
In addition to lower level infrastructure redundancy, such as in routers, switches and load balancers, the Windows Azure Fabric Controller tries to spread roles across an isolated physical infrastructure. This reduces the likelihood of application failure because of hardware failure. The use and configuration of fault domains are part of the datacentre topology and infrastructure, but what is defined as a fault domain is unknown, and largely irrelevant, to consumers of the service. Microsoft does observe that one of the fault domains is the rack in the datacentre. Having your application spread across multiple racks will definitely reduce the likelihood of hardware failure bringing down the entire application.

#### VIP (Virtual IP) swap
When performing a major upgrade of an application, it can take quite a long time for all of the instances to start up and even longer to make sure that everything is working properly. This can have a major impact on availability because the application may not perform adequately while it is starting up and it may not be safe for public access while it is being tested. Windows Azure has a feature called Virtual IP Address Swap which allows a new deployment to be instantiated in a staging environment and assigned a publicly accessible domain name, which can be switched in seconds with the production domain name. This allows the new version of the application to be running and ready to accept traffic as soon as operators are confident that it is working properly. An important part of the VIP swap is that the previous version switches over to staging so that if, for some reason, the new version is not working, it can be swapped back to the previous version very quickly.

#### Upgrade domain
Minor upgrades to the application, such as changes to configuration, do not warrant the rigour of a staging environment and VIP Swap. Windows Azure has upgrade domains (also known as update domains, although there is a difference) to make it easier and to ensure availability. When an update occurs, roles need to be recycled, and it would impact availability if they were done all at once. Windows Azure deals with this by grouping roles into upgrade domains (a default of 5). Each upgrade domain is upgraded in turn, while one is being recycled, all the others handle traffic, and once the Fabric Controller is satisfied that an upgrade domain is working properly, it moves on to the next one.

#### Windows Azure SQL Database
The differences between Windows Azure SQL Database and SQL Server largely exist to enable it to work in a multi-tenant environment and to provide for higher availability. Windows Azure SQL Database doesn't have the traditional concept of a database cluster to support availability (which is largely about shared storage) and instead replicates the data automatically across three databases (explaining the need for a clustered index and other limitations). If a single database fails, Windows Azure SQL Database will take care of making one of the replicas the primary and start-up another to replace the failed database.

Windows Azure SQL Database also has [federations](http://msdn.microsoft.com/en-us/library/windowsazure/hh597452.aspx) which are useful in building available systems by providing a mechanism to scale out the SQL database. Windows Azure SQL Database federations shard (or partition) data across multiple databases and allow queries to be written that access the shards at the database level, rather than having the application know which shards are needed.

#### Traffic manager
The [Windows Azure Traffic Manager](http://msdn.microsoft.com/en-us/library/windowsazure/hh744833.aspx) allows the distribution of user traffic across Windows Azure hosted services. It is not like a network load balancer as it directs the traffic by applying policies to DNS queries, rather than routing the actual traffic. While it can be used in applications within a single datacentre, the benefits of the traffic manager are realised when the application is deployed across multiple datacentres. The traffic manager can be used in the following scenarios:


* Performance — directs the user to the "best"/"closest" deployment. For example, directing the user to the "best" deployment between US South and West Europe.
* Failover — traffic is redirected to another deployment if the primary goes down. For example, all traffic is directed to US North and if it goes down send all traffic to US South.
* Geomapping — allows users from defined geographic locations to be directed to particular deployment. For example, all users from US are directed to US North, all users from Asia are directed to US North, and all users from Europe are directed to West Europe.
* Ratio — sends traffic to different deployments based on fixed ratio. For example, direct 20% of user traffic to US South and 80% to US North.

## Organisational behaviour
The biggest test of availability is not just the technical issues, but how an organisation responds to outages before, during, and after one occurs. Organisations that scrimp on availability spending and then have senior managers running around looking for heads to roll when something happens, generally do not handle availability problems very well. When applications are under load and system downtime has revenue impacts, even the coolest organisation can crack under the pressure; making mistakes, blaming the wrong people and generally taking longer to restore application health.

How organisations and individuals behave when failure occurs is a big subject and out of scope for this book. The business processes, management practices, training and documentation for fault-tolerant organisations can be a large, complex and specialised field. The aerospace/airline industry is a good example of organisational behaviour during failure taken to (valid) extremes; from the amount of training pilots receive to handle failure scenarios, to the processes of air traffic control and airports to sort things out, to the engineering and safety systems in the aircraft themselves.

If your organisation has established processes for business continuity, or other practices to handle outages or other panic scenarios, then make use of those to start with. Key to the organisational response is being prepared for diminished health, reduced availability and outages by responding correctly and learning from failures, rather than running around in panic during a failure and wondering what to do. The availability model should at least highlight the organisational processes as shown below:

* Plan — the [health model](#HealthModel) and [operational model](#OperationalModel) are key parts of the planning for reduced availability. Extensive coverage of these models will enable better recovery and part of the overall organisational drive is to ensure that sufficient effort is put into these models.
* Recover — again, the health and operational models should contain the detail, including both the organisational and technical aspects for recovery from failures. The models don't cover organisational aspects such as ownership, accountability and resource plans (for recruiting, training, coverage of shifts, and so on). These additional aspects may need to be covered, or at least introduced, to existing functions within the organisation that need to deal with them.
* Review — after failures that don't meet the agreed SLAs, a review of what went wrong needs to take place. In addition to root cause analysis and determining a technical reason for the failure, other contributors to failure which are organisational in nature must be looked at. Perhaps key people were not available or had not transferred enough skills? Maybe the increase in traffic was expected by the business, but operations were not notified (see the [lifecycle model](#LifecycleModel))? Whatever the reasons, they need to be looked at objectively and lessons fed back into the models, or changes made to the application and operating environment, in order to reduce the likelihood of similar failures in the future.

### Operations role
Operational staff are ultimately tasked with the responsibility for application availability. The processes within operations, and their relationships with business and development teams, are critical to application availability. Apart from technical ability, tooling and monitoring, operations needs to be able to respond quickly to diminished health. This needs to be done with a clear and rapid understanding of the business impacts, the root cause and a way of recovering the application without making things worse. Depending on the nature of the fault, this may need to be done within a high-pressure environment that may not be conducive to meticulous planning and the clear thinking (including the consequences of actions) required to respond. While some problems could be fixed quickly by internal operational staff, in some cases operations may need to request help and exert pressure on other parties, such as suppliers or the development teams. The ability to transfer some of the recovery tasks to a third party, while maintaining ownership, buffering others from pressure and co-ordinating the recovery is one of the most difficult skills that operational staff require.

### Developer role
Developers have two primary roles in availability. The first is to engineer the required availability, and the second is to be on-hand to fix problems in a production environment. The culture of building availability is difficult to foster in a development team as it may not be seen as the most important. One of the ways to get developers more concerned about availability is to make them 'feel the pain' by increasing the role that they play in day-to-day operations. The practice of developers performing more 'devops', as part of their daily tasks, is becoming common in modern applications. Developers, after all, have the most knowledge about how their part of the system works and are best suited to address problems. Additionally the rapid release cycles of services within an application mean that developers have to take a more hands-on role when rolling out new functionality.

This approach may not work in many organisations that are structured to more traditional principles. In these environments, the interface between operations and developers needs to be clearly defined so that when a failure occurs, developers can be included in the recovery (to determine the root cause or fix a defect). Processes between operations and developers need to be clarified upfront and not made up during a major outage. 

## Availability as an engineering discipline
The building of highly available systems is not new and has been practiced for decades in other engineering disciplines such as telecommunications, where satellites in space need to be reliable. What is new is that mainstream applications that previously only had to serve a handful of users during working hours, are now having to handle millions of users constantly (and for a significantly reduced cost). Those same users are highly intolerant of application outages or poor performance. As a result, architects, software designers and developers who would normally not have cared much for availability, now have to turn their attention towards it.

Fortunately, availability (and scalability) are becoming widely accepted as core skills. The previous generation of availability principles, such as Joe Armstrong's ['6 Rules of high availability'](http://www.infoq.com/presentations/Building-Highly-Available-Systems-in-Erlang) (Isolation, Concurrency, Failure Detection, Fault Identification, Live Upgrade and Stable Storage) are being rediscovered and applied to modern application architectures.

Availability is also emerging within products, particularly NoSQL databases, where principles of availability are embedded within the products. This is also a source of good engineering practice. Many of these databases are built with cloud deployments in mind and cater for scalability, eventual consistency, distributed data, high throughput and other aspects required by highly available systems. The continued innovation and addition of new entrants into the market, accompanied by detailed analysis and discussion, also adds to a wider discussion of availability principles amongst developers.

Massive web properties such as Google, Twitter, Facebook and Netflix are pushing the boundaries of availability of web applications and frequently share their lessons in open forums and developer-focussed conferences. They have also founded open source projects of the products that they have developed to solve their problems. [Cassandra](http://cassandra.apache.org/) comes from Facebook; [Hadoop](http://hadoop.apache.org/) comes from Yahoo (based on Google papers). These projects, with their roots in highly available applications, lend credibility to availability engineering practice. Many of these web properties are seen by developers as 'cool' and their approaches are emulated by developers worldwide.

## Summary
Modern applications have a significantly higher expectation of availability due to large, popular applications doing it so well and users increasingly moving towards web based applications for daily work, consumption and social engagement. Applications that fail now have their outages flashed on Twitter and discussed at length on Hacker News within minutes. This creates serious perception problems for any application or service trying to gain market share. Any application launched needs to pay serious attention to availability to the extent that it becomes one of the primary application features, not just a non-functional requirement added to the bottom of the nice-to-have list.

Cloud platforms such as Windows Azure do indeed provide some of the availability needs as part of the service but only at the infrastructure level, and at the expense of performance and latency. Application developers still have a lot of work to do to build available applications on top of that infrastructure. Building such availability is non-trivial and can be very expensive in terms of development effort and skills required, so the challenge remains to get the balance between availability engineering and cost.

Going through the process of building the availability model forces attention to be placed on the availability requirement; so that the underlying business rationale can be understood and cost-effective techniques and approaches developed to satisfy the availability needs. The involvement of all stakeholders in building the model (business, application architects, developers, testers, operations) ensures that everybody understands their responsibilities and the role that they play in implementing an available solution.

### Steps
1. Develop the availability SLAs.
2. Understand the cost implications of implementing availability, so that this can be communicated clearly to the business.
3. Understand the business rationale for availability
4. Select the appropriate development approaches for availability based on their ability to satisfy the requirements within the project budget.
5. Create a focus on availability across the development team and the rest of the organisation, which matches the availability expectations of the application.


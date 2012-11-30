# <a name="LifecycleModel">Lifecycle Model</a>

One of the primary benefits of cloud computing is the ability to handle non-linear workloads, as depicted in the well-known diagram below of the four main reasons for irregular load.

![Patterns of non-linear load](./images/Lifecycle-009.png)

With linear (or slow, steady growth) workloads, the cost benefits of cloud computing over an extended period of time cannot be realised and the TCO model seems to favour more traditional hosting architectures. Since the ability to handle spiky traffic is a primary reason for the move to cloud computing, it makes sense to model the system for the expected load whilst it is operational. 

There are many important reasons to develop the lifecycle model, aside from understanding the cost benefits of a pay-per-use model. These include: 

* Influence on architectural decisions — developing for 'infinite scale', or some other vague indication of the requirement, may be unnecessary. The architecture needs to be developed with realistic expectations in mind. This greatly influences the approaches of the management of state (within the data model) as it is the most complex part of the architecture to scale.
* Load and performance test requirements — simple statements such as 'the system should handle 10 million page impressions a week with a response/render time less than two seconds' fail to take into account varying workloads over time. This can lead to a testing focus that is too narrow, resulting in unexpected failures when the system goes live. The lifecycle model provides input into load tests that need to be conducted for varying workloads.
* Operational readiness — while a business may understand expected load because it is obvious to them, it may be less obvious to operational staff. This results in a lack of communication (coupled with the myth of instant scalability) and an inability of the system to respond to spikes in demand. A lifecycle model captures demand over various periods of the application's operational lifecycle. It allows operations to plan accordingly by scheduling maintenance, pre-initialising capacity (and the reverse — reducing capacity), staffing the support desk and other measures to prepare for usage increases.

## Developing the lifecycle model
The lifecycle model should be fairly simple to develop as most of the information required should exist. It may be difficult to tease out of participants however, not because they are recalcitrant, but because they have not had to consider the impact of business events on applications with such rigour. It is best to sit down with as many people from the business as possible, from various departments, to hammer out the details.

Development of the lifecycle model requires a clear understanding of usage depending on the standard consumption of the product, as well as the effect of events on top of usual business. It is also important that you are able to understand the effect of usage across the various decomposed workloads, as it may not be uniform.

Although it may be simple to list the factors, it can take a while to consider the detail of every single factor, and the impact of a number of factors occurring simultaneously. Rather than getting bogged down trying to think of every possible scenario, try and identify the factors that have the highest impact or which are most likely to occur.

### Capturing phases and events
The most important part of the lifecycle model is to capture the phases and events that impact the usage or availability of the application. Most of them will be product based (customer, business or market), such as the daily lifecycle exhibited by users in a handful of time zones. Other phases and events will be technical in nature, such as the deployment of a new release.

#### Phases
![Phases](./images/Lifecycle-001.png)

Phases are changes in usage or availability that are spread out over a longer time period. Most applications exhibit higher demand phases depending on when users are most likely to access the application and where they are based. The above example shows two distinct daily phases during peak hours. Some phases are predictable, such as rush-hour events, some may be planned, such as the rollout of a new marketing campaign, and some may be the result of unexpected events, such as a news item that drives traffic.

#### Events
![Events](./images/Lifecycle-002.png)

Events happen at a specific point in time and can have either a short or longer-term impact on usage or availability. The example above shows the event of 'Sale starts' will occur at a specific point in time and the application will be subject to additional load as the sale continues. Most events are followed by a phase; the event would be the start of the sale and the phase would be the time that the sale is running. Some events can have a high impact that is short lived, such as a celebrity Twitter mention, which may cause a massive peak load but things return to normal very quickly. Technical events, such as deploying a new release to production, may have no impact on usage and availability at all but still needs to be recorded and planned for in the lifecycle model.

### Business factors influencing usage
The application will be measured against business factors and as such they are the most important factors to include in the lifecycle. They form the basis for the domain knowledge on which the business competes and differentiates itself and, to a large extent, they are out of the control of the application designers.

#### Business as usual
Much of the spikiness of the application can be put down to normal business conduct. Behavioural patterns of target customers, a response to marketing, product launches etc. are all part of what the business is used to or should have uncovered in the research for their new venture.

If the application (or something similar) already exists within the business, valuable data in the form of web analytics will be available. Check the quality, accuracy and validity of the analytics and use it to gain key metrics and insight, rather than relying on it as a single source.

##### Temporal cycles - Daily, weekly, monthly and yearly 
By far the most common lifecycle to describe is related to cycles that repeat over time. This is often directly related to the product of the application itself and when consumers will interact with it. For example:

* Daily cycles — a weather application may be under load in the morning when people are getting ready for work, a news application at the beginning of the working day and a gambling application at night. 
* Weekly cycles — a sports application may be more popular at weekends and business news during the working week.
* Monthly cycles — luxury shopping may be under more load after payday and online banking at the cusp of a month.
* Yearly cycles — fashion shopping, gift shopping and travel are highly seasonal, but there are some less obvious cycles, such as academic years, sporting seasons and so on.

##### Planned usage spikes
Many surprise traffic increases are actually planned by the business but they simply failed to warn IT, either because they 'forget' or assume that the systems will cope. These planned increases in usage can be the result of:

* Marketing campaigns with a specific launch date for print, TV, web or other media.
* Product launches that may or may not have a formal campaign around them.
* Conferences or other events where the product may get coverage.
* Promotions on products, such as discounts to remove excessive stock.

Any warning about planned spikes is better than none. It is easier to prepare for extra capacity, even with as little as one hours' notice, than it is when the application is under excessive load with operational staff potentially misdiagnosing performance issues.

##### Internationalisation
An application that caters to an international audience will respond differently to one that is local. It may be difficult to understand the behaviours of distant customers, but it could be a worthwhile exercise for bigger target markets.

* Time zones - an international application may have usage that 'follows the sun', with spikes coinciding with different times of the day around the world. While this may average out the usage, it could be compounded where an evening traffic spike in one region coincides with a morning one in another.
* Localised events — a business operating in Europe may not know of events occurring in the United States that affect usage or vice versa. Europeans, for example, are oblivious to Superbowl Sunday.
* Cultural and social differences - it is difficult to understand what a foreign market finds interesting, responds to or even what their patterns of application usage are.
* Platform differences - in one region customers may access the application on a laptop and in another region they may prefer a mobile device. This can affect behaviours and bias usage towards particular workloads.

#### External influences
While some usage increases may be in the business's control, others may not. Indeed many business models for applications, such as news-oriented applications, are based on external factors. While in most cases it is impossible to predict when these events occur, it is important to identify the types of events and their probable impact when developing the lifecycle model. This will enable the architecture to be tailored accordingly and provide some mechanism for operations to increase capacity in time, or be able to decrease capacity once an identified event has run its course. Examples of events include:

* External media — most obviously news sources, but could include popular TV programmes or sporting events that impact the customer base
* Business markets — even niche applications may need to respond to movements in business markets. This may affect business users as well as consumers.
* Expected events with unknown time — sometimes things are guaranteed to happen, it is just difficult to know when. For example, car insurance claims may drastically increase when it rains; you need sufficient capacity for when it rains, but don't know exactly when it is needed.

#### Cross workload contagion 
In some cases, it may be easy to understand the knock on effects of increased usage. If the application has one web role, any increase in one part of the web application will impact all parts of the web application. Indeed, the reason for workload decomposition is to insulate the impact increases in usage in one workload from the other. For example, a web application that places messages on a queue for processing will not have an impact on the queue reader if the load is high. The number of items in the queue and the length of time each message spends in the queue will simply increase.

In other cases, contagion across workloads is unavoidable and is often related to shared state. The obvious example would be shared database access, but these are usually engineered out of the application anyway. Occasionally we will see business events (as part of a workload) impact other workloads. For example, if an e-commerce application performs a large update to the product catalogue (e.g. increasing prices) the load this places on the catalogue index may impact the searching functionality of the website. This may not be obvious at first glance.

While it is not necessary to try and map out all of these impacts in the lifecycle model, it illustrates the importance of uncovering the events that occur in the business so that the application and the architecture can reduce the contagion of one workload by another.

### System events in the lifecycle model
Not all workloads and events that put the system under load, or affect its availability, are from end users. The lifecycle model needs to consider these system events as they can have an impact on the performance and availability of the application.

#### Releases
With new or evolving applications, new releases may be frequently pushed to production and could influence:

* Increased customer usage because new features are being tried out. 
* Variations in the impact on the underlying infrastructure due to code, data model and other changes.
* Instability from insufficient testing or new edge cases requiring developers or other resources to be on hand.
* Version differences between various components
* Schema changes and data migration to new schemas

#### Upgrades and patches
Perhaps with a lower impact than releases, small upgrades, patches, bug fixes and similar activities can affect the production environment. These cannot be easily predicted and usually have a sense of urgency. It is worthwhile to document the people that need to be available or on call when patches are made within the lifecycle model, as well as follow-on activities such as testing.

#### Testing
Testing in cloud computing differs from traditional environments in the sense that different testing needs to be done and it is on the same infrastructure as the production environment. For example, testers may want to run tests from a different region on a production environment to see what realistic latencies will look like. The test strategy needs to be considered when developing the lifecycle model so that testing activities do not come as a surprise to operators or customers.

#### Scheduled maintenance tasks
Although cloud computing applications are engineered to have little need for what would be considered maintenance within a normal data centre, some activities do need to be catered for in the lifecycle model. On Windows Azure in particular, the need for operating system patching, clearing out of log files and similar maintenance tasks are unnecessary. Maintenance tasks that may exist include:

* Database backups and shipping to on-premise site.
* SQL Server specific maintenance such as index rebuilding.
* Cache invalidation.
* Poison message or dead letter queue clean-up.
* Incomplete transaction clean-up (such as expired registration confirmations).

### System workloads versus normal workloads
While in most cases it is easy to classify a workload as 'system' or 'business', occasionally 'business' workloads can be lumped together with 'system' just because they seem technical. This means that the lifecycle model can be incomplete because insufficient understanding and planning goes into these workloads. Examples include:

* Integration batch runs — typically run overnight and import data into the system. Importing data using cron jobs is seen as technical, but the business has a need for the data. Some import processes are time dependant, can run for a long time, put the database under load and have an impact on data consistency.
* Data aggregation — a cron job that takes data from the day and summarises it. Again, it can create a high load on the database, but quick access to aggregates information the next day is required by the business.

These types of workloads should already be defined in the workload decomposition, but the scheduling of their execution is part of the lifecycle model.

## Describing lifecycles
One of the biggest benefits of cloud computing is the ability to scale and handle any load an application may be subjected to. So why is it necessary to detail the application and workload lifecycles? Cloud computing applications still need to be engineered for scalability and data about lifecycles is vital in order to make the correct architectural and design decisions. In the context of Windows Azure, just because the infrastructure and platform scale well (as provided by Microsoft), it does not mean that the application will do so automatically.

It becomes necessary, therefore, to describe the lifecycles sufficiently in order to provide input for design, development and operations.

### Describing load
It is difficult to find a metric for describing load that is meaningful across all workloads, as they will respond differently depending on the underlying components and architecture. For example, a web workload will be linearly affected by the number of users. A cache is not similarly affected by the number of users, but is affected by the number of cache misses, and the database is only affected when users post data to the database. The number of 'users' or 'services' calling the application will be the most generally understood metric. 

In some cases it may be preferable to use another metric, depending on what is relevant to the technology or when existing data is available. Web applications may use page impressions, databases may use transactions and in message oriented applications it may be preferable to use the number of messages. It may be necessary, during design, to convert the number of users (as confirmed and understood by business) into more technical metrics. For example, for a particular use case, a single user performing an action may translate into a number of messages, database operations, cache hits/misses and page impressions.

### Graphing load
Graphs showing load over time are a useful part of the lifecycle model.

* Graphs can be drawn quickly on a whiteboard.
* Graphs convey a lot of information visually — making them easy to understand and quick to interpret.
* Graphs can be understood by everybody.

As important as graphs are, they should not be over used or overtly relied upon.

* If the source is a scribble on the whiteboard, graphs are likely to be inaccurate.
* They are difficult to redraw in digital form.
* Engineers will try and decode the curve, when often there is no meaning to the curve.
* Graphs should be used in situations where information about load needs to be conveyed quickly, or the source of the graph is real data.

### Describing reduced availability
Most lifecycle phases and events will be described and designed for an increase in user load. Reduced availability also needs to be understood, such as increased response times, increased latency or the inability to process certain transactions.

Reduced availability can be caused by:

* Deployments — where parts of the application are unavailable or underperform while the deployment is in progress.
* External services — may not be able to handle the peak load that your application can, either due to infrastructure limitations or even a contractual arrangement that supports a maximum throughput.
* Maintenance tasks — cloud applications should be engineered for automated or negligible maintenance tasks that affect availability. In some cases, such as when there is legacy code, it may be unavoidable to need to run tasks such as database index rebuilding.

### Describing capacity
Cloud based architectures are designed to scale and the available capacity should not be a limitation that needs to be considered. There are cases where compute capacity is limited, such as when there is a fixed budget for compute resources, or use is made of bulk purchases of capacity (e.g. [Windows Azure 6-month plans](https://www.windowsazure.com/en-us/pricing/purchase-options/)).

The biggest limitation on capacity is the available operational resources and their skill levels. Describing the operator working shifts and even their holidays is important; planning a marketing event that increases traffic when operators are unavailable is not a good idea.

### Describing the nature of phases
Not all phases are the same; some can be short and abrupt whilst others can be long and drawn out. Describing the primary phases in as much detail as possible will help immensely when architecting the solution.

#### Load
Describing the load over time is the most important part of the phase. The load should be expressed in a metric that is understood by everyone on the project and can vary between workloads. For example, the web front-end may be described in terms of page impressions per second and a back-end service may be described in terms of messages processed per second. Variations can also be found which are feature specific, such as expressing searches per second or product page views per second. Not all features or workloads need to be covered in broad detail. Pick the ones that are of concern to the business and their targets and those that are likely to place significant load on compute resources, such as image processing or complex database queries.

The description of the load should include the peak value, which is ultimately what the application needs to handle. If other values are meaningful, such as average, they can also be provided.

#### Duration
The duration of the phase needs to be described. This can be in seconds or even months (seasonal businesses have phases that last long periods).

#### Shape
Not everyone is able to express the shape of a curve in the correct mathematics or using the correct term, but knowing what the shape looks like and being able to express it is important. The example below shows some basic shapes that are significantly different and tell us a lot about the load over time.

![Shape of phases](./images/Lifecycle-003.png)

#### Technical metrics
Describing phases using technical metrics such as I/O stats, cache hits and misses, and CPU load should be used very carefully. They may be useful if the data is available from a legacy application and is well understood, but can lead to confusion and irrelevance because very few people on the project team understand them. The exceptions may be latency (in milliseconds) and bytes transferred, which seem to be well understood technical metrics, due to a higher level of consumer understanding of how they impact user experience and costs.

### Describing events
Similar to phases, events also need to be described, but in less detail. Events generally precede a phase (and the phase is the important part to describe) or they are unpredictable, both in terms of when they occur and what their impact will be.

Events that are part of phases can be described as such. An 'annual sales' phase, for example, will have a start event (announcement or time of sale start), a peak (depending on the demand it could be soon after the start or days later if it relies on word-of-mouth), a long tail (as stock gets depleted or most customers have seen the items on sale), and an end (when the sale closes).

Events that are less predictable also need to be described. These events are the result of an external influence, such as a relevant news item or a positive review by an influential reviewer. For less predictable events, try and describe:

* Probability — what is the chance of the event occurring? For some unlikely events (such as being mentioned by Lady Gaga) don't bother describing the event. Other events may need to be described, even if their likelihood of occurring is low (such as your star actor being caught up in a scandal), so that you can be prepared for an application response.
* Event window — when is the event likely to occur? If it is most likely to occur during working hours, then lining up operational staff will be less of a problem.
* Magnitude — events that cause a 5% increase in load are not worth describing. Make realistic estimates on the magnitude of the impact on load that the event will have.

### Describing product lifecycles
Product lifecycles are understood by technical members of the team and should be easily pulled together by the Project Manager. The actual application, whether whole or in parts (services) will be launched, fixed, upgraded and eventually decommissioned. This should be described within the lifecycle model as follows:

* Beta release — whether public or private beta, it will be deployed at some point and need to handle load, but availability will not be important and load may not be high.
* First release — depending on the application and the marketing of the launch, the first release can put the application under such high load that it collapses (see the launch of the [Pottermore web application](http://insider.pottermore.com/2012/03/waiting-for-pottermore.html)).
* Bug fixes — fixing of defects, whether done as scheduled releases or as emergency deployments, can impact availability.
* Additional features — changes that users are waiting for, or UX changes that require users to learn new things, can increase load or support queries. 
* Termination of services — while the entire application may continue to exist for a long time, some services may be deprecated from time to time and dependent services migrated.

## Example
Consider an example of a web application that takes holiday bookings. This is a fictitious example to demonstrate the questions and outputs of the lifecycle model.

The first step would be to extract long-term figures out of the business plan. The business plan would not be based on page impressions, but on revenue, which would be related to the number of bookings. Some translation between the business metric (bookings) and a technical metric (page impressions) may be required. This example also shows relevant events (launch, added features).

![Long-term plan](./images/Lifecycle-004.png)
 
Holiday bookings are seasonal, so we expect to see different phases throughout the year. The vertical axis should depict something that would translate under stable load, such as average number of page impressions per day. The example below shows the annual 'holiday booking season' phase, the 'late booking' phase and the 'winter quiet period' phase, pointing to a need for higher availability during the booking phases.

![Annual phases](./images/Lifecycle-005.png)

Perhaps people book holidays when they have the Monday blues or when they are planning a last minute weekend away. So there will probably be a weekly lifecycle, as illustrated below.

![Weekly lifecycle](./images/Lifecycle-006.png)

The daily lifecycle is the most commonly seen and understood. In the example below, the application is for the US and has virtually no traffic at night. Additionally peak times for making holiday bookings (such as at lunchtime) are spread across a longer period from the east to the west coast.

![Daily lifecycle](./images/Lifecycle-007.png)

It is good practice to produce views on all of the primary workloads identified in the lifecycle model, as they may have completely different usage patterns. It is not necessary to describe them on the same graph, but it can help to illustrate the differences. The example below shows that bookings mirror searches (but with a lower number) and a data import happens overnight.

![Multiple workloads](./images/Lifecycle-008.png)

## Summary
Too often developers are asked to build an application with only a vague idea of how much load it is going to be under and when. This results in a solution that is not fit-for-purpose, either over-engineered or completely unable to cope with the volume of traffic. While cloud computing solves this problem to a degree, architects, application designers and developers still need ballpark figures to work with otherwise they can land up with a completely inappropriate design. Legacy applications have useful historical data, but new developments have virtually no insight as to the expected load. This lack of insight is often because the business has not thought things through in enough detail. They may have some target figures and 'everyone knows' that traffic will be light after hours, but these loosely defined numbers are not enough on which to base an application architecture. Developing the lifecycle model forces poignant questions to be asked of the business, and how the business finds the answers can help their understanding of the market, their customers and the application.

Developing the lifecycle model early in the design process is a good place to kick-start the architectural approach and provides a meaningful indication of what needs to be built.

### Steps

1. Understand the business factors that influence application usage.
2. Understand system events.
3. Identify relevant lifecycles.
4. Describe the lifecycles.


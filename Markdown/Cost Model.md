# <a name="CostModel">Cost Model</a>

The costs of hosting an application in the cloud are often given a cursory look during assessment, and then largely forgotten about until the big bills arrive. Unfortunately, once high bills start to arrive, it is too late to change the application enough to get them under control. Developing a cost model and an approaches for dealing with costs are integral to the application lifecycle. Traditional applications provide the luxury of being able to work out detailed costs in advance and then forget about them until the application has to be migrated to a new physical platform. The variable costs for cloud applications do not provide this luxury and have to be part of the architecture, implementation and operations of the application.

## Costs differences in the cloud
Of all the differences between cloud based applications and traditional applications, the cost differences are the most well known. Virtually everybody understands that public cloud services are paid for as they are consumed. This makes the cost models very different, but also influences architecture, development, operations and virtually all aspects of building and operating a cloud based application.

### "No upfront costs. Pay only for what you use."
The pay-per-use model is the most widely known and, at least at a high level, most widely understood aspect of cloud computing. This level of understanding and the description of the pay- per- use model are extensively discussed, documented and analysed all over the web and even in consumer oriented media. It is assumed that the reader is familiar with this model and an overview of this model is not included here.

### Service consumption
Beneath the hyped, pay-per-use model is a less understood, but more fundamental, difference. Cloud platforms do not sell infrastructure, they sell services. It is possible to purchase physical infrastructure on a pay-per-use basis, from traditional web hosting suppliers, but this is more a financial arrangement than the purchase of infrastructure or platform services. 

Everything that you buy on the cloud is purchased as a service. This means that there are a lot of things surrounding the basic offering that are bundled as the service. Part of the service would obviously be power and cooling, but it might may include operational services to maintain the platform, networking services, storage, billing and other support services. In the case of Windows Azure, services are also included that facilitate the availability of the application, operating system patching, integration services and others.

This abstraction of physical infrastructure to services requires a mind shift for architects, developers, operators and even business. While conceptually it may seem simple, it does have far reaching consequences that are not immediately apparent. For example, a provided ‘service' may only have certain network throughput associated with it, and nothing can be purchased to make it faster, because no ‘go faster' service exists. This means that implementers need to be acutely aware of the services available, how they are consumed, what their capabilities are, and how much they cost.
 
### Self-service
The self-service aspect of cloud computing has two side effects that are relevant to costs: 

* The consumption of services by individuals can get out of hand. For example, developers are able to create implementations that use more roles than necessary because the capacity is seemingly available for them to use. Testers can spin up hundreds of instances used for testing and leave them running idle for weeks on end.
* Additional costs that would normally be rolled up in internal IT can be incurred by the project. Operational tasks that need to be done on the management portal, or using the Windows Azure Service Management REST API, may need to be performed by members of the project team because internal IT has no interest or skills. These costs could mount up as they may require a dedicated person for the entire project development duration.

### Need for understanding of costs
Traditional applications require that only a few people are aware of and understand the costs. Most of the implementation team, particularly developers and testers, have no interest, exposure or use of physical infrastructure pricing information. Yet the costs of cloud applications are far more directly influenced by small decisions made by individuals on a day to day basis. For example, the method that a developer chooses to access objects in storage using a transaction per object or batching a bunch together can have far reaching cost implications. While most of these costs are trivial, costing pennies per transaction, they can add up when the volume is high.

A lack of understanding of costs can also mean that individuals make decisions based on incorrect assumptions on the cost implications, without thinking them through and considering whether or not the effort is worthwhile. For example, a developer may spend days implementing a sophisticated inter-service protocol that compresses data in order to save on data egress costs only for it to turn out to be a cost saving of $5 a day in a live environment; hardly worth the cost to develop and the increased risk associated with un-maintainable code.

Costs need to be understood by all members of the implementation team.

* Architects need to have the best, up-to-date understanding of the costs. Many design decisions have to factor in development and operational costs as trade-offs so a firm grasp of the implications is vital.
* Developers, particularly senior developers and feature leads, need an understanding of costs at the granular level. They should be aware of how much things are going to cost when working with large numbers of transactions (such as Access Control Service transactions) or coming up with designs that spread processing or storage load (such as database sharding)
* Business should have more understanding of costs than they are used to, or are comfortable with. Design and implementation options presented by architects require an understanding of costs in order to assess the long term financial impacts of the trade-offs. After all, it is the business that will pay the monthly bill, so they need to be sure the costs are what they were expecting.
* Testers need to understand the costs so that their results from testing during development are fed back into the cost model. They also need to be able to determine the value or feasibility of conducting long running soak or scale tests.
* Operators need to understand the cost implications of scaling the application in a production environment so that it can be factored into their availability processes. They also need to know how much it costs to get large amounts of data on-premise for backup or integration purposes.

## Opportunities presented by cloud cost models
The pay-per-use service method of acquiring, utilising and disposing of compute resources is revolutionising the application landscape. It allows applications to be developed for new markets, with new features and greater reach than ever before. Cost models are fundamental to opening up the opportunities, and most cloud assessments start to become interesting as soon as the hosting costs are estimated.

Cloud computing cost model opportunities include:

### Pay-per-use
Pay-per-use is the most important and well-known opportunity. As mentioned under ‘differences', the ubiquity of information and analysis of the pay-per-use model means that it is unnecessary to cover it here. 

### Higher awareness of costs
You would think that the availability of unlimited capacity would mean that people would disregard the cost implications of what they are doing, but it seems that the opposite is true. Every time a developer suggests putting some code in a separate role, they tend to think about the cost of a role, multiplied by the number of instances and the fact that it will cost money. This causes developers to second-guess many implementation decisions. This is not obvious, time consuming or energy sapping, but does seem to occur and is leddriven by senior developers and architects who are always asking the cost question. Perhaps it is because of the granularity of the costs, where they are at a level that developers are aware of their influence, rather than a tiny blip on a huge machine, that makes people aware of the costs. Sometimes the influence is small, and sometimes great, but there is no doubt that the cost models offered by cloud computing have a huge influence on the architecture of the application being developed.

### Cost optimised applications
While it may not be desirable to be continuously optimising for cost, some applications do have the requirement to drive down costs. Maybe the business model is based on a resource-intensive, low-margin business, like video processing and storage, and requires that the application aggressively drives down costs. The cost models allow creative solutions to optimise costs, such as making use of resources for brief periods and only when needed.

### Price reduction over time
As cloud providers compete with one another and hardware gets cheaper, cloud services are also getting cheaper. While this makes perfect sense, it is common for enterprise IT to have static costs that don't drop for years at a time, due to the long-term leasing of what will soon be outdated equipment.

## Problems with cloud cost models
The freedom of cloud cost models brings its own problems.

### Immaturity of cost modelling tools
The current state of cost modelling, billing information and other necessary tools to facilitate the management, control, understanding and modelling of costs, is wholly inadequate. Customers are presented with a bill at the end of the month with little insight as to how it came about. Yes they can see the detail of what was billed, but it may not make sense in the context of overall load for the application or revenue generated from the application.

Over time, cost modelling tools have to improve. Businesses are still coming to terms with cloud operating costs at a conceptual level and haven't started asking vendors for the tools that they need. At this stage, we probably don't even know what the real cost modelling needs of business are.

For now, each team will have to make do with a hand-rolled, spreadsheet model and use this to get on top of the costs.

### Business case mismatch
While technical people may get excited about the availability of infinite capacity, that enthusiasm will probably not be shared by the business exposed to the risk of an infinitely large invoice.

There is a risk that the implementation team engaging on their first cloud project will build an architecture that is misaligned with the business case. Scaling up of the number of instances may be easier to consider against the business case, after all, increased load should have some business value. Creating a lot of isolated roles with persistent message based interfaces may be architecturally correct in the scenario, but may not be supported. The business case may not want such high availability, fault tolerance, or even performance under load.

Traditional applications, with a largely fixed infrastructure, have long lead times for increasing capacity that are accompanied by meetings and detailed cost/benefit analysis. The absence of such processes, and the degree to which costs are influences, are already coded in and is a problem that needs consideration.

### Costly surprises
Unmonitored costs can result in nasty billing surprises. Costs need to be monitored at multiple levels, from the actual code (for example, the efficiency of storage transactions) through to deployment (for example, under-utilised instances). If they are not actively monitored and controlled, costs can build up and become difficult to track and fix.

Cost surprises can also be thrown by unexpected load, or an unexpected type of load. With pay-per-use billing, access to a resource that is unplanned can result in huge bills for little return. For example, if a publicly facing page had a video served up using the applications bandwidth and suddenly went viral, what would happen to the costs? Potentially tens of thousands of dollars would be spent just to serve up something to non-customers, and, if there was insufficient monitoring, would only be picked up in the billing run.

### Need to understand detail
The [pricing information page on Windows Azure](https://www.windowsazure.com/en-us/pricing/details/#) is relatively simple. TThere are less than thirty individual prices across all services, which is a short list, when you consider how many different SQL Server SKUs exist (or any other Microsoft product). But that apparent simplicity hides the truth. The complexities are in the detail. It is not more detailed pricing breakdowns where the complexities lie, but how things are counted. When is a ‘transaction' counted or not? How is database storage calculated per hour? Is a staging instance counted? When *this* service talks to *that* service, is the data transferred counted as data out?

These details need to be broadly understood by the implementation team from developers who make lots of decisions every day that affect the cost, to testers who need to reconcile their tests against expected billings. Unfortunately, these details aren't very accessible as there is no single place that describes all of the details. The information may only exist on blog posts that may or may not be found and be up-to-date (see [this post on storage billing transactions](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx)).

These low level complexities, combined with the immaturity of the cost tools and the inaccessibility of information, mean that a lot of effort needs to be invested in making sure everyone understands how the costs work to the right level of detail. This may be a problem for less experienced teams, where even those who should know, are unaware of the details.

### Disregard of costs
Developers are notorious for disregarding the hosting costs of the applications they build. A culture exists that more hardware can be thrown at the problem, and in a developers eyes, hardware isn't that expensive. In many ways, it is acceptable in traditional environments for developers to ignore the cost details. For example, in an on-premise datacentre the cost of sending a message across the network is probably not measured, so has an irrelevant cost from the developer's perspective.
This disregard of costs, across all team members, doesn't transition well to cloud applications, where the awareness and control of costs is more important. Not only do we have to deal with communicating the complexities of costs, but we have to alter the apathy towards cost concerns.

### Reconciliation of billing against architecture
Related to the lack of cost model tools is the difficulty in knowing how architectural decisions impact the cost. Initially it is optimistic to estimate the cost of architectural decisions. The initial assumptions become invalid as the design moves through to development accompanied by scope change, where the view on costs and the architectural view diverge. An architect can look at a monthly bill of services on the one hand, and look at the architectural representation on the other, and fail to come up with a meaningful explanation of the costs against the architecture. If the business is complaining that the costs are too high, or wants to plan a future release, how does the architect address costs if there is no easy way to tally the two?

On-premise, things are simpler, with fewer services, fewer physical machines and an easier way to reconcile the architecture and the cost. Traditional applications have a physical machine, or a few machines, per tier and a database in the back corner, so architects have a fairly good idea how design changes will affect the number of machines required.

The integration of architectural, development, billing and operations tools does not exist, and is unlikely to exist any time soon. This creates additional load on architects and senior developers to try and keep track of the knock-on costs that result from implementation decisions. 

### Over-optimisation of costs
All this attention on cost ramifications can result in decision paralysis where implications are over-analysed. If you put three people in a room for a couple of hours discussing a cost issue, you should be sure that the savings are significantly more than the cost of having people discussing and assessing them.

Developers can become too focussed on cost and prematurely optimise when it is unnecessary, which takes the focus away from delivery. Testers can decide not to perform a test because of the cost to run it, at the risk of letting a serious defect through the cracks. Operators can delay responses to diminished health, while they work out how much it will cost to scale up, leaving paying customers frustrated.

Team leaders and managers need to make everyone aware of cost considerations, but also strike a balance with what is practical. It is important to remember that the hosting costs of an application are only part of the overall liability. Delivering the best product quickly, for example by optimising for development costs and time to market, is often far more important and financially relevant.

### Licensing of commercial software
Many commercial software packages are built for a non-cloud environment and are intended to be hosted on manually configured and stable hardware. Licences don't always translate easily into a cloud environment, where servers have automated installs and run redundantly or in a load-balanced configuration. As a result it can be difficult to license third party software to run on a cloud platform and the costs can be prohibitive, especially where the software itself requires a licence key that is generated for each machine.

### Complexity of cost model
Traditional computing environments may require effort up front to sort out the costs, with quotes, payment plans and discounts from various vendors. But this will be based on an estimate of what hardware platform is needed to run a particular load. Once the order is placed, the platform costs for the application become relatively fixed until the application is launched, and implementers do not have to worry too much about cost implications. Cloud applications are far more dynamic and have costs influenced by implementation decisions throughout the development process, as well being directly and discreetly related to load. This is ultimately where the cloud computing cost complexities lie, as the opportunities offered by a pay-per-use cost model are accompanied by inherently complicated fees.

## Developing the cost model
An awareness and understanding of cost issues is mandatory. Actively developing, fine tuning and communicating the cost model for the application should be enough to ensure that cost issues receive sufficient attention without stifling creativity, innovation and rapid delivery. The following sections describe the contents and activities for the cost model, which should be seen as a necessary, but not overbearing, part of the application lifecycle.

### Clarify the business stance on costs
The pay-per-use model of cloud applications is often the most interesting part of the proposition to business. In other cases, the scalability may be of interest to the business and the costs are secondary, as they will map linearly to their income. The first objective in cost modelling is to obtain a clear and definitive statement from the business about the permissible costs, and how the business case is, or isn't, supported by optimal costs.

It may seem pointless to engage with business to get a statement about costs, after all, business is always going to ask for the cheapest possible. It is necessary to discuss the costs with the business in order to establish a working framework and have business answer the difficult questions on how much they are prepared to pay for features. For example, the business may ask for a level of availability that is impossible without (expensive) geo-redundancy and, when pushed, may actually realise that such high availability is not really required.

### Identify the owner of cost model
Cost issues run as a common, yet thin, thread across all disciplines within the implementation team and most individuals have too much else on their plate to worry about backfilling cost-related data. Neither project managers nor product owners have sufficient technical depth to understand the cost model in sufficient detail to take day-to-day responsibility, so a technical person needs to get involved.

A technical person should be assigned as the owner of the cost model, but it does not have to be the most senior person. Someone who shows interest, understanding and enthusiasm can become the cost model owner in the same way that some teams have an application security owner. While cost models may seem drab, there is sufficient technical detail to maintain the street cred of any technical person.

The cost model owner's primary responsibility will be ensuring that it is maintained as development progresses after initially being put together by more senior members of the team. This person will need to keep track of the application evolution (such as when more roles are added), interface with testers to get meaningful data on consumption of resources, and provide an interface to facilitate business involvement in small cost decisions made by developers on a daily basis. 

### Describe cost influences for all team members
Developing a culture of awareness and interest in the cost model is the most important function to be performed as part of the development of the cost model. Cost awareness should be as common and pervasive as security awareness.

At the beginning of the project there probably won't be much understanding of the costs, especially if the project is the first cloud project undertaken by the team. It is impossible to describe in sufficient detail all of the cost issues, and some of them will be unknown. Even if all of them were known it would be overwhelming to dump them on team members on the first day. Concern and attention to the cost model is a journey that will be different for all team members depending on their particular remit, so communication of the cost issues by describing the influences, is a good place to start. These can be described as a checklist of questions that particular roles need to think about. Example questions are listed below but you will need to develop you own complete list that is tuned to your project.

#### Architects and senior developers

* Have you considered the data transfer costs of your redundancy strategy?
* Will you implement a role for each endpoint or try and combine processing?
* Does the workload need to be decomposed, or can all the work be done in one process?
* Which is cheaper, reprocessing or serving up from storage?
* Where is the cache stored and how much does it cost to serve when under load?
* Is there a cost for the session state; are there alternatives to reduce this cost?
* Is this service too chatty? What are the cost implications if the service were less chatty?
* How frequently does data have to be synchronised with the on-premise servers? Have you presented the cost implications of high frequency updates to the business?

#### Developers

* Can this code be moved into a separate role, or combined with another, without significant rework?
* Have you optimised the use of batch operations and continuation tokens of storage transactions?
* Is it worthwhile compressing a message before sending it? Does that reduce the storage cost? What about the cost to compress and decompress it?
* Have you considered copying static data to the local disk when the role is initialised? Is there so much data that it works out as more data transferred than reading from source as needed?
* Do you slow down the polling on queues when they are mostly empty?
* Do you have mechanisms to control how much logging you do?
* Will the browser client call the external service or will you do it server-side?
* How are you optimising the page payload send down to the browser?
* Do you make sure that database queries return as few rows as possible?
* Have you profiled the ORM to make sure that it is not fetching too much data?
* Are you shutting down your development roles running on the cloud when you are finished with them?

#### Testers

* Do you know how to determine the optimal instance size?
* Do you know what the limitations are on each instance size, such as network throughput?
* If you run the same scripted test day after day, do you know if the costs are significantly different each time it is run?
* Do you have a definition of what would be considered a 'cost defect'?
* Do you have tests specifically for cost overruns?
* Do you know how much it will cost to run a production simulation?
* What is the cost of running staging instances when testing production upgrades?
* Are you shutting down roles when your tests are complete?

#### Operations

* Have you analysed affinity groups between the database and the applications?
* Are the databases the smallest size? Do you have a process to monitor when they need enlarging?
* Do you delete data, like backups and logs? Is the cost of not deleting them significant?
* Do you have an explicit agreement with business as to how much you can scale?
* Is the instance cost for each role optimal for load and redundancy?
* Do you have a mechanism to scale down instances on a daily basis during off-peak periods?
* Are all the roles that are currently running actually being used?

#### Business

* Have you been clear to the development team how important or not operational costs are?
* Do you know the financial implications of large traffic spikes?
* Have you instructed operations how much they are allowed to scale when responding to load?
* Have you discussed with internal IT the costs of bringing data on-premise in order to determine the best frequency?
* Do you have a cost model that shows costs for redundancy, backups and other non-critical components?
* Do you know what the cost implications are for implementing your availability strategy? For example the data transfer costs for geo-redundancy.
* Have you assessed the risk of being unable to pay your bill? What happens to the application and data if you can't?

### Develop a custom cost model
As much as the [pricing calculator](https://www.windowsazure.com/en-us/pricing/calculator/advanced/) on the Windows Azure website is useful and accurate, it is intended for sales purposes and not to be the basis for cost modelling going forward. Various web-based pricing calculators seem to crop up from time to time, but fall into disuse and should be checked for credibility before commitment.

Ultimately it is up to the implementation team to develop, maintain and enhance their own pricing model and it is likely that it will be composed of one or more Excel spreadsheets. The first version will probably be very similar to the Microsoft pricing calculator, which is a great place to start, and then evolve over time to a level of sophistication that's useful to the team.

When developing the pricing model, make sure that the following items are included:

#### Current price reference
Windows Azure pricing changes frequently. It is sometimes difficult to keep track of, particularly with all of the services available. The cost model needs to feature the latest prices, and there is no API, so the [pricing details page](https://www.windowsazure.com/en-us/pricing/details/) needs to be frequently checked. Announcements of pricing changes are not guaranteed to make it through inbox noise.

#### Architectural or platform bias
Since the pricing model will be developed specifically for the application, you have the option of presenting it in a way that mirrors the architecture, or keeping it fairly standard. A pricing model with an architectural bias will allow changes to be made, not to the number of instances but to named services, interfaces, specific databases or any other architectural element within the application.

An architecturally biased pricing model will be easier to present and be understood by a more casual user, such as a business user who has little knowledge of the underlying platform, and is useful over longer periods by being a better reference for operators. Unfortunately because the model is fixed to the current architecture it may not be possible to model architectural changes, and it is also likely to be only suitable for a one-off usage.

Make a decision early on whether to stick with an architecturally biased cost model or to develop something more generic. It may be that you identify one during the technical POC, but be aware that it is the model that you will probably end up using for a while.

#### Price variations during application lifecycle
As Windows Azure pricing changes frequently, make sure that the model allows for price variation. Pricing changes are generally not known far in advance, so don't attempt to model prices months down the line. The model should be able to record what prices were in the past, so that historical analysis of cost information is accurate and meaningful.

#### Inclusion of other models
The Windows Azure pricing calculator is one dimensional and does not allow for different load scenarios. The custom cost model needs to cater for other models and specifically include parts of those models that are relevant such as:

* [Lifecycle model](#LifecycleModel) — this is the most important model to include, which will require different capacity at different times (more than just daily).
* [Workload model](#WorkloadModel) — depicting workloads separately on the cost model will allow analysis to be done on whether or not they can be combined, optimised or scheduled differently.
* [Capacity model](#CapacityModel) — including scale units as a basis for calculating costs may be necessary to estimate applications that need to scale.
* [Test model](#TestModel) — will help plan the capacity and associated cost of large-scale testing activities.
* [Integration model](#IntegrationModel) — integration with on-premise services such as databases and Active Directory needs to be properly modelled as the costs may need to be managed.
* [Data model](#DataModel) — understanding where data resides, and how services consume the data, needs to be included in the model as it provides input into storage transaction counts, database sizes, data egress and other cost significant aspects.

#### Record of cost incurred
In order to perform meaningful analysis of costs over time, the cost model should include historical data, and preferably raw data that comes from billing. It may also need to be separated for different uses (development, testing, production) and should, if possible, be at a granular enough level to map it against the lifecycle model. If at all possible, use the ‘Service Info' attributes of the billing information to try and reconcile the billing to specific architectural elements; however this may prove to be more hassle than it's worth.

#### Non Microsoft costs
Without weighing down the cost model with all project costs, consideration should be made for costs that are not billed through Windows Azure. These could include:

* Third party services such as email services.
* Licenses for third party software installed on roles. 
* On-premise costs, such as storage or configuration and maintenance of on-premise services.
* Costs for third party application monitoring and alerting, such as Newrelic and PagerDuty.

#### Use third party cost tools
Due to the current immaturity of cost models, it is expected that tools to help with cost modelling will become available over time, although this will only start once Windows Azure has a billing API. Keep an eye out for tools and assess their value. 

### Model specific optimisations
Some cost optimisations can have a big impact on the monthly bill and some turn out to have such small savings that it is hardly worth it. When questions about cost optimisations arise, don't always rely on guesswork. Go away and model them in some detail. Be specific about including non-platform costs such as development, testing and deployment. Only once all of the costs are factored in can a definitive statement about the optimisation be made. In many cases you will find that cost differences are negligible and are offset by high development costs.

### Develop tactics for controlling costs
A crucial part of the cost model is the approach to getting costs under control (under control does not just mean cheap, but within expectations and estimates). Due to the impact that team members across different disciplines can have on costs, this has to be a multi-pronged approach. Tactics should be developed that suit each role, such as:

* Continuously review costs (see [more detail below](#ReviewCosts)).
* Measure costs (see [more detail below](#MeasureCosts)).
* Creating a culture of being aware of cost implications — every person on the project has some impact on the costs, whether it is a senior developer designing complicated service interactions, or a tester running a series of tests. If all team members are aware of cost implications, costs can be brought under control.
* Encourage specialisation — costs implications can be hidden in the detail of the service being consumed and there is no way everybody can know all of the detail. Having ‘go to' people, and specifically not just the architect, ensures that the coverage of cost details is high enough, and specialists will endeavour to have all the answers at their fingertips. Individuals can specialise in storage transactions, Windows Azure SQL Database, data egress, staging environments, or any other particular part of the Windows Azure services.
* Involve business in cost decisions — business needs to contribute to the decisions relating to cost optimisations and understand the cost related complexities of the architecture. Ultimately it is the business that will be stuck with the bill and their guidance on whether or not to optimise in a particular case is necessary.
* Align the costs to the business case — if the business generates more revenue from increased load then it makes sense to scale up as much as possible to handle the load, but this isn't the case with all applications. Understanding the business case, and aligning the cost model to it, is a necessary step to controlling costs.
* Allow testers to create cost defects — testers should be able to identify a piece of functionality as being functionally correct and performing adequately but being too expensive to run. Making this part of the testers' mandate, and allowing them to generate test cases specifically for costs, will generate a huge amount of coverage for cost control.
* Don't make assumptions about costs — all too often things are more expensive, but detailed modelling shows it as being a dollar or two a day, which may be hardly worth the discussion or development effort. "Show me the numbers" will be a useful retort to make sure that costs are under control as awareness about actual costs will tend to increase.
* Scour the web for cost saving tips — people are always running into cost issues as their assumptions on costs are challenged. The details of these observations, often blogged by developers, provide valuable input data. See these examples: [Unveiling the Unforeseen Cost and Tips to Cost Effective Usage](http://wely-lau.net/2011/12/02/unveiling-the-unforeseen-cost-of-windows-azure-storage-transaction/) and [Five ways to reduce Azure Costs](http://www.opstera.com/blog/bid/136402/Five-ways-to-reduce-Azure-Costs).

### <a name="ReviewCosts">Ensure that costs are continuously reviewed</a>
Cost models are often built during the initial project assessment and then shelved, resulting in shock at the first bill. It is necessary to continuously review costs, in order to keep them under control, and update the cost model on an on-going basis. This should go hand-in-hand with the maintenance and development of the cost model described above.

Detail of the cost related activities at various stages of development are described below:

#### Qualify

* Model costs in a basic fashion using the Windows Azure pricing calculator.
* Include the calculations as cost factors for the viable projects.
* Check the cost estimated against the business case.

#### Prove

* Develop a cost benchmark for the subset of functionality being proven as part of the overall business case.
* Measure the costs incurred and extrapolate to a production simulation

#### Design and Development

* Develop cost model details for services, data transfer and storage.
* Develop team-wide patterns for solving particular problems that have a cost implication and align those with the cost objectives of the application.
* Model costs for availability decisions made.
* Make cost a part of code review processes.

#### Test

* Develop tests specifically for cost defects.
* Continuously measure costs during testing and feed back into the cost model.
* Create a meaningful schedule of expensive tests (load, scalability) that deliver the most value for the costs incurred.

#### Production

* Continuously check that monthly bills are in line with the estimates provided by the cost model.
* Establish processes for approval of extra capacity for scaling.
* Develop maintenance tasks that clear up unneeded or unused resources (e.g. backups, staging roles).
* Use monitoring tools to ensure that instances are not running idle. Max out the CPU, memory and bandwidth wherever possible.

### <a name="MeasureCosts">Measure costs</a>
Unfortunately, the lack of tools and a billing API for Windows Azure makes it difficult to measure costs. It is then left to the business to pay a monthly bill they do not understand and with no clear idea on how costs can be managed. The cost model, perhaps in the custom spreadsheets developed, should include discreet methods to measure costs. This measurement will initially be based on importing the CSV exported by the billing interface and performing some analysis on it.

* Try to combine the cost analysis with other data, such as activity logs, so that costs can be determined to be anomalous or a natural extension of the load that the application is subject to.
* Compare the measured costs against planned costs if possible. This is particularly useful immediately after the application has gone live, where cost anomalies need to be detected quickly.
* Try to map the measured costs to specific workloads to identify their value. This will allow changes to workload configuration, scheduling or combining of workloads after release.

### Document implementation and arguments
The final step in the cost model is to document specific aspects of the implementation that have been influenced by cost issues. This helps to:

* Develop cost patterns that can be used by future projects
* Store a record of the argument for a particular optimisation when it is queried in future.
* Provide a basis for targeting changes, where either the cost of the service has changed or the business rationale for the optimisation no longer exists.

## Summary
The primary sales pitch for cloud computing relates to the pay-per-use nature of the costs. In order to realise the benefits, a clear understanding of the cost complexities is needed. With cloud computing applications, costs are directly related to code that is written, which creates opportunities for costs to get out of control. This may be the result of decisions being made that are inconsiderate of costs, or defects that run up costs undetected.

![Cost model adaptation](./images/Cost-001.png)

As illustrated in the diagram above, the cost model is never fixed and changes over the application lifecycle. This adaptation of the cost model as more data becomes available, such as from architectural changes or test billing results, is the most important aspect of the cost model. Too often costs are estimated upfront and never touched again. While this may work in traditional environments, on the cloud it can create serious problems.  

### Steps

1. Clarify the business stance on costs.
2. Identify the owner of cost model.
3. Describe cost influences for all team members.
4. Develop a custom cost model.
5. Model specific optimisations.
6. Develop tactics for controlling costs.
7. Ensure that costs are continuously reviewed.
8. Measure costs.
9. Document implementation and arguments.


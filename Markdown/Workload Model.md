# <a name="WorkloadModel">Workload Model</a>
Traditional n-tier architectures are largely influenced by the underlying infrastructure. Stateless web servers in a web farm, a big application server or two, and a single monolithic database is a recognisable pattern. The physical servers, storage, and networking underlying the application have been primary influencers of the application architecture, where administration of physical devices is costly, hardware within a data centre is standardised, and the same storage, power and networking is applied uniformly across all machines. This infrastructure basis has led to applications being architected to fit the physical model where, for example, certain functionality runs on lower grade web farm servers and application servers sit on higher-grade machines and host unrelated functions.

Cloud computing provides a layer of abstraction where the physical infrastructure, and even the appearance of physical infrastructure, has less of an impact on the overall application architecture. So instead of an application being required to run on a server, it can be decomposed into a set of loosely coupled services that have the freedom to run in the most appropriate fashion. This is the foundation of the workload model because what may be considered an appropriate way to run for one part of an application may be wildly different for another, hence the need to separate out the different parts of an application so that they can be dealt with separately.

When architecting for the cloud, we don't create all of these decomposed services just because the platform allows it. After all, it does increase the complexity and require more effort to build. In the context of cloud computing, this architectural pattern has, amongst others, the following benefits:

* Availability — well-separated services create fault isolation zones, so that a failure of one part of the application will not bring everything down.
* Increased scalability — where parts of the application that require high scalability are not held back by those that do not.
* Continuous release and maintainability — different parts of the application can be upgraded without application-wide downtime.
* Operational responsiveness — operators can monitor, plan and respond better to different events. 

Consider an example of an e-commerce application and the two distinct features of catalogue search and order placement. Even though these features are used by the same user (consumer) their underlying requirements differ. Ordering requires more data rigour and security, whereas search needs to optimally scale. A search being slightly incorrect is tolerable, whereas an incorrect order is not. In this example, the single use case of searching for and ordering a product can be decomposed into two different workloads, and a third if we count the back-end integration with the order fulfilment process.

The workload model requires that features be decomposed into discrete workloads that are identifiable, well named, and can be referenced. These workloads form the basis of the services that will deliver the required functionality. The workloads are also used in other CALM models to establish the architectural boundaries of services as they apply to specific models.

## The workload model, application architecture and architects
The workload model forms the basis for application architecture fundamentals and the objective is to use the concept of decomposing application functionality into clearly defined workloads. The workload model should not be seen as simply a document, but rather a tool to influence the correct architectural decisions.

Decomposing application functionality is not an easy task and requires the architect to apply their skills and experience. It may involve input from other members on the team, and it may take a few attempts to get it right. Less experienced architects, or those that are not used to thinking about architectural approaches that encourage the use of loosely coupled services, are encouraged to seek help and advice from others. Other architects, whether from the same organisation or external consultants, may be invaluable in reviewing the workload model.

The role of a good architect cannot be understated when developing the workload model. There are no easy metrics to check to see whether or not the defined workloads are correct (unlike the cost model for example, where costs have to be in a certain range). The validation of workloads can only be done by a capable architect able to understand the far-reaching consequences of their decisions. Because the workload model influences so many design decisions, the architect needs to be continuously thinking in terms of the workloads, and even adjusting them as other parts of the architecture are defined.

## Decomposing Workloads
There are no easy rules for decomposing workloads which is why it should only be tackled by an experienced architect. An architect with little cloud computing experience will probably err on the side of not enough decomposition. The challenge is identifying the workloads for your particular application. Some are obvious, while others less so, and too much decomposition can create unnecessary complexity. Workloads can be decomposed by use case, features, data models, releases, security, and so on.

As the architect works through the functionality, some key workloads may become clear early on. For example:

* Separating the front-end workloads (where an immediate request response is required) can be easily distinguished from back-end workloads (where processing can be offloaded to an asynchronous process). 
* Scheduled jobs, such as ETL, need to be kicked off at a particular time of day.
* Integration with legacy systems.
* Low load internal administrative features, such as account administration.

For example, the diagram below depicts an application that has separate workloads for each (primary) feature of the application.

![Feature workload decomposition](./images/Workload-001.png)

The same application can separate workloads based on asynchronous processing, as illustrated in the example below. The [CQRS](http://martinfowler.com/bliki/CQRS.html) (Command Query Responsibility Segregation) pattern can be implemented this way.

![Async processing workload decomposition](./images/Workload-002.png)

More complex workload separations can be developed. In the diagram below, the same application has a separate front-end workload, as well as separate workloads for each data model.

![Feature workload decomposition](./images/Workload-003.png)

## Indicators of differing workloads
Determining how to decompose workloads is the responsibility of the architect, and experienced architects should take to it quite easily. The following indicators of differing workloads are only a guide, as the particular application and environment may have differing indicators.

### Feature roll-out
The separation of features into sets that are rolled-out over time are often indicators of separate workloads. For example, an e-commerce application may have the viewing of product browsing history in the first release, with viewing of product recommendations based on browsing history in a subsequent release. This indicates that product recommendations can be in a separate workload to simple browsing history.

### Use case
A single user, in a single session, may access different features that appear seamless to the user but are separate use cases. The separate use cases may indicate separate workloads. For example, the primary use case on Twitter of viewing your timeline and tweeting is separate from the searching use case. Searching is a separate workload, which is implemented on Twitter as a completely separate service.

### User wait times
Some features require that the service provides a quick response, while others have a longer time that the user is prepared to wait. For example, a user expects that items can be removed from a shopping basket immediately, but are prepared to wait for order confirmations to be e-mailed through. This difference in wait time indicates that there are separate workloads for basket features and order confirmation.

### Model differences
The importance of workload decomposition in the design phase is because all other models that need to be developed in design (such as the data model, security model, operational model, and so on) are influenced by the various workloads. Using our e-commerce example, without identifying search and ordering as separate workloads, we would get stuck when developing the security model as we would either end up with too much security for search (which is essentially public data, and has low security) by lumping it together with the higher security requirements for orders, or the reverse, where we are exposed to hacking because orders are insecure.

In the process of working through the models, a clue that workloads are incorrectly defined is when a model doesn't seem to fit cleanly with the workload. This may indicate that there are two workloads that need to be separated out. Whilst it is better to clearly define the workloads early on, it is possible that some will emerge later in the design, or indeed as requirements change during development. The problem, of course, is that when new workloads are identified they need to be reviewed against models that have already been developed, as at least one model would have changed.

Below are some examples where a difference in a model indicates the possibility that the feature is composed of two different workloads:

* Availability model — When developing the availability model, if one feature has higher availability requirements than another, then it may indicate that there are separate workloads. For example, the Twitter API (as used by all Twitter clients) needs to be far more available than search.
* Lifecycle model — The lifecycle model may show that a particular feature is subject to spiky traffic or high load. In order to be able to scale that feature, it should be in a separate workload to those that have flatter usage patterns. For example, hotel holiday bookings may be spiky because of promotions, seasons or other influences, but the reviewing of hotels by guests may be a lot flatter. So, hotel reviews may be in a separate workload.
* Data model — The data model separates data into schemas that may be based on workloads, so getting the workload model and the data model aligned is important. Features that use different data stores indicate possible workload separation. For example, the product catalogue may be in a search optimised data store, such as SOLR, whereas the rest of the application stores data in SQL. This may indicate that search is a distinct and separate workload.
* Security model — Features or data that have different security requirements can indicate separate workloads. For example, in question and answer applications the reading of questions may be public, but asking and answering questions requires a login. This may indicate that viewing and editing are separate workloads.
* Integration model — Different integration points often require separate workloads. While some integration may require immediate data, such as a stock availability lookup and will be in the same workload as other functionality, the overnight updating of stock on-hand may be a separate workload. 
* Deployment model — Some functionality may be subject to frequent changes while others remain static, indicating the possibility of separate workloads. For example, the consumer-facing part of an application may update frequently as features are added and defects fixed, whereas the admin user interface stays unchanged for longer periods. The need to deploy one set of functionality without having to worry about others can be helped by separating the workloads.

## Implementing workloads as services
Workloads are a logical construct, and the decision about what workloads to put into what services remains an implementation decision. Ultimately, many workloads will be grouped into the single services, but this should not impact the logical separation of the workloads. For example, the web application service may contain many front-end workloads because they work better together as a single service. Another example is the common pattern to have a single worker role processing messages from multiple queues, resulting in a number of workloads being handled by a single role.

The decision to group workloads together should happen late in the development cycle, after most of the CALM models have been completed, as the differences across models may be significant enough to warrant separate implemented services.

## Identified workloads
The primary output of the workload model is a list of workloads, with some of their characteristics, so that they can be used and referenced in other CALM models. For each identified workload:

1. Name the workload.
2. Provide a contextual description of the workload. Bias the description towards the business requirement so that all stakeholders can understand it.
3. Briefly highlight relevant technical aspects of the workload that may influence the model. For example, the workload may have special latency requirements, or need to interface with an external system. These aspects should be quick and easy to read through for all workloads when developing the models.

You should end up with enough detail on the workloads to feed into the rest of the design process.

## Summary
The brevity of this description of the workload model masks the enormous value that it provides. As every application and situation is different, it is not possible to explain how to decompose individual workloads in this book. It is the responsibility of the architect to figure out how workloads decompose in their particular situation.

Workloads are used extensively in all CALM models and form the basis for many of the architectural decisions. Decomposed workloads are at the core of cloud computing architectural approaches as they mirror the architectural desire to build loosely coupled services. They should be given sufficient attention at the beginning of design and continuously revisited and updated throughout the application lifecycle.


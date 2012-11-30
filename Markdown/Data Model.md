# <a name="DataModel">Data Model</a>

The capture, storage, processing, and retrieval of data, make up the majority of features of any application. Creating the data model as a work stream focuses the required attention on data handling as a fundamental concern of the application and architecture.

Like other models, the data model should not be seen as a model to be completed and finalised before development commences. It should evolve over time, which makes sense architecturally, but should have enough in the early stages in order to ensure that the correct architectural decisions are made, and so that developers can continue without needing to rework any data access code.

## How the data model has changed
How modern applications are built has fundamentally influenced database models, the approaches to modelling data and how resulting data models are implemented within applications. While the public cloud has not been wholly responsible for this shift, the patterns adopted in cloud computing applications, and the nature of applications that run on the cloud, have been drivers for this change.

As little as ten years ago, the de-facto approach to data was to store it all in a single, monolithic SQL database that was strictly controlled, had full transactional control, and was modelled as close to third-normal form as possible. Although non-relational databases have existed for decades, the SQL database had emerged as the standard and looked to stay that way due to trust, stability, tools, and developer adoption and acceptance. The arrival of data-centric applications that handled a different type of data, such as Google's search engine, focussed attention on the suitability of alternative data stores that supported the data requirements of high scale web applications. Google, Amazon and similar web properties were seen as having higher data throughput than the traditional stalwarts of data processing, banking and finance, which have more traditional (mainframe and SQL) data stores. This focussed attention on the architectures that the likes of Google and Amazon were using for data, and brought non-relational data stores (key-value stores in particular) into the mainstream. The well-publicised scaling problems with applications such as Twitter or Craigslist and their solutions that replaced, augmented, or adapted their traditional SQL oriented databases with caches, sharding, document databases, and other fit-for-purpose data stores that were decidedly not a pure SQL RDMBS solution. Over time, large web properties have highlighted specific areas where traditional database approaches have failed, such as the ability to handle the data requirements of social graphs (Facebook). Even non-web applications have felt the pressure of increasing data requirements, from complex event processing to the enterprise application of map-reduce technologies. Application architects have been encouraged by the successes of high profile applications. Their own applications are subject to higher loads, and their existing data models have been compromised or adapted anyway (such as by denormalising data structures or adding caches). This compromise and adaptation has resulted in an appetite for the alternative data stores seen in high profile applications within their own projects.

The context of emerging support for data models that are not exclusively SQL based is important. Problems that have been addressed by large web properties are the same that are being seen by emerging applications and business models. The characteristics of high availability, high load through an increasing number of devices and users, complex datatypes and, importantly, the need to serve content at a very low cost per user, are becoming base requirements for all applications. The lessons, patterns, and technologies of the early adopters of data storage variety are directly applicable, relevant, and are being actively adopted by developers.

Regardless of support for SQL databases in general (often by database vendors), or in specific domains in which they excel, the awareness of the need for alternative data stores is gaining serious and credible traction. Even a wall-to-wall SQL-only environment will, at some point, need to implement a cache in front of the database. This is only a hop, skip and jump from serious consideration of distributed cache, which alters the particular applications database landscape and affects the data model.

## The influence of cloud computing on data models
It would be disingenuous to suggest that cloud computing applications alone have been the cause of the changes to data models. It is the style of applications developed for the cloud that have had a significant impact. Whether running on a public cloud platform such as Windows Azure, or in a dedicated datacentre, applications that take advantage of distributed databases on top of commodity hardware have had a profound influence. These modern applications have to serve a huge number of users for marginal cost. They are broken down into discrete services that share nothing but interface contracts. The developers that build them use frameworks that treat data differently; no longer bowing to senior datacentre DBAs but rather wanting active involvement and supporting the 'devops' culture. The nature of the applications, the frameworks, and the developer culture has given rise to NoSQL, which has changed the incumbent data model and supporting database technologies. Despite the proliferation of traditional database technologies on cloud platforms, mainly in an attempt to appeal to the enterprise market, the cloud computing culture will continue to exert influence on how we build data stores to support applications well into the future.

## Risks of inadequate data modelling
When it comes to data and databases, a degree of laziness has crept in to general database development practices. SQL databases have been solid, reliable, well understood, and mostly coped with the demands of applications and development frameworks. Data, over time, became less of an issue and has been treated with disdain by developers that have little concern over the 'persistence layer' — trivialising the role that databases play in applications. This has not been solely the result of developer attitudes, but the draconian role DBAs have played in distancing developers from data responsibilities, and the continued improvement in tooling and frameworks which remove the need for developer attention to databases, such as the adoption of ORMs. Continued database performance increases, due to hardware improvement (particularly available memory and disk I/O performance), have also contributed to increased database reliability.

This blasé developer focus results in repeated problems such as poor database performance due to suboptimal queries (often due to an over-reliance on the ORM), or excessive load on the database because it is used as a single store for all shared state. These problems exist in all applications, but a lack of attention to data, and the data model, becomes a serious concern in cloud applications, as illustrated by the examples below:

* Database performance at scale. Shared state is the bane of high scalability. Web servers scale out well and individual servers can serve requests in isolation of any others that may be doing the same. Databases, whether designed to scale-out in a distributed manner or configured for scale-up on a single machine, have to be far more cognisant of other processes. Databases must ensure that they have the latest version of a piece of data, blocking reads while particular updates take place, or making sure that constantly changing data is backed up and fault tolerant. At scale, when thousands of users are trying to access data, for concurrent reads and writes, the data model and underlying database technologies become the single most significant performance bottleneck. Inattention to data concerns, and inadequate data modelling, cause applications to collapse when they are placed under significant load. The result of poor database performance when an application scales may not be solved easily. Applications running on cloud platforms run into infrastructure limitations due to the commodity devices on which the infrastructure is built, as well as other limitations on bandwidth, latency, disk performance, and available memory. The mantra of 'throw more hardware at the problem', which is generally a scale-up solution, is not applicable on an infrastructure-limited cloud platform. The problem is not solved by on-premise infrastructure either, where the provisioning of capacity, with the accompanying cost and lead times of sophisticated hardware, is measured in months, rather than minutes. In either case, the poor performance of the database cannot be solved quickly enough to satisfy users and customers who inevitably move to alternatives with no intention, or likelihood, of returning.
* Shared databases encourage tight coupling. Cloud applications endeavour to separate independent workloads into discrete services. The interface between services should be at a scalable endpoint, not the underlying database. The implementation of a single data model, particularly one implemented within a single database, discourages the use of independent services. This means that the application components cannot be loosely coupled, as the database or data model forms the basis of the tight coupling and this cannot easily be removed. The result of tightly coupled services is that they are less fault-tolerant, less able to scale, and more complex to deploy and operate.
* Security is a major issue to be dealt with in cloud computing projects. Insufficient data modelling can expose the data to security threats.
* Application availability is one of the primary requirements for all applications, and it is often the database, as a single point of failure, that has the most impact on it. Availability problems can be easily addressed by, for example, replicating data or using eventual consistency models, but availability problems need to be understood, addressed, and modelled in the data model to ensure that availability is as expected.

## Data is widely dispersed
The simplistic view of data is that a user captures some data, it is processed and stored in a database, and some more data is retrieved from a database to display to the user. This monolithic thinking, where there is one database that is written to and read from, is a common view and is an oversimplification of the problem of handling data in applications. Even applications that have a single primary database will still tend to use multiple data stores such as caches and client-side data stores.

The example below shows a typical web application and tracks the data stored, generated, retrieved, and presented for what would be considered a simple operation such as a user capturing some data and presenting a result.

![Multiple Data Stores](./images/Data-001.png)

The example illustrates no less than eleven data stores that are used for a simple feature. Some are not considered 'core' databases but are important, necessary, and useful in their own right, such as the log and behaviour data stores. Every one of these data stores needs to be modelled, in the context of the service that they perform, in order for them to be developed properly. All eleven have structure, performance requirements, availability issues, latency, security, operations, deployments and other aspects that need to be considered in sufficient detail.

The following list contains most of the data stores that need to be considered:

* Client-side data — such as client cache, cookies and other state management data stores that are used. Desktops and mobile devices may have more sophisticated stores that need to be modelled in more detail.
* Static content — such as images and even static (JSON) data lists that are accessible by a browser. Static data is almost never perpetually static, and needs to be refreshed, versioned and even secured.
* Session state — whether on the client or the server, session state requires particular attention. Determining what data is part of the actual session, rather than transactional data that needs to be kept beyond the session, is the first step of the data modelling problem.
* Service Contracts - external providers of data, where control over the data is limited, have service contracts for data that need to be modelled and included in the consuming application.
* Messages — messages have structure that needs to be modelled, and most messaging approaches favour reliable messaging, meaning that the data is persisted in a data store. While this persistence is mostly taken care of by the messaging service, it does introduce additional data problems, such as multiple message deliveries, that need to be addressed.
* Business transaction data — the core data within an application, whether stored in a SQL or NoSQL database, needs to be well modelled. Fortunately most architects and designers, backed up by DBAs and IT governance, give this kind of data sufficient attention.
* Function optimised data — some data may be stored in a specific way to serve a single function. For example, data can be transposed into documents that are indexed by search servers (such as SOLR), and these documents need to be correctly modelled in a structure that can be indexed and satisfy the search requirements.
* Blob data — and other 'unstructured' data needs to be modelled, if not for the basic structure that exists, but for security, performance, bandwidth, duplication across a CDN, and other aspects.
* Data analysis — traditional OLAP, data mining, and newer big data approaches using map-reduce, event processing and related tools need to be understood and modelled in detail. The ability to perform analysis as quickly as possible for the lowest storage and compute cost is an important consideration in cloud-based architectures.
* Transactional reporting — the production of reports from the primary transactional database can bring a database to its knees. Modern, high-load scalable web applications need specific attention given to transactional reports so that primary application performance and availability are not impacted.
* Data synchronisation — with applications comprised of multiple, loosely-coupled services, the sharing of data across service boundaries needs to be understood. Where data resides, how the master database is updated, what can be shared, and so on are important aspects of data synchronisation that need to be modelled.
* Cache — the cache is one of the most important data stores as its impact on performance and scalability is significant. Unfortunately, it is often left up to individual developers as to what data to cache, creating scalability problems in production. What data to cache, setting expiry rules, warming up of the cache when a service loads, and so on, need to be modelled in sufficient detail in order to get the most benefit.

## The need for multiple data stores
There is no one-size-fits-all solution to data and databases.  Models that underlie specific technologies solve specific problems that may not be applicable anywhere else. For example, a cache data store needs to serve up data very quickly, and is less concerned with transactional correctness or data loss than a SQL database would be. That same correctness and concern about data loss makes SQL a poor choice to satisfy the demands of a cache.

The diagram below demonstrates how different database needs, supported by different technologies, have different strengths and weaknesses. SQL, for example, has high consistency and high availability per node, but a low partition tolerance and a low ability to handle flexible database schemas. Whereas the low latency of a cache is offset by its equally low consistency.

![Data store features and trade-offs](./images/Data-002.png)

Fundamental to the data model and cloud-based architectures is the realisation that multiple data stores are required to satisfy the varying data demands of the application. The development of the data model is not about finding the single database technology, but about uncovering the application requirements and matching them to technology choices. The subsequent architectural challenge then becomes dealing with the multitude of databases, minimising the effort and skill required to keep them running, and to keep a handle on data flowing between them.

## The emergence of NoSQL
NoSQL, probably as a result of the name, is often perceived as an anti-SQL movement. It is not anti-SQL, but is it a movement with significant support from the software development 'hacker' community. NoSQL is about the selection of database technologies that are architecturally appropriate to the task at hand. It just so happens that many tasks are not appropriate to SQL, resulting in what is seen as an anti-SQL position.

NoSQL is beyond the context of this book, but understanding the reason for the emergence of database technologies that are not based on SQL is important, in order to develop an application data model correctly. The drivers behind the emergence of NoSQL are rooted in the demands of modern applications. Any application that is assessed to be a good candidate for cloud computing will exhibit characteristics that make a monolithic SQL database ill-suited. These can be summarised as:

* A business case that cannot support high, up-front hardware and licensing costs.
* A need to minimise operational costs resulting from long-term maintenance agreements and dependency on specialised skills.
* High read to write ratio.
* Little business demand for high degrees of data consistency.
* Unstructured or binary data that does not fit well in a third-normal form relational model.
* High data volumes that exceed the economically meaningful value of single scaled-up databases.
* Developer attitudes, where the SQL database is not considered sacrosanct, result in the acceptance of other technologies to achieve the application objectives.

These are discussed in more detail below.

### Commodity devices
Enterprise SQL databases generally run on high performance hardware that is engineered to specifically serve the needs of the database. They contain multiple high performance processors (including RISC-based), significant amounts of high performance RAM, and a storage subsystem that spans high performance NAND storage on the bus (e.g. [Fusion I/O](http://www.fusionio.com/)) to high throughput networking, and large SAN storage systems. These enterprise database servers can hardly be called 'servers' as they are made up of a sophisticated assembly of high performance servers, networking equipment, disks, specialised software, monitoring tools, and operators with specialist skills and experience.

Such a high performance infrastructure does not exist as a usable unit on the cloud (it may indeed exist to run the platform, but is not accessible to customers). Instead, customers have to make do with relatively small commodity servers that run as virtual machines on a shared infrastructure that can never be optimised (or even analysed) for a specific customer purpose. SQL databases prefer as much processing power and memory on the same bus coupled with a high throughput, low latency, and low collision disk subsystem. Also, high availability SQL is generally implemented as clusters, which share storage systems and require significant network capacity. Few of these requirements can be addressed with a shared, commodity virtualised infrastructure. As a result, applications running in cloud environments have been forced to implement databases that can perform adequately on a relatively low performance infrastructure, meaning that traditional SQL databases are a bad fit.

### Cost of procurement
Enterprise IT vendors have made good money from high performance enterprise databases; be those hardware suppliers like IBM, or database vendors like Oracle and Microsoft. The high prices for enterprise software licenses of database products go hand-in-hand with licenses for other components (such as backup and storage), as well as long-term maintenance agreements. Not only do the infrastructure and licenses cost a lot, but installation and setup often needs to be performed by specialised and expensive consultants who assemble the database on site.

This cost of procurement can exceed software development costs, and any budget spent on infrastructure cannot be spent on adding features or attracting customers. Such a bias in cost towards infrastructure can be debilitating for the business plan. The ability of NoSQL technologies to run on commodity hardware (as provided by cloud providers), the ability to scale easily (reducing the up-front infrastructure commitment), coupled with the free license of open source, creates a compelling case to investigate the technical suitability of the significantly cheaper alternative.

### The nature of data
Modern applications satisfy varying data needs within a single application, such as:

* Transactional structured data.
* Binary data, such as media, which can be served in blocks or streamed.
* Searching on data which changes infrequently.
* Different data structures contained within a single 'document' structure.
* Data generated as a side effect of normal operations, such as log data.
* Data that spans stores and structures but is grouped within a social graph (or other graph, such as 'those who bought this, also bought that')

Of the above examples, only traditional structured data is well suited to SQL, the rest either do not work at all or are suboptimally addressed by SQL. All of the others require approaches to data that are not strengths of SQL. The degree to which any of these are required may differ from one application to the next, but the crucial point is that a typical, modern application will have multiple types of data that need to be stored and accessed.

### Data access patterns
Modern applications have variable data access patterns which are different from traditional enterprise applications. For example:

* Web applications generally have read:write ratios where the number of reads is orders of magnitude more than writes. This necessitates the use of database caches in order to avoid placing excessive and unnecessary load on the database.
* Consumer oriented applications have little need for absolute data consistency. It is tolerable (and not even noticed) if data is a little out of date.
* A single application can have a high read:write ratio (for example, viewing the catalogue) as well as a need for high write:read (for example, logging) or an even ratio (for example, social updates). This requires radically different approaches to data within a single application.
* Consumer oriented applications have very little user customisation for queries (there are no custom reports), so a database query language (and the supporting underlying model) is less important, as queries are purpose built in the application layer.
* Modern applications tend to integrate with other services in real-time wherever possible (such as handing off to fulfilment or querying the social graph for recommendations), and have very few after-hours batch operations.

These variable data access patterns, as well as the introduction of some that are unique to modern applications, heavily influence the application data models, as well as the underlying data stores.

### Data volumes
Modern applications, particularly those exposed to consumers, generate significant amounts of data. The data generated may not be in the form of business transactions, but in the data that is generated that has some secondary value. Where applications in the past may only have concerned themselves with dealing with the current operation a user is performing, modern applications need to keep track of past operations (for example, recent items viewed), and derive potential future operations (for example recommendations). These high volumes of data may only be relevant and useful for a short period (for example, trending topics in an individual's social graph), or may want to be kept for later analysis (for example, log files), where value can only be extracted when combined with other data points or datasets.

Storing and querying huge amounts of data in a SQL database, while possible, may not be appropriate or economical. In a world of low margins and free-to-use services, the cost of storing formal transactions in a SQL database may be viable, but storing tens of thousands of pieces of information in an expensive data store for a user that has no immediate business value may not make economical sense.

The cost per transaction and cost per query of a SQL database may not make sense within the business case. For example, the cost of a SQL database (and infrastructure) to run the equivalent of a one hour query on a 1,000 Hadoop cluster weighs heavily against SQL. Similar considerations on write-heavy operations favour technologies like [Apache Cassandra](http://cassandra.apache.org) over SQL.

### Developer attitudes
Trying to generalise the relationship that developers have with SQL databases is impossible. Some developers are dismissive and see it merely as a persistence store. Some have significant SQL skills and can get the most out of the database. Others rely on SQL as a crutch to make mistakes appear less costly. One thing that is clear about the relationship between developers and databases over the last few years is that it is becoming more distant. Some developers have embraced ORMs in an attempt to abstract the problem away; others have moved away from SQL wherever possible and adopted alternative data stores. Many enterprise developers have either had their intimate SQL relationship ruined by DBAs, or have been turned into database developers, where stored procedures are where the magic happens.

The developer attitudes and their influence on the database architecture cannot be underestimated. Unless an application already has an existing database and supporting engineering practices, developers are the architects, and their influence on the application data store is profound. Where application architectures drive database architectures, alternative data stores are more likely. Some of the influences on developer attitudes towards databases include:

* The unwinnable ORM war — Developers have been arguing about Object Relational Mapping (ORM) and ORM frameworks for so long that every few years a new generation of developers pick up the gauntlet and argue about it all over again. ORMs are an okay, but not perfect, solution to a difficult problem of mapping between objects and the relational model. After a while, developers get fed up with the effort, compromise, and arguments, and look for alternatives where mapping is unnecessary. They often find document databases a good solution, hence the rise of [CouchDB](http://couchdb.apache.org) and [MongoDB](http://www.mongodb.org).
* Seeking alternatives for database bottlenecks — When applications need to scale, it is inevitably the database that is the most difficult component to scale. Even non-scaling applications spend more time waiting for results from the database (any database, not just SQL) than anything else. Inevitably developers look to profile and optimise these database performance bottlenecks and will, at the very least, start looking at what can be cached, so that database round trips can be saved. This leads to looking at distributed cache, cache expiry, eventual consistency, and other principles, techniques and technologies that veer away from SQL as a solution.
* Writing stored procedures isn't cool — As recently as 2005, the 'standard' pattern for data oriented applications was to encapsulate all data access in stored procedures and only interface with the database via stored procedures. This meant that most developers were writing some of the most important code in a stored procedure language rather than the language of the application stack. Over time, developers have moved away from stored procedures, influenced by software engineering principles (too much logic in the database language cannot be tested), advances in languages and frameworks that make application code better and more 'fun' to write (such as LINQ in .NET), and the use of ORMs (a paradigm incompatible with the task specific nature of stored procedures). Modern developers, although they may have good database skills, tend to avoid stored procedures, making the migration to another database platform far easier.
 

### <a name="DatabasesOnACID">Databases on ACID</a>
One of the most important and useful features of SQL RDBMSs is their support of ACID transactions. ACID refers to the properties (Atomicity, Consistency, Isolation, and Durability) that guarantee that transactions are processed reliably. Mainstream SQL databases fervently support ACID and other principles to ensure that data is always consistent and reliable, and while it is not necessarily the most important part of a SQL database, it often positioned as such. The driving of such features and the knock-on effects of database backups, fail-over scenarios and technologies, tools and operating environments becomes so all-encompassing that the business need for ACID becomes lost in all the sales hype and rhetoric.

When analysed in detail, the actual need for immediate database consistency is very low, and there are few scenarios where it is absolutely necessary. It turns out that the demand for digital perfection is not practically applied to the real world where things get lost, broken, miscounted or stolen. Data architect Pat Helland (from Microsoft and Amazon) spoke about ['Screw-ups and Apologies'](http://blogs.msdn.com/b/pathelland/archive/2008/05/02/link-to-the-video-of-the-irresistible-forces-meet-the-movable-objects.aspx) where he observed that even with absolute data consistency, things are bound to go wrong outside the database, and business has to develop approaches to deal with the resultant problems (such as apologising to customers). The example he references is that in an e-commerce scenario, a stock system can have an exact count of widgets available, but when the order is placed for the last item, it may have been run over by a forklift. The response to the customer if the item was run over by a forklift is indistinguishable from the response if there was inconsistent data. In both cases the response would be an apology. The argument being that since businesses have to cater for 'apologies and screw-ups' anyway, why try and aim for the perfection of consistent data (with the associated cost, performance degradation, and potential disappointment)?

Werner Vogels (Amazon CTO) is quoted as saying, "If you're concerned about scalability, any algorithm that forces you to run agreement will eventually become your bottleneck. Take that as a given". This refers to the architectural effort and cost associated with consistency (agreement between nodes). This is important for applications that need to scale, as the first step in building out the architecture is determining the need for consistency. Putting all the architecture in place to support an unnecessary requirement is wasteful and pointless.

If there is a low need for real-time consistency across databases or concurrent connections, how do we deal with consistency problems if they occur? There are a number of techniques, the first being the ability to handle inconsistency within the business model (apology-based computing), or having processes to cope with errors (see [Starbucks Does Not Use Two-Phase Commit- Gregor Hohpe](http://www.eaipatterns.com/ramblings/18_starbucks.html)), and the other techniques are variations of eventual consistency. Eventual consistency refers to the model where a specific piece of data may not be consistent across nodes at a point in time, but eventually will be. Asynchronous processing and idempotent data, all of which should be familiar in distributed computing (cloud) environments, support this model.

The impact of this thinking on SQL databases is devastating. Even though ACID is not the only good thing about SQL, it is one of the features that have been aggressively marketed for decades as being very important. Dismissing the need for ACID undermines the carefully assembled value proposition of SQL databases and allows other databases to gain a foothold based on their own particular strengths.

### Cost of operations
Whether it is in the very nature of the difficulty of the problem that SQL databases are addressing, the sophistication of particular SQL products, the need to understand product idiosyncrasies, or the secret-handshake cabal of SQL DBAs, the cost of operating a SQL database is significant. Not only is the supporting infrastructure costly, but so too are the licenses, on-going support agreements and, most importantly, the cost of specialised DBA skills.

DBAs can have a particularly hard time keeping databases going, especially when those databases are under extreme load. Not only do they need the specialised database skills, but they also need a lot more business knowledge than one would think necessary. An intimate understanding of the business implications and reasons for long running batch operations becomes expensive knowledge to keep around. To top it all, DBAs are paged when things break and are often blamed as the cause; it is expensive to keep highly skilled scapegoats around.

> As observed by [Werner Vogels - Amazon CTO](http://www.allthingsdistributed.com/2009/10/amazon_relational_database_service.html) "Structured data management systems are traditionally served by relational databases but these sophisticated systems have their limitations, especially when it comes to scale and reliability. Often they also require tremendous expertise to operate efficiently and reliably especially when scaling up. Of course, a significant portion of the structured data world does not require RDBMS features such as complex transactions and relations, and can be served by a simpler, much more agile system."

So while something like Windows Azure Table storage has only few features compared to its SQL counterpart, the cost of operating Windows Azure Table Storage is minimal. Most of the storage part is handled by the service, and a database technology that doesn't support indexes or advanced queries leaves little need for a specialised resource for database optimisation. The high differential between the costs of specialised DBAs, and generalist operators that loosely fall under the 'Devops' banner, is significant for the cost optimisation of applications that scale.

### Giving up the pursuit of an absolute truth
One of the promises of the relational model supported by SQL databases was the idea that a single database could contain an accurate model of the business. Both in terms of semantic accuracy, with normalised structures properly representing the business domain, and in terms of database accuracy, supported by consistency and application agnostic API. Those ideals of an 'absolute truth' contained within the database have been dashed. Business spans multiple databases in multiple organisations or business units, held together by the duct-tape of integration technologies that have dubious mappings, odd integration schedules, and few reliable ways of ensuring that data is clean and accurate. Under-performing databases have been 'optimised' for application performance, de-normalising data or spreading data across systems and databases. Database professionals have all but given up on having a single model and single database for the business as it is simply unattainable and impractical.

Against this backdrop of multiple data sources for applications, the spreading of data across multiple technologies for a single application is both tolerable and standard practice. It allows for the selection of the correct type of database for different parts of the application, and the 'integration' between different databases.

## The case for SQL
The above discussion on SQL versus NoSQL should not be considered an anti-SQL position, and most within the NoSQL 'movement' try and express NoSQL as 'Not Only SQL'. This acknowledges that SQL does have an important, well-deserved, and valuable place in data storage and retrieval. Any overtly anti-SQL sentiment is largely focussed on being against the perception that SQL is the only solution to all SQL problems.

When developing the data model, make sure that SQL is not overlooked, and be cognisant of its strong points and where it fits into the application architecture. The points below highlight some of the compelling benefits of SQL:

### The relational model is pretty good 
The relational model that sits at the core of SQL is a good one. It allows for the data model to be encapsulated and developed against with relative ease. Other databases may be optimised for a specific purpose and therefore not have as good an underlying model. Document databases, for example, store individual atomic documents, and are unable to support cross collection (or table, or relation) queries with the same ease as SQL.

### SQL is a well understood standard 
SQL has been used extensively within enterprises for about twenty years, and that time has allowed it to become the entrenched standard. Because of specialised features on different databases, the tools between different database platforms are incompatible with one another (SQL Server tools cannot really be used on Oracle, and vice versa), but many of the skills are (at least developer skills), and database drivers and frameworks have abstracted some of the idiosyncrasies away. Ultimately the basic operations between databases are very similar, including operational processes. Contrast this to other database technologies, where the skills are specific, and generally not transferable. This lack of transferability of skills is not just because the products are different, but also because the underlying model is different. Windows Azure Table Storage and MongoDB, for example, have completely different approaches to data and different underlying models (key-value versus document).

### Applications and tools for SQL
Apart from the tooling by database vendors themselves, thousands of applications and tools support SQL. From large scale ERP systems like SAP, to specialised ETL tools, to web application content management systems. Virtually every product that is available, whether off-the-shelf, or custom built, supports SQL Server, or Oracle, or MySQL, or all three. This is not only unavoidable when you need a particular product in your solution (such as CMS), but also when integrating with other tools and applications.

### Vendor support
Although the on-going support agreements can be very expensive, the support of SQL databases by vendors is extensive. Well-established documentation, refined over the years, preferential and effective support services, consulting services, migration and upgrade tools, and many other services exist for the mainstream SQL databases. Newer databases, many based on open-source, do not offer the same type of support. While community support may be extensive and paid-for support available, it does not compare to large vendor support (at least for enterprises).

### Framework support
Just about every development environment, persistence framework (such as ORMs) and language, supports SQL. From ODBC, ADO.NET, native drivers at the low level, to higher level support of product specific SQL syntax, the support of SQL is extensive. Having to take your favourite framework and find or learn a new database API for another data store may be too much to take on.

### Suitability to low scale applications
SQL shows its cracks at scale. Large databases or those under heavy load need to be custom configured, continuously optimised and carefully nurtured. This is not the case for smaller databases, which are easy to use, self-contained and very little hassle (the definition of 'smaller' is a bit vague). If an application does not require heavy database needs, using a SQL database to do everything may be the easiest, simplest, and most cost effective solution.

### Flexibility
The philosophy of modelling the business semantics accurately in the relational model, combined with the declarative nature of the SQL language, makes SQL very flexible in its support of varying requirements. Once you have defined a structure correctly, you can assume that various combinations of inserts, queries, joins and other operations can be performed on the structure without needing to know up-front what those operations will be. Because other databases are optimised for specific purposes, they are less flexible in their ability to be applied to all the data needs of an application. For example, document databases need careful thought about what constitutes a document (or collection), as it may not be applicable for another, as yet unknown purpose. Windows Azure Table Storage suffers from this problem, where the choice partition and row keys have a big impact on how the data store can be used.

### Performance
For all the noise about SQL under performing in particular scenarios, it generally performs very well. Virtual Machine infrastructure available on cloud platforms is getting more memory, faster I/O (such as SSD support), and faster processors. This makes a cloud based 'commodity' machine a beast that can handle significant load. Poor database performance can also be linked to bad architecture, such as inadequate caching, or bad implementation (such as N+1 queries resulting from ORM use), or over-optimistic transaction isolation. These performance problems can be addressed in the application without throwing out SQL. Virtually any database will under-perform if it is used inappropriately or put under unnecessary and excessive load. NoSQL is still subjected to the same misuse with potentially more damaging effects.

### Post-relational features
SQL database vendors have long been aware of the limitations imposed by the rational model and have, over the years, extended SQL to support 'post-relational' features. This doesn't magically turn them into solutions to every problem (the strength of alternatives is that they are optimised for specific problems), but does add useful functionality in some cases. These features should be used sparingly and with careful consideration, but are useful and powerful if used responsibly. For example, an application that needs geospatial datatype support for storing the location of retail outlets may find it better to use the geospatial functions of SQL, rather than a full-featured GIS solution. 

SQL databases support some interesting and useful features, such as XML fields, semantic search, file-system binary objects, geospatial datatypes and queries, and non-SQL languages for stored procedures (Java, .NET CLR etc.). Interesting in Windows Azure SQL Database is the support of sharding in the [federations feature](http://msdn.microsoft.com/en-us/library/windowsazure/hh597452.aspx), which facilitates the automated scale-out of SQL on Windows Azure.

## What data to model
Data models are simplistically thought of as static structures that can be modelled once and are applicable to the entire application. Typically models are produced that represent either the physical database using ERD (Entity Relationship Diagram) notation, or the application object model using a UML class diagram. Seldom are models produced that describe both or that describe any interaction or mapping between the two. In addition to static structures, models are required to depict the flow of data between services, but this practice has fallen out of standard practice.

The first question is what data do I need to model? Do I model the SQL database, the message structures or the data archive data store? The short answer is that everything needs to be modelled. In modern applications data exists and is persisted in so many places which all need to be carefully understood. For the purpose of ensuring that the application model doesn't stomp all over the application object or domain model, it is better to define *everything* to be all data that is persisted. So where is data persisted?

* On the client in cookies, local databases or files.
* Web server session state or cache.
* Distributed cache.
* Transactional databases such as SQL.
* Databases used for analysis and BI.
* Databases used for specific purposes, such as for search.
* Staging databases used for integration.
* Flat files (or flat database structures), such as with log files.
* Databases external to the application or organisation.

## <a name="DetermineDataSchemas">Determine data schemas</a>
Some of the architectural principles in cloud computing applications (workload model, scalability model, availability model) result in loosely coupled and independent services. In order to implement loose coupling, there should be very little shared between the applications, other than network infrastructure. The sharing of databases should be carefully considered and avoided where scalability, fault isolation, and other requirements become important. As a result, applications can have multiple database schemas.

Consider, for example, a typical e-commerce application. If we look at the stock/catalogue data, we can see that it can be separated into multiple schemas that are related to different workloads, such as:

* The 'master' catalogue that contains pricing, suppliers, distributors, seasons, and all other data needed to plan and operate the business. Administrative users would be the primary users of this data, and it is important that it is correct and accurate.
* The 'stock' schema would be a subset of the master catalogue, and contain the exact number of items for each SKU in stock. This data would result from logistics and distribution processes, and is important for order fulfilment. The load on the database may be quite low if the ratio of product views on the web to orders is high.
* The 'store catalogue' schema is the one that customers use to view products via the web. This database will be read-only and optimised for search. It will contain a subset of the master catalogue, and include links to images and related products. This schema may also contain, possibly in a separate database, all product reviews made by customers.
* The 'recommendations' schema stores browsing history of products viewed, wish lists, orders, and other data that could be used for product recommendations. The recommendations could be derived in real-time, or by a process that mines or analyses the data.

The above example illustrates that something as simple as a store catalogue can, and should, be treated as multiple schemas (and most likely multiple databases) depending on the workload and the requirement. It is easy to imagine that the master catalogue, which is frequently updated by a handful of staff members, has completely different requirements (performance, integrity, availability, etc.) to the schema that thousands of customers are using when browsing the store.

The process, when trying to determine the schemas, is to identify significant requirement variations. These will highlight suspect areas where data is either different, or needs to be handled in a different manner.

### Indications of schema separation
One of the biggest architectural challenges is determining the different schemas within a single application, or collection of services. Getting the schema separation wrong can result in an application that cannot cope under certain scenarios, or is difficult to maintain or extend. The indications listed below can be used as a starting point for identifying different schemas, or as a sanity check against schemas that have already been identified:

* Business separation — In many cases, even though data may be logically contained within a single schema, there may be a clear business reason why they have to be separate. Perhaps the data 'belongs' to a different organisation or department that is responsible for the integrity of the data, or which owns the underlying asset represented by the data (e.g. accounts).
* Service separation — Decisions to separate an application into services because of different workloads (as per the [workload model](#WorkloadModel)), clearly indicate separate schemas. Different services tend to avoid the sharing of a single database resource for availability, capacity, loose coupling, and other reasons.
* Volume — Large differences in the amount of data, either as the number of entities (rows) or the storage required, can strongly indicate the need for separate schemas. For example, in short-term insurance, there can be significantly more quotations produced than actual policies sold.
* Throughput (Velocity) — Similar to volume (after all, high throughput results in high volume), throughput defined as the number of entities/rows processed per second can indicate a separate schema. For example, the logging of products viewed by customers (used for recommendations and analysis) is going to result in orders of magnitude more writes than writes to the logging entities than the product entities.
* Variation — Data that has structural variety can indicate schema separation through the need to store the data in different databases suited to a particular structure. For example, transactional data (such as orders) can be stored within SQL, and searchable data (such as products) can be stored in a [Lucene](http://lucene.apache.org/)-backed database.
* Integrity — Some parts of an application may require more data integrity than others. For example, ensuring data integrity is very important for orders, but less so with product recommendations.
* Availability — When modelling availability, variation of service and feature availability becomes apparent, and this variation also applies to the availability of the underlying data. There is little point in storing data in a highly available database when the application itself neither needs, nor is capable of, high availability. 
* Security and regulatory compliance — Cases where different data has different security requirements, especially when the requirements for specific data are more stringent than the rest of the data, may indicate the need for a separate schema. Increased security should be backed by a business reason. Similarly, regulatory and compliance issues may require that some data, such as customer or credit card data, is treated differently to other data.

### Clearly identify shared data resources
Where data cannot be separated into different schemas and separate databases, the applications have to share the same physical resource. This can create a performance bottleneck and a single point of availability failure. There are cases (such as identity) where it is unavoidable. Shared data resources should be explicitly and clearly identified, so that they can be reviewed for absolute necessity, and dealt with as elements that require special engineering attention.

## Model each schema
Once the schemas have been identified they needed to be modelled in sufficient detail.

### Level of detail and format of models
It is neither practical nor necessary to model all data in absolute detail. Excessive data modelling in the absence of application design and architecture can also inhibit the design and development process. This data biased approach would effectively paint the application architecture into a corner that may be difficult to get out of.

The detail and format of the data model in each schema will largely depend on the development approaches, methodology, type of project, and make-up of the implementation teams. It will also be influenced by the technology choices that are made for both the application and the underlying database. For example, a detailed third normal form entity relationship diagram will not be applicable to a problem that is going to be solved by, say, a key-value store. The experience on the team and the maturity of traditional database modelling techniques, although logically applicable, need to be adapted to handle database requirements that go beyond traditional SQL databases.

The model detail described below extends beyond the database structure, and encourages broader questions about data and the database. The primary consideration, at least during the initial stages of design, is that a one-database-fits-all solution does not exist, so more fundamental questions need to be asked about the data. Therefore, the model detail should contain information about availability requirements, the need for data consistency, the physical location of the data, the types of queries to be run, and other aspects that are seldom asked with traditional databases. These details need to be questioned, addressed and documented.

### <a name="DataModelDetail">Model Detail</a>

#### Structure
The data structure would seem to be the simplest and most obvious part of the data model. This is true with applications that are underpinned by a SQL database, and are built using a bottom-up approach where the business semantic requirements are captured in the physical SQL database. This bottom-up approach is popular where applications are not greenfield developments, where architects have a strong database background and bias, and in environments where DBAs and data specialists have more power and architectural influence than their application development counterparts.

Modern applications and modern software engineering development approaches are not necessarily structured bottom-up and are more concerned with the domain model (from [Domain-Driven Design](http://domaindrivendesign.org/)), which is implemented in the application layer. This application model centric approach puts physical database modelling much later during implementation. Once their classes are built and tested, developers will begin the process of developing or generating a physical database model that then gets mapped to the domain.

The reality in most enterprises is that the relationship between application developers that support a top-down approach and database professionals that support a bottom-up approach ranges from an uneasy truce to outright hostility. Both have their (valid) points of view, the debate will never be settled, and resolving it is definitely out of scope for cloud projects. For the purposes of the data model in the early stages of design, either approach is good enough, but the bottom-up approach does run the risk of making premature assumptions about the physical database technologies and implementation. Using a SQL biased model, such as an ERD (Entity Relationship Diagram) or even physical database scripts, serves as valuable input to the application design process (to help create the Domain), but should not be seen as the final structural model.

The structural model should be a logical representation, rather than a physical one, and contain metadata that represents the structure that is as close to the (logical) domain as possible. Depending on the particular development methodology it can be an ERD, Class Diagram, XSD, or similar model that uses the notations of the modelling language. It can also be a simpler and less formal diagram created in something like Visio, which uses simple shapes and a less rigorous notation.
The model should at least represent the following:

* Entities/Classes
* Primary Keys/Object Identifiers
* Natural Keys
* Relationships/Foreign Key Constraints/Collection Properties
* Data types
* Naming, particularly Entities/Classes and Fields/Properties

The data structure model is expected to evolve, and become more fixed over time as the application is implemented. Some parts will end up as specific classes, with accompanying detailed SQL scripts and mapping for the database. Others may stay logical, as application developers make use of data stores where structure is less important. By the time the structure reaches those levels of detail, its influence on the overall approach to data is minimal. The initial logical structure, as part of the data models developed during design, are the most important for providing valuable input.

##### Unstructured data
Where data is deemed to be 'unstructured', it may be tempting to skip modelling the structure. Unstructured generally refers to data that doesn't fit well into a relational model, or has a structure that is variable at runtime. Unstructured does not mean that it has no structure at all. Depending on the type of data, the structure model should contain identifiable attributes that will be useful when processing the data, such as source, date, length and tags.

#### <a name="DataModelDetailVolume">Volume</a>
Data volume refers to the static volume of persisted data, not the number of reads/writes (dealt with as velocity/throughput). The following metrics relating to data volume need to be understood for each schema:

* The number of entities.
* The size of the data to be stored.

More volume leads to bigger databases and bigger databases require:

* More storage.
* More memory (to fit as much of the data and indexes in memory as possible).
* More load (as a lot of data needs to be processed for queries).
* More complicated operations (in order to maintain availability).

It is important to extract the number of entities that need to be stored from the business case and the [lifecycle model](#LifecycleModel). The primary entities are important, where some heuristics can be applied to additional entities. For example, in an e-commerce application, it is important to establish the number of products, as the primary entity, and come up with averages for the number of SKUs per product, number of reviews per product, and so on. Bearing in mind the structure model, and the application developer bias, it is easier to count the volume of classes that need to be serialised and persisted, where the collections contained within those classes are persisted with them. For architectures that tend towards a document database, counting the documents in a collection and the size of documents is also quite simple.

In order to determine volume, the size of each entity also needs to be determined. Again it may be preferable to apply some rules to determine related entities or the sizes of contained collections. The length of fields is important, including the datatype (which determines the physical size of the field). How full the variable length fields will be, by using averages, is more useful than the maximum size. Depending on the database technology used, the physical size of the index can be very important, so it may be useful to include an indication as to which fields are indexed. 

#### <a name="DataModelDetailVelocity">Velocity/Throughput</a>
The rate at which data needs to go in and out of the database becomes the main consideration in terms of both application and database design. It is obvious that understanding, documenting, and verifying (based on an accurate source) that data throughput becomes an important part of the data model.

However, because of application design, the database velocity is impossible to determine up-front. It can be precisely measured in a production environment, but when designing the database it is difficult to come up with definitive numbers. The application may be designed to process data asynchronously (effectively delaying and throttling write operations), or may make extensive use of a cache to reduce database load. Running applications are also subject to variations of load over time (as uncovered in the [lifecycle model](#LifecycleModel)), and may have different (and unexpected) usage scenarios that result in a high number of (application) cache misses that affect the database load.

The data model should contain estimates on the following:

* Average and peak reads — the number of rows/entities that are read from the database. This should reflect more simple operations and not complex joins (see query needs below).
* Average and peak writes — the number of rows/entities that are inserted and updated. These will generally be fairly simple operations but may include more complex multi-table transactions.
* Average and peak inserts/appends — appending data impacts databases differently to updates (such as updating of indexes), and should be described separately. Schemas that are mostly inserting data (such as log file appending) are likely to have a different database technology to one that constantly updates an existing dataset.
* Average and peak blocking writes — While not required for all schemas, where locking can become an issue, estimating how frequently this will occur is important for application and database design.

Since exact numbers will be impossible to determine during design, it is preferable to document the velocity as part of the application velocity and allow the developers, architects and database specialists to use that information to estimate as and when it is necessary, and as the design evolves. This can be achieved as follows:

1. For each workload (from the [workload model](#WorkloadModel)).
2. Describe the use cases (features) that require database operations.
3. Express each feature's database operation against the logical data structure model.
4. Then extract the peak and average number of times the feature is used from the [lifecycle model](#LifecycleModel).

This would result in something like the following example:

The product search workload consists of the search and view features:

* Search is a read-only operation against the [product] only (excluding reviews, etc.).
* Searches will average 20 per second and peak at 500 per second.
* View needs to view the entire [product], including [reviews] and [personalised recommendations].
* There will be an average of 10 views per second, peaking at 200 per second.
* Each time that a search or view is performed the data about the search/view needs to be stored for analysis and recommendation.
* Authenticated users need a search/view history stored against their account.
* Authenticated users will perform 25% of active searches/views.

The above example contains critical information about data volume while not attempting to be specific. It should be a sufficient expression of volume for the data model, provided all other significant workloads and features are similarly described.

#### Performance
Database performance is one of the most important aspects of data in modern applications, particularly applications that need to scale and are exposed to large user bases. Perhaps database availability is on a par with performance, but other features such as data integrity, flexibility, and security (surprisingly) take a back seat to performance. Hence the rise of NoSQL databases that are performant, but only fit a narrow set of other requirements.

Requirements drive architecture and technology choices, which in turn feed back to an adjustment of the requirement based on cost effective delivery. The performance information contained in the data model needs to capture both sides; the requirements and the performance capability of the underlying technology.

##### Performance requirements
Application performance requirements can be lifted from existing models ([availability](#AvailabilityModel) and [health](#HealthModel) models). These requirements need to be turned into database performance requirements by stripping down to the database calls.

As an example, consider a requirement for a web page that states that the page must render on the client within 2 seconds. The time taken to deliver a page to the client leaves a database time of 400ms.

| Step 	| Time Taken	|
| ----------------------------------------	| ------	|
| Page transmission time (200kb at 8Mbs) 	| .25s 	|
| Page browser render time 	| .5s 	|
| Web server identity and state check 	| .25s 	|
| Web server page generation time 	| .5s 	|
| Server compression and encryption 	| .1s 	|
| Total (page generation, render, transmit) 	| 1.6s 	|
| Remaining time for database access 	| .4s 	|

Having a figure such as this is a good place to start. Combining it with the volume and velocity provides a requirement that can be used to make decisions about the database. 

##### Volume/velocity/performance statements
The above example can be stated as:

* There are 100,000 products (volume).
* There are on average 20 product views per second, peaking at 200 per second.
* The product data needs to be retrieved within 400ms.

The above example statement about the volume/velocity/performance of data is detailed enough for initial architectural and design considerations (and considerably more than most projects have available). Such statements should be created for each of the primary entities, and include all of the major (or most used) features against those entities.

#### <a name="DataModelDetailFormat">Format</a>
If it is possible to model the structure of the data beyond a logical model, the model should contain the formats of the data. The amount of detail depends on the requirement, but typically more detail on the format will be required with unstructured data, data that comes in documents from external sources (such as XML documents), and data generated by applications (such as log files).

Common formats are listed below:

* SQL - Tables, rows, fields and stored procedures.
* Document serialised application object (XML, JSON).
* Binary data (audio, video).
* External documents.
* Feeds (e.g. RSS).
* Logs (files or API).

#### <a name="DataModelDetailDataSource">Data source</a>
The source of the data needs to be described in the data model. This is useful for determining:

* The rate at which data can be produced. User captured data cannot be produced at the same rate as application generated data (such as a financial rates feed). 
* The flexibility of the source to change how it sends data.
* The accuracy of the data.
* How much the data can be trusted (consistency, security etc.).

Data can come from many different sources. The most common classes are listed below:

* Core application data (that results from normal usage scenarios).
* Application log data.
* Operating system or platform log data.
* External transactional data (such as shipping confirmation from logistics partner).
* External analytics data (such as data from Google analytics).
* External datasets (such as a customer profiling dataset).

The identification and classification of data sources allows designers to prioritise the development of architectures for each one (core application data needs the highest priority), and feed it in to the planning and project management. Depending on the team, some data sources can be dealt with by completely different teams, such as teams that are focussed on integration work.

#### Query requirements
One of the biggest problems with traditional (SQL) databases is that they have become such a valuable store of business data. Because of this value, the demands for extracts and information out of them become so high that they are unable to serve their primary functions. Many of those primary functions even get forgotten, as the database becomes the source for all business critical information. The amount of data stored, and the flexibility of the query language, means that the database can serve almost any request, and these are often put in batch runs that take longer and longer. Eventually the suitability of the database is questioned as it collapses under unreasonable load.

Cloud architectures tend towards loosely coupled services that share minimal resources in order to meet availability, scalability and other requirements. This extends to the avoidance of shared database resources (see [shared data](#SharedData) below). An application should discourage the sharing of its data beyond the core requirements of the application. The first steps in modelling the query needs are:

1. Determine if the query is part of the core application functionality, or if it is a nice-to-have because of the value of the data. Too many non-core queries will bog down the database and impact the availability of the application.
2. If the query is not core functionality, consider where the data can be copied in order to satisfy the query. Look at another service that can receive data asynchronously, and store the data for future, nice-to-have, and unpredictable use.

Not all queries place the same load on the database. The queries considered core to the application and schema should be briefly described in the data model in terms of the following:

* Complexity — Single table/entity queries that make use of indexes and are forward-only (no grouping or ordering), are considerably less resource intensive than complex joins. The ability of SQL to support complex queries that can be created by people with rudimentary SQL skills (or at least no insight into the underlying impacts of the query) is the main reason for poor database performance and DBA headaches. Complex queries do not need to be avoided, it may be part of the requirement, but being clear about the level of the complexity of queries is crucial to include in the data model.
* Execution time — The time that it takes for a database to return a result is primarily dependent on the query complexity, the volume of data that needs to be queried (especially when table scans are required), and the amount of data that needs to be retrieved. The data model should explore execution times of queries and strategies to reduce them. For example, retrieving fewer rows, reducing complexity, and so on.
* Query execution environment — Traditionally the database was considered the place to do all the 'heavy lifting' with queries, but other approaches are becoming popular in order to make optimal use of database and compute resources. Where the queries actually run, which may be split between different parts of the application, needs to be described in the data model. Options include:

	* Database — the query runs on the database server (typical SQL model).
	* Application - the application retrieves some data from the database and performs additional processing to get the result. The application may derive values (calculated fields), or combine one result with another from a different data source.
	* o	ORMs and sharding are executed in the application layer and are more specific types of querying. When using an ORM, the object that is loaded into the ORM may be partially or fully loaded and the application will chose what parts of the object it is interested in, rather than building database specific queries. Manually sharded databases require some application pre and post-processing in order to obtain the required result. 
	* Query tools and environments — Some queries can be executed on platforms that are specifically developed to handle queries. Environments such as Hadoop and its supporting tools (Pig and Hive) are such an example.
	* Dirty reads — the requirement consistency of the data returned from a query is fundamental to implementation decisions (see [consistency](#Consistency) below). Allowing for lower levels of consistency, and more dirty reads, means that queries can be tuned for faster performance and lower load directly against the database. This can be accomplished by using a 'read uncommitted' transaction isolation level, or other techniques, such as the use of cache, or materialised views. High frequency and high load queries must have their tolerance for dirty reads explicitly stated in the data model. 
	* Schema changes — queries that are tightly coupled to physical structures can cause problems if the underlying schema changes. For example, Windows Azure Table Storage, which requires some creativity when determining row and partition keys, will have queries tightly coupled to the keys, and they cannot be altered without having to rework queries. Any queries that are less able to handle schema changes should be explicitly noted in the data model.
	* Ad-hoc queries — one of the strengths of SQL is the ability to support ad-hoc queries, whether giving the users query tools, or requesting data from support staff. Databases that have poor support for ad-hoc queries (such as Windows Azure Table Storage) may need to export data, or have code written to support ad-hoc requirements.

#### Geographic location
Cloud computing platforms provide the capability to store data in datacentres around the globe. Most applications will be hosted in a datacentre in the same region as the primary business and will host the data and application as close to each other as possible (via affinity groups in Windows Azure).

##### Sovereignty
When storing data on a cloud computing platform the data sovereignty needs to be considered. Data sovereignty encapsulates any country-specific regulatory and compliance rules that organisations must adhere to in terms of data storage. For example, there are regulations that require that data collected about European customers, must be stored in Europe. Many of these sovereignty requirements are a myth, and a project must uncover the precise regulation and interpret it according to the requirement, application and data. It may turn out, for example, that data can be stored anywhere as long as it is encrypted. Or it may be that the regulations only apply to some data, and not all. The data model will need to detail any data which is subject to regulations that limit where it can be geographically stored, including supporting analysis.

##### Latency
When the application and database are not co-located (for reasons such as availability), latency to the database is likely to become an issue. Because of this, it is rare to have geographically dispersed data, but if it is needed, it must be clearly described in the data model. Since latency and throughput is such a big problem, the data model should also contain some sampling and tests of what the platform is able to support.

##### Data gravity
Data gravity is the tendency for applications that consume data to gravitate towards the data store. This is because of the cost of data transfer, latency, available bandwidth, and ease of operations. The data model should consider data gravity and note that data that is of high importance and high volume will have a higher gravity. In enterprises, this could become an issue as there may be a resistance to running applications in the cloud just because that is where most of the data is. This might result in valuable data not being used at all.

#### <a name="Consistency">Consistency</a>
Monolithic SQL RDBMSs have allowed application developers to take immediate data consistency for granted. Immediate consistency is a consistency model where the result of an update is immediately visible to all observers and is common in monolithic databases because the data is in a single, shared location. Exclusive locks on resources, two-phase commits and immediate refreshes of updated data have allowed data consistency to become easy, and not worth another thought. The problem is that high database consistency is difficult to maintain at scale and with high availability; two attributes that are important in modern applications.

At some point scalable and available databases have to migrate away from a monolithic infrastructure, and move to a more distributed database. This is due to the physical or financial scale-up limitations of monolithic databases. When moving to a distributed database model, maintaining high levels of data consistency becomes a bottleneck that requires significant engineering effort, cost, or other resources, to address.

A constant questioning of the data consistency business requirement is needed. Business always seems to ask for immediate data consistency, often out of habit, or because they don't really understand the implications of what they are asking for. Questioning the need for data consistency doesn't mean making compromises on other ACID properties. Atomicity requires that a transaction must not partially succeed (through faults of part of the transaction or power failures), and a transaction that is not atomic leaves the database in an inconsistent state. It is this 'inconsistent state' that causes people to ask for immediate consistency when they are actually asking for atomicity.

The default position for any architect is that immediate consistency is unnecessary, and that eventual consistency will be the consistency model that is implemented. Once this position is created, the architect must also ensure that opportunities are created to challenge the eventual consistency position and model. This allows for flexibility in consistency options, and in turn allows for database choices that are biased towards other aspects, such as performance. This position should be clearly stated in the data model, and any exceptions should also emerge. 

If eventual consistency is the default model, some situations may arise where this is problematic. These need to be discussed, and the compromises and solutions documented in the data model. For example, in an e-commerce application business may be under the assumption that an order immediately reduces the available stock, so users viewing a product will not see that there is stock when it has just been sold. Using eventual consistency there is a chance that one user (on one database node) will see stock availability that doesn't exist (on another database node). This is addressed by looking at how frequently this can occur, and what the business implications are e.g. it may be a problem for allocated seat ticket sales for a popular concert. If the business can get away with an apology (perhaps accompanied by a discount voucher), or if stock inconsistency in the warehouse is frequent in business anyway (as discussed in [databases on acid](#DatabasesOnACID)), then immediate consistency may be deemed unnecessary. The architect can point out that even with immediate database consistency there is still a chance of a user assuming that stock exists due to the inconsistency of the data in the browser (which may be minutes out of date if the user is busy with something else). In this scenario, solving the consistency problem requires more than database effort, but for live updates to be sent to the browser too; something that would often seem unnecessary.

#### <a name="SharedData">Shared data</a>
'Shared data' can mean different things to different people, and it is important to be clear about the 'sharing' part of the term. Interconnected systems, whether as part of a single application with loosely coupled services, or as a loose assembly of different applications, need to share data. They will share product information, customer information, code tables (such as region lists), configuration, and even health information. The sharing of data is both necessary and desirable for applications. It is the ability to share the same data, and present it in different contexts for different audiences, which allows applications to be developed in order to take advantage of new opportunities. The sharing of data presents two opposing problems:

1. If the shared data is in a single place, and the applications need to share a single resource, the resource becomes a bottleneck (single point of failure and subject to excessive load).
2. If each application, or part of an application, keeps its own copy of shared data there is a tendency for divergence from the master, data to be lost, and data integrity to suffer.

When talking about 'shared data', make sure that you are explicit about whether or not it is a 'shared resource' or 'shared database' in order to avoid confusion.

The sharing of data between applications and services is an important part of the data modelling process. Places where data is shared, but not the resource, indicates a clean service boundary, and subsequently a data store or data model that could be optimised for the specific service.

The sharing of data, and database resources, is probably the single biggest problem in worldwide IT — probably with more vendors, products, technologies, methodologies, and people than any other aspect of IT. This *overview* of what needs to be considered in the sharing of data is just that, and readers are encouraged to seek out additional material and specialists, depending on their needs.

##### Techniques for sharing data
Data is shared in a number of different ways, clearly modelling how data is shared between services helps to define the service interface, the contract, the format, and allows the optimal data store to be chosen. Some of the logical data sharing approaches are listed below:

* Shared resource — the common approach to sharing, where a single database is used by all parts of the application.
* Replication — separate databases running the same software, such as SQL Server, and the same schema. Vendor provided replication tools ensure that data is copied between multiple databases.
* ETL (Extract Transform Load) — The most common way of sharing data in and across enterprises. ETL tools, such as SQL Server Integration Services, or custom code is run, typically in batches. These tools take data from one database (or file exported from a database), and put it in another while performing necessary transformation on it in order to fit the target schema.
* Full export/import — a database (or parts of a database, such as a single table) can be exported in full and imported into the target database. Usually this overrides all data in the target.
* Read to cache — applications can read data out of a master database, and load it into the cache. The cached item then exists in a separate physical data store from the original data. 
* Messaging  — applications that have a lot of asynchronous messaging contain data in the message body. TThis can be considered "Don't care" data sharing, where the sharing of data is necessary to process the transaction, rather than having to be managed in a separate database or schema.

##### Reconciliation of data changes
The big problem when sharing data is what to do when changes occur in the consuming database (or databases). The reconciliation of changes, and updates to the master, can become problematic. This is more frequently a problem in enterprise applications where there are so many applications that have been built up over years, with applications running in parallel after acquisitions, and everything barely held together by nightly batch runs that try and copy data back and forth. It can happen in green-field modern applications too.

Consider the example discussed in [determining data schemas](#DetermineDataSchemas) of a master catalogue that publishes daily to the web store catalogue. It would be fair to expect that a feature is needed to be able to update product descriptions on the web store if errors are detected, as a live update of an erroneous description would be needed, rather than waiting for the next publish. How does that update get reflected back in the master, so that the next publish doesn't stomp all over the web store catalogue again? What if the master catalogue doesn't respect changes from any consumer application, either because that is the business rule, or because it is an off-the-shelf-product that doesn't have such a feature? Is the update back to the master done directly and automatically by the web store catalogue application? Or does the user, who makes the change, have to log in to both applications and make the change twice?

Some of the approaches to reconciling data are described below:

* Expiry — shared data can be set to expire and after expiry the master can be re-fetched. Caches work based on expiry.
* Full refresh — the master is always the master and the consumer data is periodically cleared and refreshed with the master data.
* Master Data Management (MDM) tools — [MDM](http://msdn.microsoft.com/en-us/library/bb190163.aspx) is the enterprise solution to this reconciliation problem, particularly where data is considered an asset (such as accounts data), and regulatory and compliance issues need to be addressed. MDM has tools, services, vendors, licenses, and dedicated resources to deal with the problem enterprise-wide.
* Multiple versions (irreconcilable) — it may be unnecessary to reconcile, and different databases can maintain their own versions of the data. Using the master/store catalogue example from above, the store catalogue can keep its changes to descriptions in its own database and always use those regardless of the master description.
* Manual — for low volumes, it may be possible for staff to perform updates on both systems. This is common where shared data is part of an application config file that is updated by operators.
* Custom update service endpoints — One of the most architecturally correct ways of handling reconciliation is for the master database to be exposed as a service that has service methods/endpoints that permit updates. This way, any updates can be properly applied (partial updates or resolving conflicts), be performed asynchronously, as well as being able to apply layers of authentication and authorisation. The problem is that it can be costly in terms of development and maintenance effort, for both the master service and the consuming service.
* Ignore — By far the easiest way to handle reconciliation is to ignore it and not be concerned about updating the master. In order to ignore reconciliation it is important to adapt the business processes (or at least make them aware) to be less dependent on data consistency, sharing minimal data, and making sure that explicit calls are made to the master (when data consistency is important to the process). The technical options and considerations include:

	* Expiry — Cache is the most common example where reconciliation is ignored. Caches are generally not updatable, so they never have to be reconciled with the data. Rather, items put in the cache are made to expire either explicitly or by time based rules.
	* Delete when done — Consuming applications can keep the shared data only as long as it is necessary, and then deleting it when it is no longer needed. For example, the customer details may be required by a fulfilment application, but can be deleted once the order has been successfully shipped. This reduces the risk of data in the consuming application being updated on the assumption that it is reconciled, or used when it is totally out of sync with the master.
	* De-normalising and modelling data as temporal — Data on consumers can be modelled as being fundamentally temporal (related to time), and is often easily implemented within a de-normalised structure. For example, customer data is required to fulfil an order, but rather than creating a 'customer' structure or table, add the customer attributes to the order structure. This allows for not needing a customer entity that needs to be reconciled, as the data is only valid for the one order.

##### Data sharing model detail
The data model should contain the following detail on data sharing:

* Data flows — data that crosses the service boundary, either incoming or outgoing.
* Contracts that are established between the source service and the consumers of its data. The contracts are physically implemented as RESTful services, RPC calls, database APIs, and others, but should be able to be expressed logically.
* Security of the interface, authentication, and authorisation of the consumers' needs to be understood.
* Data Consumers — Even if contracts are rigid, the consumers of the data need to be listed and understood. This allows for the risks to the data (based on its usage) to be established.
* Master data approach — if the service is providing master data, how consumers reconcile back to the master (if at all) needs to be identified.

#### <a name="DataAvailability">Availability</a>
Data and database high availability is one of the most complex parts of a solution to architect. The problem with data availability is that databases generally tend to contain shared state, and the loss of a single node can take out the entire application. This makes database availability more complex than service availability.

Data and database availability needs significant attention in design, development, testing, and operations. This section deals specifically with data availability in the context of the data model, but should be read in conjunction with the [availability model](#AvailabilityModel).

##### Data availability outcomes
Databases, as a bottleneck and single point of failure, are often a basis of application availability. The following availability outcomes are heavily influenced by data availability:

* Poor performance — Monolithic databases are frequently bottlenecks of the application. Databases may not be able to cope with the load they are put under. Queries will timeout, deadlocks occur, and a host of other performance related issues result from an overloaded database.
* Application availability — As a single shared resource, and often the only single shared resource of an application, the unavailability of the database can be the root cause of cascading faults.
* Loss of data — Databases can fail without recent backups or with the database in an inconsistent state. Applications can delete or corrupt data due to a bug. Rushed deployments, untested scripts, and operational panic can also cause a loss of data. Data loss can occur and not be noticed for a long time, often when it is too late to easily recover.

##### Data availability influencers
Before developing the approach to data availability, it is necessary to understand the availability influencers, because it is the probability of occurrence of a type of failure that largely determines the choice of technology and architecture. For example, a high traffic web application is probably more likely to have database performance issues than infrastructure failure, so an architecture that is performant would be more important (in terms of availability) than one that can recover from hardware failure. The following are primary influencers of data availability:

* Datacentre infrastructure issues — failure of something in the infrastructure hardware or software is surprisingly common. Individual disks may fail, network connectivity to particular nodes may be lost, memory leaks can cause a node to grind to a halt, or any number of problems can occur with specific hardware, networking or operating systems.
* Database performance — the most common cause of reduced data availability is the inability of the database to respond to requests. Databases that struggle under load cause database operations to take too long, and applications to time-out and fail.
* Application errors — as the primary interfaces to databases, application defects can cause problems with databases. Applications can erroneously update data, execute too many queries (such as from misconfigured ORMs), execute sub-optimal queries, or even delete data by mistake.
* Temporary datacentre outage — outages of public cloud datacentres are well known and highlighted by technology news sites. Temporary outages will probably last for a few hours and impact availability of specific databases once every year or two.
* Platform-wide outage — similar to temporary datacentre outages, platform-wide outages do occur occasionally, but are more difficult to deal with because they are not limited to a particular region or logical fault zone.
* Datacentre destruction — the complete destruction of a datacentre, where all data in a particular datacentre is permanently lost. This has not happened to any of the major cloud providers yet but should still be catered for.
* Internet access outage — enterprise applications where users or consuming services are on the local network, and the application or data on the public cloud are susceptible to loss of Internet access.
* Data corruption — corrupt data, caused by application errors, database faults, or other problems can result in impacts on data availability due to incorrect results, or the time taken to rebuild and fix corrupt data.

##### Techniques for data availability
The data model should contain, for each schema, the approaches taken to ensure that the availability requirements are met. The approaches taken to data availability, particularly those that are implemented at the application level will be the same as for general availability and should be contained within the [availability model](#AvailabilityModel). Since the data model will be developed at the level of the schema and will reference particular database technology choices, the detail should be more specific than is contained within the availability model.

The base principles for highly available data (for systems that never fail) are the same whether it is for a communications satellite in space or a large web application. These principles can be summarised as:

> Parallel storage and parallel retrieval with isolated stores.

This means that when data is written it is to more than one place (parallel storage). A process needing data should retrieve it from more than one place, in case one source has failed (parallel retrieval). Data stores should be shared as little as possible, so that there is less likelihood of a rogue process reducing the availability of a data store (isolated stores). These simple principles are not easy to achieve because of the nature of shared state, the volume of data, and the development costs. Practical compromises have to be made; after all, not all applications have a requirement for such high availability (where the system never fails). Some of the approaches to increasing data availability are detailed below:

* Data isolation — a service should have its own data source that is not accessed by any other services. This reduces the likelihood of unexpected load, data corruption, or other problems caused by other services that are not immediately controllable. Isolated data also improves the recoverability of the service. If other services require data, it should be shared by giving them copies, rather than sharing a single resource.
* Reduced dependency on a particular data source — a dataset that only exists in one place is effectively a single point of failure. The dependency on a single data source should be reduced by storing data in an alternative store (even a cache can be considered an alternative), and having an alternative access path to that data store. Applications can, for example, be developed to retrieve data from both the underlying SQL database, and the document contained within the search index.
* Eventual consistency — not aggressively pursuing immediate consistency opens up architectural options that increase availability. Eventually consistent data reduces the demand on single nodes, thereby removing performance bottlenecks and failure points.
* Distributed databases — distributed databases allow for load to be spread across multiple nodes, i.e. the same data to be stored in a multiple places (including multiple datacentres), which decreases the impact of failure of a single node. Distributed databases require eventual consistency, and should be considered a single store, as a software defect in the database software itself can bring down all nodes.
* Database snapshots — taking database snapshots so that point-in-time restores can be made is a common technique for restoring the database to a good, known state after data corruption.
* Data replication — where databases have the same technology and schema, data can be replicated from one node to another. Replication is frequently used for geographically dispersed applications, or as a technique for sharing data. When applied to availability, replication can be used to synchronise data to a passive standby node, and is particularly useful when applied between geographically separated datacentres as part of a geographic disaster recovery strategy.
* Fail-over — because SQL databases are not distributed, the most common technique for availability is to use a database that supports fail-over. Fail-over can occur through automated replication or shared storage. The big problem with fail-over for application developers is that the switch-over to a new node can take a few minutes and requires retries or other fault handling.
* Application fault tolerance — Applications need to be able to handle data availability problems. This could include retries for transient faults, switching to alternative data sources, delayed processing and, at the very least, graceful failure that communicates the problems clearly.
* Platform reliance — one of the advantages of cloud based data and storage services is the high degree of availability, durability, and fault tolerance built-in. This means that nothing needs to be specifically implemented, as the platform will take care of some of the most serious issues. For example, Windows Azure Storage has built-in support for redundantly copying data to multiple locations, so durability is of little concern to application architects and designers.

##### Windows Azure specific data availability features
Windows Azure has been engineered with availability in mind and has built-in support for availability, as outlined below:

* Windows Azure SQL Database has a default configuration where data is stored in three replicated databases, and if the primary node fails, the connection will fail-over to one of the other two, and a third database will be instantiated. Other [operator accessible features](http://msdn.microsoft.com/en-us/library/windowsazure/hh852669.aspx) include database copies, backup to Windows Azure Storage, and data synchronisation.
* Windows Azure Storage is designed as a highly available redundant storage service that underpins many features of Windows Azure, including drives for virtual machines. Apart from service availability, data is stored in multiple locations, using either [locally redundant storage](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/08/introducing-locally-redundant-storage-for-windows-azure-storage.aspx) or geo-redundant storage.
* Virtual Machines do not have any built-in support for high availability when used as hosts for databases, and it is up to the database software to satisfy availability needs.

##### Data model availability detail
In terms of availability, the data model should initially contain the availability requirements for each schema. This should be fairly easy to lift out of the availability model, and review specifically for data.

As the architecture and design progresses, more can be added to the data model, particularly the approaches to availability. There will be some overlap with approaches documented in the availability model, and care should be taken to deal specifically with data and databases. As architectural decisions are made and database technology choices are finalised, the availability will be dependent on the solution chosen, and further detail may be unnecessary. Also, different schemas will share the same approaches, architecture and technology, so repetition per schema is unnecessary.

Ultimately, the detail described in the data model is to elicit discussion, reduce risk and find solutions, so sufficient detail is required to be able to make the decisions. For some applications and schemas it may be quite simple, but for a high throughput web application that serves millions of users, a significant amount of detail is necessary.

#### Security
The data model should not attempt to address all security issues relating to data. Data security should be dealt with more holistically within the [security model](#SecurityModel), rather than independently within the data model. Only covering security in the data model exposes the database to the risks and threats of an unsecure application.

The data model can be used to highlight specific, known security requirements of the schema. These requirements can then be fed into the security assurance process in the security model. Such requirements include:

* Statements about privileged access to raw data, including physical datacentre access.
* The need for encryption of data when it is stored (either at application or database level).
* Security of backups.
* Security requirements of transport protocols used for data access.
* Level of database exposure to data access endpoints. For example, does a RESTful API really need full table access, or should some authorisation be applied?
* The requirements for different database access credentials across multiple services.

#### Health monitoring
The [health model](#HealthModel) needs metrics for data and database operations. The data model should list available and useful metrics to be tracked for health monitoring. This allows engineers to determine the best source of metrics, such as the application, database environment, or platform API. Ensure that the metrics are made available for the following:

* Performance — including execution times of queries, I/O performance (if available), and network latency.
* Volumes — including number of transactions (total and per second), amount of data transferred and number of rows/objects
* Storage — database size, and storage space used.
* Data sharing — health of replication, and similar data sharing methods, and rate of messages sent or processed.
* Failures — query timeouts, deadlocks, and update/insert exceptions.

##### Monitoring data in Windows Azure
Data in Windows Azure is usually stored in Windows Azure Storage, in Windows Azure SQL Database, or in self-hosted databases in virtual machines. Virtual machines are easy to monitor as they support all of the profiling and performance monitoring that is accessible to the deployed operating system or database.

Windows Azure Storage offers health monitoring metrics that are accessible via the management portal, and can be viewed on the Azure monitoring dashboard. Storage accounts can be configured to monitor data via varying levels of verbosity, and the data retention period can also be set. For more information, see [How To Monitor a Storage Account](https://www.windowsazure.com/en-us/manage/services/storage/how-to-monitor-a-storage-account/) in the Windows Azure documentation.

Because it runs on a multi-tenant environment, Windows Azure SQL Database does not have the same monitoring options that are familiar to DBAs monitoring SQL Server. There is a distinct lack of OS level metrics which are usually viewed in Perfmon. Windows Azure SQL Database has [dynamic management views](http://msdn.microsoft.com/en-us/library/windowsazure/ff394114.aspx) that can be queried for information on blocked or long-running queries, resource bottlenecks, poor query plans, and other database specific information that excludes lower level metrics, such as disk I/O.

#### Operations
Considerations need to be made in the data model for the operational requirements and effort, as they can vary greatly depending on the technology choices. SQL databases tend to have higher operational demands than non SQL services (such as Windows Azure Table Storage). NoSQL databases that are hosted within Windows Azure (such as MongoDB) may require lower operational effort, depending on how they are used. Knowledge and skills for monitoring SQL databases are also more widely available and better understood than NoSQL databases. This means that although SQL databases require more operational support, the learning curve will be shallower, and the likelihood of mistakes lower.

As with health monitoring, the operational detail should be contained within the [operational model](#OperationalModel), and care should be taken not to include such information in the data model. The data model should only document specific aspects of the data, and the schema in particular, that are relevant to operations. The following operational tasks should be documented within the data model:

* Health monitoring — how health metrics collected about databases, and database operations, are monitored, and how operators respond to diminished health.
* Scaling — one of the responses to diminished health may be to scale the database. This needs to be thought through carefully, as it is potentially one of the most complex tasks to be performed by operations. The ease at which operators can scale the database, against the realistic requirements, has a significant input on database design. Depending on the technology, it may be as simple as adding extra nodes, or more complicated such as re-sharding a database, or even having significant downtime, as a monolithic database is moved to a larger infrastructure.
* Backup and restore — requirements and processes for backing up data, securing backups, testing backups, and restoring backups.
* Deployments — how changes are made to the database when application deployments are made. This influences the database design, as services may be independently deployed, and database changes should be limited to a single service and/or schema. Data deployments may include schema changes or the updating of data.
* Data corruption resolution — Operators are frequently called in to fix a database after it has been corrupted due to application errors, partial data imports, mistakes, or even database software issues. The data model should at least try and identify the probability of corrupt data and the impacts of it, in order to determine the need for explicit operational processes to repair data.

## Assess data storage options
Once the detail has been documented for each schema, a set of technologies should begin to emerge. This will be additionally influenced by non-technical factors such as:

* Available database skills.
* Organisational bias towards a particular technology. 
* Existing licensing or support agreements.
* Maturity of the technology.
* Suitability of the technology vendor (e.g. open source may not be seen as enterprise friendly).
* Experiences from other teams.

This book does not attempt to cover the database decision process. The answer to such big decisions is always 'it depends...', and the detail is contained within the data model. An architect that has such a good level of detail in the data model is in a good position to make the correct decisions about the database. There are some points that are worth highlighting which are discussed below.

### Cover all data
Take care not focus only on transactional data (e.g. SQL), or the technology choices are likely to lean towards a one-size-fits-all technology that may be unsuitable in some cases. Make sure that time is spent on other data stores such as:

* Cache.
* Persistent messaging.
* BLOB and rich media.
* Unstructured data, such as log files.
* Data storage for analysis and BI.
* Feature optimised databases, such as search data.

### Rationalise technology choices
The data model encourages modelling for each schema. This places a focus on the database needs for particular services, rather than trying to share a single database resource between multiple services. When choosing specific technologies it makes sense not to choose different databases for each schema and/or services. Confronted with, say, MongoDB being best suited for one service and CouchDB for another, it would be better to choose one for both. Even if the databases are separate instances, which they probably should be, the benefits of shared knowledge, operational procedures, known performance characteristics, and simplicity, favour making compromises to fit a particular technology across multiple services and schemas. 

### Assess candidate technologies
Once technology choices are narrowed down, they need to be assessed in order to make a decision on the best database choices. Assessment of candidate technologies need not be arduous, and the cloud computing platform provides a mechanism to test and deploy on a production quality infrastructure without having to wait for test hardware to be procured and installed.

#### Research
The first step to assessment is to perform the necessary research on the candidate technologies. Since most of the choices will either be from well-established vendors (such as Microsoft), or well-known open-source projects, the research is fairly easy to do. The following steps are necessary when researching the technologies:

* Find the experts — experts seem to congregate on specific websites and forums. Microsoft technologies have a number of experts that frequent the Microsoft forums. Open source technologies have experts that frequent product specific forums, but views (to varying degrees of being balanced) can be found in the (Hacker News)[http://news.ycombinator.com/] threads. Paid-for expertise can also be found through consulting services, or the packagers of open source distribution.
* Establish the current state — whilst not too relevant to SQL databases, the opinion, suitability, and features of other databases change more frequently. Be careful not to use information that is too old and make sure that both sides of the argument are understood. For example, blog posts are often titled "Don't use \<some product\>", relating a specific scenario where the product let them down, only to be lambasted by some really smart people on a Hacker News thread.
* Establish support available — a selection of an unfamiliar product is going to create a steep learning curve and support is important. Find out what support exists from the vendor, from specialised fee-based consultants, and from the community. Be careful about putting too much faith in vendor support on a per issue basis. Also, look for training resources such as presentation recordings.
* Find out the known issues — most technologies have well-known issues. This is less of a problem with well-established platforms, such as SQL databases, but does exist with other products. Some of the issues may be by design, but still require compromises or additional effort to work around (such as the lack of indexes on Windows Azure Table Storage).
* Ask friends and colleagues — many developers work on side projects, or have recently joined from projects that may have used one of the candidate technologies. Many of the technologies that are considered 'new' have been used on smaller projects for years.

#### Technical Assessment
Assessment cannot be performed without writing code, doing a deployment, and pushing around some data. Opinions gleaned from the Internet are not necessarily applicable to the application that needs to be developed. Again, the technical assessment need not be arduous, and a detailed assessment cannot be performed on every possible angle or edge case. Enough technical assessment needs to be performed in order to gain sufficient confidence that the specific technology will satisfy the requirement. These steps can be followed:

* Establish what is being assessed — Are you assessing the eventual consistency between nodes? Or the performance under heavy read load? Or the availability when an instance fails? Or the ability to handle unstructured data? Be specific as assessing the entire product will take too long.
* Technical spikes — Spin up an instance, create a database, write code and run it to perform the assessment. This gives valuable input into the development environment and operational processes too.
* Performance — Specifically test performance. Performance can vary greatly between your particular deployment environment and other reference sites. Performance is impacted by network latency, disk I/O, and other parts of the infrastructure, and the behaviour of the product on the production infrastructure absolutely has to be assessed.
* Data access API — New technologies require the mastery of a new data access API. Developer productivity has to be assessed. Some of the technologies have APIs that are very easy for developers to master (such as document databases), and others may be more complicated. How the application will behave at scale, with the new API needs to be understood. ORMs, for example, may seem easy to develop against, but become problematic when performance is crucial.
* Operations — Keep an eye on how easy the database will be for operators to use when doing the assessment. Assess how easy it is to perform backups, deployments, monitor health, and so on.

#### Approach to abstraction
The ability to swap out one data store for another once an application has been delivered is an unachievable holy grail. Applications architectures that intend to engineer database portability, will cost too much in development, or make many compromises where the strengths of individual products are not taken advantage of. But with new technologies there is significant, and valid, concern about the risk of choosing a technology that may turn out to be unsuitable.

When assessing a specific product, pay attention to how the data access can be abstracted. Look at how it implements standard data access patterns, and whether or not it takes specific advantage of features that need to be written in the application code. From this, formulate a practical approach as to how much data access abstraction to put into the application architecture. 

### Select a technology per database class
A typical cloud computing application will have more than one database. The semantics of the definition of 'database' can be argued one way or the other, so as a looser definition it could be better argued that there will be more than one data store where data is persisted. Part of the assessment of storage options is to understand that there are different classes of database (or data stores that persist data), and it is unlikely that one database will fit all purposes. In terms of rationalisation of the technology choices, there should generally only be one choice per class. If there are multiple technology choices in a class, there has to be a very good architectural reason.

The table below lists classes that will be common to most modern applications:

| Database Class 	| Examples 	|
| ---------------------	| ---------	|
| Cache 	| Windows Azure Caching, Memcached 	|
| Persistent messaging 	| Windows Azure Queues, Service Bus Queues 	|
| NoSQL (Non-ACID database) 	| Windows Azure Tables (key-value), MongoDB (document) 	|
| SQL database 	| Windows Azure SQL Database, mySQL 	|
| Blob (and files)	| Windows Azure Blob Storage Service, Windows Azure Drive	|
| Business Analytics 	| SQL Server Analysis Services, Hadoop 	|
| Feature specific	| SOLR (search) 	|

## Problematic Data
Some types of data present special challenges and need added attention in the data model. These become particularly problematic at scale due to the bandwidth, processing cost, or bottlenecks created by shared resources. Although not a definitive list, pay attention to the following:

* Blobs — Blobs (binary large objects) are used to store unstructured data in a binary format, such as images or video. Blobs can create storage issues, deletion problems (if a user wants all of their data removed), security issues (they should generally be browser accessible), and others. Due to their size, Blobs may also need to be distributed to CDN (Content Delivery Network) edge nodes to improve performance.
* Identity — Storing identity natively in an application should be avoided if possible. Apart from the hassles of resetting passwords, security needs to be based on solid identity to work properly, and if identity is broken, security is compromised. Use should be made of third-party identity providers where possible (as encapsulated in Windows Azure Active Directory).
* Network and graph structures — It is tempting to include functionality that relates many things for the benefit of the user experience. This has been encouraged by the more 'social' nature of applications, where activity of friends and other circles is expected to be visible and current. Databases that need to store and navigate a social graph (or any other graph, such as related products) in real-time are hard to get right at scale. Take care when promising functionality based on graph data.
* High volume, low value — A lot of data has a very low 'per row' value. For example, tracking the geospatial co-ordinate from a mobile device when a request was made may be useless as a single data point. Such data only becomes valuable when combined with other datasets, aggregated, analysed and, most importantly, actioned for some clear benefit. Such data can have an insignificant cost to record and store and can become really expensive to extract value from. By all means record as much data as possible, but be careful of the load this data places on the application, and about overstating its value in its native format.

## Big data
When 'cloud' and 'data' are mentioned together, it is quickly followed by 'big data'. Big data is currently a hot topic. Much of the interest is reasonable as the amount of data being generated by consumer-oriented applications, with millions of users, is both phenomenal and valuable. It is modern applications providing the functionality to users that enable data to be collected. Much of this data is behavioural or generated as a side effect of user interactions, rather than users explicitly capturing data. Those same applications are running on cloud platforms, or distributed commodity-hardware environments that underpin the ethos of big data; namely the use of distributed data and processing (in a map-reduce style) to get answers quickly and cheaply.
The dark side of big data is the hyping of the technology by vendors desperate to cash in on the 'next big thing'. Big data, almost by definition, requires a lot of storage, processing and network infrastructure. This is bound to attract vendors that need to retain (or grow) market share in a rapidly changing, on-premise datacentre environment. 

The truth of big data is somewhere between hype and reality. Applications do indeed generate a lot of data, but the generation of data, and the need to analyse it, doesn't make it a big data problem. Sometimes the big data tools, such as Hadoop, are the best tools to extract value from the data, and sometimes more simple data access is good enough, especially when the questions are both known and simple. Indeed the initial principles of big data, started as the '3 Vs' of big data (Douglas, Laney. Gartner ["3D Data Management: Controlling Data Volume, Velocity and Variety"](http://blogs.gartner.com/doug-laney/files/2012/01/ad949-3D-Data-Management-Controlling-Data-Volume-Velocity-and-Variety.pdf)),are recognisable as part of the data problem of modern applications. They are:

* Volume — the amount of data exceeds physical limits of vertical scalability.
* Velocity — the speed at which data moves in and out of the system, whereby the decision window is small compared to data change rate.
* Variety — the range of data types and sources.

These are echoed in the [model detail](#DataModelDetail) that needs to be generated for each schema, namely [Volume](#DataModelDetailVolume), [Velocity/Throughput](#DataModelDetailVelocity), [Format](#DataModelDetailFormat), and [Data source](#DataModelDetailDataSource).

Since these initial principles, another 'V' was added (variability, the variations in interpretations of the same data) to make '4 Vs'. The madness of adding Vs has not stopped and there is now validity, volatility, viscosity (resistance to flow), value, and possibly many more.

While big data should be on the table when looking at the data requirements of an application, it should be treated with caution. Big data solutions should be used appropriately and not promise to magically solve all data problems.

## Summary
Choosing approaches to data and specific underlying database technologies is one of the most important steps in developing an architecture for cloud-based applications. The breadth of the problem, the compromises that need to be made, the complexity of the underlying technologies, and the risks can be daunting to any implementation team. Glossing over the data model only delays (and increases) the risk, so early and focussed attention on it is imperative. To make sure that the data model doesn't become overwhelming, focus on the requirements and match them to approaches; inevitably a technical fit will emerge.

To simplify the effort:

* Focus on schemas that are derived from the workload model. Small chunks of data will be easier to understand.
* Make sure that the business requirements are clear, and make sure that there is an underlying reason for the requirement. Specific areas include the need for real-time consistency, availability, and the need for data flexibility in terms of multiple uses and queries.
* Make sure that the development team is actively involved — many of the solutions to data come from development practices and approaches, as much as it comes from DBAs.
* When choosing 'new' technologies, build a small technical spike, and deploy it on the cloud. This tests assumptions and gives developers a feel for what it takes to implement.
* Don't be tempted to use every tool and technology available. Stick with those familiar to the implementation team (such as SQL), and minimise the number of other technologies used.
* Don't wait until the data model is 'finished' before development, but use it to make quick decisions to get the development going. Continually add to the data model during development.

### Steps
1. Determine data schemas.
2. Model each schema.
3. Assess data storage options.


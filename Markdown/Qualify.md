# <a name="Qualify">Qualify</a>
Businesses embark on software development programmes to achieve their business goals, not (to the surprise of many developers) simply to play with the latest technology.

Any new technology or process can be considered if there are business **opportunities** that can be seized by adopting the technology or existing **challenges** that can be overcome. The new technology may offer something more cost effective, quicker to bring to market, lower risk or simply beyond the capabilities of existing technologies. Cloud computing is such a new technology. Embarking on a cloud computing project for little business benefit is not worth undertaking and should be qualified out. One possible exception is delivering a 'simple' application for the purposes of learning the technology, which lowers risk for the 'real' project. Even then, the most significant project to be undertaken at a later date will still need to be identified.

The process of qualification is cyclical and composed of two primary tasks. Firstly, candidate projects need to be selected based on business need. Secondly, these projects need to be assessed in terms of their cost, risk and benefit. This assessment can be fed back into the selection process to narrow down the candidates, which in turn can then be assessed further.

At the end of the qualification process, the primary output is the business case for the cloud computing project as well as documents that facilitate the design process.  

![Qualification](./images/Qualify-001.png)

## The Value Proposition 
Business needs to understand the value of running an application on Windows Azure, rather than the benefits of the technology. Most of the information provided to, and understood by, technical people is about the platform and technology that make little sense to the business.

The difficulty is creating an environment where existing mental models which may be based on traditional IT are cast aside. This enables business to see opportunities that are candidates for cloud projects. Perhaps the business is used to the long lead times of their existing IT and would not even consider an opportunity that needs to be brought to market quickly? Perhaps they are used to large, up-front IT costs and therefore cannot adopt fail-fast business tactics?

Ideas to get the discussion going include:

* Rapid provisioning — this may get the business thinking about bringing new products to market faster than they thought possible
* Ability to handle and afford brief high loads — this may get the business thinking about social campaigns, pop-up virtual shops of similar activities that run for short periods.
* Fail-fast tactics — if up-front infrastructure investment is minimal, perhaps business will be prepared to take a chance, and if the opportunity doesn't pan out, be able to shut the application down.
* Applications that need to pivot. Because the infrastructure is provisioned as needed, the application may be able to adapt to change based on customer input. 

### Prioritisation and focus 
Once opportunities have been identified, they need to be whittled down to a selection of candidate projects that require a more detailed assessment. The most likely candidate projects can be prioritised for assessment, where they can be focussed on individually.

### Mutual Benefit 
The application developers, whether in the form of an external organisation or an external provider, also need to see the value in undertaking the project. This is often overlooked in the enthusiasm for solving the business need and can result in failure if not done correctly. Apart from the obvious benefit of margin, the candidate projects also have to be possible from a resource availability perspective, allow for reasonable development time and do not promise what cannot be delivered.

## Assessment 
Assessment is a primary task driven by the technical team, as it is the technical team that can get a handle on the costs and risks. These costs can include development effort, processes for operations, migration and platform costs. The technical team also needs to provide input to assess the risks of the development. This can include technical complexity, integration risks, timescales and available skills. Business may also contribute by adding their business risks and costs to the assessment.

> Note: The assessment has to be done by people that are familiar with cloud computing and have at least some experience in building applications on Windows Azure. If such experience does not exist internally, it is strongly suggested that an external supplier is approached for consultation.

The costs, risks, and benefits need to be discussed and described. They can also be weighted, summed and plotted in order to create a view on viable projects. Factors to consider are listed below.

### Business Value Assessment 
This assessment attempts to identify those projects that are of highest value to the business. An application that increases revenue by 20%, for example, would be of higher value than one that makes sure that stationary drawers are fully stocked. Specifically look at factors that make particular projects better suited to running on the cloud. 

#### Increase 
* Support of business agility — being able to bring products or campaigns to market quickly. Most revenue increasing projects fall into this category, but it is not limited to revenue. Depending on the business, this agility could include improved customer service, improved logistics processes, debtor management, and so on.
* Spiky usage patterns — high, unpredictable growth, regular traffic spikes and planned campaigns.
* Operational efficiency — reduction in the number (or cost) of full-time IT staff to feed and water infrastructure.
* Fail-fast business tactics — ability to try out an idea without significant sunk infrastructure costs.

#### Decrease 
* High complexity — an overly complex solution to the business need may decrease the value to the business. This decrease in value may manifest itself as increased time to market over an on-premise solution due to the higher risk of complex solutions.
* Existing legacy application — an application that is currently running and cannot take advantage of the benefits of the cloud may waste resources if moved to Windows Azure.

### Cost Factors 
The influence of the cloud on cost, over on-premise solutions, is well-covered ground, so it should be fairly easy to run through the cost factors. Despite marketing to the contrary, not everything on the cloud is cheaper than a traditional on-premise application, so make sure that you include any factors that increase the cost.

#### Increase 
* New developer skills required — developers need to learn a new API and new approaches to processing data.
* New deployment and operational processes — existing processes used for deployment and operations need to be adapted for Windows Azure.

#### Decrease 
* No upfront purchase of infrastructure.
* No need for overestimating of required capacity.

### Risk Factors
New cloud computing projects that are started within organisations that are new to developing for the cloud will have higher risks than their traditional on-premise alternatives. Since the objective of the assessment is to choose one cloud project over another, look for particular warning signs (such as compliance) that would make building the application for the cloud significantly risky.

#### Increase 
* Compliance and security — it is possible that there is a regulation that makes it difficult to store data in, or process on, Windows Azure.
* Low tolerance for latency — some applications need low latency, which is difficult to achieve on Windows Azure. 
* Legacy Applications — trying to run applications on Windows Azure that are not designed for it is possible but can cause problems.
* Legacy Integration — some integration points may require private protocols that cannot be easily accessed from a publicly hosted client.
* Fear, uncertainty and doubt — there may be influencers in the organisation that are anti-cloud and will voice concerns over data storage, security, compliance and other problems highlighted to them by incumbent IT suppliers and operators.

#### Decrease 
* Early testing — architectural assumptions can be tested very early on in the development cycle because a 'production' platform is always available.
* Non-fixed infrastructure — as the application matures and changes, the underlying Windows Azure platform configuration can accommodate the changes.
* IaaS — an architectural bias towards an infrastructure-based (IaaS) solution as opposed to a platform-based (PaaS) solution can decrease the short-term risks. Although it is not recommended, in some cases it may be practical if re-engineering for PaaS is considered too risky.

### Assessment Output 
After assessing the costs, risks, and business value across the candidate projects it should be possible to identify the projects that have the highest value for the lowest possible risk and cost. This knowledge can be fed back to business to qualify the final project and write the business case.

The assessment also provides some early warning for the implementation team regarding possible problems as well as highlighting the expected benefits to the business. This will influence the design, reducing risk during development and ensuring that the objectives are met.

![Graphed qualification results](./images/Qualify-002.png)

The assessment also provides some early warning for the implementation team on problems that may be encountered as well as highlighting the expected benefits to the business. This will influence the design, reducing risk during development and ensuring that the objectives are met.

### Assessing the suitability of the application for Windows Azure
The assessment process of formally weighing up value, costs and risks of various projects should provide a clear indication of the best candidate projects. A double-check of this process is to look at some of the things to seek or avoid when choosing whether or not an application should run on Windows Azure. These are described below for reference but because each application is different, and may not fall directly into one category or another, the diligent weighing up of value, costs and risks should still be applied.

#### <a name="ThingsToAvoid">Things to Avoid</a>
Many Windows Azure applications are undertaken where a target project is intended as a 'Proof of Concept' and the effectiveness of the application will be assessed before others are undertaken (the assessment may be conducted formally, informally, by business or IT). In this scenario, there are some applications that should be avoided if at all possible, otherwise the project may fail and remaining Windows Azure endeavours will be scuppered.

##### Too much legacy
The Windows Azure style means that applications are assembled somewhat differently. That means that existing application architectures follow a style that either clashes completely with the Windows Azure style (such as assuming fault tolerant architecture) or at least make it difficult to rework (such as an over-reliance on a high throughput RDBMS). Even though the Windows Azure style is relatively new, it would be ridiculous to expect that there won't be any existing approaches, code, tools and technologies brought into a Windows Azure application. At the very least we need to bring existing skills to bear, such as .NET development capability. Windows Azure provides compelling support of established and credible technologies, such as Windows Azure SQL Database (as a variation on the well respected SQL Server). However, there are some legacy indicators to look for that may rule out an application for Windows Azure.

###### Dependence on Premium Products
The Windows Azure style is to avoid commercial, high-end technologies as much as possible because many commercial products have a high dependency on the underlying infrastructure. A customised ERP system may be complex to run on Windows Azure because it has not been developed specifically for the PaaS model of Windows Azure and because it has a high dependency on high-throughput SQL Server, complex manual configuration and load on individual servers. Additionally, premium products have licensing options that are immature for running on Windows Azure, and besides, running all the various pieces of legacy applications only in virtual machines, doesn't make much architectural sense. Applications that have a high dependency on these products should be passed over until their vendors mature the products and make them more Windows Azure friendly.

###### Data on ACID
If, after an extensive analysis of database requirements, you discover that the bulk of the application has a high write:read ratio and that most operations require consistent transactional operations, then you should avoid building the application on Windows Azure. Although possible, there are applications, such as high read web applications, that will realise much better benefits than an ACID oriented application. The question as to whether ACID is required is explored in detail in the [data model](#DataModel).

###### Single Instance Components
Because we need to design for failure, we need to put redundancy into our application layer. Some components that are commonly used are single instance, non-distributed components. This can become a problem if the functions that they perform are long running. Consider, for example, an ETL (Extract Transform Load) operation executed by SSIS (SQL Server Integration Services). The SSIS task may need to be scheduled, and run on a single machine for a long time (importing thousands of rows). If SSIS falls over, or the instance it is running on falls over, it may be difficult to detect, rollback and restart. This may not just be a limitation of SSIS but also the design of the ETL operation, and illustrates a single instance operation that should be avoided.

###### Organic Installations
Many third party applications, particularly those from small vendors, have a long, custom installation process. This may involve installing software, making configuration changes in many files and applying a number of patches before the application can be used. The Windows Azure style leans on components that have fairly simple installation and configuration that can be scripted. In Windows Azure, backups should be used for data and not configuration. Applications or entire instances, and the dynamic naming and IP addresses of instances, means that applications with complex configurations done by hand should be avoided. If the application cannot be baked into a package and easily deployed using the Windows Azure SDK and tools, then it should also be avoided.

Persisted virtual machines, virtual networks and other IaaS features of Windows Azure do allow some organic installations. It should be seen as an exception for components where there is little choice, rather than an architectural principle. An application that is mostly IaaS is not a primary candidate for running on Windows Azure.

##### Complex Regulatory Compliance
Windows has certifications and compliance for 	ISO 27001, SSAE 16/ISAE 3402, EU Model Clauses, and HIPAA BAA, but crucially, no support at the time of writing for PCI-DSS. This compliance should take care of many of the general requirements that applications may require and in some cases (PCI DSS) requires integration with a compliant third party payment provider or a certified on premise system. However, regulations vary across industries and countries and are the domain of regulatory specialists to untangle them and advise what needs to be done in order to comply. Because public cloud computing is still in its infancy, many regulations seem to work against cloud computing principles. How, for example, is data expected to be stored in a single country if it makes sense to distribute it around the world to take care of availability or latency? 

Indeed many regulations are expensive to implement and favour large multinationals that have extensive in-house infrastructures and hordes of legal advisors and auditors on hand. This doesn't fit with the Windows Azure model of enabling SMEs, who may not be aware of regulations that affect their business or their customers.

This will undoubtedly change in time. Most of the concerns about regulatory compliance come from a lack of concrete legally backed (and tested) knowledge and are often based on myths of misunderstood regulations. For example, the UK Data Protection Act is widely understood to prevent the storage of data outside of the UK. Closer inspection of the act reveals that it is just 'personally identifiable' information that needs to be dealt with and it may be overcome by simply encrypting the postal code. 

While attention should be given to necessary regulations any application that needs to operate in a highly regulated industry or region should be avoided. If your application must comply with regulations, get started early and involve the right stakeholders (legal, audit etc.). It is far better to encounter regulations, so that they can be considered within the implementation, than to fail just before go-live when legal has to give the application the once-over.

##### Low Latency Tolerance
Certain types of applications have requirements that are closely tied to extremely low and controllable latency. The obvious example is in financial trading, but others exist in manufacturing (monitoring equipment) and other areas. Because the underlying commodity infrastructure is beyond our control, there is no way to tweak the infrastructure to gain a few milliseconds by adding extra fast disk, networking or other infrastructure. Applications that have a low tolerance for latency should be avoided, as the architectural solutions for achieving low latency in Windows Azure are labour intensive. Bear in mind, those applications that need to be performant may have a tolerance of high latency; the low latency requirement is very specific and is unrelated to general performance and responsiveness.

##### Big Bang Implementations
Any organisation's first Windows Azure implementation is likely to face a few difficulties, take longer and cost more than was initially expected (or hoped).  The platform is different, the tools are different, new skills are required, and the approach to development may be quite different too. If things are going to be late or cost more, it is better if they are small things. The fallout from a small project with a developer or two that overruns by a few days is going to be easier to deal with than a large project with a large team that overruns by months. So 'big bang' architectural implementations that bet the entire business or entire departments (however the politics in a particular enterprise may work) on a wildly successful implementation should be avoided. The first implementation should be one of the following:

###### Small tactical project
Chose a fairly simple project; perhaps a microsite to test social media, such as an application that integrates with Facebook. The idea is to take something small, low cost and tactical that can probably be thrown away once the campaign has run its course. This way any error margins in estimation have a low time or cost impact and usable business functionality can be developed and deemed a success. The throwaway nature means that the next project can then be reworked based on the inevitable lessons, and the long-term commitment is low. Internal IT will kick up little fuss at a 'crapplication' and business will not be frozen by risk aversion.

###### Extension to an existing solution
Assuming existing systems exist, there is always a need for added functionality to support the business. This is not necessarily an embedded piece of functionality that is part of the core processes, but is often related to the transfer of data used for other purposes. It could be the frequent (daily/weekly/monthly) importing of data that is used for further analysis, reporting or some other operational function. Assuming that the data in question is not subject to complex regulations (or internal control), the application can satisfy a long standing business need that IT has never been able to get around to. Again, the ability to have something low risk, low cost and successful, where lessons can be learned and taken forward, makes a fairly simple application a good candidate for a first project. This type of application also opens the door on hybrid public/private cloud applications, which is both architecturally relevant and an important discussion subject within any enterprise.

###### Greenfields iterative development
If no suitable tactical projects can be found, and the decision is made to start from scratch, it is imperative that valuable interim functionality is delivered incrementally and as soon as possible. This is part of the agile approach, and the reasons for this are discussed at length in the agile community. The important aspect is not the agile processes part (e.g. scrum), but the iterative deliverable of working software. For a Windows Azure project, you have to be deploying to production and serving up production traffic (even if it is pre-launch) in order to address the issues of availability, scalability, monitoring and so on. This can become a problem when re-platforming or replacing an existing system, where the application cannot be swapped out until all features are complete. In such a case, it would be better to develop new functionality first as extensions to the existing solution, before starting the big rewrite.

#### Things to Look For
When selecting a candidate project for Windows Azure you need to select one that plays to its strengths. When you have finished you want people to say, "Wow, we could never have pulled that off using our normal stuff!" You don't want a "Meh…" response where the application is not that much cheaper, easier or deployed faster than any other application that comes out of existing IT practices. While most of the success of a project is down to the team, technologies and architecture, choosing a business need that ticks all the right boxes for a Windows Azure solution is key to creating a successful base from which to launch other AWS initiatives. Look for the following attributes for good candidates.

##### A need for agility
While cost (or operational expenditure rather than capital expenditure) is often taken for granted as the biggest advantage of Windows Azure, when the numbers are run this is not so obvious. By far the biggest advantage of Windows Azure over traditional IT, is the ability of IT infrastructure to respond immediately to the agile needs of the business. Not the needs of IT or developers wanting to play with a shiny new toy, but the needs of the business. If the business that you are serving is lethargic, happy with revenue streams and set in its ways, whether because it is an old dinosaur or constrained by industry, then no amount of agility from developers is going to be taken seriously.

##### Spiky usage patterns
One of the biggest advantages and equally one of the biggest technical challenges with Windows Azure is the ability to scale up and scale down capacity depending on demand. The advantage is in the rapid provisioning and decommissioning of capacity and its associated financial benefits. Obviously an application that has flat usage and a finite number of users (which translates into predictable traffic) cannot take advantage of the benefits of scalability, because there is no need.

>*The Scale of Spikes* — one man's spike is another's little blip and it is important that all stakeholders including developers, operations, business and finance, share the same metric when describing spikes. A web application may not break a sweat with a few thousand extra requests an hour, but business may think that it is enormous. After all, a small increase in web traffic may translate into huge sales that may be difficult to fulfil. 

###### Wishful Thinking
Everybody wants to build the next Facebook or Twitter, and the likelihood of this occurring is close to zero. So building a scalable application just because someone hopes that it will become the next big thing, sets everyone up for disappointment. An application that has a hundred thousand users is still significant, but not if the plan had one million in the first month. The same goes for a campaign launched in the hope that it will go viral. These days viral traffic is seldom accidental but the result of clever marketing. So check that the traffic spikes expected are reasonable and match them with the business plan before assuming that the application usage will be spiky in nature.

###### High, Unpredictable Growth
The business plan may expect high growth, but cannot plan accurately for the growth. The growth may be related to marketing campaigns, product development, operational readiness, manufacturing capability, competitor response or any number of business related issues that might affect, either positively or negatively, the growth in regular traffic. The key with unpredictable growth is that there is a clearly defined market of a large enough size that can be tapped with the right marketing spend, partnerships or other reasonable investment as per the business plan. Windows Azure offers an advantage in this scenario as there is no need to plan for the provisioning of infrastructure at all. Assuming that there is a direct correlation between traffic and revenue (or market penetration, if that is the business objective), as the traffic increases more Windows Azure resources can be brought online in order to serve the load. 

Not all high growth is unpredictable. In some cases, such as the migration of a fairly static set of customers from one system to another, the growth can be predicted. In these instances, the ability to plan, source, fund, and provision infrastructure within a traditional datacentre may work out best. So look for an application that has high growth that cannot be predicted and your Windows Azure application will really shine as it easily responds to the demands.

###### Regular Traffic Spikes
When building an application on Windows Azure, you have to build it so that services can be started and stopped without affecting the overall behaviour. The side effect of this is that the ability to increase, and importantly decrease, available resources is built into the application. This means that an application with regular spikes in traffic is perfectly suited to Windows Azure because it is easy to respond to the extra, or reduced, load. The frequency of spikes needs to be carefully understood. Are the spikes during the day, week, month or year? This will determine your approach to handling them. Pre-paid, six month plans can be bought for baseline traffic, and pay-as-you-go can handle short-term unpredictable load. Existing roles may even be able to cope with intra-day spikes if there is enough capacity, or the service is degraded. There are so many options open on the Windows platform, that the optimal and low hassle handling of regular spikes shows off the best of a Windows Azure application.

###### Planned Campaigns
While we often think about unexpected demand, in most cases it is part of a particular business campaign. Perhaps it is an end of season sale, the announcement of a new product or a new advertising campaign. Traditionally applications and their infrastructure would be sized according to the expected demand during planned campaigns, such as having enough capacity for the holiday season sales. In most cases business wouldn't even bother to tell IT that a campaign was planned or underway. 

With Windows Azure, the applications can be engineered for the demand of planned campaigns and only provisioned once the campaign is underway. While it may be possible to autoscale the application, it would be preferable if IT had a close relationship with the business in order to be prepared for additional demand. This could include running a few tests, just to make sure that the system can handle the demand, or spinning up additional roles manually, pre-populating caches, and so on, to ensure that the application is immediately responsive.

##### Operational Efficiency
While we mostly concern ourselves with scalability of infrastructure and applications, it is the operational processes that support those applications that need to be scaled. Consider for a moment providing scalability in a non-virtualised environment, where an increase in traffic requires the purchasing and provisioning of new physical hardware. You can picture the stress and panic within an operational team to power, network, plug in and configure the new servers; if they have enough time to do it at all.

Windows Azure allows operators to easily (and even automatically) provision and maintain required infrastructure. Not just roles but also queues, databases and other infrastructure. This tends to suit applications that require a high degree of operational efficiency. Striving for operational efficiency may sound like something that everyone would want to do, but Windows Azure encourages the practise by reworking the approach and architecture. Most enterprise environments endeavour to gain efficiency within existing processes that, by comparison to Windows Azure, exist within a broader inefficient environment.

Much of the inefficiencies in enterprises are arguably valid and necessary. For example, operating a highly specialised Oracle platform supporting core ERP systems may be inefficient but necessary, because of vendor support of the platform, existing investment in infrastructure, regulation and risk. In this case, speciality of the underlying physical assets may make sense and should not necessarily be argued.

On the other hand, some business cases exist where running an application on the incumbent infrastructure does not stack up, and the costs blow the business case out of the water. For example, the marketing department wanting to run a promotional campaign for a specific product such as a Facebook campaign, may not be able to absorb the cost of three additional support staff working in shifts, DBAs on call or even the installation and configuration of a server or two. After all, any money spent operating the application could be utilised to attract people to the application in the first place.

There are many plans for applications where the cost of operating infrastructure has not been considered or simply has to be lower. These applications should be sought out and built on Windows Azure. Building an application that needs to be operationally efficient, and building it properly, is a great way to take advantage of what Windows Azure oriented applications will offer.

##### Fail-fast Business Tactics
Start-up businesses are primarily looking for plans that they can get up and running quickly, with little investment and, if it works, continue evolving. If it fails, it can be terminated quickly without too much investment. These plans are based on the principal that something is worth trying if the costs and other risks are low. In established businesses, similar applications could be the offering of a new product on top of an existing platform. For example, creating an application to sell a new short-term insurance product on top of the existing systems by packaging it in a unique way. While fail-fast tactics support business models that require more than just IT, they generally have a large IT component. Windows Azure oriented applications suit these business plans; not only because the applications can be rapidly provisioned with minimal hardware and licensing investment, but also because the applications can be architected from the beginning with scalability in mind. So if the application becomes successful, it can continue operating without having to pay for a rework in order to make it more mainstream.

##### Applications that need to Pivot
Within entrepreneur and start-up circles, the term 'pivot' refers to changing the product that was initially planned to something else because customer take-up was too slow or customers asked for radically difference features. Translated into application requirements, rather than business strategy, this means that the application requirements may need to change quite radically and quickly.

The ability of an application to support this pivot in the evolving business plan requires certain architectural elements that match the practices encouraged within Windows Azure oriented architectures, such as loosely coupled services. Some components in a well-architected application, such as user login and registration, can be reused in the new application without a significant rework. 

Beyond the software architecture, the infrastructure in a Windows Azure oriented application can change as the application changes.

While not necessarily that common within an enterprise environment, the concept of pivoting is echoed in applications that are highly responsive to changing user demands. This is not an excuse for lazy business analysis or failure to manage scope creep, but a genuine evolution of requirements as the application starts being used.

#### Considering Hybrid Applications
To software engineers, it may make sense to run the entire application on Windows Azure, and they seldom consider that applications can span Windows Azure and a traditional datacentre. Many of the options listed in '[Things to Avoid](#ThingsToAvoid)' may be perfect candidates to run part of the application on-premise.

It would generally be architecturally irresponsible to create an application where a single synchronous process has to span completely different systems; but in some cases, it is unavoidable. Perhaps an existing authentication mechanism exists that can and should be used? In which case it should be wrapped up in a service and called from the Windows Azure application. Maybe there is a requirement for integration with an existing order viewing and fulfilment system that supports processes far beyond the application front-end?

This opens a can of worms (and associated pain) of application integration which can only be avoided in a perfect or isolated world. The integration with existing (or even new) applications in and around the enterprise has to be considered when building applications for Windows Azure. Integration and integration patterns are discussed in further detail in the [integration model](#IntegrationModel), but in the context of this discussion about what to look for and what to avoid, the following should be considered:

* Avoid synchronous integration where possible — you can build a highly scalable and available system, but if a web request is dependent on, and has to call, another service before rendering a result, it needs to be up to the same standard. For example, if you need to pass orders to another system for fulfilment, it may be preferable to add them to a queue and send them in a separate process, just in case you get a traffic spike that you can handle, but the recipient system cannot.
* Keep contracts simple and concise — when sending data to other systems, try and keep the interface down to a clear method call (REST or RPC style) with a simple document (such as JSON) and only the bare minimum of fields. Interfaces with large schema compliant XML documents are unwieldy and prone to errors and breaking schema changes.
* Keep as much data to yourself as possible — the ability to store data in the optimal data store (say a NoSQL database), and on the application cache are fundamental scalability techniques and this needs to be in your application's control. Having to re-query an external (service based) data store every time you need to fetch a crucial bit of data, will create a scalability bottleneck that may never be overcome.

When building a large and complex system on Windows Azure it is better to focus on what the platform is good at; public facing scalable web applications (in terms of our reference architecture). Some aspects of the application may be suitable for your Windows Azure application, but you can leave them out when it isn't a requirement. You could plan to re-architect them in the future, but for now, it could be easier to use existing infrastructure, licenses and people to do the work external to Windows Azure on the incumbent platform and architecture. If your application has a requirement for data to be sourced from the central corporate database, you might as well use the existing ETL tools and services running in the on-premise datacentre to run the data into your database; rather than trying to do it in Windows Azure. Otherwise you may not have licences for the tools, the services may not run well in a designed-for-failure environment (like SQL Server Integration Services), and you will end up re-architecting years of enterprise duct tape for something that could be easily done by existing enterprise resources for a fraction of the cost and hassle.

So while it may be tempting to try and run everything in Windows Azure, it is not always possible or practical. The reasons can be valid technical ones or the result of people unwilling to commit to Windows Azure. When architecting for Windows Azure it is important to consider the broader needs of the organisation and, in that consideration, realise that on premise or co-located infrastructure has its place. The resultant architecture would therefore not be a Windows Azure only solution, but one that is a hybrid of Windows Azure and traditional IT environments.

## Business Case 
In assessing the costs and risks, the technical team provide valuable input and estimates for the project. Business can then select the best project to undertake and using the information gleaned during the assessment (representing technical input) can go about developing the business case.

## Summary
Cloud computing platforms are suited to many different types of applications. The addition of IaaS features to Windows Azure also means that you can run just about any application on Windows Azure. However, some types of applications really shine on the cloud, and others are better suited to a traditional on-premise datacentre.

The qualify process is about picking out the best candidate applications for cloud computing. This is done by assessing the risk, cost and business value across all candidate projects — resulting in the right project filtering to the top. The qualify CALM model helps make sure that the right application is chosen at the beginning of the project. The wrong choice can be a total disaster and scupper the adoption of cloud computing, and the right choice means that the application should deliver to, or exceed, expectations.

### Steps

1. Create a list of candidate projects.
2. Perform a business value assessment of each project.
3. Assess the costs of each project.
4. Assess the risks of each project.
5. Narrow down the candidate projects to viable projects.
6. Develop the business case for the viable projects.


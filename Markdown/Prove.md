# <a name="Prove">Prove</a>
Despite the best efforts in qualification, the most informed technical considerations and the best non-cloud development skills, cloud applications are still risky. The most important part of embarking on the development of an application for a cloud platform is to reduce this risk level as early as possible to what would be considered normal for the development team. This is achieved by making a conscious effort to prove the architecture, approach, and specific technologies.

Most of the proving is done by developing a Proof of Concept (POC) in which code is written, deployed, and run on the Windows Azure platform. Other ways to prove the solution include looking at case studies, getting vendor support, and developing technical spikes, but by far the most important is to run code on Windows Azure. Not only is it relatively easy to run applications on Windows Azure, it is also the only way to get a realistic view of the suitability of the application for Windows Azure, the architectural approach, and the capability of the implementation team to deliver.

The ability to run the POC in a production environment is what makes applications built for the cloud so compelling. On-premise applications cannot be tested early on for suitability to the production platform, because at the stage that the POC is performed no production platform exists, and it would be too expensive to set one up just for the POC. Even the use of lab environments (often hosted at third parties) will not prove the entire stack. With Windows Azure, a POC can be deployed to production, in a production configuration (perhaps with hundreds of instances), and extensively tested as part of the POC exercise. This can last for a short period, possibly a few hours, and simply shut down. This means that the cost of building a POC is not wasted on procuring, setting up or running infrastructure. Budget is rather spent on the most important part, namely the writing and execution of the code.

In addition to the increased need for a POC in an application developed for the cloud, the ease at which it can be done, relative to a traditional application, should ensure that the case for a POC is justified and accepted.

## Why develop a proof of concept?
The enthusiasm to build applications for the cloud does not automatically mean that a particular application is well-suited to run on the cloud or on Windows Azure in particular. Perhaps it won't work well within the regulatory environment of the business? Perhaps it has a dependency on a database configuration that cannot be run on virtual machines? These reasons, although they should have been used to qualify the application initially, still need to be double checked. The end of a POC is the best place to finally review the suitability of the application for the cloud.

Even if the application is a good cloud candidate, certain architectural and technical assumptions have been made, and these need to be tested. For example, Windows Azure SQL Database may be chosen as the data store, but nobody really knows how it will perform. A POC either eliminates that uncertainty or uncovers performance issues that need to be addressed, possibly by testing within the POC by the selected alternative data store.

### De-risking
The primary role of the POC is to de-risk the project as early as possible. This de-risking is achieved by:

* Ensuring that the assumptions made in qualification are correct.
* Ensuring that the application is indeed a good candidate for cloud computing.
* Confirming that the cost implications are not significantly different from what was expected.
* Confirming that major technical decisions are correct in terms of the requirements (performance, availability, cost, etc.).
* Uncovering areas that require special attention and that need to be architected or planned for.
* Confirming that development effort estimates are reasonably accurate.
* Providing insight into areas that have been overlooked. These areas can be technical in nature, or related to the organisational environment.

A good project manager, together with the technical lead or architect, will start the project with areas of concern. The architect and technical lead will have technical concerns, such as the performance of the database, whether or not instances failover as expected, or how simple it is to debug a service. Project managers will have concerns relating to cost, development effort, and making sure that there are no hidden surprises. Each of these concerns can be red-flagged as risks at the beginning of the project, and the POC should clear those flags, or at least provide further information to be used to manage the risks.

### Improved confidence
On projects where there is little or no experience with developing applications for the cloud, there will be areas where members of the team are unsure. This lack of confidence may not indicate risks per se, but at least an air of doubt and vagueness. Individual operators, for example, may have read up on how to deploy a worker role, and how to swap it into production from staging, but have no experience to be sure that it works as described. Maybe there is a missing step, or some particular knowledge that the operator doesn't have, that could take days to resolve.

This lack of confidence can be damaging to the project. There is a lack of morale, reluctant acceptance of the project vision, and no confidence in effort estimates provided. The POC allows all members of the implementation team to get their 'hands dirty' with the environment and allay any personal doubts that they may have.

The POC increases confidence in:

* Qualification — when the application, even in the crude form of a POC, is seen running, the confidence in the qualification of the chosen candidate application increases. This includes some degree of the confirmation of the risk and cost issues.
* Platform — despite seeing presentations, or even running simple applications in Windows Azure, nothing beats the running of a functionally representative POC in a production Windows Azure environment to confirm that it does work as promised. A deployed POC can be started, stopped, scaled up, monitored, and shut down as if it were deployed in a production environment. The confidence that the platform does work can be fairly simple to achieve, but has a significant impact on the project. 
* Design and Architecture — the most important reason for developing a POC is to ensure that the application design and architecture is correct. The first parts of workload decomposition (see [workload model](#WorkloadModel)) will be seen in action, together with the beginnings of loosely coupled asynchronous services. Tactics developed in the [availability model](#AvailabilityModel) will be tested, and the chaos monkey can be set free in order to test the availability approach (by arbitrarily shutting down instances). The primary data store should be selected by the time the POC starts, as well as some secondary ones (such as blobs and cache), allowing for the decisions made within the [data model](#DataModel) to be confirmed. For most members of the implementation team (including decision makers), the architectural approach will be unfamiliar, and proof of the architecture is important to get support and buy-in from all members of the team. Teams that are staffed by smart developers will have to explain and motivate their approach, and code running in production is the best way to do it. 
* Delivery — Senior members of the team, particularly non-technical members, such as the project manager and project sponsor, need confidence that the application and the team can deliver. The application running on Windows Azure should prove that the platform is up to the task and that the operators know how to deploy and run it. The functions that have been developed within the POC should prove that the developers can deliver.
* Operations — Operational staff are often the most wary of the cloud. They see all sorts of problems that may not exist, including the threat to their jobs. The POC should increase the confidence of operators, about how the platform works, the fact that a cloud platform is not a toy, and confidence in their own job security.

### Input into models
Although it is expected that by the time the POC commences, significant work is done with most of the CALM models, at such an early stage of the project the models are far from complete. Some models should be mostly complete (such as the [lifecycle model](#LifecycleModel), others will be finalised much later in the development cycle (such as the [cost model](#CostModel)). The successful running of a POC will provide valuable input to models that need further development. Examples of this input are listed below:

* Cost Model — An application running on Windows Azure consumes billable resources, even if it is just a POC. The amount of resources that are consumed, and their cost implications, can be analysed in detail and fed into the evolving cost model.
* Health Model — The POC will often be the first time that realistic monitoring data is seen in the logs and the portal. A running POC will also not run smoothly and will need to be debugged. How it is determined to be running in a state of diminished health (or not) is useful to the health model.
* Capacity Model — The POC can be tested with various configurations and instance sizes; this is useful information to feed into the capacity model.
* Operational Model — As most operators will be unfamiliar to Windows Azure, the running of the POC in a production Windows Azure environment will give them a good idea of how to operate the application. This is only useful if operators themselves do the deployments and look for problems. Often, during POCs, the developers do it themselves, which may be quicker, but is less useful in terms of contribution to the operational model.
* Data Model — While most of the data model will be complete before a POC runs, vital information such as volume (size of data), throughput and performance may be useful to refine it.
* Test Model — The POC is a good opportunity for testers to try out their test model and test plan on a deployed application. The feedback on the process can be integrated back into the test model.
* Availability Model — Like the data model, much of the availability model will already be complete by the time the POC runs. Since the POC will include simulated failures, this may require an updating of the availability model. At the very least, it will provide useful metrics, such as failover time for databases.

### Reach
For enterprises that are used to developing and releasing applications into their own on-premise datacentre, releasing a cloud-based application can uncover previously unnoticed organisational and internal IT behaviour. Deploying a POC, particularly when it has some integration with existing applications, helps towards understanding some of the broader business and IT responsibilities of the organisation. For example:

* The POC may require a hybrid active directory federation, which requires involvement by internal IT.
* Internal IT networking rules and procedures may need to be challenged or adapted in order to, for example, punch holes in the firewall for access to Windows Azure SQL Database, or to set up a VPN. Networking and security people are, quite rightly, unwilling to make arbitrary changes, and need to be properly convinced of the necessity.
* Internal audit and compliance processes may need to be involved before an application (that collects some or other important data) is deployed on infrastructure owned by a different organisation.
* Other architects within the enterprise, or architecture steering committees, may want to validate and approve the design.

### Side effects
The focus of the POC should be to deliver as much as possible within the allocated time and budget and to de-risk any architecturally significant requirements (see [What to prove?](#WhatToProve) below). There are some side effects in the process of developing a POC, which although should not be the primary reason for the POC, are nevertheless useful in the overall context of the project. These side effects include:

* Training — Since a POC is, almost by definition, work done on something that is unknown, by the end of the POC the members of the team will know more about the cloud and Windows Azure than when they started. A POC is about delivering a short-term project, so there is no time to take it slow and let people take their time learning the details. However, the project can be structured in such a way as to maximise the training benefit. This is why it is important to have testing done by testers, and not developers, and likewise with deployments done by operations.
* Estimating — After running through a full deployment to Windows Azure, from conception to deployed and running code, the entire implementation team will have a much better understanding of what it entails to develop an application for Windows Azure. This understanding should translate into better estimation, after all, most errors are due to the [cone of uncertainty](http://construx.com/Page.aspx?cid=1648), and the removal of that uncertainty improves the accuracy and quality of the estimates.
* Re-use of POC assets — A POC should generally contain code that is going to be discarded before the primary development starts, otherwise too much time is wasted on a level of quality and engineering that is, at least for objectives of the POC, unnecessary. It is prudent however, to try to make sure that some of the assets are re-usable. It should be possible to harvest some code, deployment scripts, configurations and other deliverables from the POC to give the primary project a kick-start.

## <a name="WhatToProve">What to prove</a>
A POC is not the first sprint of a long project. It needs to be a mini-project that has a scope of work, a set of deliverables and a plan. Rather than just diving in to code, spend some time making sure that the POC proves the correct things and that everybody is clear on what needs to be delivered.

### State the constraints and objectives
It is important to clearly state up-front what the objectives of the POC are. The overall objective will be to make sure that the qualification of the project is still valid and that the key risks identified are being reduced and further managed. The project may be assessing the suitability of Windows Azure, in which case the POC should include extensive use of Windows Azure features. The project may be assessing the ability of data stores to cope with the volume, in which case more attention will be paid to building data stores, data access, and subjecting the application to load tests.

The POC will also be subject to constraints, and these need to be clearly stated before commencing, as part of the project plan for the POC. Constraints may include, time, budget, people, access to internal resources (such as IT), and availability of third-party resources (such as consulting services).

### Prove all models
Fundamental to the CALM guidance is the need to do up-front design work on the application. It is not practical, nor expected, to have completed all of the models in detail (some models, such as the cost model, are only 'complete' towards the end of the project), but all models will have to be worked through, to some degree, before the POC starts. Starting the POC without spending a few days working through the CALM models will result in a POC that mostly proves the wrong thing. For example:

* The lifecycle model provides important data as to the load that the application will be subject to.
* The workload model will provide necessary insight into the individual services that will need to be built in the POC.
* The data model will provide candidate database technologies and data access approaches that need to be verified.
* The test model provides approaches to how the success or failure of the POC will be determined.
* The health model helps developers implement the necessary monitoring, which is needed to see how the POC is performing.
* The operational and deployment models will help operators determine if the operational environment is workable.

The CALM models contain the design, architecture, approach and processes, all of which need to be proven.

### Perceptible business value
Architects and application designers are tempted to focus the POC on challenging or interesting technical issues. The desire to try out new technology can also influence the choice of features to develop in the POC. The POC is funded by the business, for business reasons, and as the customer, they should have something that they can showcase and understand the value of.

While there is significant value in de-risking the project, particularly as it impacts downstream costs and delivery, a checklist of managed technical risks may not be enough to present to a non-technical audience in order to push the project to the next phase. Consider the situation where business has to present the successful project to senior management for their approval in order to commit resources to the rest of the project. In such a situation, it is useful to show a working application, with pages to render and buttons to press, rather than just the output from a performance log file.

The POC, while it is mainly about tackling technical risks, needs to address the needs of the business by delivering something that business can relate to. A successful demonstration, a test of real functionality by a focus group, or an example of integration with existing applications, is crucial to taking the technical successes of the POC and using them to push the project forward. Some time may have to be spent on the user interface development of a major feature, with some UX and graphic design work too.

### Avoid low-risk requirements
Although developing something that is business-centric is important, it needs to be balanced against the risk of spending too much effort building features that don't fulfil the need to de-risk technical aspects. If the objectives of the POC are to prove cloud computing, it wouldn't make sense to spend too much time developing the web client; all of the effort spent on the UI may be wasted, as it is not considered a risk that needs to be managed. Similarly, time should not be wasted on implementing the full breadth of data access and trivial features such as sending emails.

### Determine the architecturally significant features
Regardless of the amount of time or budget allocated to the POC, it is only ever going to be able to deliver a subset, and probably a very small subset, of the overall application functionality. The choice of the functionality to prove is crucial. Choosing the wrong functionality will mean that the POC is unable to de-risk enough of the issues, reducing the value of the effort. Technical people will want to take a technical component, such as a single service, and to make it functionally complete, with all of the software craftsmanship and quality that we generally expect from them. The choice of functionality may be architecturally significant as it is seen as the component that has the most complexity, or the most issues. They may optimise the performance, make it unit testable, refactor, and make sure that it contains minimal technical debt. This understandable focus on proper engineering and quality could result in a POC that does one thing very well, but is not broad enough to deal with multiple issues, and it may not look like much to a non-technical audience. For this reason, it is important to focus on architecturally significant _features_, rather than components or services.

In order to achieve this we must map high-level features to architectural significance. If this is done for all requirements, the best candidate features will emerge. The table below is an example of how this is achieved. The high-level features are listed, and their need for a more complex architecture is assessed. The architectural needs are chosen based on what emerges from the design work as being the most significant, and should be geared towards cloud-oriented needs.

| Feature 	| Scale 	| Performance 	| Availability 	| Decoupled 	| Integration 	| Data complexity 	| POC Priority	|
|---------	|-------	|-------------	|--------------	|-----------	|-------------	|-----------------	|-----	|
| Feature 1 	| Low	| Low	| High	| High	| Low	| High	| 2	|
| Feature 2	| High	| High	| High	| High	| Low	| Low	| 1	|
| Feature 3	| High	| Low	| Low	| High	| High	| Low 	| 3	|
| Feature 4	| Low	| Low	| Low	| Low	| High	| High	| 5	|
| Feature 5	| High	| Low	| Low	| High	| None	| Low	| 4	|

The POC should deliver end-to-end features, not necessary complete in terms of functionality, but in terms of touching the technical issues in the stack. The choice of a feature also satisfies the need to deliver something that makes sense to the business. The more time that is allocated to a POC, the more features can be developed; bearing in mind that the preference is to cover more features briefly, rather than one in detail.

## How do you prove that it will work?
The architecture and application design of projects can only be proven by running code. The development and deployment of code is also the most time consuming and may not always provide the most convincing proof for the effort spent on development. Besides, some parts may be so complex that the amount of code needed to be written to prove a particular aspect may simply be too much for a short project. Other ways to prove an approach, design, or technical choice need to be investigated, as they may answer a lot of questions for a lot less effort.

Some of the ways to prove that the application will work are discussed below:

### Functional Proof of Concept
A functional POC, where a subset of functionality is developed and run on Windows Azure, is the most widely understood technique to prove that the application works. This is also the technique that is most commonly associated with the term 'POC'.

Functional POCs have the following benefits:

* They are targeted at risks that have been identified for the specific project.
* They deliver demonstrable features specific to the application.
* The outputs are understood by the business.
* The extensive effort required ensures high degrees of involvement by many stakeholders.

Functional POCs do suffer from the following problems:

* They can be expensive to run. A fully staffed team running for a couple of weeks can burn a significant budget.
* They run the risk of producing a deliverable that cannot be understood by everybody, thus making their value, and the ability of the team to deliver, questionable.
* They require a significant amount of design work to be completed to ensure that the proposed architecture is implemented.

### Technical Spikes
Technical spikes are used to prove specific architectural decisions. For example, an architect may want to implement a specific caching technology, but before handing it over to the team for implementation, will want to write some code against it and get it to run in a production environment, just to make sure that it behaves as expected.

Technical spikes are focussed, and generally developed by one person, and offer the following benefits:

* Allow proving to be done at any stage of the project, even when development is well under way.
* Are inexpensive to do, and can often be fitted into the normal workloads of individuals.
* Can be targeted and deal with a specific technical issue.

However, technical spikes are not as good as a functional POC:

* Their specific focus may not address all of the risks.
* They may be done too late to have a significant impact.
* Planned technical spikes may never be done, as individuals run out of capacity to perform them once the project gets underway.  

### Evidence
One of the easiest ways to prove the suitability of an application for Windows Azure is to look for similar success stories. Within Microsoft enterprise sales, this is a commonly used and frequently requested method of proof. These similar success stories are manifested as case studies that can be viewed and, depending on the nature of the parties, can even involve site visits and demonstrations. 

Using case studies as evidence for proving an approach has the following benefits:

* They are inexpensive to perform.
* They are usually quite quick to conduct.

But they have the following well-known disadvantages:

* They will never match the exact requirements.
* They are sales-oriented, so may be embellished.

### Vendor support
Microsoft, in particular, is approachable for help when building an application on Windows Azure. Particularly when the application is either large or for a worldwide recognised brand. Microsoft provides many ways to support POCs, which includes direct support from the Azure product group, TAPs (Technology Adoption Programs) for customers using features that are not yet released, and CTPs (Community Technology Previews) for features that are not yet in production. In some cases, a particular business unit within Microsoft may even sponsor the POC by providing resources or funding.

Vendor support, if accessible, is probably one of the best ways to prove an application. Direct support by the people that work on the platform is immensely valuable. This support may include:

* Access to skilled and experienced people who can answer difficult questions.
* Influence on the design in the early stages, so that the implementation team does not go down blind alleys.
* Validation of the design, and recommendations where it may be wrong.
* Insight into the product roadmap, that may mean that architectural decisions should be different, based on inside knowledge of Windows Azure features that may still be in development.

The benefits of support by the vendor include:

* A massive injection of skills onto the team that would be impossible without the vendor.
* Years of experience from successes and failures on the platform.
* Inside information into behaviour of features that may not be quite as they are marketed.
* Funding, if received, can make a big difference to the project budget.

Unfortunately, getting vendor support may not be easy:

* The vendor has limited capacity and cannot help every customer.
* Getting vendor support requires special vendor relationships.
* Extensive vendor support is seldom provided to small applications from unknown brands. The costs taken on by the vendor have to either be offset by revenue from the application or a marketing benefit.

### Consulting
Consultants, whether as individuals or as part of a consulting company are a good way of getting skills on board to prove an application, and to build the POC. Reputable consultants come with these skills, experience, and a desire to get things done.

The benefits of using consultants include:

* Experience of implementing similar applications, so consultants can point out problem areas and offer solutions.
* Consultants are especially useful when time is short and your own team is not sufficiently skilled.
* Consultants have networks of relationships with colleagues and vendors that may not exist within the internal team.

Consultants do have some drawbacks:

* Since consultants need to keep finding more opportunities, they may stay on the project too long. This may be problematic if they are only wanted for the POC.
* Good consultants are more expensive than internal resources and fees may be too much for the budget.

## When to do the POC
A POC should be done as early in the project as possible, but there are different points in the project that proving work can be done, depending on the means. 

* After the project has been validated — Immediately after validation very little design work has been done (across all the CALM models), so proving means are limited to evidence-based proofs, such as looking at case studies. Depending on how much the project can be talked about, bouncing the application concept and the initial architectural ideas off the community may also help validate the architecture. Since no design has been done, the easiest means may be to appoint an experienced consultant who can give the approach the once-over for an initial validation.
* After the initial design — The functional POC should be completed after the first-pass of the design. Once all of the models are complete, enough information exists to state the POC objectives, identify the features to develop, pick the technical areas that need attention, and plan the delivery of the POC.
* Multi-step POCs — For larger projects, it may be unnecessary to prove the entire application at once, and the POC can be completed in multiple steps over a longer period. For example, the consumer web facing part of an application, which needs to be highly available, scalable and cost-effective, may be proven first. Then, once the development is underway, the back-end integration POC can be done, possibly by a different team.
* Just-in-time architecture — Agile methodologies are often interpreted as eschewing up-front design because it smells too much like Waterfall, the arch nemesis of agile. Throughout the project, the YAGNI principle (You Ain't Gonna Need It) delays major architectural decisions for as long as possible. The approach of delaying architecture _may_ work, but is highly dependent on the skills of the implementation team, the engineering culture, and the organisational support (e.g. project management acceptance of refactoring). In these environments, proving the architecture and design can be a continuous process that makes use of frequent technical spikes.

## Time and effort
Proving an application approach and architecture needs a balance between doing enough to deliver meaningful proof and not taking too long to get it done. Functional POCs should try to address as many of the risks as possible, but at the same time not fall into the trap of being bogged down trying to do too much. Often the scope, time, and effort of the POC will be limited, not by technical reasons, but by deadlines, budget and resource availability constraints placed by the business.

The amount of time spent will vary depending on the team and the project being undertaken, but the following points should be considered:

* POCs should not try and undertake tasks that belong in the development phase. This means minimal high-quality engineering, and incomplete functionality.
* POCs are most effective when completed over a shorter period by a full team. A lone developer taking months to do a POC may not satisfy all of the objectives. A full team can get things done quickly and make sure that there is a common understanding of the architecture before development commences.
* Functional POCs should have a definite end date, with deliverables checked-in and assessed. They should not gradually turn into full-blown development projects.
* A nervous business environment, where there is concern about the viability of the application or the cloud, will need quick results to reduce anxiety. Resourcing and planning a POC to deliver quick results will help secure funding and resources for the development phase.
* High risk projects, such as those that deal with regulations, or where the primary business processes are being reworked into the application, will benefit from considerable effort spent on proving the architecture and approach. It is more important to make 100% sure on a crucial application than a small 'test' application.
* POCs need to be scheduled so that the full development project can start soon after the conclusion of the POC. This ensures that skills are not lost as people move back to where they came from. If the main development project is not ready to start soon after the conclusion of the POC (for example, by not having funding lined up), consider delaying the start of the POC.

## Failure to prove
A POC that fails to prove the viability of an application, approach, or architecture is still a result. Not only did the failure potentially save the business from committing resources to something that was destined for failure, but also lessons are learned along the way. Make sure that the POC does not end up as a project that absolutely has to succeed because of the investment in it by the business or individuals. If the project cannot be proven after a short POC, then the likelihood of long-term success is low, and making it work by changing the scope or measure of success of the POC will not increase the long-term viability of the project.

When a POC fails to prove the application, value is still added. Business is more aware of the implications of the cloud, architects have a better idea of how things work, and developers have more hands-on experience. Failure to prove is an opportunity to go back to the drawing board, either by selecting a different candidate project, by reworking the architecture to make it more suitable, or by starting a programme to change the internal influences on the project (such as internal IT and networking). Try and make sure that the outcome of the POC has value by documenting the lessons, keeping the source code, and encouraging team members to retain and develop their cloud skills. Another opportunity is bound to present itself, and a POC team that has learned what they can from a failure to prove, will be far more prepared, and have more to contribute to the next project.

## Summary
As detailed in [qualify](#Qualify), some projects are more suitable to cloud computing than others. Too often we see projects started on the back of enthusiasm (often driven by technical people), without clearly thinking the project through. Fortunes can be wasted on applications that simply don't fit on a public cloud platform. Not only is this a wasted money and effort but also ruins the opportunity for other applications that may be perfectly suited.

The purpose of the qualification process is to think through the viability of the project, but it is still subject to optimism and enthusiasm. The development of a POC, with custom code that is deployed and running on production Windows Azure, is where the reality sets in. The POC is the definitive running test that eliminates wishful thinking, positive marketing, and misinterpretation.

The POC is one of the most crucial steps in the early phases of the project, provided it is done correctly and at the right time. 

### Steps
1. Complete a first-pass run through of all CALM models.
2. Plan the POC (scope, objectives, constraints, time, etc.).
3. Investigate the use of evidence (case studies), vendor support, and consulting means of proof.
4. Develop a functional POC that implements an architecturally significant feature on a production Windows Azure deployment.
5. Perform technical spike proofs as and when necessary during the project.
6. Assess the viability of the application, architecture, and approach at the end of the functional POC.


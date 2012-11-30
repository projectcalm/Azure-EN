# <a name="IntroducingCalm">Introducing CALM</a>
Cloud computing promises that an application can be hosted in the cloud and immediately take advantage of infinite scalability, high availability, low maintenance and no upfront cost. Like any good marketing message there is enough truth to that promise to deem it correct, but things are a little more complicated than that. The benefits of cloud computing can only be realised by an application that is engineered specifically for, or adapted to, a cloud computing environment. However, the skills to achieve this are scarce and knowledge unclear. So if cloud computing is the future (at least for some applications), how do we go about building 'cloud engineered' applications without the seemingly necessary and obviously unavailable skills? 

Of course existing non-cloud development teams are not incapable, but they do need to be made aware of what needs to be done differently, will need additional skills or training, and will need to follow a process that encourages thinking about the application in a way that implements engineering that is better aligned to the cloud.

The challenge is that existing material tends to be too focussed on lower level technical tasks, such as working with the SDKs or infrastructure, and seldom covers topics that are shared by other domains, such as scalability patterns or data consistency. It can be argued that cloud computing offers a new paradigm for business, in which case such technical depth is of little use.

The constant focus on developer technologies means that individuals may have high confidence in their ability to deliver, due to familiarity with base technologies, but are not aware of risks until it is too late.  Simply put, development teams embarking on their first cloud development, regardless of their technical abilities, will struggle to ship as expected; because they don't have a process to follow which ensures they ask the right questions of the business or which focuses their attention on unfamiliar aspects.

However when it comes to training these development teams to deliver cloud applications, they are usually ahead of the skill curve and, in most cases, have done a fair amount of tinkering and research before recommending that an application be developed in the cloud. So we are talking to mature and capable individuals with a vast array of technical capabilities.

Our solution was to make assumptions about what they would know, taking Application Lifecycle Management (ALM) as a familiar set of practices, in order to highlight the differences in cloud computing application. This allows experienced teams to identify unfamiliar areas and ensure they are covered. The decision to address the entire application lifecycle is deliberate, because while the application code (say ASP.NET) is familiar to most developers, it is the operational environment and the designs that have the greatest influence on the application's architecture and implementation.

CALM (Cloud Application Lifecycle Management) applies specifically to Windows Azure and is the result of this demand to highlight what needs to be done across the entire application lifecycle. The format is easy to read, immediately useful and not prescriptive to a specific framework (other than Windows Azure) or methodology. This allows development teams to make use of CALM within their existing development processes by adding to, rather than replacing, current practices.

Most cloud guidance is either a statement about the market (players, maturity, technology etc.), a set of solutions to particular problems (the cookbook guide), or a walkthrough of a fictitious case study. Whilst these are useful training aids, neither format is directly applicable to the project at hand and doesn't play a part in actively reducing risk. CALM has been developed as a guide that can be used throughout the project and outlines the specific steps and deliverables that are required. This is achieved by working with specific models that can be built quickly, but which can also be evolved in more detail as the project progresses. The primary models addressed are the Workload Model, Lifecycle Model, Capacity Model, Availability Model, Health Model, Operational Model, Deployment Model, Security Model, Cost Model, Integration Model, Test Model and the Tenancy Model, with supportive processes around qualification, proof of concepts and development approaches.

CALM describes the differences, opportunities and problems in cloud computing applications and the specific output required for each model. The output cannot be produced at once, or during a specific stage of the project, and the models describe what needs to be produced at what stage. For example, in the deployment model, having 'an idea' of how an application is deployed is critical early on in the project, but the model needs to evolve to depict exactly how an application is deployed by the time it is handed over to operations.

Whilst developed specifically for Windows Azure, CALM deliberately spends little time discussing the underlying technology. Firstly, the principles being communicated are not technology specific and the solutions to them are architectural practices rather than technical implementations. Secondly, too much technical detail would be overbearing for a broad audience who need to understand the problems and solutions (a tester has no need for much detail on Windows Azure). Finally, there is plenty of material and reference for the technical detail, so the communication of low level technical detail is largely a solved problem.

Whilst not filled with detailed code samples, CALM is not theoretical and does not shy away from practical use. It contains definitive references to deliverables and is scattered with examples that apply to the given topic. The approach and examples have been jointly developed by Microsoft and Minttulip, bringing together years of 'from the field' consulting experience by senior practitioners.
Whatever use is made of CALM, from a single reading to a reference companion, we are confident that by highlighting differences and areas of focus we can reduce the risk of costly mistakes. After all, we know that the cloud can deliver; we know that technical solutions exist, and we know that our teams have the ability. We just need to make sure that it all works together in a way that delivers the best product.

## Who should read CALM?

* Software Architects, Senior Developers and Developers should read through CALM to understand how the content applies to their particular project. They are the people making the day to day decisions that have far reaching impacts on architecture and the ability of the application to live up to its delivery promise.
* CTOs and Development Managers should at least read through the 'problems' and 'opportunities' presented in each model. This will help them contextualise the cloud hype and how it maps to their own organisation's capabilities and initiatives.
* Business representatives should read the introductory sections of each model in order to understand how they can contribute to the project.
* Project Managers should understand the deliverables of the models in detail so that they can be discussed with the implementation team and worked into the overall project plan.
* Cloud computing concepts will be new to Testers who have a critical role to play in making sure that everything works, and cloud computing concepts will be new to them. CALM will help them to understand both their responsibility and the architectural decisions they will need to validate.
* Operations staff are responsible for maintaining availability on a platform that has high expectations and is assembled in a completely new way. CALM will ensure that the ability to operate the application remains a fundamental requirement.

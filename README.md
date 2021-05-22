
About
=======

A collection of maxims and reflections about microservices and all around them.


What are Microservices, anyway?
================================

* Microservices are both an architectural style and a set of cultural practices, underpinned by 5 (five) key principles: 

 1) autonomy - each Microservice can know about existence of others ones, but they are egoists and don't care
 2) resilience - Microservices work together and on their own, because in a distributed system a service can’t trust its collaborators
 3) transparency - we need to know what is going on in this cloud. In order to be more confident in a service one can reliably deploy and monitor it. Service monitoring should include log aggregation and service-level health
checks - use ELK for that
 4) automation - need this if we have 10^n microservices, where n -> infinity
 5) alignment - consistency is the key. 
 
 
* As a result microservices reduce friction in development, enabling autonomy, technical flexibility, and loose coupling.

* Designing microservices is challenging because of the need for adequate domain knowledge and balancing priorities across teams. Take a look at Amazon - their services reflect the organization's structure: 1 team = 1 service. As a result it’s crucial to understand the product domain when designing microservices.

* Services expose contracts to other services. Good contracts are 

 1) succinct
 2) complete
 3) predictable
 
* Complexity in long-running software systems is unavoidable, because it is impossible to deal with complex problems using only simple instruments, as it is impossible to build a microchip in XXI century using screwdriver. But one can deliver value sustainable in these systems if you make choices that minimize friction and risk.

* Reliably incident-free (aka “boring”) deployment reduces the risk of microservices by making releases automated and provable. Dockerizing helps to improve automation and reliability as containers abstract away differences between services at runtime, simplifying large-scale management of heterogeneous microservices.

* Failure is inevitable, and we have to accept this fact stoically (with chocolate and beer): microservices need to be transparent and observable for teams to proactively manage, understand, and own service operation.

* Teams adopting microservices need to be operationally mature and focus on the entire life-cycle of a service, not only on the design and build stages.

* Microservices are highly applicable in systems with multiple dimensions of complexity — for example, breadth of product offering, global deployment, and regulatory pressures.

* Service interactions may be orchestrated or choreographed. The latter adds complexity but can lead to a more loosely coupled system.

* API gateways are a common pattern for abstracting away the complexity of a microservice architecture for front-end or external consumers.

* Avoiding divergence and isolation requires standards and best practices to be similar across multiple teams, regardless of technical underpinnings.


In the beginning was a Monolith
================================

* Individually, microservices are similar internally to monolithic applications.

* A microservice application is like a neighborhood: its final shape isn’t prescribed but instead guided by principles and a high-level conceptual model.

* The principles that guide microservice architecture reflect organizational goals and inform team practices.

* The architectural plan should encourage growth along good lines, rather than dictate approaches for the overall application.

* A microservice application consists of four layers: platform, service, boundary, and client.

* The platform layer provides tooling, plumbing, and infrastructure to support the development of product-oriented microservices.

* The very first Microservice is a Monolith, but the second one is not; 2 is a magic number.

* Synchronous communication is often the first choice in a microservice application and is best suited to command-type interactions, but it has drawbacks and can increase coupling and fragility. So move away from it.

* Asynchronous communication is more flexible and amenable to rapid system evolution, at the cost of added complexity. This is the aim.

* Common asynchronous communication patterns include queues and publish-subscribe (just synonyms for Kafka)


Transactions
=============

* ACID properties are difficult to achieve in interactions across multiple services; microservices require different approaches to achieve consistency.

* Coordination approaches, such as two-phase commit, introduce locking and don’t scale well.

* An event-based architecture decouples independent components and provides a foundation for scalable business logic and queries in a microservice application.

* Biasing towards availability, rather than consistency, tends to lead to a more scalable architecture.

* Sagas are global actions composed from message-driven, independent local transactions. This is just a log of all what is happend. They achieve consistency by using compensating actions to roll back incorrect state.

* Anticipating failure scenarios is a crucial element of building services that reflect real-world circumstance, rather than operating in isolation.

* Implement queries across microservices by composing results from multiple APIs.

* Efficient complex queries should use the CQRS pattern to materialize read models, especially where those query patterns require alternative data stores.


Reliability
============
* Failure is inevitable in complex distributed systems — it is necessary to consider fault tolerance during design phase.

* The availability of individual services affects the availability of the wider application.

* Choosing the right level of risk mitigation for an application requires careful consideration of the frequency and impact of failure VS the cost of mitigating against potentially rare events.

* Most failures occur in one of the following 4 (four) areas: 

1) hardware
2) communication
3) dependencies
4) internally.

* Cascading failures result from positive feedback and are a common failure mode in a microservice application. They’re most commonly caused by server overload.

* One can use retries and deadlines to mitigate against faults in service interactions. You need to apply retries carefully to avoid exacerbating failure in other services.

* Use fallbacks — such as caching, alternative services, and default results — to return successful responses, even when service dependencies fail.

* Stop propagate deadlines between services to ensure they’re consistent across a system and to minimize wasted work.

* Circuit breakers between services protect against cascading failures by failing quickly when a high threshold of errors is encountered.

* Services can use rate limits (aka throttling) to protect themselves from spikes in load beyond their capacity to service: tell user to wait a couple of secs before clicking on button once again

* Individual services should expose health checks for load balancers and monitoring to be able to use.

* One can effectively validate resiliency by practicing both load and chaos testing (f.e. via Chaos Monkey framework)

* Apply standards — whether through proxies or frameworks — to fall into the pit of success and build services that tolerate faults by default. Spring Boot is a good choice in the beginning



The Basis for everything
=========================

* A microservice chassis allows for quick bootstrapping of new services, enabling greater experimentation and reducing risk.

* The use of a chassis allows you to abstract the implementation of certain infrastructure-related code.

* Service discovery, observability, and different communication protocols are concerns of a microservice chassis, and it should provide them.

* Although the microservice architecture is often associated with the possibility of building systems in any language, those systems, when in production, need to offer some guarantees and have mechanisms to allow their operation and maintenance to be manageable.

* A microservice chassis is a way to provide those guarantees while allowing fast bootstrap and quick development for you to test ideas and, if proven, deploy them to production.


Deployment, part1
==================

* Deploying new applications and changes must be standardized and straightforward to avoid friction in microservice development. Write Python scripts which execute all the rest bash scripts, deploy apps, write other scripts, etc

* Microservices can run anywhere, but ideal deployment platforms need to support a range of features, including security, configuration management, service discovery, and redundancy. K8s is the best choice, for local testing use MiniKube

* A typical service as a group of identical instances, connected by a load balancer - see any service from AWS as a model example (AWS is a front-runner when it goes to microservices culture)

* Instance groups, load balancers, and health checks enable autohealing and autoscaling of deployed services.

* Service artifacts must be immutable and predictable to minimize risk, reduce cognitive load, and simplify deployment abstractions.

* One can package services as language-specific packages, OS packages, virtual machine templates, or container images - the last option is the best one, even though has some overheads and drawbacks (images are expensive to build)

* Being able to add/remove a single instance of a microservice is a fundamental primitive operation that One can use to compose higher level deployment.

* One can use canaries or blue-green deployments to reduce the impact of unexpected defects on availability.


Deployment, part2
==================

* Packaging microservices as immutable, executable artifacts allows you to orchestrate deployment through a primitive operation — adding or removing a container.

* Schedulers and containers abstract away underlying machine management for service development and deployment.

* Schedulers work by trying to match the resource needs of an application to the resource usage of a cluster of machines, while health-checking running services to ensure they’re operating correctly.

* Kubernetes provides ideal features of a microservice deployment platform, including secret management, service discovery, and horizontal scalability.

* A Kubernetes user defines the desired state (or specification) of their cluster services, and Kubernetes figures out how to achieve that state, executing a continual loop of observe-diff-act.

* The logical application unit on Kubernetes is a pod: one or more containers that execute together.

* Replica sets manage the lifecycle of groups of pods, starting new pods if existing ones fail.

* Deployments on Kubernetes are designed to maintain service availability by executing rolling updates of pods across replica sets.

* One can use service objects to group underlying pods and make them available to other applications inside and outside of the cluster.



Deployment, part3
==================

* A microservice deployment process should meet two goals: safety at pace and consistency.

* The time it takes to deploy a new service is often a barrier in microservice applications.

* Continuous delivery is an ideal deployment practice for microservices, reducing risk through the rapid delivery of small, validated changesets.

* A good continuous delivery pipeline ensures visibility, correctness, and rich feedback to an engineering team. See the Heroku CI/CD pipelines as a model example

* Jenkins is a popular build automation tool that uses a scripting language to tie multiple tools together into a delivery pipeline.

* Staging environments are invaluable but can be challenging to maintain when they face a high volume of independent change.

* One can re-use declarative pipeline steps across multiple services because standardization makes deployment predictable across teams.

* To provide fine-grained control over rollout and rollback, you should manage the technical activity of deployment separately from the business activity of releasing a feature.


Monitoring, part 1
===================

* A robust microservice monitoring stack consists of metrics, traces, and logs.
 
* Collecting rich data from your microservices will help you identify issues, investigate problems, and understand your overall application behavior.

* When collecting metrics, you should focus on four golden signals: latency, errors, traffic (or throughput), and saturation.

* Prometheus and StatsD are two common, language-independent tools for collecting metrics from microservices.

* One can use Grafana to graph metric data, create human-readable dashboards, and trigger alerts.

* Alerts based on metrics are more durable and maintainable if they indicate the symptoms of incorrect system behavior, rather than the causes.

* Well-defined alerts should have a clear priority, be escalated to the right people, be actionable, and contain concise and useful information.

* Collecting and aggregating data from multiple services will allow you to correlate and compare distinct metrics to gain a rich overall understanding of your system.

Monitoring, part 2
===================

* One can set up a logging infrastructure using Elasticsearch, Kibana, and Fluentd, and distributed tracing using Jaeger.

* A logging infrastructure can generate, forward, and store indexed log data that allows searching and correlating requests.

* Distributed tracing allows you to follow the journey of execution of requests through different microservices.

* Alongside metrics collection, tracing allows you to better understand how the system is behaving, identify potential issues, and audit your system anytime.



# microservices

## Introduction
* What is a Service?
    * a piece of software which provides functionality to other pieces of software.
    * communication between a client and a service normally happens over the network using a communication protocol.
    * Such system is called to be following a services oriented architecture(SOA).
    * Main idea behind SOA is that instead of keeping a functionality inside a package module, you keep the functionality in a service. This functionality can now be used by multiple clients.
    * It allows to scale up a softwre when demand increases by increasing the servers.
    * Services have interface contracts with clients. This enables the clients to upgrade as long as the contract is honored.
    * Stateless
* Microservices Introduction
    * evolution of SOA
    * SOA + Design Principles = Microservices
    * [Scalable](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling), Flexible, 
    * Multiple Services
    * Small service with a single focus
    * Lightweight communication mechanism
    * Technology Agnosting APIs
    * Independent data storage
    * Independently Interchangeable(upgrade, enhance , fix without touching other microservices)
    * Inedependently Deployable
    * Distrubuted transactions
    * Centralized tooling for management
* The Monolithic
    * Typical enterprise application
    * No restriction on size(since there is a just one big package, whatever you add, gets added to that 1 package)
    * Large codebase
    * Longer to develop new functionality
    * Integration testing takes longer
    * Deployment takes longer
    * Bug fixing takes longer(as you need to find it first in the whole big package)
    * Hidden functionalities that might be useful to other client(because 1 big package, this package just serves the application, hasnt' exposed that functionality as a service)
    * Fixed Tech stack.
    * High coupling
    * if a part of system fails, the whole system fails.
    * Scaling requires scaling the whole package.
    * Longer time for CI because change in one part has to run tests for the whole system.
    * One database change can affect every part of system.
    * only advantage is that it is easier to replicate across machine, because it is just one thing. To get a whole microservices powered application, you need to setup each and every microservices to replicate.
* Emergence of Microservices. Why Now?
    * Need to respond to change quickly(upgrade, fix, scale, rollout)
    * More reliability(doing its job at any given time)
    * Business domain driven design(department to microservices mapping)
    * availability of automated test tools(every change in a microservice can immediately trigger a test for integration)
    * availability of automated release and deployment tools.
    * availability of on-demand hosting technologies(very easy to spin off another VM to scale)
    * don't even need to own physical machine(cloud architectures)
    * Multiple tech stacks(you can have whats best suited for a microservice)
    * Asynchronous communication technology.
    * Simpler client side and server side technologies, which generally use open communication protocol like HTTP REST.

    * Benefits
        * Smaller development time(smaller scope, multiple teams working concurrently on there microservice, no worries as long as you honor the contracts).
        * Reliable, faster deployment
        * frequent updates.
        * Decouple changeable parts(if a service tends to change a lot, it can, as long it honors the contract)
        * Increases security by compartmentalization (eg: each microservice has its own database, if one database gets compromised, other are still secure, not all data is compromised).
        * Increased Uptime(upgrade one microservice at a time).
        * Fast issue resolution(as issues are now scoped).
        * Highly scalable and better performance.(just scale the microservice which is required to).
        * Right tech stack
        * Scoped and focused ownership, increases knowledge about that microservice.
        * enables distributed teams.

### Design Principles
#### High Cohesion
* Single Resposibility Principle
* Should focus on one thing and it should do it well
#### Autonomous
* A microservice should not change because its client or any other service/microservice it itself is using, changed.
* Loosely coupled with every external system.
* Goes vice versa, a change in a microservice should not dictate change to other client/microservice. It should honor all its contracts.
* Stateless (one request should not be dependent on any other previous requests)
#### Business domain centric
* A microservice should represent a business domain(business function)
* Bounded context from DDD(you always work within a scope, your environment is setup just for that particular business function)
* Identify boundaries
* There will be times when you wouldn't understand that a particular code should go to which microservice, shuffle the code in between microservices to find out where it should sit. Or create another microservice for this related code.
* Responsive to business change.
#### Resilience
* We have to be ready for failure when it happens.
* might happen if a microservice is down.
* might happen if the communication line is down.
* might happen if another third party system is down.
* whatever the failure, our microservice should degrade the functionality or by defaulting the functionality(WHENEVER IT CAN).
* example of degrading the functionality: lets say the promotions functionality is broken, so our UI microservice choose not to show this functionality.
* example of defaulting the functionality: lets say the postage functionality is down, then orders microservice can decide upon a default postage amount.
* having multiple instances of microservices. when a microservice starts, it registers itself to the load balancer, when it goes down, it deregisters itself, so that the load balancer will not even redirect the request to that microservice.
* program a microservice(when using other microservice) to handle:
    * exceptions/errors
    * delays
    * unavailability
* program to handle network failures/delays/unavailability. choose to degrade/default.
* validate input
#### Observable
* Overall application should be observable as a whole somehow
* System health : status, logs
* Centralized logging
* Centralized monitoring
* This is needed because we now have distributed transactions, easy to pinpoint problem, deploy quickly.
* Easier for locating capacity planning(scaling)
* can do usage analytics on a microservice(this is what will help in scaling).
* can do business analytics by logging particular metrics(which form different KPIs).
#### Automation
* automation tools to reduce time in testing, setting up environment, regression testing
* CI to give immediate feedback on check in
* pipeline for deployment, CD.
* automation makes it more reliable.
## Microservices Design
### High Cohesion
* Identify a single focus, usually by identifying the business domain.
    * even a business domain can be further broken down.
* your microservice should not change for more than one reason.
* Overall objective is the create a system which is scalable/reliable/flexible
### Autonomous
* Independently changeable
* Independently Deployable
* Loosely coupled
* compartmentalization
* communication with other microservice can be:
    * synchronous/connected (immediate response from the server, but response only as a token)
        * to make it decoupled, asynchronous concept is used where you send a request along with a callback address. The server then sends back a token notifying that your request has been registered. then when the actual task is completed, the server sends a request to the callback address.
    * disconnected (in the form of events publish/subscribe)
        * client publishes an event for a task by sending a message to a message broker. The server is listening to those event, it picks up, finishes it and sends a message to the message broker saying that the task is completed. the client is now listening to those events and picks it up.
* Technology agnostic API (use open communication protocols like REST over HTTP)
* Avoid client libraries, for example if you have used windows web services, it used to provide wsdl files, which was used to create proxy classes for the consumer of the web service. Now if the web service change, the proxy classes will have to be updated.
* Using client libraries can also restrict the tech stack for the consumer.
* fixed contracts.
* always keep the internal and shared models separate (concept of DTOs).
* Avoid chatty exchanges(didn't understand this)
* Avoid sharing databases.
* Avoid sharing third party libraries
* Microservice ownership by team
* Versioning
    * Avoid breaking changes
    * Backwards compatible(new features are added, but what used to work should still work)
    * Use Integrated tests to ensure backwards compatibility
    * sometimes you can not avoid breaking changes, this is when you need to have a multiple versions of microservice running(may be change the url/postbody/headers)
    * semantic versioning Major.Minor.Patch.Build
    * coexisting endpoints(you have both v1 and v2 running), this gives users time to migrate
### Business Domain Centric
* Approach
    * Identify coarse microservices
    * review benefits of splitting further
    * if a business function is implemented by a microservice, then all other clients/microservices should call this microservice.
* You can have a microservice just for Data(CRUD) or just for functions.
* Be ready to Merge/Split microservices
* explicit interface for outside world
* Splitting because of technical reasons(should follow HIGH cohesion though).
### Resilience
* We cant have entire system going down because of one failure.
* Design for known failures.
    * Failure of downstream services(system on which our microservice relies on to complete its task)
    * Degrade/Default functionality. Should be a design time decision.
* Design system to fail fast.
    * use timeouts to find out if dependency microservice has gone down. If a service doesn't respond within timeout, assume its down.
* Network outages and latency
    * use timeouts
* Monitor and log timeouts
### Observable
* Real time monitoring
* Monitor for CPU/Memory/DiskUsage, etc.
* Expose metrics within the services
    * Response times,
    * Timeouts
    * Exceptions and errors
* Business data related metrics
    * example: number of orders
    * average time from basket to checkout
* Monitoring tools to find out trends, provide aggregation/ drilldowns
* Monitoring tools to compare between servers.
* Alert system if some measure goes beyond.

* Difference between Monitoring/Logging is that Logging is detailed information about events whereas monitoring is count/sum/average/*somemeasure* etc of those events.
* Startup/shutdown
* code path milestones
    * request response decisions
* Structured Logs
    * Level
        * information
        * error
        * debug
        * statistics
    * Date and time
    * Corelation ID (like a common ID for the distributed transaction)
    * Host Name(machine, a machine can have multiple microservices)
    * Service name Service instance
    * Message (like call stack, stack trace)
### Automation
* Use CI tools
* Use CD tools
## Technology for Microservices
### communication
* synchronous: RPC(sensitive to change), HTTP/REST, Usually slow
* asynchronous: Message Broker(like 1 single Message Broker for all the microservices)
    * Message Queuing protocol
        * Microsoft Messaging Queue(MSMQ)
        * RabbitMQ
        * ATOM(HTTP to propogate events)
    * Asynchronous challenges
        * Complicated
        * Reliance on Message Broker(if it goes down, whole system goes down)
        * No Visibility of transactions because responses are asynchronous
        * Managing the messageing queue. Always look out for performance issues
* In a real world, its always a mix of both synchronous and asynchronous communications.
### Hosting Platform
#### Virtualization
* VMs as host
* Foundation of cloud platforms
    * Platform as a Service(PAAS)
    * Azure, AWS, Google cloud
* Downsides
    * Takes time to setup
    * Takes time to load
    * Takes resources(shouldn't be a problem as the main OS on which VM is running knows that it is supposed to host VMs and will have a lot of resources)
* Unique features
    * Take snapshot
    * Clone
* Standardised and Mature
#### Containers
* Unlike VMs, containers do not run the full OS, they run the minimal amount to run the service.
* Isolate services from each other. Its a common practice to run one service per container.
* since they are not full OS, they use less resources.
* you can have more containers on a host than VMs
* since they are small, they are faster
* growing cloud platform support
* mainly linux based, windows is still working on it.
* not yet standardised.
* examples: docker, rocker, glassware
#### Self Hosting
* refers to normal data center hosting that we used to do before cloud hosting came into picture.
* Implement your own cloud
    * Implement your own virtualization platform
    * Implement your own Containers
* Use of physical machines
    * single service on a server
    * multiple services on a server
    * But if you really want to use the benefits of microservices architecture, you will end up using some kind of cloud hosting technique.
* challenges
    * Long term maintainence
    * Need for technicians
    * Training to IT staff
    * Need for space every time you want to scale.
#### Registry and Discovery
* When we need to connect to a microservice, we need to know:
    * Host, Port, Version
* One way is to have a Service Registry Database, which holds information about all the microservices and therey host, port and version
    * when you implement a microservice, or when you create another instance of a microservice to meet demand, on startup these microservices register themselves on this database.
    * when a microservice stops responding, it removes itself from this database
* Service registration/deregistration are a problem with Self-hosting as you will have to implement them on your own. Cloud providers have automatic way for this.
    * the service registers itself
    * third party registration, any service startup is listened to and registered.
* how to clients look for services in this Service Registry Database
    * either they directly query the SRD -> Client Side Discovery
    * Or there is a Gateway(proxy) that does it for you. -> Server Side Discovery
### Observable
#### Monitoring Tech
* Centralised tools
    * Nagios
    * PRTG
    * Load Balancers
    * New Relic
* Desired features
    * Metrics across servers
    * automatic and minimal configuration
    * Client libraries to send metrics
    * Test transactions support
    * Alerting
* Network Monitoring
* Standardised Monitoring
    * Central tool
    * Preconfigured VMs and containers
* Has to be Real Time
#### Logging Tech
* Portal for centralised logging data
* examples: Elastic log, Log stash, Splunk, Kibana, Graphite
* Client logging libraries within each microservice, ex: Serilog.
* Desired features
    * Structured logging
    * Logging to work across servers
    * automatic or minimal configurations
    * Corelation/context Id for transactions
* Standardised Logging
    * Central tool
    * Template for client library(so that the format, structure is the same standard across the system)
### Microservices Performance
#### Scaling
* how
    * Add another instances
    * Add resources to the current instance
* Automated or on-demand
* PAAS provides auto scale option
* Automated scaling is only possible because of VMs and containers
* Physical host servers are slow
* After scaling you will need Load Balancers(usually in the form of API Gateways)
* When to scale
    * Performance issues
    * Monitoring Data(when system is already reaching its potential)
    * Capacity planning(when it shows that the usage is going to increase in future)
#### Caching
* API Gateway
* Client
* Service
* Implementation should be Simple
* Handle the expiration properly
#### API Gateway
* Help with performance
    * Load Balancing
    * Caching
* Help with
    * Central entry point
    * Exposing services to the clients
    * One interface to many services
    * holds reference to SErvice Registry Database, and can quickly locate the microservice host/port/version
    * Acts as a Router for routing to specific instance of service
    * Authentication/Authorisation, can have a security microservice interacting with it.
### Automation tools
#### CI
* TFS, Teamcity etc.
* Desired features
    * Cross platform
        * windows builders, java builders etc.
    * Source control integration
    * send notification
    * IDE integration (optional)
* Map a Microservie to a CI Build
* Avoid one CI build for all microservices
#### CD
* Cross platform
* Central control panel
* simple to add extra deployment targets
* integration with CI tool
* must support multiple environments
* support for PAAS
## Moving Forward with Microservices

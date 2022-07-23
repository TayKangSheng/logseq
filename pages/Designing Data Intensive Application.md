- Chapter 1: Reliable, Scalable, and Maintainable Applications
  collapsed:: true
	- In the past we have applications that are compute intensive, but these days most applications are data intensive. Many applications needs to store data, cache data, create indexes (eg Elastic Search), stream data, process / transform data. The speed at which data is managed has become a bigger issue or limiting factor rather than whether Raw CPU power is enough.
	- An application's Data System is usually built from standard building blocks put together to offer a functionality. For example, in a Rails app, the Data System can be a combination of RDBMS, Redis and ElasticSearch put together to offer a set of data through its API. Most engineers do not build their data storage engine because the existing standard building blocks are perfectly good tool for the job.
	- Its important to understand the needs of your application and choose the right building blocks to build your Data Intensive Application. This book will take us through principles and practicalities of designing a data system; and analysing existing tools.
	- Fundamentals of a Data System: Reliability, Scalability and Maintainability.
		- When we design our special purpose Data System for our Application, there are many factors to consider based on the guarantees we want to provide. For example performance of our API, scaling with increase in load on our Application, correctness and completeness of the data etc.
		- 3 things broad category we can focus on:
			- Reliability. Roughly defined as "continuing to work correctly, when things go wrong."
				- Faults are things that can go wrong or a component deviating from its spec. A reliable system is fault-tolerant or resilient. But there is no system that is tolerant of every kind of fault, so a fault-tolerant system will be a system that is tolerant of a certain set of faults depending on what is important.
				- Faults can be induced deliberately, for example randomly killing process without warning; or passing in certain parameters to the API to trigger an edge case that will crash the app; error in application configuration changes.
				- Faults are usually tolerated but in some cases we might choose to prevent it.
				- What are some examples of Faults?
				  card-last-score:: 3
				  card-repeats:: 1
				  card-next-schedule:: 2022-07-27T14:06:28.851Z
				  card-last-interval:: 4
				  card-ease-factor:: 2.36
				  card-last-reviewed:: 2022-07-23T14:06:28.859Z
					- Hardware Fault. Hard Disk crash, RAM becomes faulty, Network cable unplugged, Power blackout etc.
						- Existing solutions includes hardware redundancy like additional power generators as backup power, hot swappable CPUs, RAID configuration for disks.
						- However with the rise of Cloud, Data Centers and VMs, sometimes entire VMs disappears. Therefore there is a move towards systems that can tolerate the loss of entire machines by using software fault-tolerant techniques in preference or in addition to hardware redundancy.
					- Software Errors. Software bugs, runaway process that clogs memory / disk / cpu, cascading failures where failure of one component triggers a snowball failure effect on other components.
						- Usually caused by outdated assumptions which has changed overtime or some edge cases that the software has not covered.
						- Usual solutions includes thinking about the assumptions and interactions of the design of the component with others; monitors for correct behaviours in production; automated tests. But there is no one size fits all.
					- Human Errors. Configuration errors that cause outages or components in Application to not work as expected.
						- Usual solutions includes designing with right abstractions to prevent error from happening; provide sandbox environment to test configurations; allow quick rollback if wrong configuration is applied; good management practice and training.
				- Reliability is important sometimes for catastrophic reasons (Nuclear Power Plant) and sometimes for good user experience (Shopify). Sometimes we cut corners because of reasons but we should be conscious of what we are trading them for.
			- Scalability. The ability of a system to cope with increased load (growth in different dimensions).
				- Describing Load
					- Growth in different dimensions are called Load Parameters. Load Parameters are decided by you based on your system for you to succinctly describe load on your system. When these Load Parameters increases, without design to scale, they will cause a performance degradation of your system. For example, requests per second to your API, read to write ratio to your database, simultaneous active users in a chat room, hit rate on cache etc.
				- Describing Performance
					- Performance are described using metrics that your system guarantees (SLA) or is desirable. Like throughput, response time, accuracy etc. Percentiles are preferred over average values as it shows a clearer picture of the performance of your system (eg. the response time for the 99.5 percentile)
					- When load parameters increases, how is the performance of your system affected? How much resource increase do you need to maintain performance?
					- Interesting story. Amazon describes response time requirements for internal services in terms of 99.9th percentile because the customers with the slowest requests are often those who have the most data on their accounts, which also means they have made the most purchases. Maintaining performance for them is valuable. But 99.99th percentile proves to be too costly.
				- Coping with load
					- With Performance metrics and Load Parameters, we can test if our system can maintain good performance with increase in Load Parameters.
					- For hardware resourcing, can be scaled automatically or manually. Scaling up (single machine) is usually simpler but can get costly exponentially. Scaling out (multiple machines) is also known as shared-nothing architecture. A good architecture includes a mix of both strategies.
					- Distributing stateful data systems can introduce a lot of complexity, so common wisdom for now is to keep your database single node until scaling cost or high availability requirements force you to make it distributed. As the tools and abstraction for distributed systems get better, this common wisdom might change.
					- Theres no 1 size fits all strategy for scalable architecture so it depends on your Application. Usually scalable architecture is based on assumptions of which operations will be common and which will be rare. Common operations should already be captured by Load Parameters.
			- Maintainability.
				- Well known that majority of the cost of software is not in the initial development, but in its ongoing maintenance. Bug fixes, keeping system up, responding to failure, adding new features, repaying technical debt etc. However many people dislike maintenance of legacy systems. We should design our system in a way that minimises pain during maintenance and thus not letting it become another "legacy" system.
				- Operability: Making life easy for operations
					- Good operations can work around bad software, but good software cannot run reliably with bad operations. Operations team are vital in keeping a software system running smoothly. Some examples of responsibilities are:
						- Monitoring health and restoring service if it goes into a bad state.
						- Investigating root cause of problems such as system failure, degraded performance.
						- Maintenance tasks and Security patches.
						- Keep tabs on how different systems affect each other, so that a problematic change can be avoided before it causes damage.
						- Defining processes to keep application stable.
					- Good operability means making routine task easy and operations team can focus on high-value activities. For example
						- Provide visibility to the performance and internal workings of the system.
						- Provide good support to allow automation for routine tasks
						- Good documentation.
						- Avoid dependancy on certain hardware and make maintenance of hardware easy.
						- Give operations team some freedom to control the system or override defaults when needed to keep the system in control.
					- Basically think from a long term maintenance point of view.
				- Simplicity: Managing Complexity
					- Large projects get complex and slows down everyone who needs to work and maintain the system. It also increases the risk of introducing bugs, makes software hard to read and reason, increases the chance of hidden assumption, consequences and interactions getting overlooked. Some symptoms include explosion of state space, tight coupling of modules, tangled dependencies, inconsistent naming and terminology, hacks, special-cases to work around issues etc.
					- Complexity is defined as accidental if it is not inherent in the problem that the software solves but arises only from the implementation.
					- One of the best tools for removing complexity is abstraction. Good abstraction is hard, but it can hide a great deal of implementation detail behind clean simple-to-understand facade. It also easier to reuse and leads to higher quality software when abstraction is improved.
				- Evolvability: Making Change Easy
					- As time passes, requirements changes too. How easily can we change a data system to adapt to the new requirements. Often it is closely linked to simplicity of the system as simpler systems are easier to modify than complex ones.
	- Summary
		- 3 principles to keep in mind when designing Data Intensive Applications.
			- Reliability. How tolerant is your system to faults and continue to work as expected.
			- Scalability. How performant is your system when load increases.
			- Maintainability. How easy is it to keep the lights on.
		- There is no 1 size fits all and depends on the requirements of your application. However there are many patterns and techniques that are frequently used by many different type of applications. We will explore them in the following chapters.
	-
- Chapter 2: Data Models and Query Languages
**Migration and Modernization:**
- Migration - process of moving assets from on-prem to cloud. Eg: applications, data and workloads 
- Legacy systems on-prem do not offer a lot of flexibility - cloud provides more agility, scalability, mechanisms for disaster recovery as well as cost savings by moving to a pay-as-you-go model. 
- Modernization - process of enhancing and updating IT systems to leverage cloud technologies. Involves moving from legacy systems to cloud-based env to improve operational efficiency, scalability, performance. 
- Why cloud modernization - 1) agility: needed for the fast moving future and new tech innovations 2)cost efficiency: number of resources used will decrease, saving costs. 3)innovation: access to advanced tech already embedded in cloud tech - such as ai/ml models 4)scalability - easier to scale up and down based on demand. 
- When to choose what: most important point - understand the age and maintainability of the legacy system. 
- Migrating an application - lift and shift. Modernizing it - breaking down the application to pieces and then taking each part and building it with efficiencies and reduced costs. 
- Which comes first, modernization/migration - depends on the use case. 

**6R's AWS (Now 7R):**
- ![[Pasted image 20251125105205.png]]
- *Rehosting:* lift and shift - lift servers and apps from on-premise to cloud. 
- *Replatforming:* Also known as “Lift, Shift and Tweak”, this strategy is a slight variation from re-hosting. The core architecture of the application remains the same, but a portion of the application or the entire application is adapted to exploit new cloud features
- *Repurchasing:* repurchasing out-dated licenses, servers, security amis that are not compatible with the cloud you are migrating to. 
- *Refactoring:* involves converting a legacy monolithic application into a new, highly decoupled, and cloud-native architecture
- *Retiring:*  phasing out applications that are no longer required. 
- *Retaining:* keeping some applications on-premise that are very tied to the legacy systems. 
- *Relocating:* relocating all the servers - either to different clouds or to different on-premise locations

**Phases of Migration:**
- Assess: Understand the IT landscape to make migration decisions. Data usually provided by client and categorize apps based on criticality. Migrate from lower -> high prod envs. 
- Mobilize: Preparing a detailed migration plan. Landing zone, networking, full arch plan, migration strategy, cost. 
- Migration: Execute the migration plan. Migrate from lower -> high prod envs. 
- ![[Pasted image 20251125110057.png]]

**Monolithic vs Microservices:**
- *Monolithic* - traditional approach for designing a s/w where an entire app is built as a single unit. All components are tightly integrated with one another. Any changes - have to make changes/update sand redeploy the entire architecture. Simple and easy to develop, faster to develop and deploy  - however can become difficult to maintain and scale as the complexity of the app grows. Moreover there is no isolation between components so if a single component fails the entire application fails. 
- *Microservices* - application independent of each component, all loosely coupled and each have their own data storage system and each communicate with each other via queues/other comm mechanisms. Apps can be built as a collection of small, independent units each representing a certain business capability - one unit only does one job. Simplicity, flexibility, resilience due to decoupling, and agile - each component can be developed independently by different teams - however it is more complex to develop compared to monolithic, theres increased overhead since the decoupled components have to talk to one another, deployment complexity and monitoring/debugging can be more challenging.
- Seperating frontend, backend and db is not an example of microservices architecure. Having independent separate service for one business capability is an example of a microservice architecture. 

**Migrating from monolithic to microservices:**
- Assessment and Planning
- Decomposition 
- Service identification and design 
- Technology selection for each service - what tech stack and where it has to be deployed 
- Infrastructure setup - where the microservices are going to be deployed 
- Implementation 
- Data Management - to make sure each service is independent of each other - and to decide what type of data management tool has to be used for data 
- Integration and interoperability - check which services have to communicate and transfer data between each other 
- Testing and Validation 
- Deploy and monitoring 
- Incremental Rollout and Refinements - automation 
- Organizational alignment and culture. 
- ![[Pasted image 20251125112249.png]]

**Task:**
- Create 3 tier monolithic app - eg docker image that has everything run in a single ec2 machine 
- Decouple all the different parts and deploy as a microservice using docker/any other app 
- Look at service in GCP on migration of monolithic app to microservices. GCP Migrate to Containers. 

**Solution:**
- Monolithic architecture - three tier application in docker, web nginx image, db mysql, and backend flask with two routes - dbtime, and /. Orchestrate everything via docker compose and deploy 
- Microservices architecture - web: nginx serving html page, backend: greeting-service, time-service for the two routes in flask, db: greeting-db and time-db. (dedicated db for each service is required. )
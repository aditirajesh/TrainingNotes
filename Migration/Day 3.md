**Discovery Tools:**
- *Agentless Collector:* centralized tools/ scanning device that communicate with the systems over a network without requiring agent installations
- *Agent Collector:* Installing an agent on-premise/ where the application/servers live for scanning purposes. 

**Output of assessment phase:**
- current state analysis 
- workload assessment 
- risk and readiness assessment 
- cost estimation

**Landing zone:**
- creating initial infra for migration 
- service by aws - provides a starting point from which organizations can quickly launch and deploy workloads and applications. 
- from one root account can have a centralized network setup 
- enables you to connect ad to the identity provider - all users will be residing in the ad and all the identity providers will be in aws. 

**Wave planning:**
- organizing the migration process into waves or phases based on factors like: app dependencies, criticality, business and tech constraints 
- related servers are grouped together into one wave -> servers can't just be migrated randomly especially if dependencies are present between servers -> timeline is also set for each wave. 

**Server migration:**
- process of moving an organization's infrastructure from on-premise to another data center or cloud provider. 
- Why? : upgrading tech, cost savings, geographic expansion, hardware limitations or faults on the older servers. 
- How? : configure server, settings, permissions and authorizations -> ensure data is present correctly -> transfer data -> put the new server through quality assurance/validation -> switch dns to direct users to the new server. 

**AWS MGN:**
- go in the source server and install agent -> communicates with the target site in aws -> continuously replicates server data in target -> highly automated. 
- when changes are made in the source site there will be some incremental snapshots made in the target server to make track of the changes made (snapshots - in specific to volumes.)
- ![[Pasted image 20251127113754.png]]


**Cutover:**
- moment when the final switch is made from the old system to the new system
- Ensure: minimal downtime, rollback plan in case of failures, communication to stakeholders about the cutover timing and any expected impacts. 

**Disaster Recovery:**
- preparing for an event that might lead to system downtime - such as power outages, hardward failures, natural disasters etc and ensure applications are still available in case such an event occurs. 
- Why? : ensure business continuity, system security, prevent data loss, high availability, customer retention. 
- Types: backup (regular backup of data), disaster recovery as a service, virtualization (virtual version of physical resources) , disaster recovery sites (off-site locations to restore operations and data in case of disaster)
- Key metrics of DR plan - RPO, RTO
- Recovery point (RPO): backup data present right before the disaster. Point in time where the latest backed up data is available 
- Recovery time (RTO): after disaster has happened how long it takes to get the system to its normal functionality 
- ![[Pasted image 20251127115647.png]]
- Failover: primary source site unavailable -> have to switch to the backup 
- Failback: moving back to the primary site once it is up and running again. Move back is done since most users will be in the primary region. 
- AWS DRS: similar to MGN where agent does the replication from source to target -> has a failback replication service to achieve the same. 

**Task : Migrate a three tier architecture using amazon MGN**

Overview of architecture in region B:
![[Pasted image 20251128082452.png]]
![[Pasted image 20251128082502.png]]
![[Pasted image 20251128082522.png]]

bastion-sg-aditi:
inbound: ssh only on my ip, outbound:all

backend-target-sg-aditi:
inbound: ssh from bastion, icmp from web server, mysql from db on 3306 
outbound: all + mysql 3306 for db 

db-target-sg-aditi:
inbound:mysql by backend, outbound: all + mysql to backend sg 

web-target-sg-aditi:
inbound: http https all, ssh my ip
outbound: all

sudo wget -O ./aws-replication-installer-init https://aws-application-migration-service-us-west-2.s3.us-west-2.amazonaws.com/latest/linux/aws-replication-installer-init

sudo chmod +x aws-replication-installer-init; sudo ./aws-replication-installer-init --region us-west-2
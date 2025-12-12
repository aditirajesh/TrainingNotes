**What: Database Migration:**
- Involves moving data from one database environment to another. There should be no downtime of data and no loss of data as well + data integrity.
- Applicable for small as well as large scale businesses 
- Sometimes can also migrate from one db type to another 
- Two types of db: SQL and NoSQL db with different vendors. 

**Migration step 1: Database Assessment:**
- Need to take care of data as well as stored procedures, functions. Need to also migrate the database functions and the structure. 
- Database Assessment - How much data, how much real-time data is present is also analyzed. Involves evaluating the current state to identify issues and plan for migration. 
- DB Assessment Steps: Initial assessment (list all the databases and identify potential issues with the target env), DB health check, Reporting
- Tools for assessment: Azure Migrate, AWS schema conversion tool, google database migration service, etc. Third party tools: DBMaestro, Flexera. 
- ![[Pasted image 20251126111708.png]]

**Migration Step 2: DB Migration:**

- Database Migration - Pre-Migration planning (ensure a complete backup of the data is taken and confirm target env), Migration Execution (use the chosen tool to migrate the data and ensure validation is done to ensure no loss of data), Cutover (sync any remaining data from the source to the target and redirect app traffic to the new database.)
- After cutover the original database still aren't shut down to make troubleshooting in case of failures easier. 
- DB Migration Types: homogeneous and heterogeneous. Why? : because of the effort required for migration, risks and validations as well as the tools required for the migration will differ. 
- Homogeneous DB Migration: Both source and target db are of the same type. Generally simpler since the db structures and features are compatible across the environments. 
- Heterogeneous DB Migration: Source and target db are not of the same type. Data transformation and conversion needs to be done. Need to look into security models, db features that might not be available in the target, as well as the data type conversion, storing procedures, views, etc. 
- ![[Pasted image 20251126113047.png]]


**Migration Step 3: Post Migration Validation:**
- Ensuring that the migration was successful and the new db works as expected. Checks app functionality, performance, and data integrity. Reporting: documenting the migration process, logging issues faced and confirming db meets all the operational requirements. Also helps for audits and creates a knowledge base for future migrations
- ![[Pasted image 20251126113535.png]]
- Data integrity checks: compare row counts, checksums, spot checking key business tables for reference data. Performance testing: ensures the transactions performed before will be the same even after the migration
- Migration summary: contains all the steps followed during the migration, issues faced as well as the resolution. Validation report: includes all the performance tests 
- Compile findings: the final documentation is compiled and shared with all the stakeholders. 
- *Bin-Log replication* - any new transactions coming to db-A will come to db-B (db-A is the original database and needs to be replicated/moved to db-B): this eliminates the need for the db assessment tools. 
- A complete end-to-end testing of the db, application, features that will be used by the users are tested by the dummy users before migrating the actual users to the new database. 

**Database Version Upgrade:**
- Stay on the same db engine but move from the older to the newer version
- Why? : end of support for the older versions, security and compliance requirements that might be better in the newer versions, newer features and performance improvement. 
- Objective is to upgrade to the new version while ensuring compatibility and minimal impact on operations. 
- Pre-Upgrade Assessment: taking the full backup before starting the upgrade. Run compatibility checks using tools SQL Server Upgrade Advisor to scan the db for deprecated features, changes that might break during move and fetches compatibiltity issues that must be fixed before/after upgrade 
- Performing Upgrade: continuously monitor logs, and performance metrics and run the upgrade with the appropriate tool. Upgrade can also be done in phases to reduce risks 
- Post-Upgrade Validation: run queries to ensure the db functions as expected, and ensure all applications interfacing with the db are operational 


![[Pasted image 20251126115117.png]]

**Solution:**
1) Networking Setup:
   - Create a VPC with atleast three private subnets - one private subnet hosts the ec2 instance containing mysql (source db), the other two hosting the amazon aurora dbs (target db)
   - Create an rds subnet group and include atleast two private subnets present in two different availability zones. 
   - Create a security group for the ec2 instance -> add outbound rule all -> inbound rule from the dms_sg
   - Create a security group for the aurora instance -> add inbound rule for ec2 and dms sg on port 3306/5432. 
   - Create a security group for the dms replication instance -> no inbound rules as it is a managed service, and an outbound rule to all traffic/ to the ec2 and aurora sg specifically 
   - Make sure IGW and NAT gateways are configured with the proper routing for public and private subnets where the source and target endpoints will be hosted. 
   - Create a bastion in the public subnet with a public ip to ssh into the ec2 instance present in the private subnet. Make sure to edit the sg rules to allow ssh from the bastion host ip

2) EC2 Instance:
   - Manually install mysql/postgresql packages into the ec2 instances, create a dedicated user with a password for a database and populate it with some tables. 
   - In the postgres conf file, make sure that postgres is listening on port 5432 (change if mysql accordingly) for all traffic within the vpc subnet, not just local. This is what makes sure that postgres in the ec2 instance is listening on the port for connection with the replication instance 
   
3) Aurora RDS:
   - Create an amazon aurora RDS with postgres/mysql support - configure it with a username and password and make sure to create it in the same vpc the ec2 instance was created, except in a different subnet/az. Attach the aurora sg created previously
   
4) DMS:
   - Create a replication instance in dms and attach the dms sg to it, create it in the same vpc aurora and ec2 were created. Make sure it is in the public subnets. 
   - Create two endpoints - one source and one target. For the source endpoint, make sure to mention the user that has all privileges on the schema that needs to be migrated, along with the password and make sure the port is also correct. Since its an instance, the ssl certificate can be set to none. 
   - For the target endpoint, mention the master user and password and make sure to mention a database already created in aurora which all the tables will migrate to. 
   - Test these endpoints' connection with the replication instance. Once the connection is successful, you can move to creating a migration task. If this is successful, the migration is complete. 
1. What is Amazon RDS?
Amazon RDS is a managed relational database service that supports multiple engines like MySQL, PostgreSQL, MariaDB, Oracle, and SQL Server. It handles provisioning, patching, backup, scaling, and replication.

2. What database engines are supported by RDS?
MySQL
PostgreSQL
MariaDB
Oracle
Microsoft SQL Server
Amazon Aurora (MySQL & PostgreSQL compatible)

3. 3. What is Amazon Aurora? How is it different from other RDS engines?
Amazon Aurora is a proprietary engine developed by AWS. It offers 5x the performance of MySQL and 3x PostgreSQL. Aurora automatically handles replication, fault-tolerance, and high availability across multiple AZs.

4. How does RDS handle backups?
RDS supports:

Automated backups (daily snapshots + transaction logs)
Manual snapshots (user-initiated)
You can restore to any point-in-time within the retention period (1–35 days).

5. What is Multi-AZ deployment in RDS?
Multi-AZ is used for high availability. It replicates your DB synchronously to a standby instance in another AZ. During failover, RDS automatically switches to the standby.

6. What is Read Replica? How is it different from Multi-AZ?
Read Replica is for scaling reads (asynchronous replication).

7. Multi-AZ is for failover/HA (synchronous replication).

Can we take a backup of a read replica?
Yes, RDS allows taking snapshots of read replicas, which can also be promoted to standalone DB instances.

8. How does RDS handle failover?
In Multi-AZ, failover happens automatically to the standby instance in another AZ if the primary fails (e.g., due to maintenance, AZ failure, etc.).

15. How do you upgrade an RDS instance?
Use minor version upgrades (automated or manual)

Major upgrades must be initiated manually, and often require downtime

14. What is RDS Proxy?
RDS Proxy is a fully managed, highly available database proxy for RDS and Aurora. It improves application availability and performance by pooling connections and handling failovers more gracefully.

Connection pooling is a technique used to reuse existing database connections instead of creating a new one every time an application makes a database request.

13.Can you encrypt an existing RDS DB?
No, you cannot encrypt an existing unencrypted RDS instance. You must take a snapshot and restore it with encryption enabled.

0. Can you SSH into an RDS instance?
No, you cannot SSH into RDS. It’s a managed service — you access it via the database client/endpoint.



| Access Type      | Allowed? | Why                                                |
| ---------------- | -------- | -------------------------------------------------- |
| SSH into RDS     | ❌ No     | RDS is a managed service, AWS hides the OS         |
| DB Client Access | ✅ Yes    | Connect using DB endpoint, port, username/password |

1. How do you secure an RDS instance?
Use VPC Security Groups

Use IAM for access control

Enable Encryption at rest and in transit (TLS)

Use KMS for key management
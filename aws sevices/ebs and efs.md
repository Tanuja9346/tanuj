🔹 1. What is EBS in AWS?
Answer:
Amazon EBS (Elastic Block Store) provides persistent block-level storage for Amazon EC2 instances. It behaves like a hard disk and is used for storing OS files, databases, and application data.

What are the types of EBS volumes?
Answer:

| Type                      | Use Case                     | Performance          |
| ------------------------- | ---------------------------- | -------------------- |
| **gp3**                   | General-purpose SSD          | Up to 16,000 IOPS    |
| **gp2**                   | Previous generation SSD      | Baseline performance |
| **io2/io2 Block Express** | High-performance SSD         | Up to 256,000 IOPS   |
| **st1**                   | Throughput HDD (streaming)   | Big data, logs       |
| **sc1**                   | Cold HDD (infrequent access) | Archive storage      |

Is EBS region-specific?
Answer:
Yes, EBS volumes are AZ-specific. You cannot attach a volume in us-east-1a to an instance in us-east-1b.

4. Can EBS volumes be shared across instances?
Answer:
Yes, using EBS Multi-Attach (for io1/io2 volumes), but:

Only supported on Nitro-based EC2 instances

Requires special care to avoid data corruption

5. 5. How does EBS backup and restore work?
Answer:

Use EBS snapshots, which are incremental backups stored in S3.

Snapshots can be used to create new volumes or restore data.

🔹 7. What is the difference between EBS and Instance Store?

| Feature     | EBS               | Instance Store                           |
| ----------- | ----------------- | ---------------------------------------- |
| Persistence | Persistent        | Ephemeral                                |
| Detachable  | Yes               | No                                       |
| Use Case    | Long-term storage | Temporary data like cache, scratch space |

-----------------------
efs:
1. What is EFS in AWS?
Answer:
Amazon EFS (Elastic File System) provides shared, scalable file storage for Linux workloads that can be mounted from multiple EC2 instances simultaneously.

🔹 2. What are the EFS storage classes?

| Class                      | Description                          |
| -------------------------- | ------------------------------------ |
| **Standard**               | Frequently accessed files            |
| **Infrequent Access (IA)** | Lower cost for rarely accessed files |

3. Can EFS be mounted on multiple EC2 instances?
Answer:
Yes, it supports multi-AZ, concurrent mounts, making it ideal for web servers, container workloads, and shared data environments.

🔹 4. EFS vs EBS vs S3 – Differences?

| Feature   | EFS             | EBS                 | S3              |
| --------- | --------------- | ------------------- | --------------- |
| Type      | File System     | Block Storage       | Object Storage  |
| Mountable | Yes (many EC2s) | Yes (1 EC2)         | No              |
| Use Case  | Shared storage  | OS, DB, apps        | Backup, archive |
| Protocol  | NFS             | Attached via device | HTTPS/REST      |

.

🔹 8. How is EFS encrypted?
Answer:

Encryption at rest using AWS KMS.

Encryption in transit using TLS (enabled with mount helper).

9. EFS vs FSx – When to use what?
| Feature    | EFS                      | FSx (for Windows/Linux)              |
| ---------- | ------------------------ | ------------------------------------ |
| OS Support | Linux                    | Windows, Lustre, ZFS                 |
| Protocol   | NFS                      | SMB (FSx Windows), Lustre            |
| Use Case   | Web apps, shared configs | Enterprise apps, HPC, AD integration |

7. What are EFS Access Points?
Answer:

Access points simplify access control by defining POSIX user/group permissions.

Useful in multi-tenant applications (e.g., Lambda, ECS with Fargate).



Block storage doesn’t store this kind of metadata with each block. It only stores raw data, and it’s up to the file system (like NTFS, ext4) to organize and track which blocks belong to which files.

Metadata means information about the data — like:

Filename
Creation date
File type
Owner


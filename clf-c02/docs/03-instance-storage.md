Instance Storage
================

An Elastic Book Store Volume (EBS) is a network drive you can attach to your EC2 instances while they run. It allows your instances to persist data, even after their termination. They can only be mounted to one instance at a time at the Certified Cloud Practitioner level; one EBS can be only mounted to one EC2 instance associate level.

A good analogy is associate an EBS Volume as a network of USB stick. The free tier enable us 30 GB of storage for general purpose per month.

Keep in mind that an EBS Volume is a network drive(i.e., not a physical drive). It uses the network to communicate the instance, which means there might be a bit of latency. Also it can be detached from EC2 instance and attached to another one quicklu.

For other side, the EBS Volume is locked to an Availability Zone (AZ), so an EBS Volume in `us-east-1a` cannot be attached to `us-east-1b`. To move a volume across, you first need to snapshot it.

Lastly, you have a provisioned capacity. You get billed for all the provisioned capacity and you can increase the capacity of the drive over time.

The next diagram is and example of EBS Volume distribution:

![EBS Volume example](../assets/images/03A-ebs-volume-example.png)

It is important to keep in mind the delete on termination attribute. We can control the EBS behaviour when an EC2 instance terminates; by default the root EBS volume is deleted whe the attribute is enable. If it is disabled any another EBS volume is not deleted. They can be controlled by the AWS console and the use case is preserve the root volume when the instance is terminated.

> **About EBS Multi-Attach:** For volumes `io1` and `io2` we have a EBS multi-attach feature. However, from  an AWS Cloud Practitioner exam perspective this out of scope for the exam. In order to keep the course simple and accessible, I have left out this feature from the course. If you are curious to learn about EBS Multi-Attach, you will find it in the AWS Certified Solutions Architect Associate course, or in the AWS documentation.

EBS Snapshots
-------------

A snapshot is a backup of your EBS volume at a point in time. It is not necessary to detach volume to do snapshot, but it is recommended. With a snapshot you can copy them across a availability zone or region. The next image is an example of how we can use a snapshot in two regions.

[EBS Snapshot](../assets/images/03B-ebs-snapshots.png)

The snapshots have the next features:

- EBS snapshot archive allows to move a snapshot to an archive tier that is 75% cheaper. It takes between 24 to 72 hours for restoring the archive.
- Recycle bin for EBS snapshot allows to setup rule to retain deleted snapshots so you can recover them after an accidental deletion.

AMI Overview
------------

Amazon Machine Image (AMI) are customization of a EC2 instance. So, you add your own software, configuration, operation system, monitoring task and so on. An advantage is that the AMI have a faster boot because all your software is pre-packaged.

AMI are build for a specific region and they can be copied across regions. You can launch EC2 instances from three alternatives:

- A public AMI, that is provided by AWS.
- Your own AMI, and you have to created and maintain them yourself.
- An AWS marketplace AMI, that is an AMI that someone else made but it is potentially do for sells.

The AMI process for an EC2 instance is the next one:

1. Start an EC2 instance and customize it.
2. Stop the instance for data integrity.
3. Build an AMI; this will also create EBS snapshots.
4. Launch instance from other AMIs.

The below image summarize this steps:

![AMI Process](../assets/images/03C-ami-from-ec2.png)

EC2 Image Builder
-----------------

EC2 image builder is used to automate the creation of virtual machines or container images. With this service we can automate the creation, maintain, validate and test EC2 AMIs. It can be run on a schedule (e.g., daily, weekly, or whenever packages are updated). The next image summarizes the process the perform the image builder:

![EC2 Image Builder](../assets/images/03D-ec2-image-builder.png)

This is a free service and you only pay for the underlying resources.

EC2 Instance Store
--------------------

Remember that EBS volumes are network drives with good but _limited_ performance. If you need a high performance hardware disk, use EC2 instance store because they are better for input output performance. The EC2 instance store is ephemeral; it is that lose their storage if they are stopped. So we can use them for buffer, cache, scratch data or temporary content. Keep in mind that you are exposed to the risk of data loss if hardware fails. Then the backups and replications are your responsibility.

EFS Elastic File System
-----------------------

The elastic file system manages a network file system (NFS) that can be mounted on 100s of EC2. It works with linux EC2 instances in multiple availability zones. It is highly available, scalable, expensive, pay per use without planning capacity. Below it is an example of three EC2 instances consuming the same EFS filesystem:

![Elastic File System](../assets/images/03E-efs.png)

Let's outline the difference between EBS and EFS. In EBS we had a EC2 instance in one AZ; then the EBS volume can only be attached to one instance in one specific AZ. So, the EBS volumes are bound to specific AZ. If we want to move the EBS from one AZ to another, we should use an snapshot. Keep in mind that the snapshot is a copy and not a in-sync replica.

In the other side, the EFS is a network system that whatever is on the EGS drive is shared by everything that is mounted to it. That means that independently the number of instance that we had in different AZs, at the same time all these instances can mount the same EFS drive making a shared file system. The next image summarizes the difference between the two services:

![EBS vs EFS](../assets/images/03F-ebs-vs-efs.png)

There is a storage class you need to know for EFS and it's called EFS infrequent access or EFS-IA. This storage class is going to be cost-optimized for files that you don't access very often. So, for example, you don't access these files every day, then, this storage class will give you up to 92% lower cost for storing the data compared to the other storage class, which is called EFS standard. If you enable EFS-IA, then EFS will automatically move your files to EFS-IA, based on the last times they were accessed and something called a lifecycle policy.

Shared responsibility model for EC2 Storage
-------------------------------------------

As usual, shared responsibility is important going into the certified cloud practitioner exam. So we need to understand where is the responsibility for AWS and yours regarding EC2 storage. The next table summarizes these responsibilities.

|                        AWS                        |                        You                         |
|:-------------------------------------------------:|:--------------------------------------------------:|
|                  Infrastructure                   |      Setting up backup / snapshot procedures       |
| Replication for data for EBS volumes & EFS drives |             Setting up data encryption             |
|             Replacing faulty hardware             |      Responsibility of any data on the drives      |
| Ensuring their employees cannot access your data  | Understanding the risk of using EC2 Instance Store |

Amazon FSx
----------

Amazon File System eXternal (FSx) is a service to get third-party high performance file systems on AWS. So, in case you don't want to use EFS or S3, then you can use FSx to manage these file system and currently the offer is:

- FSx for Lustre
- FSx for Windows File Server
- FSx for NetApp ONTAP (out of the scope of this notes)

In the case of FSx for Windows file serve, you have a fully managed, highly reliable, and scalable _windows native_ shared file system built on Windows File Server and supports SMB protocol plus Windows NTFS. Also it is integrated with Microsoft Active Directory and can be accessed from AWS or you on premise infrastructure. The next image show how AWS FSx works with a Windows client over SMB.

![Amazon FSx for Windows](../assets/images/03G-fsx-for-windows.png)

For other side, the Amazon FSx for Lustre (Lustre is derived from "Linux" and "Cluster") is a fully managed high performance, scalable fire storage for **High Performance Computing**. The common uses are machine learning, analytics, video processing, financial modeling, etc. This service scales up to 100S GB/s, millions of input/output operations with sub-milliseconds latencies. The next diagram illustrate how FSx for Lustre works:

![Amazon FSx for Lustre](../assets/images/03E-fsx-for-lustre.png)

EC2 Instance Storage Summary
----------------------------

- **EBS Volumes:**  are network drives attached to one EC2 instance at a time that are mapped to an availability zones and can use EBS snapshots for backups and transferring EBS volume across availability zones.
- **AMI:** creates ready-to-use EC2 instance with our customizations.
- **EC2 Image Builder:** automatically build, test and distribute AMIs.
- **EC2 Instance Store:** for high performance hardware disk attached to EC2 instance that lost the data if the instance is stopped or terminated.
- **EFS:** network file system that can be attached to 100s of instance in a region.
- **EFS-IA:** cost optimized storage class for infrequent accessed files.
- **FSx:** service to get third-party high performance file system on AWS.

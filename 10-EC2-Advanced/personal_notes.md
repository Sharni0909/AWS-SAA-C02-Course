EC2 provides virtualizations as a service. It is a IaaS 

Enhanced Networking

Hypervisor in AWS - Nitro

EC2 instances are virtual machines ( OS + Resources ) 

E2 instances runs on EC2 Hosts ( these are physical servers) 

EC2 Hosts are shared or dedicated hosts 

Ec2 Host - will run in single AZ. 

EC2 is Availability Zone Resilient

EC2 have local hardware ( cpu and memory ) and also have local store ( called as instance store ) 

2 types of networking - storage and data networking 

elastic network interface is provisioned in a subnet which maps to physical hardware on EC2 host

instances can have multiple network interfaces even in different subnet but in same AZ

EBS - runs in an AZ and allocates volumes to instances of subnets in same AZ

**It is a network based storage product**

host,network and persistence storage is all AZ resilient 

Instances of same type and generation usually share same EC2 host

![image.png](attachment:30ad2ed5-bf9c-428f-a682-abde01e824fa:image.png)

![image.png](attachment:fec3437a-ad96-44d8-bb44-1791eb369386:image.png)

## EC2 Instance Types

![image.png](attachment:c0b78e87-472f-4cf7-9dac-485e55cf7aea:image.png)

Additional features and Capabilities - GPUs or FPGAs ( Field Programmable Gate Arrays - Special type of CPUs)

### EC2 Categories:

![image.png](attachment:8e6b860f-9dc1-4533-b2aa-00aee0a56551:image.png)

General Purpose

Compute Optimized 

Memory Optimized 

Accelerated Computing - GPUs and FPGAs

Storage Optimized - Scale I/), datawarehousing

![image.png](attachment:a106ca00-53ce-4736-8fa1-6659418232c0:image.png)

Additional Capabilities:

a- AMD cpu 

d - NVMe storage 

n - network optimized 

e - extra storage, cpu or memory

![image.png](attachment:3467e24b-4480-452c-bf5f-08c1b952bb26:image.png)

https://aws.amazon.com/ec2/instance-types/

https://ec2instances.info/

Storage Types

Direct or Local Storage - Instance store that is attached to EC2 Host 

Network attached Storage - EBS - Volumes delivered over the network by EBS

Ephemeral Storage - temporary storage - Instance Store 

Persistent storage - EBS

 3 storage types:

Block storage: Volumes presented to OS as collection of addressable blocks. Mountable and Bootable. Highly performant and physical storage for scalability

File storage: Presented as file share and has structure. Mountable but not bootable as it doesn’t have access to low level storage

Object storage: flat storage that is a container as S3. Key and value/object

### Storage Performance

IO or Block Size - size of blocks of data can be written to the disk. 16k,64k or 1meg

IOPS - IO operation storage system can accommodate per second

Throughput - rate of data a storage system can store 

IO(Block) size * IOPS = Throughput

## **EBS Architecture**

EBS is a block storage. it takes raw physical disks and presents allocations of physical disks as volumes and can be read using blocknumbers. these volumes are unencrypted and can be encrypted using kms

you can attach one EBS volume to multiple EC2 instance as well

volumes can be detached and reattached and it has its own life cycle

EBS can provision volumes based on different physical storage types ( SSD, high performance SSD, mechanical types ), different storage sizes and different performance profiles

billed based on GB-month

![Screenshot 2025-07-13 at 3.11.38 PM.png](attachment:4469bb42-dd8f-4713-89e8-8de77accf098:Screenshot_2025-07-13_at_3.11.38_PM.png)

## EBS Volume Types

**General Purpose SSD - GP2**  
It is used for boot volumes, low latency interactive applications like dev/test

any volume less than 33.3 gb gets 100 IOPS as default and anything higher gets 

3*GB size + 100 IOPS per second. 

Can burst 3000IOPS per second irrespective of credits available in the bucket 

Above 5 tb - it gets 16000 IOPS

![image.png](attachment:9425f530-da6a-4a29-9444-69a253335034:image.png)

General Purpose SSD - GP3

3000 IOPS + 125 MiB/s is standard for all volume sizes 

volume can range based on the size

High performance and high throughput than GP2 and is also cheaper 

GP2 Max throughput is 125MiBPS vs GP3 is 1000MiBPs

can be used for virtual desktop,low latency application, booting volume

![image.png](attachment:baaf4b35-1fe1-47f9-90a3-2cfe68b651b0:image.png)

**Provisioned IOPS - SSD ( io1/2)**

you can provision IOPS irrespective of volume size but have to pay price for provisioned IOPS along with volume size 

64000 IOPS per volumes - 4x GP3

upto 1000 MB/s throughput

3 types 

io1 - 4Gb to 16GB io1/2

io2

Block Express - 64GB is maximum

Each instance has limitations 

io1 - 260K IOPS & 7500 BM/s

io2 - 160K & 4750 MB/s

io2 Block Express 

![image.png](attachment:0c8a2bba-526c-4d5f-a209-1e4c6d6a0975:image.png)

**HDD**

It is a physical storage volume and provides low performance.

2 types 

st1 - cheap and is used for datawarehouse , big data

Max 500 IOPS, 250 MBs/TB

sc1 - very cheap and is mainly used more like backups/archives

Max 250 IOPS , 80Mbs/TB

![image.png](attachment:809ca535-4d93-4020-b8fa-5d23b19a7d81:image.png)

**Instance Store Volumes:**

Block storage devices, local to EC2 host 

they are ephimeral/temporary storage 

High performance and throughput than EBS 

EC2 instance can have instance store volumes and need to be attached at launch time only 

cannot attach instance volumes later 

price of an instance includes instance store volumes 

when the ec2 instance resizes , moves to a different ec2 host or fails, instance store volume and the data in it is lost

Instance Store vs EBS 

Use EBS:
Perisistent storage or resilient 

Storage is isolated from instance

EBS:
Resilient w/in- app backup storage - depends can be EBS or Instance store

High Performance - can be both

Super High Performance - Instance store

Cost - less for instance store

EBS 

Cheap - St1 or Sc1

Streaming or throughput - ST1

ST1 and SC1 cannot be used for booting 

Boot - Gp2/Gp3

GP2/3 - 16000 IOPS Max

Io1/Io2 - 64000 IOPS MAx 

Block Express - 256000 IOPS

Max of 260K for EBS 

More than 260K - should go for Instance store

![image.png](attachment:d7305af7-28a2-40dc-87cf-1e29a5745f34:image.png)

### EBS Snapshots

EBS volume snapshots are stored in S3 and they are region resilient.

First Snapshot is a full backup and the next ones are all incremental snapshot 

Incremental Snapshots are self sufficient so that if one incr is deleted the date gets moved to 

next increments before deletion 

Charged only for the used storage for snapshots vs ebs is charged for allocated storage 

New EBS = High performance 

EBS created from snapshots are low performance and lazy loading 

It reads from snapshots to fetch data that is not loaded yet on EBS 

you can perform force read of all data immediatedly called FRS ( Force Read Snapshots) which will be immediate 

50 FRS per region 

Instance restart - it will be on same Ec2 Host and as Instance store volumes are attached to EC2 Host.. restarting instance will not loose data but if we stop/start EC2 instance will be now connected to different EC2 host and the data is lost

## EBS Encryption

when a volume is created - you can add encryption to the volume 

encryption can be aws managed kms key - aws/ebs

or custom managed key 

KMS keys are used to generate encrypted data encryption key ( DEK) 

Each volume will have 1 DEK or encryption key generated 

When the volume is empty or an instance is launched, Ec2 host decrypts the data encryption key 

and store the key in memory of EC2 Host 

when ec2 instances stored the data in ebs volume - it goes through ec2 host and ec2 hosts encrypts/decypts the data and store the encrypted data in EBS

Snapshot will store encrypted data and new volume created will also share same key as it has encrypted data 

when the ec2 instance get start/stop, decrypted key on ec2 host will be lost 

OS itself isn’t aware of encryption as encryption is happening b/w ec2 host and ebs 

it does using AES-256 

If OS needs to do encryption then we need to configure volume encryption called as Software disk encryption 

you can use both EBS encryption and software disc encryption but doesn’t make sense

no cost for kms key on ebs 

## Network Interfaces, Instance IPs and DNS

Ec2 Instance will have 1 primary Elastic Network Interface 

Can have 0 or more seconday ENI 

ENI attributes → 

It has MAC Address that is  hardware address of the interface and it is visible inside  OS 

Primary private IPv4 address which is associated to DNS 

0 or more secondary IPs

0 or 1 public IPV4 address → has its DNS but DNS will route to private IPV4 address inside VPC and public IP address outside VPC

NAT/IGW takes care of associating priavte and public IPV4 address

0 or more IPV6 address

1 elastic IP address per 1 private IPV4 address

When an elastic IP address is associated, it removes public IPv4 address 

If elastic IP address is removed then a new IPv4 address is associated 

Security groups can be attached to ENI → this will effect all IPs associated to ENI

so if we want to have multiple IPs for an ENI then use multiple security groups 

Source/Destination Check → if it is enabled Ec2 instance act as NAT instances and check if the source and destination address are matching if not traffic is discarded 

![image.png](attachment:d5c76f45-3af2-4e6c-856d-e78af39d8da3:image.png)

Secondary ENI → same attributes as above. Only difference you can detatch and attach to different EC2 instances 

ENI + Mac = Licensing

Many legacy softwares installed on secondary ENI can detach and attach to ec2 instances so the license can be moved across 

![Screenshot 2025-07-26 at 4.46.19 PM.png](attachment:3a78ac2c-10b4-41e5-95a0-35517ab79e52:Screenshot_2025-07-26_at_4.46.19_PM.png)

## Amazon Machine Image

AMI is a container and doesn’t have any data

they are the images of EC2. like a template and can use this template to create multiple EC2s 

we can launch EC2s using amazon provided amis or community provided by us 

When a Marketplace AMI is used to launch an instance, cost includes instance cost + any additional AMI cost related to software licensing)

![image.png](attachment:fe916686-32e3-485d-ae26-28dd1bab044b:image.png)

AMI Lifecycle has 4 phases

1. launch: use AMIs to launch EC2 instance

EBS are attached to EC2 instances using device Id which is unique

configure:

create image : Instance + Volume + data 

when an AMI is created, first snapshots are taken of volume and snapshot is referenced in the AMI known as **block device mapping (** table of data: link snapshot ID to the volume /device ID)

launch: when an ec2 instance is created, snapshots are used to create ebs volumes and attached to the ec2 instances using same device Ids stored in block device mappings

![image.png](attachment:1226d1a5-003e-4783-8531-3c62d80aa263:image.png)

### Important Notes

AMI → One Region

AMI Baking → create an AMI from a custom configured ec2 instance + application

AMI cannot be edited

Can be copied b/w regions ( including snapshots)

![image.png](attachment:2e2e5424-29f8-4f1a-93a8-91f77ab26a15:image.png)

Billing: AMI contains EBS snapshots so cost will be incurred for snapshots

## EC2 Purchase Options

They are nothing but launch types

### Default - On Demand

It is avg of anything. 

Instances of different sizes of different customers share resources and they can be isolated 

Per second billing of an instance running + additonal associated resources such as storage

**Usecases:**
No interruptions 

no capacity reservations . If the capacity is limited, reserved capcity purchase iption receives priority 

it offers predictable pricing and no discounts

no upfront costs

Short term or unkown workloads 

### Spot Pricing

Cheapest option to get access to EC2 capacity as AWS is selling spare capacity at discouted prices

AWS provides 90% discounts on spot pricing 

You will only every pay current spot price irrespective of customers maximum spot price.

If the current spot price is more than the customers max spot price then the instances gets terminated 

Spot instances are not reliable as on-demand instances are given high priority 

Usecases:
Can tolerate interruptions for workloads and can rerun 

bursty capacity need - image processing

anything which is stateless as state is not stored on instances and disruption is okay 

### Reserved

long term consistent usage of EC2

If there’s no reservation then customer pays full price per/second 

If there’s a reservation for matching instance - you will pay reduced or no price/second 

You can make reservations in an AZ which will provide capacity as well 

You can make reservatios per region which will not provide capacity 

Reservations can be done for 1 yr or 3yrs ( it has more discounts)

3 different payment options:

All are for 1 or 3 yrs
No upfront cost, pay reduced price/second whether instance is being used or not 

some upfront cost + lower price/second 

all upfront cost and no price/second  

![image.png](attachment:3a4b02ad-d3ea-4d9c-9435-17d4163d0ee3:image.png)

Standard Reservations:

cheapest ec2 running 24/7/365 for 1 or 3 years

### Capacity Reservation

when there is a failure, the ec2 instances with capacity reservations are given high priority , second on demand and then spot instances. so capacity reservations should be used to use instances without any interruptions

3 types:
Regional Reservation: Across a region ( multi- AZ), no capacity reserved but discounted price.Min 1 yr or 3 yr term

Zonal Reservations: Within one A - discounted price and capacity reservations. Min 1 yr or 3 yr term

On demand Capacity Reservation: Capacity is reserved with full price on-demand ; no discounts and no term limit

![image.png](attachment:93f2eeea-adc1-4ceb-8426-b10587422868:image.png)

### Scheduled Reservations

long term usage but only needed for certain schedules 

cheaper than on-demand as capacity is reserved for only scheduled time 

Batch Processing or Financial data analysis report /week

**Restrictions**: doesn’t support all instaces or regions; min of 1 year and 1200 hours /year 

## Dedicated Host

EC2 Host is dedicated to you. Pay for the host itself and it is of A,C,R and it has all resources that is expected of physical machine 

memory,I/O, network etc

EC2 Host - capacity management has to be done 

![image.png](attachment:3aa7cf8d-de5b-4fda-a03e-ad3fe78b3338:image.png)

Usecases:
Licensing based on sockets/cores

### Dedicated Instances

With the dedicated instances, they are launched in an ec2 host of the same customers but is not shared with other customers 

don’t own or share hosts

No capacity management required and no dedicated host costs but have to pay one time upfront cost per hour usage + instances cost price/second 

![Screenshot 2025-07-29 at 6.58.19 PM.png](attachment:32f49e3c-938c-4e28-a531-1c6bda834a28:Screenshot_2025-07-29_at_6.58.19_PM.png)

## EC2 Savings Plan

It’s an hourly commitment for 1 or 3 yrs 

general compute reservations which provide access to compute resources -ec2,fargate and lambda is 66% cheaper than on demand costs 

ec2 savings plan -only ec2 with flexibility on os is 72% cheaper than on-demand

if hourly commitment is 20$ then upto 20$ it is cheaper for compute after that it is on-demand costs

EC2 Auto Recovery

Auto recover works for an instances that has ebs volume but not instance store

it could simply restart or create new instance in new ec2 host

## Horizontal and Vertical Scaling

Vertical Scaling: Increase the size of an instance 

as the size increases -prices increases very mcuh as the larger instances have premium costs

when the instance size increases - it reboots which will disrupt the application temporarilu 

there is a cap on instance size increase 

Horizontal Scaling: Increase no of instances 

More granular and less expensive 

As the instances increases, user request may go to any instances through Load balancer and has sessions are an issue so need to have application support or off-host session ( externally maintained in a db)

No disruptions and no limitation on no of instnces

## Instance Metadata

EC2 service provides all information/data about instances 

It access all instances 

http://169.254.169.254/latest/meta-data

It provides information of instances such as 

Environemnt: service, host details

Netwroking: Using ec2 service is the only way to find public ipv4 address of an instance 

authentication: how to access the service; can be used to connect to ec2 instance using ssh keys

user data 

cannot be authenticated or encrypted - can restrict access to 169.254 ip

# Advanced EC2

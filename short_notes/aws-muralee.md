What is Cloud Computing
- Provids services like networking , computes, storage, dbs over the internet
- instead of maintainning physical H/W or infra locally let's can manage provides the services to access users on-demmand over the internet 

Benefits of Cloud Computing:
- Sclable
- Cost Efficiency[pay-as-you-go]
- Flexibility[access resources anywhere with an internet connection]
- Reliability[provides high avaialbiltiy]
- Security[follow robust securtiy rules]

Types of Cloud Computing:
- Public Cloud[aws,azure,gcp..]
- Private Cloud[on-premises]
- Hybrid Cloud[public+private]

Types Of Cloud Services:
1. Iaas
- responsible to managing and maintain softwares and applications deployment
- For example as endusers we are not bother about
  - Networkig,Storage,Servers,Virtualization vendor take care
  - OS,App Runtime, applications we take care
2. Paas
- No need to worry about OS,Middleware,Runtime vendor will take care
- beanstalk
- lambda
- gateway
- ecs
3. Saas
- Fully managed service we just use what they provide

region: geographical location
az's: actual datacenters are there

EC2: Create servers offers resizable compute capacity
- scalbility
- variety of offers instances
- ensure secuirty using sg and key-value pair
- integrations with other services
- flexible pricing

Types of instances:
1. General purpose[t,m]
2. Compute Optimized[c][C5]
3. Memory Optimized[r,x1][R5,R4]
4. Storage Optimized[d,i]
5. GPU
6. FPGA

/dev/xvda = amazon linux
/dev/sda1 = ununtu

status checks:
1. System Status Checks
2. Instance Status Checks
3. Volume Status Checks

EIP = is astatic IP which is not change for example you instance on every reboot public IP change so if we attach EIP on reboot also IP change
Can we Attach EIP to spot instance?

1. Spot instances
2. On-demand[used]
3. Saving Plans[Actually we were utilizing saving plans]
4. RI
5. Dedicated Hosts
6. Dedicated Instances
7. Scheduled Instances
8. Capacity reservations

Regarding issue with ec2 what you do
- opening aws ticket
- requesting service quotas[improve count of vpc's,EIP,instances]
- To raise requests related to IAM you can only do it from north virginia
- 99% we were using support plan

- User data runs only once during the first EC2 boot. If you stop and start the instance or update the script, it won’t run again.
- To run scripts on every change, we use `null_resource` with triggers like file hash. It re-executes the script when the content changes.

EBS
---
- EBS volumes are highly avialalble relable persistent volumes to store data permanently we were using EBS volumes
features: elasticity,durability,snapshotting,encryption,availability

Volumes Types:
1. General Purpose[gp2,gp3]
   gp2 = defaut volume it supports only throughput not iops
   gp3 = less cost compare to gp2 and iops configurable
- WHat type of volume do you use currently? we are using gp2 planning migrate to gp3 
2. Provisioned IOPs[io1,io2]
3. ThoughPut optimized[HDD][st1]
4. Cold HDD[sc1]

volumes are az bound means instances in same az instance only can be attachable 

in real time 99% we did not use KMS to encrypt the data

Mount the volume process:
- blkid
- lsblk
- sudo file -s /dev/xvdf
- mkfs -t xfs /dev/xvdf
- yum install xfsprogs
- mkdir data
- sudo mount /dev/xvdf /data

I have EBS volume modify volume 400GB to 500GB after again modify volume from 500Gb to 600GB it is possible you have to wait for 6 hours for second volume modify

Best way to detach the volume first login to the server and unmount it then detach otherwise may be data corruption happends withou unmount detach

snapshots are point-in-backup of EBS volumes which are incremental what ever changes in previous snap shot those only backup
When you do snapshot it will take entire state of volumes[data,config,setting]
snapshots are stored in s3
for backups and DR we mainly keep take the snapshots

In realtime we have 100GB ... 1000GB ... 1TB

If take snapshot of encrypted volumes does it encrypted or not
If you take non-encrypted volume how can you make encrypted

AMI is a preconfigured template that has OS,settings,app
By using template we can run as many instances we want it brings immutability
If we want to move AMI to another region we need to first take snapshot then copy snapshot to another region

instance store[temporary] = comes with ec2 instance when you restart ec2 instance

Amazon Data lifecycle = automatic creation,retention copy and deletion of snapshots  and AMI's 
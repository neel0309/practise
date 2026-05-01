
| Scenario                                                            | Single-AZ | Multi-AZ standby | Multi-AZ cluster (1 writer + 2 readers) | Aurora Serverless |
|---------------------------------------------------------------------|-----------|------------------|-----------------------------------------|-------------------|
| Personal blog, 200 visits/day                                       | x         |                  |                                         |                   |
| Hospital OT scheduling app, low traffic but zero downtime tolerated |           | x                |                                         |                   |
| E-commerce site, read-heavy, traffic spikes on Fridays              |           |                  | x                                       |                   |
| Internal report tool, used 30 min/day, idle the rest                |           |                  |                                         | x                 |
| Discord-like product needing millions of small DBs                  |           |                  |                                         | x                 |

`Personal blog, 200 visits/day`  ->  this is a personal blog so it doesnt needs Multi AZ as 
there is no priority.Low traffic. Less cost

`Hospital OT scheduling app, low traffic but zero downtime tolerated` -> As it is critical to keep the application
up and running we need Multi AZ.Low traffic so no need of Read replicas.

`E-commerce site, read-heavy, traffic spikes on Fridays` -> Read heavy so 2 readers. It scales as well            

`Internal report tool, used 30 min/day, idle the rest` -> When it is idle Aurora will scale down                                       

`Discord-like product needing millions of small DBs` -> Aurora supports Multi tenant so isolation.

# Why is a database fundamentally a *single-writer* system by default? What does "sharding" solve and why is it usually done at the application layer, not the infra layer?

Database fundamentally a *single-writer* by default because of the ACID properties.
When two writes happens simultaneously DB needs to decide which writes will go first and also
needs to make sure that both writes are successfully and not one write overites
other (Consistency).

Sharding is splitting of data into smaller pieces called shareds.
so DB dont have to handle large data as sharding makes the data into smaller
pieces which require less IO and also helps to reduce the load of the CPU.

Application is aware of which shards a query belong to and DB is not aware of it 

# Look at the storage type options (gp2, gp3, io1, io2, magnetic). 
# Explain in your own words: for a 100 GB database getting hammered with writes, why does the disk matter more than CPU/RAM? 
# What does IOPS mean and why does it scale with disk size?

```
gp2 --> IOPS tied to size
gp3 --> we can choose iops independently so not dependency on the size
io1/2 --> High-performance, provisioned IOPS
magnetic --> slow outdated 
=========
IOPS stands for  Input/Output Operations Per Second
It is number of read and writes operation our disk can handle per second.
The same way gp2 was made iops * size. More the size of the DB more the iops

```
# What is connection pooling? If your app makes 1,000 concurrent DB connections at ~10 MB each, what's the rough memory cost on the DB server? How does a 50-connection pool change that math?

````
A connection pool sits between the application and DB. It keep a fixed number of connections to DB.


Without pooling:
1,000 connections × 10 MB each = 10,000 MB = 10 GB RAM
1,000 connections × 10 MB = 10,000 MB = 10 GB RAM

With pooling:
50 connections × 10 MB = 500 MB RAM

So we can avoid the RAM usage with pooling
````

# Look at the encryption at rest option. AWS-managed KMS key vs customer-managed KMS key vs CloudHSM-imported key — when would you choose each?

````
AWS-managed KMS key --> AWS creates, owns, rotates and manage the keys.
The cost is less. 
When we trust AWS and dont have a security team of our own

customer-managed KMS key --> We create and own the key in AWS KMS.
Little higher in the cost than AWS-managed KMS key.

CloudHSM-imported key --> You provision a dedicated CloudHSM cluster, and KMS is configured to use your HSM as the key store. 
Highest cost.
When we are working in a domain which needs us to maintain the keys
of our own.
````

TODO:

# Look at the routing policy options — Simple, Weighted, Latency, Failover, Geolocation, Geoproximity, Multivalue, IP-based. For each, give a one-sentence real use case. The transcript explicitly assigns this as homework.

````
Simple --> When we have a single endpoint.
Weighted --> When we plan to roll out a new version of our application and
we need to route 10% to v2 and 90% to v1.
Latency --> When we want to direct the users to low latency region. Example 
Mumbai -> ap-south1
Failover --> when we want to send traffic to DR Region when primary region
gores down
Geolocation --> when we want users to view region specific content.
Geoproximity --> When we want to shift users toward a cheaper or less loaded
region.
Multivalue --> Return multiple healthy IPs for basic load balancing with health checks
IP-based --> Route traffic based on the IP ranges.
````
# Why is CPU-based scaling alone often the wrong policy for production? Give two scenarios from the transcript (Uber-pattern, Medium-pattern, Black Friday) where you'd want a different approach.

````
CPU scaling reacts to compute saturation and it stays low in other production issues like
like I/O, queues, or external dependencies.

Uber-pattern - Spikes of events pile up in queues/streams. Workers may be mostly waiting on I/O or external APIs → CPU stays low.
we should put scaling on requests-per-second to drain the queue and keep latency bounded.

Black Friday - Massive, known-in-advance spike
we should Scheduled / predictive scaling or pre warm as when the traffic comes 
we are prepared.
````
# What is a scheduled scaling policy and when does it beat dynamic scaling?

````
A scheduled scaling policy is when we pre define exact times to increase or decrease capacity
instead of reacting to metrics like CPU.
It beats dynamic scaling on the below pointers:
1. Predictable traffic patterns
2. Sudden spikes

````
# Explain the cooldown period and why a 100-second warmup matters (hint: think about what an instance is doing during user_data).

````
A cooldown period is basically a buffer time after a scaling action during which Auto Scaling waits before making another decision.
When your Auto Scaling Group launches a new EC2 instance, it doesn’t immediately treat that instance as ready rather it waits till the 
wwarm up completes.

Why the 100-second warmup matters :

1. OS boots
2. packages install
3. Gunicorn / app server starts
4. app becomes reachable on port 8000
````
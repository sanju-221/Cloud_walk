# 1. What is EC2?
Amazon EC2 (Elastic Compute Cloud) is AWS's virtual machine service. It allows you to run virtual servers (instances) in the cloud within minutes.

```text
Physical Server (Traditional)       EC2 Instance (Cloud)
──────────────────────────────       ────────────────────────

Buy hardware (weeks/months)          Launch in minutes
Fixed CPU, RAM, storage              Choose instance type
Physical location                    Any region globally
You maintain hardware                AWS maintains hardware
Pay even when idle                   Pay per second/hour
```
Key EC2 capabilities:

- Launch virtual machines with various OS (Linux, Windows, macOS)
- Choose CPU, memory, storage, and networking capacity
- Scale from 1 to thousands of instances
- Full control of the OS and applications
- Stop/start/terminate on demand
_____________________________________________________________________________________

# 2. EC2 Instance Types
EC2 instance types follow a naming pattern:
```text
Instance Name:  m5.xlarge
                │ │ └── Size
                │ └──── Generation (5th generation)
                └────── Family (m = general purpose)
```
Instance Families: 
| Family | Type	| Use Cases |
|---|---|---|
| t	| General Purpose (burstable)|	Web servers, dev/test, small DBs|
| m	| General Purpose (balanced)|	App servers, gaming, enterprise apps|
| c	| Compute Optimized	| High-performance web, batch processing, ML|
| r	| Memory Optimized	| In-memory databases, real-time analytics|
| i	| Storage Optimized	| High I/O databases, data warehouses|
| p/g | GPU Instances	| Machine learning, video rendering|
| x	| Memory Optimized (extreme)| SAP HANA, large in-memory DBs|

Common Sizes (smallest to largest)
```
nano → micro → small → medium → large → xlarge → 2xlarge → 4xlarge → ...
```
Most Common for Learning
| Instance	| vCPU	 | RAM	| Use Case |
|---|---|---|---|
| t2.micro	| 1	| 1 GB	| Free tier, learning |
| t3.micro	| 2	| 1 GB	| Free tier eligible |
| t3.small	| 2	| 2 GB	| Small web apps |
| t3.medium	| 2	| 4 GB	| Moderate workloads | 
| m5.large	| 2	| 8 GB	| Production web servers |
| c5.large	| 2	| 4 GB	| CPU-heavy workloads |
| r5.large	| 2	| 16 GB	| Memory-heavy workloads |

>For the exam/interviews: T instances are "burstable" — they earn CPU credits when idle and spend them during spikes. M instances >are always-available balanced. C instances are for compute-heavy work.
_________________________________________________________________________________________________________________________
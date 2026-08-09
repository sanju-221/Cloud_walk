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

Key EC2 capabilities:

- Launch virtual machines with various OS (Linux, Windows, macOS)
- Choose CPU, memory, storage, and networking capacity
- Scale from 1 to thousands of instances
- Full control of the OS and applications
- Stop/start/terminate on demand
_____________________________________________________________________________________

# 2. EC2 Instance Types
-------------------------------------------------------------------------------------
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
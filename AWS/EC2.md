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

# 3. Launching an EC2 Instance
Via AWS Console
Step 1: Go to EC2 → Instances → Launch instances

Step 2: Choose an AMI (Amazon Machine Image) — the OS template

- Amazon Linux 2023 (AWS-optimized, free)
- Ubuntu 22.04 LTS
- Windows Server 2022

Step 3: Choose Instance type (t2.micro for free tier)

Step 4: Configure Key pair — needed for SSH access

Step 5: Configure Network settings

- VPC (default VPC is fine for learning)
- Subnet (any)
- Auto-assign public IP: Enable
- Security group: Allow SSH (port 22) from your IP

Step 6: Configure Storage (8 GB default is fine)

Step 7: Review and Launch

## Via AWS CLI
```
# Launch a basic EC2 instance
aws ec2 run-instances \
  --image-id ami-0c55b159cbfafe1f0 \     # Amazon Linux 2 AMI (us-east-1)
  --instance-type t2.micro \
  --key-name my-key-pair \
  --security-group-ids sg-xxxxxxxx \
  --subnet-id subnet-xxxxxxxx \
  --associate-public-ip-address \
  --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=my-first-ec2}]'

# List running instances
aws ec2 describe-instances \
  --filters "Name=instance-state-name,Values=running" \
  --query 'Reservations[*].Instances[*].[InstanceId,InstanceType,PublicIpAddress,Tags[?Key==`Name`].Value[]]' \
  --output table

# Stop an instance
aws ec2 stop-instances --instance-ids i-1234567890abcdef0

# Start an instance
aws ec2 start-instances --instance-ids i-1234567890abcdef0

# Terminate (delete) an instance
aws ec2 terminate-instances --instance-ids i-1234567890abcdef0
```
## Finding the Right AMI ID
```
# Find latest Amazon Linux 2023 AMI for us-east-1
aws ec2 describe-images \
  --owners amazon \
  --filters "Name=name,Values=al2023-ami-*" "Name=architecture,Values=x86_64" \
  --query 'sort_by(Images, &CreationDate)[-1].ImageId' \
  --output text
```
# 4. Key Pairs and SSH Access
## What is a Key Pair?
A key pair is an asymmetric encryption pair used for secure access to EC2:

- Public key: AWS stores this and puts it in the instance at launch
- Private key: You download once and keep safe — if lost, you cannot SSH in
## Creating a Key Pair
Console: EC2 → Key Pairs → Create key pair

- Format: .pem for Linux/macOS SSH, .ppk for Windows PuTTY

## CLI
```
# Create key pair and save private key
aws ec2 create-key-pair \
  --key-name my-key-pair \
  --query 'KeyMaterial' \
  --output text > my-key-pair.pem

# Set correct permissions (required!)
chmod 400 my-key-pair.pem
```
## SSH into Your Instance
```
# Connect to Amazon Linux / RHEL
ssh -i my-key-pair.pem ec2-user@<public-ip>

# Connect to Ubuntu
ssh -i my-key-pair.pem ubuntu@<public-ip>

# Connect to Debian
ssh -i my-key-pair.pem admin@<public-ip>

# Using EC2 Instance Connect (browser-based, no key needed)
# Console: EC2 → Instances → select instance → Connect → EC2 Instance Connect
```
> Permission errors? The most common SSH issue is wrong key file permissions. Run chmod 400 my-key-pair.pem and try again.

# 5. EC2 User Data
User Data is a script that runs once when an EC2 instance launches for the first time. Use it to bootstrap — install software, configure settings.
```
EC2 Launch
    │
    ▼
┌──────────────────────┐
│  User Data Script    │  ← Runs as root, one time only
│  (bash script)       │
└──────────────────────┘
    │
    ▼
Instance is ready with software pre-installed
```
## Example: Launch EC2 with Nginx Installed
In the console under "Advanced details" → "User data", or via CLI:
```
#!/bin/bash
# Update packages
yum update -y

# Install Nginx
amazon-linux-extras install nginx1 -y

# Start and enable Nginx
systemctl start nginx
systemctl enable nginx

# Create a simple page
echo "<h1>Hello from $(hostname)</h1>" > /usr/share/nginx/html/index.html
```
Passing User Data via CLI
```
# Save the script to a file
cat > user-data.sh << 'EOF'
#!/bin/bash
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd
echo "<h1>Hello World from $(hostname)</h1>" > /var/www/html/index.html
EOF

# Launch instance with user data
aws ec2 run-instances \
  --image-id ami-xxxxxxxxx \
  --instance-type t2.micro \
  --key-name my-key-pair \
  --user-data file://user-data.sh
```
> Debugging User Data: Check logs at /var/log/cloud-init-output.log to see if the script ran successfully.
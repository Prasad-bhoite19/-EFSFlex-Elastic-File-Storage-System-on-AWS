# 🚀 EFSFlex — Elastic File Storage System on AWS

A Next-Gen, Fully Diagrammatic, Modern & DevOps-Ready EFS Architecture Guide

Author: Prasad 👨‍💻

-----

## 🔥 Overview — What This Repo Delivers:-

This guide gives you a complete understanding of AWS EFS:

- ✔ Multi-AZ shared file storage
- ✔ Ideal for EC2, Auto Scaling, ECS, EKS
- ✔ Scales automatically — no provisioning
- ✔ POSIX-compatible file system
- ✔ 99.999999999% durability
- ✔ Perfect for: web servers, CMS, shared logs, DevOps tools

You will find:

- 📦 Architecture diagrams
- 🏗 EC2 + EFS workflow
- 🛠 Step-by-step creation
- 🔐 Security best practices
- 💡 Troubleshooting
- 🧪 Testing commands
- 🌟 Future enhancements

--------------------------------------
## 🧬 1. EFS Security

- Allow NFS TCP 2049 only for required EC2 instances
- Enable encryption at rest
- Use IAM policies for access control
- Access Points enforce POSIX permissions

--------------------------------------

## 🧭 2. ASCII Architecture (Simple View):-
```
         Users
           |
    ----------------
    |      |       |
  EC2-A  EC2-B   EC2-C      (Multiple AZs)
    \      |      /
     -------|-------
            |
      EFS Mount Targets
            |
      -----------------
      |     Amazon    |
      |      EFS      |
      -----------------
```
--------------------------------------

## 🔧 3. EFS Components Explained:-

- 📌 EFS File System
Auto-scaling, managed NFS storage.

- 📌 Mount Targets (per AZ)
Ensures EC2 instances in each AZ can access EFS.

- 📌 EFS Access Points
Provide isolated, controlled entry paths.

- 📌 Security Groups
Allow NFS traffic TCP 2049.

--------------------------------------

## 🛠 4. Step-by-Step: Create an EFS File System (Console):-

### Step 1: Go to AWS Console → EFS → Create File System:-

- Choose VPC
- Enable Automatic Backups

Performance mode:
- General Purpose (default)
- Max I/O (for massive scale)
  
### Step 2: Create Mount Targets:-

- Must create one per AZ
- Choose subnets
- Attach security group allowing port 2049

### Step 3: Attach to EC2 Instances:-

Install NFS client:
```
sudo yum install -y amazon-efs-utils   # Amazon Linux
sudo apt install -y amazon-efs-utils   # Ubuntu
```
### Step 4: Mount EFS:-
```
sudo mkdir /mnt/efs
sudo mount -t efs fs-12345678:/ /mnt/efs
```
### Step 5: Make it Permanent:-
```
Add to /etc/fstab:
fs-12345678:/ /mnt/efs efs defaults,_netdev 0 0
```
--------------------------------------

## 🖥️ 5. AWS CLI: Create EFS:-

- Create file system
aws efs create-file-system --performance-mode generalPurpose

- Create mount targets
aws efs create-mount-target \
--file-system-id fs-12345678 \
--subnet-id subnet-abc123 \
--security-group sg-123456

--------------------------------------

## 🧾 6. CloudFormation Template (Minimal EFS):-
```yaml
Resources:
  MyEFS:
    Type: AWS::EFS::FileSystem
    Properties:
      ThroughputMode: bursting

  MountTarget1:
    Type: AWS::EFS::MountTarget
    Properties:
      FileSystemId: !Ref MyEFS
      SubnetId: subnet-123
      SecurityGroups: [sg-123]
```
--------------------------------------

## 🔐 7. Security Best Practices:-

- ✔ Allow NFS 2049 only
Inbound → TCP 2049 → EC2 SG

- ✔ Use Access Points for applications
- ✔ Enable encryption at rest
- ✔ Enable automatic backups
- ✔ Use IAM + EFS policies for least privilege
--------------------------------------

## ⚙️ 8. Performance Optimization:-

- Use General Purpose mode for web apps
- Use Max I/O for 1,000+ EC2 fleet
- Prefer EFS Access Points
- Enable burst credits monitoring
- Avoid small-file-heavy workloads (EFS is optimized for large distributed workloads)

--------------------------------------
## 🧪 9. Testing EFS Across EC2 Instances:-

Create a file from EC2-A:
```
echo "Hello from EC2-A" > /mnt/efs/test.txt
```
Check on EC2-B:
```
cat /mnt/efs/test.txt
```

🟢 If you see the same content → EFS is working across instances.

--------------------------------------
## 🛠 10. Troubleshooting:-

❌ Cannot mount EFS

- ✔ Port 2049 open
- ✔ EC2 in same VPC
- ✔ Correct mount target exists

❌ File not visible across EC2

- ✔ All EC2 must mount same path
- ✔ Using same Access Point

❌ Slow performance

- ✔ Check throughput mode
- ✔ Use correct performance mode
- ✔ Remove unnecessary metadata operations

--------------------------------------
## 📁 11. Useful Commands Summary:-
```
df -h                      # verify EFS mount
```
```
mount | grep efs           # check mount points
```
```
sudo systemctl restart nfs # restart nfs
```
```
aws efs describe-file-systems
```
```
aws efs describe-mount-targets
```
--------------------------------------
## 🌟 12. Future Enhancements (Recommended):-

- Add EFS + ECS example
- Add EFS + EKS dynamic provisioning
- Add Terraform Version
- Add EFS monitoring dashboard
- Add EFS Access Point IAM examples
- Add CloudWatch log-based monitoring

--------------------------------------
## ✍️ 13. Author:-

- 👤 Prasad
- 💼 Cloud • DevOps • AWS • Infra Automation
- ⭐ If you like this repo, drop a star on GitHub!

## 📩 Connect With Me :
If you’d like to collaborate, discuss projects, or just say hello — feel free to reach out!  

### 🔗 Social & Professional Links:
- 🌐 [Portfolio Website](https://prasad-bhoite19.github.io/prasad-portfolio/)  
- 💼 [LinkedIn](http://linkedin.com/in/prasad-bhoite-a38a64223)  
- 🐙 [GitHub](https://github.com/Prasad-bhoite19)  
- ✉️ [Email](prasadsb2002@gmail.com)  

💬 Always open for opportunities in **Cloud, DevOps, and Full-Stack Projects**

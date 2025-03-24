# AWS EC2 Instance Bootstrapping with Apache HTTP Server

## Overview
This project demonstrates how to launch an AWS EC2 instance using the AWS Management Console and configure it with bootstrap commands to update packages, install, and start the Apache HTTP Server automatically.

## Prerequisites
- An active AWS account
- AWS Management Console access
- Basic understanding of AWS EC2 and Bash scripting

## Steps to Deploy an AWS EC2 Instance

### 1. Creating an AWS EC2 Instance
1. Log in to the AWS Management Console.
2. Select the region where you want to deploy the instance (e.g., N. Virginia).
3. Click on the **Services** dropdown and search for **EC2**.
4. Click **Launch instance**.
5. Under **All Services > Compute**, select **EC2**.

### 2. Configuring the EC2 Instance
#### Step 1: Choose an Amazon Machine Image (AMI)
- Select **Amazon Linux 2 AMI (HVM), SSD Volume Type, 64-bit (x86)**.

#### Step 2: Choose an Instance Type
- Select **t2.micro (Free tier eligible)**.

#### Step 3: Configure Instance Details
- Ensure **Auto-assign Public IP** is set to **Use subnet setting (Enable)**.
- Keep other settings as default.

#### Step 4: Configure Key Pair
- Select **Create a new key pair** and save it securely.

#### Step 5: Configure Security Group
- Create a new security group with rules for:
  - **SSH (Port 22)** - Allow access from your IP.
  - **HTTP (Port 80)** - Allow access from anywhere.

#### Step 6: Add Storage
- Ensure the **Volume Type** is set to **General Purpose SSD (gp2)**.

#### Step 7: Add User Data (Bootstrap Commands)
- Scroll down to the **Advanced Details** section.
- In the **User Data** section, enter the following bootstrap script:

```bash
#!/bin/bash
yum update -y
yum install httpd -y
systemctl start httpd
systemctl enable httpd
```

#### Step 8: Launch Instance
- Click **Launch** to create the instance.

### 3. Verifying the EC2 Instance
#### Check Instance Status
1. Go to the **EC2 Dashboard**.
2. Ensure the instance is in **Running** status.

#### Confirm Apache HTTP Server is Running
1. Copy the **Public IP** of the EC2 instance.
2. Open a web browser and enter `http://<public-ip>`. You should see the Apache test page.

#### Verify Bootstrap Execution
1. Connect to the instance using SSH:
   ```bash
   ssh -i <your-key.pem> ec2-user@<public-ip>
   ```
2. Check the package installation history:
   ```bash
   sudo yum history
   ```
3. Verify Apache service status:
   ```bash
   systemctl status httpd
   ```

## Troubleshooting
- If the test page does not load, verify:
  - Security Group settings (Port 80 is open).
  - Bootstrap execution by checking logs: `sudo cat /var/log/cloud-init-output.log`.
  - Apache service is running with `systemctl status httpd`.

## Conclusion
This project demonstrates how to automate the setup of an AWS EC2 instance with an Apache HTTP server using bootstrap scripts. This approach simplifies server provisioning and ensures the necessary configurations are applied automatically at launch.



# **MERN EC2 Docs**

_A step-by-step guide to deploying a MERN (MongoDB, Express.js, React, Node.js) stack application on an AWS EC2 instance._

---

## 🧰 **Prerequisites**

-   AWS account
-   Basic knowledge of Terminal Ubuntu, and Linux
-   A MERN stack project (client & server folders)
-   Domain name
-   GitHub repo access (if deploying via Git)

---

## 🔑 **Create AWS Account**

1. Sign up at the [AWS Cloud](https://aws.amazon.com/free)
2. Choose: **Business** account type (recommended)
3. Account Name: Any (does **not** need to match your project name)
4. Payment Method: Use a **Credit Card** (some **Debit Cards** may work)
5. Fill in required personal or business details
6. Complete identity verification (email/phone)
7. Log into the console once your account is active

## 🖥️ **Launch EC2 Instance**

1. In the AWS Console, search for and open the **EC2** service
2. Go to: **EC2 > Instances > Launch Instance**
3. Enter instance name: e.g., `mern-server` or your project name
4. Choose an Amazon Machine Image (AMI):
    - **Ubuntu Server 22.04 LTS** (Recommended)
5. Choose Instance Type:
    - **t2.micro** (Free Tier eligible)
6. Create or select a key pair:
    - **Key pair type:** RSA
    - **Private key format:** `.pem`
    - Save the `.pem` file securely (you’ll need it to SSH)
7. Configure other settings as needed (default is usually fine)
8. Click **Launch Instance**
9. Wait a few moments and check the instance status:
    - **Instance state:** Running
    - **Status check:** 2/2 checks passed ✅

---

## 🔐 Connect to EC2 via SSH

1. Go to the **EC2 > Instances** page
2. Find your running instance and click on the **Instance ID**
3. On the top-right, click the **Connect** button
4. Choose the **SSH Client** tab (used for terminal access)

---

## 🧭 Steps to Connect Using Terminal

1. **Open an SSH client** (Terminal on macOS/Linux, or PowerShell on Windows)
2. **Locate your private key file**  
   The key used to launch this instance is:  
   `aws-instance-login.pem`
3. **Set the correct permission** for your `.pem` file:

    ```bash
    chmod 400 "aws-instance-login.pem"
    ```

4. **Connect to your instance using its Public DNS**  
   Example Public DNS:  
   `ec2-00-0-000-000.ap-south-1.compute.amazonaws.com`

5. 🖥️ **Example SSH Command**  
   To connect to your EC2 instance, you must use it like this:

    ```bash
    ssh -i "aws-instance-login.pem" ubuntu@ec2-00-0-000-000.ap-south-1.compute.amazonaws.com
    ```

    > ✅ Make sure you’re in the directory where your `.pem` file is saved

    > ✅ Replace the DNS with your actual `EC2 Public DNS`

    > ✅ Use `ubuntu@` for Ubuntu instances, or `ec2-user@` for Amazon Linux

    > ✅ If successful, you'll be logged into your EC2 instance via the terminal.

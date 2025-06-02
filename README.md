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

---

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
    # Run this command once to set the correct permission for your PEM file
    chmod 400 "aws-instance-login.pem"
    ```

4. **Connect to your instance using its Public DNS**  
   Example Public DNS:  
   `ec2-00-0-000-000.ap-south-1.compute.amazonaws.com`

5. 🖥️ **Example SSH Command**  
   To connect to your EC2 instance, you must use it like this:

    ```bash
    # Connect to your EC2 instance using this command:
    ssh -i "aws-instance-login.pem" ubuntu@ec2-00-0-000-000.ap-south-1.compute.amazonaws.com
    ```

    > ✅ Make sure you’re in the directory where your `.pem` file is saved

    > ✅ Replace the DNS with your actual `EC2 Public DNS`

    > ✅ Use `ubuntu@` for Ubuntu instances, or `ec2-user@` for Amazon Linux

    > ✅ If successful, you'll be logged into your EC2 instance via the terminal.

---

## ⚙️ Install Node.js and NVM

1. Go to [Node.js Downloads](https://nodejs.org/en/download)
2. Select the exact Node.js version your project uses
3. Since we're using **Ubuntu**, choose the **macOS/Linux** tab
4. Choose the **nvm** installation option
5. This will also install **npm** (Node Package Manager)
6. Install NVM and Node.js Command

    ```bash
    # Download and install NVM:
    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash

    # Load NVM into current shell (without restarting terminal):
    \. "$HOME/.nvm/nvm.sh"

    # Install Node.js (version 23 in this case):
    nvm install 23
    ```

7. Verify Installations Command

    ```bash
    # Check npm version:
    npm -v   # Should print "10.9.2"

    # Check Node.js version:
    node -v  # Should print "v23.11.1"
    nvm current  # Should print "v23.11.1"
    ```

    > 📌 Make sure you're connected to the internet when running these commands.

    > 📌 You can change the Node.js version later using `nvm install <version>` or `nvm use <version>`

8. Exit from your EC2 Instance (Virtual Machine)

    ```bash
    exit
    ```

9. Reconnect to your EC2 Instance using Terminal

    ```bash
    ssh -i "aws-instance-login.pem" ubuntu@ec2-00-0-000-000.ap-south-1.compute.amazonaws.com
    ```

---

## 🗃️ **Clone Your MERN Project**

1. Frontend server

    ```bash
    git clone https://github.com/your-username/frontend-repo.git
    ```

2. Backend server

    ```bash
    git clone https://github.com/your-username/backend-repo.git
    ```

---

## ⚙️ Install Nginx and PM2

**Update Ubuntu package list**

```bash
sudo apt update
```

> This command refreshes the list of available packages and their versions.

> It's recommended to run before installing any new software.

### Nginx Commands

1.  Install Nginx to deploy our project

    ```bash
    sudo apt install nginx
    ```

2.  Start Nginx

    ```bash
    sudo systemctl start nginx
    ```

3.  Enable Nginx on boot

    ```bash
    sudo systemctl enable nginx
    ```

4.  Restart Nginx

    ```bash
    sudo systemctl restart nginx
    ```

### PM2 Commands

1.  Install PM2 to run our server 24X7

    ```bash
    npm install pm2 -g
    ```

2.  Start the server with PM2

    ```bash
    pm2 start npm -- start # pm2 start (run pm2 server), npm start (project start command)
    pm2 start npm --name "backend" -- start # or with a custom name
    ```

3.  Check server logs

    ```bash
    pm2 logs
    ```

4.  Remove (flush) PM2 server logs

    ```bash
    pm2 flush <pm2-server-name> # by name
    # or
    pm2 flush <pm2-server-id> # by id
    ```

5.  View all list of PM2 server

    ```bash
    pm2 list # show all start server
    ```

6.  Stop pm2 server

    ```bash
    pm2 stop <pm2-server-name> # by name
    # or
    pm2 stop <pm2-server-id> # by id
    ```

7.  Delete pm2 server

    ```bash
    pm2 delete <pm2-server-name> # by name
    # or
    pm2 delete <pm2-server-id> # by id
    ```

8.  Custom name of pm2 server

    ```bash
    pm2 start npm --name "backend" -- start
    ```

---

## 🌐 **Setup Frontend (React) with Nginx on AWS EC2**

1. Navigate to the Frontend Project

    ```bash
    cd frontend-repo
    ```

2. Install Dependencies & Build the Project

    ```bash
    npm install
    npm run build
    ```

3. Setup `.env` File

    ```bash
    # Example environment variables
    BACKEND_URL=/api
    ```

    > Save and Exit

4. Deploy Build Files to Nginx Directory

    ```bash
    # Start & Enable Nginx
    # copy code from build to Nginx web root
    sudo scp -r build/* /var/www/html/ # dist
    ```

    > **Note**: Make sure `/var/www/html/` is empty or only contains files from your intended deployment.

5. Enable HTTP Access on Port 80 (in AWS EC2)

    - Before accessing your frontend in the browser, make sure port 80 is open on your instance:

    1. Go to the **EC2 > Instances** page
    2. Find your running instance and click on the **Instance ID**
    3. GO the **Security** tab
    4. Click on the linked **Security groups**
    5. Under **Inbound rules**, click **Edit Inbound Rules**
    6. Add a rule:
        - Type `HTTP`
        - Port `80`
        - Source `0.0.0.0/0` (for public access)
    7. Click **Save rules**

6. Your Frontend is Now Live!

    - Open your browser and go to:

        ```bash
        http://<your-ec2-ip>/
        # or
        http://domain.com/
        ```

---

## 🌍 **Setup Backend (Node + Express) on AWS EC2**

1. Navigate to the Backend Project

    ```bash
    cd backend-repo
    ```

2. Install Dependencies

    ```bash
    npm install
    ```

3. Whitelist EC2 IP in MongoDB Atlas

    - Go to [MongoDB Atlas](https://www.mongodb.com/)
    - Navigate to **Network Access** -> Add IP Address
    - Add your EC2 public IP address or use `0.0.0.0/0` (for open access)

4. Setup `.env` File

    ```bash
    # Example environment variables
    PORT=7777
    MONGO_DB=mongodb+srv://username:password@cluster.mongodb.net/dbname
    ```

    > Save and Exit

5. Start the backend server with PM2

    ```bash
    pm2 start npm --name "backend" -- start
    ```

    > This keeps your server running in the background, even after SSH disconnects.

6. Configure Nginx to Reverse Proxy to Node.js

    - Open the Nginx default config:

        ```bash
        sudo nano /etc/nginx/sites-available/default
        ```

    - Replace or update the contents with:

        ```bash
        # ...
        # Add searver name and location
        server_name 00.0.000.000; # Domain Name or Instance IP Address
        location /api/ {
                proxy_pass http://localhost:7777/;  # (PORT - 7777) Pass the request to the Node.js app
                proxy_http_version 1.1;
                proxy_set_header Upgrade $http_upgrade;
                proxy_set_header Connection 'upgrade';
                proxy_set_header Host $host;
                proxy_cache_bypass $http_upgrade;
        }
        # Correctly redirects all unknown (404) routes to index.html
        location / {
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
                # try_files $uri $uri/ =404;
                try_files $uri /index.html;
        }
        # ...
        ```

7. Save and Exit

    - Press `CTRL + O`, Enter to save
    - Press `CTRL + X` to exit

8. Restart Nginx

    ```bash
     sudo systemctl restart nginx
    ```

9. Enable HTTP Access on Port `7777` (in AWS EC2)

    - Before accessing your backend in the browser, make sure port `7777` is open on your instance:

    1. Go to the **EC2 > Instances** page
    2. Find your running instance and click on the **Instance ID**
    3. GO the **Security** tab
    4. Click on the linked **Security groups**
    5. Under **Inbound rules**, click **Edit Inbound Rules**
    6. Add a rule:
        - Type `HTTP`
        - Port `7777`
        - Source `0.0.0.0/0` (for public access)
    7. Click **Save rules**

10. Your Backend is Now Live!

    - You can now access your backend API via:

        ```bash
        http://<your-ec2-ip>/api/
        # or
        http://domain.com/api/
        ```

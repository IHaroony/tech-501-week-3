

### **Step 1: SSH Key Pair Creation and Import into AWS**

1. **Generate SSH Key Pair in Git Bash:**
    
    - Open **Git Bash** and generate a new SSH key pair using the following command:
        
      
        
        `ssh-keygen -t rsa -b 4096 -C "your-email@example.com" 
        - **Explanation**:
            - `-t rsa`: Specifies the key type (RSA).
            - `-b 4096`: Specifies the size of the key (4096 bits for stronger encryption).
            - `-C "your-email@example.com"`: Adds a comment to the key (email address in this case).
            -
2. **Extract the Public Key:**
    
    - Display the public key so that it can be imported into AWS:
        
        
        
        `cat ~/.ssh/haroon-aws-key.pub`
        
    - **Explanation**: The `.pub` file contains the public part of your SSH key that AWS will use to authenticate you when you log into your EC2 instances.
    - 
3. **Import Public Key to AWS:**
    
    - Go to **AWS Console > EC2**.
    - In the left-hand menu, click on **Key Pairs** under **Network & Security**.
    - Click **Create Key Pair** and choose **Import Key Pair**.
    - Enter the key pair name as `haroon-aws-key` and paste the public key copied earlier into the **Key Pair Material** section.
    - Click **Create Key Pair** to finish.
---

### **Step 2: Launch EC2 Instances**

#### **Database VM (`db-app`) Setup:**

1. **Launch EC2 Instance for Database:**
    
    - Go to the **EC2 Dashboard** and click **Launch Instance**.
    - Select **Ubuntu Server 22.04 LTS** as the **AMI**.
    - Choose **t3.micro** as the instance type (Free Tier eligible).
    - Select the **haroon-aws-key** you created earlier.
    - Configure security group to:
        - Allow **SSH (22)** from your IP (for remote access).
        - Allow **MySQL (3306)** from the **App VM** (`app-vm`) only (for database access).
2. **Configure Instance Name:**
    
    - Name the instance `db-app`.
3. **Launch Instance**: Click **Launch** to create the instance.
    

#### **App VM (`app-vm`) Setup:**

4. **Launch EC2 Instance for Application:**
    
    - Similar to the database VM, launch another EC2 instance for the app.
    - Select **Ubuntu Server 22.04 LTS** as the AMI.
    - Choose **t3.micro** as the instance type.
    - Select the **haroon-aws-key** for SSH access.
    - Configure security group to:
        - Allow **SSH (22)** from your IP.
        - Allow **HTTP (80)** from anywhere (for public access to your app).
5. **Configure Instance Name:**
    
    - Name this instance `app-vm`.
6. **Launch Instance**: Click **Launch** to create the app instance.


**Step 3: Launch the Instances**

7. Click Launch Instance
8. Wait for the instances to be Running
9. Copy the Private IP of the DB VM (needed for connecting the app)


### **SSH Access to EC2 Instances**

1. **SSH into Database VM (`db-app`)**:
    
    - To access the **db-app** instance, use the following SSH command:
        
        
        
        `ssh -i ~/.ssh/haroon-aws-key ubuntu@<db-app-public-ip>`
        
    - Replace `<db-app-public-ip>` with the public IP address of your **db-app** instance.
2. **SSH into App VM (`app-vm`)**:
    
    - To access the **app-vm** instance, use the following SSH command:
        
        
        
        `ssh -i ~/.ssh/haroon-aws-key ubuntu@<app-vm-public-ip>`
        
    - Replace `<app-vm-public-ip>` with the public IP address of your **app-vm** instance




### **Database VM MongoDB Configuration**


3. `sudo apt update and sudo-apt upgrade
4. sudo apt-get install gnupg curl`
5. `curl -fsSL https://www.mongodb.org/static/pgp/server-8.0.asc | sudo gpg -o /usr/share/keyrings/mongodb-server-8.0.gpg --dearmor`
6. `echo "deb [arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-8.0.gpg ] https://repo.mongodb.org/apt/ubuntu noble/mongodb-org/8.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-8.0.list`
7. `sudo apt-get update`
8. `sudo apt-get install -y mongodb-org=8.0.4 mongodb-org-database=8.0.4 mongodb-org-server=8.0.4 mongodb-mongosh mongodb-org-mongos=8.0.4 mongodb-org-tools=8.0.4`

**Start the DB:**

9. `sudo systemctl start mongod`
10. `sudo systemctl status mongod`
11. `sudo systemctl enable mongod`

**Stop DB:**

12. `sudo systemctl stop mongod`

### **Step 2: Edited MongoDB Configuration**

Opened the MongoDB config file:



`sudo nano /etc/mongod.conf`

**Changed the `bindIp` setting** so MongoDB listens on all IPs:


`net:   port: 27017   bindIp: 0.0.0.0  # Changed from 127.0.0.1`

Saved the file (`CTRL + S`, `CTRL + X`) and restarted MongoDB:



`sudo systemctl restart mongod`

✅ **MongoDB is now accessible from other VMs in the private netwo



---
# App VM Setup

## **1. Update and Install Required Packages**

Before setting up the application, update the system and install necessary dependencies:


`sudo apt update -y && sudo apt upgrade -y`

### **Install Node.js & npm**



`sudo apt install -y nodejs npm`

✅ This installs **Node.js** and **npm**, which are required for running the application.

### **Install Nginx (Reverse Proxy)**


`sudo apt install nginx -y`

✅ **Nginx** is installed to act as a reverse proxy and direct traffic to the Node.js app.

---

## **2. Cloning the Application Repository**

The application source code is hosted on **GitHub**. Clone it to the VM.

### **Step 1: Create a Directory for the Repository**



`mkdir ~/repo && cd ~/repo`

✅ **This creates a directory named `repo` where the application will be stored.**

### **Step 2: Set Proper Permissions**



`sudo chown -R ubuntu:ubuntu ~/repo`

✅ **This ensures the `ubuntu` user has full access to the repository.**

### **Step 3: Clone the Repository**



`git clone https://github.com/IHaroony/tech-501-sparta-app-deployment.git cd tech-501-sparta-app-deployment/app`

✅ **Now, the application files are inside `~/repo/tech-501-sparta-app-deployment/app`.**

---

## **3. Install Node.js Dependencies**

Navigate to the application directory and install the required dependencies:



`cd ~/repo/tech-501-sparta-app-deployment/app npm install`

✅ **This installs all necessary Node.js packages required for the application.**




---
## **Set Environment Variables on App VM**

Your application needs the **MongoDB connection string**. Instead of hardcoding it, you **set an environment variable**.



`export DB_HOST=mongodb://172.31.63.124:27017/posts`

**To make this permanent, added it to `.bashrc`:**


`echo 'export DB_HOST=mongodb://172.31.63.124:27017/posts`



Now the connection to your posts page should all be setup aswell as your app page 


---


# **Reverse Proxy Setup

32. `sudo apt update`
33. `sudo apt install -y nginx`
34. `sudo systemctl status nginx`
35. `cd /etc/nginx/sites-available/`default 
36. `sudo nano /etc/nginx/sites-available/default`

Add this to the nano file:

nginx


`server {     listen 80;     server_name YOUR_APP_VM_PUBLIC_IP;      location / {         proxy_pass http://localhost:3000;         proxy_http_version 1.1;         proxy_set_header Upgrade $http_upgrade;         proxy_set_header Connection 'upgrade';         proxy_set_header Host $host;         proxy_cache_bypass $http_upgrade;     } }`

37. `sudo nginx -t` (check config for errors)
38. `sudo systemctl restart nginx` (restart to apply changes)

**Create inbound rule for port 80 in AWS security group**

- Test: `http://YOUR_APP_VM_PUBLIC_IP`





# **Blockers** #


# Blocker older version of node#

### **. Set Up Node.js and npm**

You initially faced issues with `npm` installation due to unmet dependencies and conflicting packages. Here's how you resolved the issues:

- **Removed broken packages** using the command:
    
    
    
    `sudo apt --fix-broken install`
    
- **Cleared the apt cache** and attempted to install Node.js and npm again:
    
 
    
    `sudo apt update sudo apt install npm`
    
- Encountered errors related to `libnode-dev` and `libnode72`, leading you to **remove the conflicting packages**:
    
    
    
    `sudo apt remove --purge libnode-dev sudo apt remove --purge libnode72 sudo apt clean`
    
- After removing conflicts, you installed **Node.js (v18.x)** from the **NodeSource repository**:
    
    
    
    `curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash - sudo apt install -y nodejs`
    
- Verified the installation of Node.js and npm with:
    

    
    `node -v npm -v`




# Another Blocker 

Blocker with reverse proxy not working after applying all correct rule
so i had use few commands 
sudo apt remove --purge nginx nginx-common -y
sudo apt autoremove -y
sudo apt install nginx -y

then access the sites-avaialvle/default and update the innfo again
ctrl s and ctrl x
reload and it worked 
that database and postpage 
# Create an AWS EC2 server based on Ubuntu Server 22.04 LTS and name it "Jenkins"
Open custom TCP 8080 on security group

# Install JDK (since Jenkins is a Java-based application)

`sudo apt update`

`sudo apt install default-jdk-headless`

# Install Jenkins

curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
    /usr/share/keyrings/jenkins-keyring.asc > /dev/null

     echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
    https://pkg.jenkins.io/debian binary/ | sudo tee \
    /etc/apt/sources.list.d/jenkins.list > /dev/null

` sudo apt-get update`

`sudo apt-get install jenkins`

# Ensure Jenkins is up and running

`sudo systemctl status jenkins`

http://<Jenkins-Server-Public-IP-Address-or-Public-DNS-Name>:8080 ![alt text](./Images/Screen%20Shot%202023-03-15%20at%2014.48.57.png)

# Retrieve the password from your server

` sudo cat /var/lib/jenkins/secrets/initialAdminPassword`

# Step 2 – Configure Jenkins to retrieve source codes from GitHub using Webhooks

1. Enable webhooks in your GitHub repository settings

2. Go to Jenkins web console, click "New Item" and create a "Freestyle project"

To connect your GitHub repository, you will need to provide its URL, you can copy from the repository itself

3. Click "Configure" your job/project and add these two configurations
Configure triggering the job from GitHub webhook:

Configure "Post-build Actions" to archive all the files – files resulted from a build are called "artifacts".

# Step 3 – Configure Jenkins to copy files to NFS server via SSH

1. Install "Publish Over SSH" plugin.
2. Configure the job/project to copy artifacts over to NFS server.
On main dashboard select "Manage Jenkins" and choose "Configure System" menu item.

Scroll down to Publish over SSH plugin configuration section and configure it to be able to connect to your NFS server:

Provide a private key (content of .pem file that you use to connect to NFS server via SSH/Putty)
Arbitrary name
Hostname – can be private IP address of your NFS server
Username – ec2-user (since NFS server is based on EC2 with RHEL 8)
Remote directory – /mnt/apps since our Web Servers use it as a mointing point to retrieve files from the NFS server

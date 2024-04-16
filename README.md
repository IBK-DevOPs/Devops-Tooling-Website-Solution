# DEVOPS TOOLING WEBSITE SOLUTION
### Devops Tooling Website Solution 
### This project is going to introduce us to a set of tools that we help Devops team in our day-to-day activities in managing, Developing and Testing, Deploying and Monitoring different projects.
### There are well known and widely used tools by multiple Devops teams but we are going to introduced to a single DevOps tooling solution that will consist of all mentioned tools.

### Jenkins-This is free and open-source automation server used in CI/CD pipeline.  It helps automate the parts of software development related to building, testing, and deploying, facilitating continuous integration, and continuous delivery.

### Kubernetes- This is an open source container-orchestration system for automating computer application deployment, scaling and management. Kubernetes is a portable, extensible, open-source platform for managing containerized workloads and services, that facilitates both declarative.

### Jfrog Artifactory – This is universal repository manager supporting all major packaging formats, build tools and CI server. Jfrog Artifactory is also the single solution for housing and managing all the artifacts, binaries, packages, files, containers, and components for use throughout your software supply chain.

### Rancher – This is an open source software platform that enable organization to run and manage Ducker and Kubernetes in production. This is a multi-cluster platform that lets operations teams deploy, manage and secure enterprise Kubernetes.

### Grafana – This is a multiplatform open source analytics and interactive visualization web application. It can produce charts, graphs, and alerts for the web when connected to supported data sources.

### Prometheus-this is an open-source monitoring system with a dimensional data model, Flexibility query language, efficient time series data base and modern alerting approach. It records metrics in a time series database built using an HTTP pull model, with flexible queries and real-time alerting. 

### Kibana- This is a free and open user interface, that let you visualize your Elasticsearch data and navigate the Elasic Stack.

### NAS – Network Attached Storage (NAS) system is a storage device connected to a network that allows storage and retrieval of data from a centralized location for authorized network users and heterogeneous clients. NAS systems are flexible and scale-out, meaning that as you need additional storage, you can add on to what you have. NAS is like having a private cloud in the office. It’s faster, less expensive and provides all the benefits of a public cloud on site, giving you complete control.

### A storage area network or storage network is a computer network which provides access to consolidated, block-level data storage. SANs are primarily used to access data storage devices, such as disk arrays and tape libraries from servers so that the devices appear to the operating system as direct-attached storage.

![alt text](image.png)

### NFS (Network File System) is a distributed file system protocol originally developed by Sun Microsystems in 1984, allowing a user on a client computer to access files over a computer network much like local storage is accessed.

### Server Message Block is a communication protocol used to share files, printers, serial ports, and miscellaneous communications between nodes on a network. On Microsoft Windows, the SMB implementation consists of two vaguely named Windows services: "Server" and "Workstation".

### FTP (The File Transfer Protocol) is a standard communication protocol used for the transfer of computer files from a server to a client on a computer network. FTP is built on a client–server model architecture using separate control and data connections between the client and the server.

### Internet Small Computer Systems Interface or iSCSI is an Internet Protocol-based storage networking standard for linking data storage facilities. iSCSI provides block-level access to storage devices by carrying SCSI commands over a TCP/IP network.

### Block-level storage is a concept in cloud-hosted data persistence where cloud services emulate the behaviour of a traditional block device, such as a physical hard drive. Storage in such services is organised as blocks.

### Object storage is a computer data storage approach that manages data as "blobs" or "objects", as opposed to other storage architectures like file systems which manages data as a file hierarchy, and block storage which manages data as blocks within sectors and tracks.

### Object storage stores and manages all data in an unstructured format and in units called objects. Block storage takes any data, like a file or database entry, and divides it into blocks of equal sizes. It then stores the data block on underlying physical storage in a way that's optimized for fast access and retrieval.

### Object storage normally uses a distributed storage environment across multiple different storage nodes or servers.

### On the other hand, block storage uses RAID, SSDs, and hard disk drives (HDDs) for storage.

### Finally, cloud file storage uses network-attached storage (NAS) in an on-premises setup. In the cloud, file storage service may be set up over underlying physical block storage.

![alt text](image-1.png)

Setup and technology used in this projects C
1. Infrastructure: AWS
2. Web server Linux: Red hat enterprise Linux 8
3. Data base server: ubuntu20.04 + mysql
4. Storage server 
5. Programming language
6. Code repository.  Git hub 

### 3 Tier Web Application Architecture With a Single Database and an NFS   Server a shared files storage.

![alt text](<image/3 tier achitecture.png>)

### It is important to know what storage solution is suitable for what use cases.
### We need to answer the following question-:
1. What data will be stored?
2. In what format?
3. How this data will be accessed?
4. By whom?
5. From where?
6. How frequent and etc
### Once all these questions are answered, we will be able choose the right storage system for our solution.

### Implementing a Business Website Using NFS for the Backend file Storage.
### Step 1- Preparation of NFS server

### Spin up a new EC2 server with RHEL Linux 8 operating system

![alt text](<image/RHEL 8.6.png>)

![alt text](<image/Prepare NFS server.png>)

### Configure LVM on the server

![alt text](<image/Logical volume created and attached with NFS.png>)

### Format the disks as  xfs

![alt text](<image/NFS fstab edited.png>)

### Ensure there 3 logical volumes which are lv-opt, lv-apps and lv-logs


### Create mount point on /mnt directory from the logical volumes as follow : Mount lv-apps on /mnt-apps 

### To be used by webserver, mount lv-logs on /mnt/logs

### To be used by webserver, logs mount lv-opt on /mnt/opt – to be used by Jenkins server in project 8.

### Install server, configure it to start on reboot and make sure it is running.

### Input below code
`sudo yum -y update`
`sudo yum install nfs-utils -y`
`sudo systemctl start nfs-server.service`
`sudo systemctl enable nfs-server.service`
`sudo systemctl status nfs-server.service`

### Export the mount for webservers subnet cidrto connect as client

### We will instant all 3 webserver inside the same subnet.

### But in the production setup, we would probably want to separate each tier, inside its own sublet for higher level security.

### To check our subnet cidr – open EC2, details in AWS web console 	and locate networking tab and open a subnet link.

### Connect NFS server to the terminal via ssh

![alt text](<image/NFS connect via ssh.png>)

### Use lsblk to view the attached volume

![alt text](<image/connect via ssh and view attched volume.png>)

### Create a single partition on each of 3 disks by using below code

'sudo gdisk /dev/xvdf sudo gdisk /dev/xvdg and sudo gdisk /dev/xvdh'
![alt text](<image/Create a single pertition of xvdf.png>)

![alt text](<image/create a single partition of xvdg.png>)

![alt text](<image/create a single pertition of xvdh.png>)

### Install lvm2 -y package.
### Run this code.
'sudo yum install lvm2'

![alt text](<image/LVM2 installed.png>)

### Then run sudo lvmdiskscan

![alt text](<image/Disk scan.png>)

### Create physical volume with below command.

'pvcreate /dev/xvdf1 pvcreate /dev/xvdg1 pvcreate /dev/xvdh1'

![alt text](<image/create phyical volume.png>)

### Add the 3 (4g) volume created with vgcreate use VG as  webdata-vg 

![alt text](<image/create volume group.png>)

### Use below code.

'sudo vgcreate webdata-vg /dev/xvdh1 /dev/xvdg1 /dev/xvdf1'

### Verify VG has been created with vgs.

![alt text](<image/verify vg is successfully created.png>)

### Create logical  volume with below code
'Sudo lvcreate -n lv-apps -L 4G webdata-vg'
'sudo lvcreate -n lv-logs -L 4G webdata-vg'
'sudo lvcreate -n lv-opt -L 4G webdata-vg'

![alt text](<image/Logical volume created.png>)

### View all logical volume created with this command

'sudo lvs'

### Verify that logical volume is created by input sudo lvs

![alt text](<image/verify that logical volume is created.png>)

### Verify the entire setup with sudo vgdisplay -v

![alt text](<image/verify the entire setup.png>)

### Then formatting the disk as xfs by using below code.

'Sudo mkfs -t xfs /dev/webdata-vg/lv-apps'
'sudo mkfs -t xfs /dev/webdata-vg/logs-lv'
'sudo mkfs -t xfs /dev/webdata-vg/lv-opt'

![alt text](<image/format app-lv with xfs.png>)

![alt text](<image/format disk as XFS.png>)

### Create mount point on/mnt directory for the logical volume as follows

Mount lv-apps on /mnt/apps
To be used by webserver mount lv-logs on /mnt/logs
To be used by webserver logs Mount lv-opt on /mnt/opt
To be used by Jenkins server.
Use below code

'sudo mkdir /mnt/apps'
'sudo mkdir /mnt/logs'
'sudo mkdir /mnt/opt'

![alt text](<image/create mount point and mount on specified apps.png>)

Mount the webdata with these command
'Sudo mount dev/webdata-vg/lv-apps /mnt/apps'.
'sudo mount /dev/webdata-vg/logs-lv /mnt/logs'
'sudo mount /dev/webdata-vg/lv-opt /mnt/opt'

![alt text](<image/mount lv apps and lv logs.png>)

![alt text](<image/mount lv-opt.png>)

### Update fstab file with sudo vi /etc/fstab

![alt text](<image/NFS fstab # ed.png>)

### install NFS server, configure it to reboot and ensure its running.

![alt text](<image/install NSF.png>)

### Run below code
'sudo yum -y update'
'sudo yum install nfs-utils -y'
'sudo systemctl start nfs-server.service'
'sudo systemctl enable nfs-server.service'
'sudo systemctl status nfs-server.service'

### Once NFS server is running, run below task to change ownership

![alt text](<image/change NFS ownership.png>)

![alt text](<image/change ownership on NFS.png>)

### Export the mount for webserver subnet cidr to connect as client

'sudo chown -R nobody: /mnt/apps'
'sudo chown -R nobody: /mnt/logs'
'sudo chown -R nobody: /mnt/opt'

### set up permission by running this command
For webserver to be able to read, write and 
Execute on NSF 
'sudo chmod -R 777 /mnt/apps'
'sudo chmod -R 777 /mnt/logs'
'sudo chmod -R 777 /mnt/opt'

![alt text](<image/setup permission and ownership.png>)

![alt text](<image/change NFS ownership.png>)

'sudo systemctl restart nfs-server.service'

![alt text](<image/restart NFS.png>)

![alt text](<image/NFS server status.png>)

### Then configure NSF for client server within the same subnet
### By using below to configure etc file

![alt text](<image/webserver subnet.png>)

'sudo vi /etc/exports'

![alt text](<image/export mounted port.png>)

1. '/mnt/apps <Subnet-CIDR>(rw,sync,no_all_squash,no_root_squash)'
This will be
1. '/mnt/apps 172.31.16.0/20 (rw,sync,no_all_squash,no_root_squash)'

2. '/mnt/logs <Subnet-CIDR>(rw,sync,no_all_squash,no_root_squash)'
This will become
2. '/mnt/logs 172.31.16.0/20 (rw,sync,no_all_squash,no_root_squash)'

3. '/mnt/opt <Subnet-CIDR>(rw,sync,no_all_squash,no_root_squash)'
### This will become
3. '/mnt/opt 172.31.16.0/20 (rw,sync,no_all_squash,no_root_squash)'
Press below command to safe
Esc + :wq!

### Then run export command to see exported mount point.
'sudo exportfs -arv'

Then run rpcinfo -p | grep nfs

To see the port used by NFS 

![alt text](<image/export fs and check the port use by NSF.png>)

### Edit security inbound rule and open the following port
1. TCP 111
2. UDP 111
3. UDP 2049
4. NFS 2049
### Safe inbound rule and exit

![alt text](<image/add security rule to open NFS port.png>)

### Then we will try and connect DB server.

![alt text](<image/data base server created.png>)

### Configure DB server via ssh.

![alt text](<image/connect database via ssh.png>)

### Install mysql  and configure mysql with below code.


### Sudo apt install mysql-server
'Sudo apt update'

![alt text](<image/install sudo apt mysql on DB.png>)

### Log into mysql

'Sudo mysql'
### create database tooling.

![alt text](<image/create BD as tooling.png>)

'create user 'webaccess'@'172.31.32.0/20' identified by 'password';'
'grant all privileges on tooling.* to 'webaccess'@'172.31.32.0/20';'
'flush privileges;'
'show databases;'
'Exit mysql'

![alt text](<image/create DB server as webaccess, grant access and flush priveleges.png>)

![alt text](<image/show databbase.png>)

### Prepare webserver to the folder.

![alt text](<image/configure 3 webservers.png>)

### We need to make sure that our webserver can serve the same content from shared storage solutions.
### In our own project its NFS server and MYSQL Database server.

### From our past knowledge, one DB can access for read and write by multiple client.
### In order to store share file that our webserver will utilize NFS and mount point previously created on our logical volume lv-apps to the folder where Apache store file to be served to the user (/var/www)
### With this process our webservers will be STATELESS, which means we will be able to add new one or remove them whenever we need and the integrity of data (in the database and on NFS) will be preserved.

### Connect the 3 webservers via ssh and configure the webservers.
![alt text](<image/connect webserver 1 via ssh.png>)

![alt text](<image/connect webserver 2 via ssh.png>)

![alt text](<image/connect webserver 3 via ssh.png>)

### Then mount logical volume created on mount server.
1. Lunch 3 webserver EC2
2. Install NSF client server
'Run sudo yum install nfs-utils nfs4-acl-tools -y'

![alt text](<image/NFS client server on webserver.png>)

3.  Mount /var/www/ and target the NFS server’s export for apps

### Then run below command to mount /var/www/

![alt text](<image/create var www mount point.png>)
'sudo mkdir /var/www'

'sudo mount -t nfs -o rw,nosuid <NFS-Server-Private-IP-Address>:/mnt/apps /var/www'
### NFS PRIVATE IP IS 172.31.33.229.

'sudo mount -t nfs -o rw,nosuid 172.31.33.229 mnt/apps /var/www'

![alt text](<image/view var www mount point.png>)

### Update NFS file with below command
'sudo vi /etc/fstab'

<NFS-Server-Private-IP-Address>:/mnt/apps /var/www nfs defaults0 0

172.31.33.229:/mnt/apps /var/www nfs defaults 0 0
Then save with :wq!

![alt text](<image/update fstab with NFS private IP.png>)

5.Install Remi’s repository, Apache and PHP

'sudo yum install httpd -y'

'sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm'

'sudo dnf install dnf-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm'

'sudo dnf module reset php'

'sudo dnf module enable php:remi-7.4'

'sudo dnf install php php-opcache php-gd php-curl php-mysqlnd'

'sudo systemctl start php-fpm'

'sudo systemctl enable php-fpm'

'setsebool -P httpd_execmem 1'

![alt text](<image/install apache & php.png>)
![alt text](<image/php & apache.png>)
![alt text](<image/install dnf.png>)
![alt text](<image/sudo systemctl start php.png>)

### Configure webserver 2
### Install client server sudo mkdir /var/www

![alt text](<image/NFS sudo yum install nfs utility.png>)

'sudo mount -t nfs -o rw,nosuid <NFS-Server-Private-IP-Address>:/mnt/apps /var/www'
'sudo mount -t nfs -o rw,nosuid 172.31.33.229 mnt/apps /var/www'

![alt text](<image/locate httpd on web server.png>)

### Locate the log folder for apache

### Use sudo vi /etc/fstab to add fstab with 172.31.33.229:/mnt/logs /var/log/httpd nfs defaults  0  0
![alt text](<image/connect webserver 2 via ssh.png>)
![alt text](<image/NFS client server on webserver.png>)

### Configure webserver 3
### Install client server sudo mkdir /var/www
![alt text](<image/create var www mount point.png>)

'sudo mount -t nfs -o rw,nosuid <NFS-Server-Private-IP-Address>:/mnt/apps /var/www'

'sudo mount -t nfs -o rw,nosuid 172.31.33.229 mnt/apps /var/www'

![alt text](<image/mount nfs on webserver.png>)

### install Remi’s repository, Apache and PHP


'sudo yum install httpd -y'

'sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm'

'sudo dnf install dnf-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm'

'sudo dnf module reset php'

'sudo dnf module enable php:remi-7.4'

'sudo dnf install php php-opcache php-gd php-curl php-mysqlnd'

'sudo systemctl start php-fpm'

'sudo systemctl enable php-fpm'

'setsebool -P httpd_execmem 1'

### Then Fork into git hub.

### To fork a github.

### Fork the tooling source code from Darey.io Github Account to IBK Github account.
### Locate a github account
### Copy its httpd
### Install git on our server with
 'sudo yum install git'

![alt text](<image/install git.png>)
 
'Then run git init'

![alt text](<image/git init.png>)

### Then run git clone by cloning Darey.io git account

### Run below command
'git clone https://github.com/darey-io/tooling.git'

![alt text](<image/clone darey .io toolings.png>)

### Fork my github.
![alt text](<image/ibk git repo.png>)
### clone ibk github
![alt text](<image/clone IBK git hub.png>)

### Cd into tooling
### Ls tooling
### Tooling contains apache-config.conf  Dockerfile  html  Jenkinsfile  README.md  start-apache  tooling-db.sql 
![alt text](<image/ls tooling.png>)

Then move html into /var/www use cp command
sudo cp -R html/. /var/www/html

![alt text](<image/copy all content inside html folder into var www html.png>)

![alt text](<image/move html into var www.png>)

### Then check ls
### apache-config.conf  Dockerfile  html  Jenkinsfile  README.md  start-apache  tooling-db.sql

### Then open port 80 on webserver inbound security

![alt text](<image/open port 80 on webserver.png>)

### Then put your public IP on webserver and apache is working.

![alt text](<image/apache site.png>)

### Then Deploy the tooling website’s code to the Webserver. Ensure that the html folder from the repository is deployed to /var/www/html by running below commands

![alt text](<image/deploy tooling website.png>)

'Sudo vi /var/www/html/functions.php'
'Then edit the path we created on DB, $db = mysqli_connect('172.31.37.55', 'webaccess', 'password', 'tooling');'
'User name webaccess'
'Password password'
'Use DB private IP ('172.31.37.55')'
### Then save with wq!

![alt text](<image/edit DB tooling path.png>)

### Update webserver configuration by  Apply tooling-db.sql script to your database using this command mysql -h -u -p < tooling-db.sql
$db = mysqli_connect('172.31.37.55', 'webaccess', 'mypasskey', 'tooling');

'sudo vi /etc/sysconfig/selinux'
### Set Selinux policy by disable Selinux
### Then restart httpd by running below command

![alt text](<image/restart httpd.png>)

### sudo systemctl restart httpd

### Update the websites configuration to connect to the database in /var/www/html/functions.php with below command.
'sudo vi /var/www/html/functions.php'

![alt text](<image/configure webserver to communicate with database.png>)




### Install MySQL on the web servers using.
'sudo yum install mysql -y'

![alt text](<image/install mysql -y.png>)

### Then cd into the tooling directory to connect to the database.

![alt text](<image/cd into toolings.png>)

### Run below command and use all the access created on DB tooling 

'sudo mysql -h 172.31.37.55 -u webaccess -p tooling < tooling-db.sql'

Check DB inbound rule and open port 3306

![alt text](<image/DB port 3306.png>)

### Then edit the mysqld.cnf file by using below command.

'sudo vi /etc/mysql/mysql.conf.d//mysqld.cnf'

![alt text](<image/edit binding address.png>)

Then run below command to restart mysql.

![alt text](<image/restart mysql.png>)

sudo systemctl restart mysql

sudo systemctl status mysql

### We can now check for the created admin in the database by running below commands
### Login into mysql

$ sudo mysql -p

mysql> use tooling;

mysql> show tables;

mysql> select from * users;

### Create in MySQL a new admin user with username: myuser and password: password:

![alt text](<image/check tables.png>)


### INSERT INTO 'users' ('id', 'username', 'password', 'email', 'user_type', 'status') VALUES
-> (1, 'myuser', '5f4dcc3b5aa765d61d8327deb882cf99', 'user@mail.com', 'admin', '1');

### Open the website in your browser http:///index.php and make sure you can login into the websute with myuser user
### PUBLIC IP ADDRESS IS http 18.188.216.209, input public IP  address on our webserver.
### Open the website in your browser
'http://<Web-Server-Public-IP-Address-or-Public-DNS-Name>/index.php'

![alt text](<image/enter public ip on webserver.png>)

### We now try to loging using our username and password created. 
### If there is problem to log in, we should check our connection to the DB.

![alt text](<image/log in with admin name & password.png>)

![alt text](<image/propitix tooling website.png>)

## CONCLUTION
### At the end of this project, we were able to learn skill and knowledge needed to create a unified Devops environment. 
### We also learned proper understanding of Devops tools and their integration into web based platform.
 










































































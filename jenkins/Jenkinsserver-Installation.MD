# Jenkins server Installation

To create EC2 instance use t2.medium (need to pay)

Use instruction from below link
https://piyushchaudhariblog.wordpress.com/2024/12/03/how-to-install-configure-jenkins-on-amazon-linux-2023/

java -version

find / -name java-17* | head -n 4 --this will show java home path

/etc/alternatives/java-17-amazon-corretto
/usr/lib/jvm/java-17-amazon-corretto.x86_64 -- need to take this path
/usr/lib/jvm/java-17-amazon-corretto

pwd -- currently on root directory
ls -lrta -- this will show all contains including hiden file. select .bash_profile to add Java home

vi .bash_profile

JAVA_HOME=/usr/lib/jvm/java-17-amazon-corretto.x86_64
export JAVA_HOME
PATH=$PATH:$JAVA_HOME

source ~/.bash_profile -- after change in .bash_profile file root user has to logout and login again to reflect the change. But if execute this command then don't need to logout

java -version

-- Attache IAM roles to the Jenkins server as this is going to communicate with EKS cluster. Needs to attached same IAM roles that was attached with EC2 instance for manageing the EKS cluster. Please check EKS-Setup.MD

------------------------------------------------------------------------------------------------
don't use below instruction as this won't work for amazon-linux-2023 EC2 instance ------------

### Prerequisites 
 1. EC2 Linux 7.x Instance

 2. Java v11

## Install Java
We will be using open java for our demo, Get latest version from http://openjdk.java.net/install/
```sh
yum install java-11*   --  to install Jenkins on the EC2 instance need java

```

### Confirm Java Version
Lets install java and set the java home
```sh
java -version
find / -name java-11* | head -n 4 --this will show java home path
/etc/alternatives/java-11-amazon-corretto
/etc/alternatives/java-11
/usr/lib/jvm/java-11-amazon-corretto.x86_64 -- need to take this path
/usr/lib/jvm/java-11-amazon-corretto

pwd -- currently on root directory
ls -lrta -- this will show all contains including hiden file. select .bash_profile to add Java home

vi .bash_profile
JAVA_HOME=/usr/lib/jvm/java-11-amazon-corretto.x86_64
export JAVA_HOME
PATH=$PATH:$JAVA_HOME
export PATH
# To set it permanently update your .bash_profile
source ~/.bash_profile -- after change in .bash_profile file root user has to logout and login again to reflect the change. But if execute this command then don't need to logout
```
_The output should be something like this,_
```
[root@~]# java -version
openjdk version "11.0.17" 2022-10-18 LTS
OpenJDK Runtime Environment Corretto-11.0.17.8.1 (build 11.0.17+8-LTS)
OpenJDK 64-Bit Server VM Corretto-11.0.17.8.1 (build 11.0.17+8-LTS, mixed mode)
```

## Install Jenkins
You can install jenkins using the rpm or by setting up the repo. We will setup the repo so that we can update it easily in future.
Get latest version of jenkins from https://pkg.jenkins.io/redhat-stable/
```sh
yum -y install wget -- this command is to install wget on EC2 server (by default EC2 instance I had created there wget has already installed)
wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo -- need to download the Jenkins from internate and store in /etc/yum.repos.d/jenkins.repo with help of wget command
### rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key  -- import the Jenkins key. this won't import Jenkins public key on Amazon Linux 2023 server. needs to use below
rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
yum -y install jenkins  -- will install the jenkins this will take the installation file/s from the downloaded repo location /etc/yum.repos.d/jenkins.repo
service jenkins status -- check the jenkins running status, By default Jenkins won't start and will be inactive and disable
```

### Start Jenkins
```sh
# Start jenkins service
systemctl start jenkins

# Setup Jenkins to start at boot,
systemctl enable jenkins
```

#### Accessing Jenkins
By default jenkins runs at port `8080`, You can access jenkins at
```sh
http://YOUR-SERVER-PUBLIC-IP:8080
```
#### Configure Jenkins
- The default Username is `admin`
- Grab the default password 
  - Password Location:`/var/lib/jenkins/secrets/initialAdminPassword`
- `Skip` Plugin Installation; _We can do it later_
- Change admin password
  - `Admin` > `Configure` > `Password`
- Configure `java` path
  - `Manage Jenkins` > `Global Tool Configuration` > `JDK`  
- Create another admin user id

### Test Jenkins Jobs
1. Create “new item”
1. Enter an item name – `My-First-Project`
   - Chose `Freestyle` project
1. Under Build section
	Execute shell : echo "Welcome to Jenkins Demo"
1. Save your job 
1. Build job
1. Check "console output"

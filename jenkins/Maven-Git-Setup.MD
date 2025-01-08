# Install and Configure Maven & git in Jenkins server

## Install Maven
```sh
yum install wget  ---  This install wget on server but generally this should be there to check execute [ wget]
-- better to download under /opt/ directory so execute [ cd /opt/ ]
wget https://mirror.lyrahosting.com/apache/maven/maven-3/3.9.9/binaries/apache-maven-3.9.9-bin.tar.gz
ls -lrt
tar -xvzf apache-maven-3.9.9-bin.tar.gz
ls -lrt
rm -rf < mvn .tar file name >
-- go to root home directory [ cd ]
ls -lrta -- will show hidden folders from there add maven in PATH
vi .bash_profile
export M2_HOME=/opt/apache-maven-3.9.9
export M2=$M2_HOME/bin
PATH=$PATH:$M2
# To set it permanently update your .bash_profile
source ~/.bash_profile

-- Validate Maven
mvn version -- this will say build failure as no proper input it will fail
```

## Install git
```sh
yum install git


```

## Assign shell to jenkins user

```sh
vi /etc/passwd
change shell from /bin/false to /bin/bash  --- for Jenkins
```


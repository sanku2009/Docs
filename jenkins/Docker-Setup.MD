## Install Docker
```sh
yum install docker
systemctl status docker
systemctl start docker
systemctl enable docker
systemctl status docker
```

## provide permissions to jenkins user in jenkins server to access docker
```sh
  cat /etc/group | grep -i docker --- this will show the group docker this was created during docker installation
  sudo groupadd docker  --- this is not required
  sudo usermod -aG docker jenkins ---- with this add jenkins to the docker group
  sudo chmod 777 /var/run/docker.sock  ---- give full access to [ docker.sock ] to jenkins
```
## Add Jenkins user into sudoers file to get sudo access
```sh
   vi /etc/sudoers
   jenkins ALL=(ALL) NOPASSWD: ALL    --- add this below the root, so that idenfy later easily
```   
   


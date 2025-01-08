### Jenkins server setup with Helm to deploy into Kubernetes cluster

## Download and Install helm 
```sh
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
```

## Test with helm command
```sh
helm version
helm list  --- this will show error if won't copy the config file from EC2 server manageing EKS cluster to Jenkins server, so follow below
```

## Copy config file from EKS Management host to Jenkins home directory
```sh
--- copy config file under .kube directory with jenkins ownership
-- login to EC2 server manageing EKS cluster
ls -lrta -- should see .kube folder and inside there should be config file ( all previous steps mentioned in full process should be followed before this)
cd .kube
ls -lrt
cat config  -- copy the contains
--- create this inside jenkins server
mkdir /var/lib/jenkins/.kube  
vi config -- paste the contains coppied from EC2 server manageing EKS cluster config file

helm list -- execute this command again

-- check document helm-command.MD
```










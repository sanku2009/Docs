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

# set up helm as by default help repo is not available and other set up if required
helm repo list -- this won't show any list
chmod 600 /var/lib/jenkins/.kube/config -- this will change permission on config file foe .kube
helm repo list -- still this will show error saying no repo list present. because installed helm3 which by default won't provide any helm stable repo. for that needs to add helm default stable repo
helm repo add stable https://charts.helm.sh/stable -- this will add helm default stable repo
helm repo list -- won't show any error
helm search repo stable  -- show all default helm chart


```










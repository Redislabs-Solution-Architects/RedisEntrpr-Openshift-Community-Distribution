# RedisEntrpr-Openshift-Community-Distribution [OKD](https://www.okd.io/)


1. Create a VM instance on GCP with following
  
Requirement  | Specification  
------------ | -------------
CPU | 8
Memory | 64 GB
OS | Cent OS 7
Disk | 30 GB

2. ssh to the machine

```bash 

sudo su
yum install -y git
sed -i 's/PermitRootLogin no/PermitRootLogin yes/' /etc/ssh/sshd_config
systemctl restart sshd
ssh-keygen -f ~/.ssh/id_rsa -t rsa -N ''
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
```

3. Now ssh to public ip address and the private ip of the machine to make sure we have passwordless access. 

```bash
ssh {public ip }
exit
ssh {private ip }
exit
```

4. Clone the repo

```bash
git clone https://github.com/gshipley/installcentos.git

```

5. Set the env varaible. Replace the public ip. Accepts all the defaults from install-openshift.sh.

```bash
cd installcentos/

export DOMAIN={public Ip}.nip.io
export USERNAME=admin
export PASSWORD=admin

./install-openshift.sh

```

6. Install Redis Enterprise. Based on [redis-enterprise-k8s-docs](https://github.com/RedisLabs/redis-enterprise-k8s-docs#configuration)

```bash

cd 
git clone https://github.com/Redislabs-Solution-Architects/RedisEntrpr-Openshift-Community-Distribution.git
cd RedisEntrpr-Openshift-Community-Distribution
oc new-project my-project
oc apply -f scc.yaml
oc adm policy add-scc-to-group redis-enterprise-scc system:serviceaccounts:my-project
kubectl apply -f rbac.yaml
kubectl apply -f crd.yaml
kubectl apply -f operator.yaml
kubectl apply -f redis-storage-class.yaml
kubectl apply -f redis-storage.yaml 
kubectl apply -f redis-enterprise-cluster.yaml
```





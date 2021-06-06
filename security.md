Host machines should be sdecured with ssh key or password hash based.

1st level access: Securing kube-apiserver
Who can acces? -> Authentication (userid/password, userid/token, certificates, external auth - ldap, service account )
What can they do? -> Authorization (RBAC, ABAC, Node Authorization, Webhook Mode)

All communication between etcd/kubectl/kube-controller/kube-scheduler/kube-proxy/kube-apiserver
are secured with TLS. 

Application within cluster can communicate to others by default.
Prevent this by Network Policy.

# Authentication in Cluster (who can access)

Bots access by service account.

1. Static Password File 
    -> csv file (pass,user,uid, group1*)
    -> pass it to kube-apiserver: --basic-auth-file=csv
    -> kubeadm: change it in /etc/kubernetes/manifests/kube-apiserver.yaml
    
    How to use?
    curl -v -k https://master-node-ip:6443/api/v1/pods -u "user1:password123"

2. Static token file
    -> csv file (token,user,uid, group1*)
    -> pass it to kube-apiserver: --token-auth-file=csv
    -> kubeadm: change it in /etc/kubernetes/manifests/kube-apiserver.yaml
    
    How to use?
    curl -v -k https://master-node-ip:6443/api/v1/pods --header "Authorization Bearer: token" 
3. Certificates

TLS 
priv key gen: openssl genrsa -out file.key 1024
pub key gen: openssl rsa -in file.key -pubout > file.out


4. 3rd party IdP (ldap)

Servers:
kubeapiserver
etcd
kubelet

Client:
admin
kube-scheduler
kube-controller-manager
kube-proxy

CA-Certificate

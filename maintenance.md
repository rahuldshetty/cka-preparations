# OS Upgrades

pod-eviction-timeout=5m0s -> Time to wait before making a node as dead.

# Move work loads to other nodes
`kubectl drain node-02 --ignore-daemonsets`

Gracefully terminated & Created on another.
node-01 is marked as unschedulable

To schedule back pods, run following command:
`kubectl uncordon node-01`

Cordon: # makrs node as schedulabled (do not move existing running pods)
`kubectl cordon node-01`

# Kubeadm upgrades

Upgrade master node & worker node

Strategy to upgrade worker nodes:
1) All at once (downtime)
2) One node at a time (move workloads and then upgrade, 0 downtime)
3) Add new nodes to cluster (move workload to new laods)

q1) what will happen to service components when we upgrade/remove nodes.


Master nodes:

Upgrade kubeadm tool: `apt-get upgrade -y kubeadm-1.12.0-00`
Check plan: `kubeadm upgrade plan`
Upgrade Cluster: `kubeadm upgrade apply v1.12.0`
Upgrade kubelet: `apt-get upgrade -y kubelet-1.12.0-00`
Restart service: `systemctl restart kubelet`

Worker nodes:
Remove workloads to other: `kubctl drain node-1`
Upgrade kubeadm tool: `apt-get upgrade -y kubeadm-1.12.0-00`
Upgrade kubelet: `apt-get upgrade -y kubelet-1.12.0-00`
Update kubelet version: `kubeadm upgrade node config --kubelet-version v1.12.0`
Restart service: `systemctl restart kubelet`
Mark schedulable: `kubectl uncordon node-01`

# Backup all resources
kubectl --all-namespaces get all -o yaml --export > all-resource.yaml

Tools: Velero by HeptIO

ETCD CLuster:
Stores state of application.
Hosted on Master node
stored under --data-dir path
Provides built in snapshot

Take Snapshot: ETCDCTL_API=3 etcdctl snapshot save snapshot.db
View status: ETCDCTL_API=3 etcdctl snapshot status snapshot.db
To Restore: 
1) Stop kube-apiserver: service kube-apiserver stop
2) RUN resotre: ETCDCTL_API=3 etcdctl snapshot restore snapshot.db --data-dir /var/lib/etcd-from-backup (new data drectory)
3) Edit etcd.service to read from new data dir:
ExeccStart=/usr/local...
--data-dir=/var/lib/etcd-from-backup
4) systemctl daemon-reload
5) service etcd restart
6) service kube-apiserver start

For all the etcd command specify:
1) endpoint (:2379)
2) cacert
3) cert
4) key

export ETCDCTL_API=3
etcdctl snapshot save --cacert=/etc/kubernetes/pki/etcd/ca.crt --cert=/etc/kubernetes/pki/etcd/server.crt --key=/etc/kubernetes/pki/etcd/server.key /opt/snapshot-pre-boot.db

etcdctl snapshot restore --cacert=/etc/kubernetes/pki/etcd/ca.crt --cert=/etc/kubernetes/pki/etcd/server.crt --key=/etc/kubernetes/pki/etcd/server.key /opt/snapshot-pre-boot.db --data-dir /var/lib/etcd-from-backup 

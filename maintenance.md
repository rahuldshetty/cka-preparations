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


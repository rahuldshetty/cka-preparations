# Manual Scheduling Pods to a Node

## Method #1 (Pod Defition)

Add nodeName field in the spec for container and set it to the node name on which the pod has to run.
Kubernetes DO NOT allow you to modify nodeName property of running pod (only during creation you can specify)

## Method #2 (Binding Object)

Create binding object JSON and send post request.
curl --header "Content-type:application/json" --request POST --data '{"apiVersion": "v1", "kind": "Binding", ...}' http://$SERVER/api/v1/namespaces/default/pods/$PODNAME/binding/


# Tains & Tolertations
Used to prevent nodes from running specific pods.


Taint: Spray a person with Taint (repellant). Bug will be intolerant of the smell and run off.

# How it works in K8s?
1. 3 nodes. Lets say Node1 wants to run only specific application. So we  taint a node (blue). By default, pods have no toleration. So no pods can go on to this cluster. 

TO enable certain pods to run on tainted node: Add toleration to pod X

## Set Taint on Node:
kubectl taint nodes node-name key=value:taint-effect

taint-effect: NoSchedule (dont run po) | PreferNoSchedule (try not to run pod) | NoExecute (evict exising pod if they can't handle taint)

Eg: kubectl tain nodes node01 app=blue:NoSchedule

## Set Toleration on Pods:
Add toleration under pod spec
tolerations:
-key: "app"
 operator: "Equal"
 value: "blue"
 effect: "NoSchedule"


# Node Selector
Label nodes with properties like size=Large/Medium/Small.
set in pod spec:
nodeSelector:
 size: Large

## How to label a node?
kubectl label nodes node01 size=Large

Drawbacks of node selector:
- Cannot place pods in nodes that satify multi condition/based on requirements
- NOt or operator not possible

# Node Affinity
Operator:
NotIn
In
Exists

Type:
> requiredDuringSchedulingIgnoredDuringExecuton (Mandatory & nodes dont exist -> pod dont schedule)
> preferredDuringSchedulingIgnoredDuringExecuton (Even when a node is not matched -> pod runs)


# Resource Requests (virual hardware)
Minimum: 1m cpu

Default Limit: 1cpu/512Mi

1G != 1Gi
1G  = 1,000,000,000 bytes
1Gi = 1,073,741,824 bytes


resources:
    requests:
        cpu: 1
        memory: 2Gi
    limits:
        cpu: 2
        memory: 4Gi 


cpu: 0.5
Mem: 256Mi
Disk: 

# Daemon Sets
Similar to replicaset. Runs one copy of pod on every node.
User Case:
1. Monitoring Agent
2. Logs Viewer
3. kube-proxy component
4. Networking - waevet-net

# Statuc Pod
kubelet => captain of node

kubectl reads file from /etc/kubernetes/manifests when a node goes offline from network. 
Only pods can be created this way. kubectl works on pod level and can create static pod.

--pod-manifest-path= while runniong service
or
define --config=kubeconfig.yaml in service & create staticPodPath: / insdie kubeconfig.yaml

docker ps command can let you view command.

Contains name xyz-nodename in the pod

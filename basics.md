# Create an NGINX Pod
`kubectl run nginx --image=nginx`

# Generate POD Manifest YAML file (-o yaml). Don't create it(--dry-run)
`kubectl run nginx --image=nginx --dry-run=client -o yaml`

# Create a deployment
`kubectl create deployment --image=nginx nginx`

# Generate Deployment YAML file (-o yaml). Don't create it(--dry-run)
`kubectl create deployment --image=nginx nginx --replicas=4 --dry-run=client -o yaml > deploy.yaml`

# Accessing Service from dev namespace
`db-service.dev.svc.cluster.local`
`<svc-name>.<namespace>.svc.cluster.local`

# Switch Cluster Context into dev namespace
`kubectl config set-context --current --namespace=dev`

# List pods in all namespace
`kubectl get po --all-namespaces`
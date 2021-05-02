# Imperative
How to do a task?
What are the steps involved?
Eg: Taxi Instructions

kubectl run --image=nginx nginx -l tier=app --port=8080 --dry-run=client
kubectl run --image=busybox sbox -l tier=app --port=8080 --dry-run=client --command sleep 1000 -o yaml
kubectl create deploy --image=nginx nginx

kubectl expose deploy nginx --port 80
kubectl expose pod redis --name redis-service --port=6379

kubectl scale deploy nginx --replicas=2
kubectl set image deploy nginx nginx=nginx:1.18
kubectl create service nodeport nginx --tcp=80:80 --node-port=30080 --dry-run=client -o yaml

# Declarative
What has to be done?
Eg: Uber
###Here we create custom nginx docker image
and push it to local repository 
then create replica-set of 3 nginx pods
and run shell script to test it

docker rmi 10.0.1.1:32000/mynginx:registry -f
microk8s ctr images remove $(microk8s ctr images list -q | grep mynginx)  

docker build Dockerfile -t 10.0.1.1:32000/mynginx:registry
docker push 10.0.1.1:32000/mynginx:registry
microk8s ctr images list -q | grep mynginx
kubectl apply -f mynginx-replica.yaml
kubectl get svc | grep mynginx
for i in {1..3}; do curl 10.152.183.147; done
http://cloud.smartisyourhome.com:32088

####/////////////// Manual kubectl apply
kubectl create deployment mynginx --image localhost:32000/mynginx:registry --replicas 3 && kubectl expose deployment mynginx --name mynginx --type NodePort --port 80 --target-port 80
kubectl get pods,service -o wide --show-labels | grep mynginx
for i in {1..3}; do curl 10.152.183.147; done
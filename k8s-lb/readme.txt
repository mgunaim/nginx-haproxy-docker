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
for i in {1..3}; do curl localhost:32088; done
http://cloud.smartisyourhome.com:32088
kubectl delete -f mynginx-replica.yaml



####/////////////// Manual kubectl apply
kubectl create deployment mynginx --image localhost:32000/mynginx:registry --replicas 3 && kubectl expose deployment mynginx --name mynginx --type NodePort --port 80 --target-port 80
kubectl get pods,service -o wide --show-labels | grep mynginx
for i in {1..3}; do curl 10.152.183.147; done


///////////////////old
# https://kubernetes.io/docs/tasks/configure-pod-container/translate-compose-kubernetes/
Convert the docker-compose file to kubernetes
curl -L https://github.com/kubernetes/kompose/releases/download/v1.34.0/kompose-linux-amd64 -o kompose
chmod +x kompose
sudo mv ./kompose /usr/local/bin/kompose
kompose convert
docker build . -t 10.0.1.1:32000/nginx-haproxy-docker_haproxy:registry

kubectl apply -f nginx-web-1-deployment.yaml,nginx-web-2-deployment.yaml

docker push localhost:32000/nginx-haproxy-docker_haproxy:registry




docker rmi 10.0.1.1:32000/nginx-haproxy-docker_nginx_web_2 -f
docker build . -t 10.0.1.1:32000/nginx-haproxy-docker_nginx_web_2:registry
docker push 10.0.1.1:32000/nginx-haproxy-docker_nginx_web_2:registry
microk8s ctr images list -q | grep nginx-haproxy-docker_nginx_web_2


docker rmi 10.0.1.1:32000/nginx-haproxy-docker_haproxy -f
microk8s ctr images remove
docker build . -t 10.0.1.1:32000/nginx-haproxy-docker_haproxy:registry
docker push 10.0.1.1:32000/nginx-haproxy-docker_haproxy:registry
microk8s ctr images list -q | grep nginx-haproxy-docker_haproxy
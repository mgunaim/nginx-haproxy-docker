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
# servers for SPEARED V2 (Simulation and Compilation)
## PhysicsEngine (gazebo) setup
1. clone repository 
2. cd PhysicsEngine
3. docker build -t physics-engine .
4. docker run -t -d -p 9090:9090 physics-engine

## Kubernetes setup
Kubernetes' infrastructure setup
This documents describes how you can set up your own Kubernetes cluster to use for the compilation server.
1.	Install k3s by running (linux only)
curl -sfL https://get.k3s.io | sh -
2.	check whether the cluster is running now
k3s kubectl get node
3.	This should yield something like 
4.	NAME     STATUS   ROLES                  AGE   VERSION
buster   Ready    control-plane,master   14d   v1.20.0+k3s2
5.	Next you should set the cluster configuration as environment variable to use kubectl more comfortable
export KUBECONFIG=/etc/rancher/k3s/k3s.yaml
6.	Now you are ready to configure your cluster. To use the compilation server you need a suitable service account that allows the compilerserver central pod to start (compilation-)jobs
7.	Therefore run
- kubectl apply -f job-controller-role.yml
- kubectl apply -f job-contoller-svc-acc.yml
- kubectl apply -f job-role-binding.yml
- kubectl apply -f compileserver-deployment.yml
- kubectl apply -f compileserver-service.yml
- kubectl apply -f compileserver-ingress.yml
8.	Now you can check the status of the cluster by running
 -	kubectl get pods
or
 -	kubectl get deployment
or
 -	kubectl get service
or
 -	kubectl get ingress
9.	It should tell you something like (here for example the deployment [option b)])  
NAME                                         READY   STATUS    RESTARTS   AGE  
compileserver-deployment-588cc459b8-mw6tr    1/1     Running   6          14d

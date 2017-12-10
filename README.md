# kubernetes
Fundamental and elementary topics and concepts 
Kubernetes - . Kubernatics is all about orchestrating containerized apps. It is orchestrator of microservices and it is platform agnostics


Big Picture
Kubernetes cluster made of the master and nodes. Master acts as cluster controller and nodes does the actual works.


application - > containernized it -> wrap within deployment object (written in Yaml, provides all the deployment details like network details, POD( single unit of deployment) --> hand this deployment object to Master node which then does the deployment.


Master - Kubernetes control panel


Consists of APiServer [front end for master],  - Kube-apiserver
    - External facing components, exposes REST APIs and message exchange happens in json. 
  - We send manifest file to master through kube-apiserver, Master read it, validate it and tries to deploy on different nodes as  POD within the cluster env 
Controller - Kube-controller-manager
    - Node controller
    - Endpoints controller
    - namespace controller

Scheduler - kube-scheduler
    - It watches api services for the new PODs and assign these PODs to nodes. 
    - Resource management, constraints resolution
Cluster storage. 
    - config and current state of the cluster stores in it . 
    - It is only the stateful part of the cluster. 
    - It uses etcd [ open source distributed key value store]. It is distributed, consistent and watchable. 
    - kubernetes uses it as 'source of truth' for the cluster. Make sure to have solid backup plan for it and protect it. 

- All these components resides within a single server. 
- We do not run user workload on Master.



NODES


pods - one or more containers packaged together to deploy as a single unit.

	1. Kubelets

		1. Main kubernetes agent
		2. Register Node with the Cluster
		3. Watch the Apiserver for the work and instantiates pods. 
		4. report back to Master with the outcome, either success ful instantiation or failure. It just reports back and left it to Master to decide the status of pods. 
		5. It exposes endpoint on port 10255 to inspect its state 

			1. /spec, /healthz, /pods 
	2. runtime container engine
	3. kube-proxy

		1. This is networking brain of node and does couple of things
		2. It assign individual network IP address to each Pod.  So if two or more containers are running inside single pod then those containers can be assessd by single IP address. 
		3. Load balances all Pods in a service

All the front end pods interacts with load balancer service and service in turn route the request to back-end pods. This load balancer is the responsibility of the kube-proxy.

Declarative model and Desired state  - kubernetes operates on a declarative model and expect from us the manifest file describing the Desired state of cluster. So we give declarative manifest file stating the desired state. Thus it does not expect the list of commands to run, rather it need the desired state of the cluster env and then it carry on the tasks to reach to the desired state.










pods



-  This is the smallest unit of deployment in the kubernetes world
- kubernetes always deploy container inside pod.
- Only one container can run within a pod. Although there are scenario where in we can run more than one container inside single pod. 
- pod is a sandbox env for the container. 
- deployment of the pod is a single atomic operation. Either its done or not done. Either all containers are available or none of them is available. 


Now depends on containers interaction pattern, we can decide to host multiple containers to single pod sharing the same network ID, volume. 







The unit of scaling in kubernetes is always the pod. When we want to scale the components in the app then we always create more pod and DO NOT add more container to a single pod. 





When pod dies then Replication Manager start a new pod and deploy the container. A dead pod can never be re instated. 




SERVICES

- They provides the higher abstraction layer for the pods and provides the consistent IP and DNS names for the group of pods. 
- services interfaces a set of closely related pods to provide the same IP and DNS and does the load balancing with the help of labels. 

Why we need service
Ip Churn - Service solves the problem of pod IP churn which is very prevalent in Pods world. 
As we know, containers always deployed in the pod and pod come with its own IP address. During pod management, many pod dies, new pods added and existing pod gets replaced by newer pods. In each of these events, pod IP address changed and if there is an ip address dependencies between pods then it creates a big manmoth tasks to manage all these ever changing Ip address ecosystem. 

Services is a kubernetes way to solve these ip dependencies. 
service is a higher layer of abstraction of all the dependent pods to get a single IP address and DNS name. Thus it abstract the interacting pods with the IP address. Service is intelligent enough to keep track of all the changing pods IP address while providing the consistent access pattern to dependent pods. 













Service uses 'label' concept to decide the interacting and linked pods. label helps service in managing its load balancing activities. Its a random load balancing supported. 












Creating local kubernetes development env - minikube











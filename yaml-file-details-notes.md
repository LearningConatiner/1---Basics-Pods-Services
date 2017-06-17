#### Standard
    
      apiVersion:
      
      # k8s resource
      kind: Pod 
      
      Metadata:
        name:
        labels:
          name:
          etc
          etc

#### Pod
          
      spec:
      # one to many container definition
        containers: 
        	- name: server
        	  image: nginx:latest
        	  ports: 
        	    - containerPort:
        	      protocol: 	    
        	- name: server
        	  image: nginx:latest
        	  ports: 
        	    - containerPort:
        	      protocol: 	
        .-.-.-	    

#### Service

	spec:
	  # very important - select a pod by label to associate the service to.
	  selector:
	    app: nginx-ex 
	  ports:
	    - port: 80
	      protocol: TCP
	      
	  # Optional: Type defaults to cluster IP. 
	  # values: NodePort - Internal Load Balancer to expose to public via nodeip:nodeport, 
	  # LoadBalancer: External Load Balancer to expose to public via external public ip, 
	  # ClusterIP: Cluster IP creates a virtual IP inside the cluster for internal access only. Does not expose to public.
	  
	  type: NodePort

1. Not all fields can be updated after creation. So, if a pod definition has to be updated, delete the pod and re-create.
2. Refer pod spec for more spec details.
3. To create pods ->

   		kubectl create -f <pod definition yaml file>
   
4. To debug pods ->

		 kubectl get pods
		 kubectl describe pods <pod name>
		 kubectl logs <podname> 

#### Namespace:

1. Resource isolation (at application and at environment level)
2. Security (RBAC)

		apiVersion: v1
		kind: Namespace
		metadata:
		  name: namespace1
		  labels:
		    name: namespace1

3. Create Namespace
		
		kubectl create -f <namespace yaml file name>

4. While creating/accessing any other k8s resources, always use namespace in the creation command.
		
		kubectl create -f <yaml file name> --namespace <name of namespace>
		
#### Container 

##### Spec details

	imagePullPolicy: IfNotPresent

-> Pulls image only if not present/already pulled
##### Logs

		kubectl logs <pod name> -c < container name> --namespace <namespace name>
		
		# To stream logs use -f option 
		
		kubectl logs <pod name> -c < container name> -f --namespace <namespace name>						 

## Scaling

1. Always scaled at pod level and not at container level. So, create multiple pods with containers based on how you want to define scaling


### _Deployment_

+ Old method: pods/services/replication controllers separately.

+ New method: pods and replica templates defined in deployment definition.

#### yaml

No need of label

	apiVersion: v1
	kind: Deployment
	metadata:
	  name: <name>
	spec:
	  replicas: <1 - n>
	  template:
	    <from metadata to spec of pod definition, labels are needed here to associate pods to service>

* Horizontal scaling -  manual

		kubectl scale deployment <deployment name> --replicas <n> --namespace <namespace name>

* Auto scaling (based on load)
	
	* CPU based autoscaling
		* Pre-reqs
			1. _Heapster_ must run on cluster.				
				Heapster -> Container metric collection framework. It runs as a pod in your cluster and auto-discovers resources and reports data via syncs such as logs, InfluxDB, and more.			
			2. _Pods_ must _request resources._				
				Resource requests are requests for a certain amount of CPU or memory from the cluster. 
		

* Rollouts 
	* Start
	* Stop
	* Resume

	
**Heapster**

		kubectl get --all-namespaces services
		
		NAMESPACE     NAME                   CLUSTER-IP     EXTERNAL-IP   PORT(S)         AGE
		default       kubernetes             10.7.240.1     <none>        443/TCP         1h
		kube-system   default-http-backend   10.7.251.127   <nodes>       80:31938/TCP    1h
		kube-system   heapster            10.7.241.193   <none>        80/TCP          1h
		kube-system   kube-dns               10.7.240.10    <none>        53/UDP,53/TCP   1h
		kube-system   kubernetes-dashboard   10.7.242.164   <none>        80/TCP          1h
		
		kubectl get --all-namespaces pods
		
		NAMESPACE     NAME                                           READY     STATUS    RESTARTS   AGE
		default       nginx-ex                                       1/1       Running   0          1h
		kube-system   fluentd-gcp-v2.0-741bv                         1/1       Running   0          1h
		kube-system   fluentd-gcp-v2.0-vnbm8                         1/1       Running   0          1h
		kube-system   fluentd-gcp-v2.0-zkp8q                         1/1       Running   0          1h
		kube-system   heapster-v1.3.0-1288166888-d4ll6               2/2       Running   0          1h
		kube-system   kube-dns-806549836-qx0nf                       3/3       Running   0          1h
		kube-system   kube-dns-806549836-v38xv                       3/3       Running   0          1h
		kube-system   kube-dns-autoscaler-2528518105-qd26n           1/1       Running   0          1h
		kube-system   kube-proxy-gke-pv-default-pool-caa55653-4xs7   1/1       Running   0          1h
		kube-system   kube-proxy-gke-pv-default-pool-caa55653-6v9q   1/1       Running   0          1h
		kube-system   kube-proxy-gke-pv-default-pool-caa55653-xn80   1/1       Running   0          1h
		kube-system   kubernetes-dashboard-2917854236-zxjv7          1/1       Running   0          1h
		kube-system   l7-default-backend-1044750973-dk2l8            1/1       Running   0          1h
		
		
       
        

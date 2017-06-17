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

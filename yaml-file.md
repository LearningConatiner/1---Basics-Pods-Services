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
	      
	  # Optional: 
	  # values: NodePort - Internal Load Balancer to expose to public via nodeip:nodeport, 
	  # LoadBalancer: External Load Balancer to expose to public via external public ip, 
	  # ClusterIP: does not expose to public
	  
	  type: NodePort

1. Not all fields can be updated after creation. So, if a pod definition has to be updated, delete the pod and re-create.
2. Refer pod spec for more spec details.
3. To create pods ->

   		kubectl create -f <pod definition yaml file>
   
4. To debug pods ->

		 kubectl get pods
		 kubectl describe pods <pod name>
		 kubectl logs <podname> 
	
5. To debug containers in a pod ->

		 kubectl logs <podname> <container name>

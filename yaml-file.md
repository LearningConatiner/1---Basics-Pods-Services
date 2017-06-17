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

### Service Type: ClusterIP

  1. Default value of type if not specified
  2. Services are accessible only within the nodes in the cluster.

### Service Type: NodePort

  1. Uses internal load balancer provided by k8s.
  2. Explicitly open firewall for nodeport in gcloud.
    
### Service Type: LoadBalancer

  1. Uses external load balancer provided by cloud provider.
  2. curl will not work in gcloud shell for all ports. verify in browser if port is not 80.
  
In this example we are using LoadBalancer.
  
		$ kubectl create -f one-pod-service.yaml 
		pod "nginx-example" created
		service "nginx-example" created
		
		$ kubectl get nodes
		NAME                                STATUS    AGE       VERSION
		gke-pv-default-pool-dcf04601-0wnm   Ready     7m        v1.6.4
		gke-pv-default-pool-dcf04601-l4fv   Ready     7m        v1.6.4
		gke-pv-default-pool-dcf04601-lt5h   Ready     7m        v1.6.4
		
		$ kubectl describe node gke-pv-default-pool-dcf04601-l4fv|grep -i address
		Addresses:              10.142.0.4,104.196.209.250,gke-pv-default-pool-dcf04601-l4fv
		
		$ kubectl get pods
		NAME            READY     STATUS    RESTARTS   AGE
		nginx-example   1/1       Running   0          1m
		
		$ kubectl get svc
		NAME            CLUSTER-IP     EXTERNAL-IP      PORT(S)        AGE
		kubernetes      10.7.240.1     <none>           443/TCP        7m
		nginx-example   10.7.247.147   35.185.105.204   80:32534/TCP   1m
		
		$ curl 35.185.105.204:80
		<!DOCTYPE html>
		<html>
		<head>
		<title>Welcome to nginx!</title>
		<style>
		  body {
		      width: 35em;
		      margin: 0 auto;
		      font-family: Tahoma, Verdana, Arial, sans-serif;
		  }
		</style>
		</head>
		<body>
		<h1>Welcome to nginx!</h1>
		<p>If you see this page, the nginx web server is successfully installed and
		working. Further configuration is required.</p>
		<p>For online documentation and support please refer to
		<a href="http://nginx.org/">nginx.org</a>.<br/>
		Commercial support is available at
		<a href="http://nginx.com/">nginx.com</a>.</p>
		<p><em>Thank you for using nginx.</em></p>
		</body>
		</html>

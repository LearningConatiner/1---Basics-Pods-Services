### Covered Concepts

* Namespaces
* Multi container pods (structuring N-tier applications)
* Inter-pod networking
* Service Discovery (with environment variables)
* Fetching logs

		$ kubectl create -f redis.yaml --namespace ns1
		pod "example-data-tier" created
		service "example-data-tier" created
		$ kubectl create -f server.yaml --namespace ns1
		pod "example-app-tier" created
		service "example-app-tier" created
		$ kubectl create -f support.yaml --namespace ns1
		pod "example-support-tier" created
		$ kubectl get pods --namespace ns1
		NAME                   READY     STATUS    RESTARTS   AGE
		example-app-tier       1/1       Running   0          5m
		example-data-tier      1/1       Running   0          5m
		example-support-tier   2/2       Running   2          5s
		$ kubectl logs example-support-tier -c poller --namespace ns1
		Current counter: 9
		Current counter: 21
		Current counter: 27
		$ kubectl logs example-support-tier -c counter --namespace ns1
		Incrementing counter by 9 ...
		Incrementing counter by 8 ...
		Incrementing counter by 4 ...




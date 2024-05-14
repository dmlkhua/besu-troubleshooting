> AWS Load balancer example

1) `kubectl apply -f lb.yaml`
2) `kubectl get svc`, copy value from EXTERNAL_IP column
3) `nslookup {{EXTERNAL_IP}}`, wait until URL is reachable, copy IP
4) set copied IP into static-nodes.cm.yaml
5) `kubectl apply -f static-nodes.cm.yaml`
6) `kubectl apply -f node0.yaml`
7) `kubectl get -o wide`, copy node0 pod IP
8) set copied IP into node0.endpoint.yaml, addresses field
9) `kubectl apply -f node0.endpoint.yaml`
10) `kubectl apply -f node1.yaml`
11) `kubectl get -o wide`, copy node1 pod IP
12) set copied IP into node1.endpoint.yaml, addresses field
13) `kubectl apply -f node1.endpoint.yaml`

// you can also start UDP echo server from echo.yaml file

> AWS Load balancer example

1) `kubectl apply -f genesis.yaml`
2) `kubectl apply -f lb.yaml`
3) `kubectl get svc`, copy value from EXTERNAL_IP column
4) `nslookup {{EXTERNAL_IP}}`, wait until URL is reachable, copy IP
5) set copied IP into static-nodes.cm.yaml
6) `kubectl apply -f static-nodes.cm.yaml`
7) `kubectl apply -f node0.yaml`
8) `kubectl get -o wide`, copy node0 pod IP
9) set copied IP into node0.endpoint.yaml, addresses field
10) `kubectl apply -f node0.endpoint.yaml`
11) `kubectl apply -f node1.yaml`
12) `kubectl get -o wide`, copy node1 pod IP
13) set copied IP into node1.endpoint.yaml, addresses field
14) `kubectl apply -f node1.endpoint.yaml`

// you can also start UDP echo server from echo-udp.yaml file to test UDP connectivity

> AWS Load balancer example

1)  create `dmlkh-besu` namespace (`kubectl create ns dmlkh-besu`)
2) `kubectl apply -f genesis.yaml`
3) `kubectl apply -f lb.yaml`
4) `kubectl get svc`, copy value from EXTERNAL_IP column
5) `nslookup {{EXTERNAL_IP}}`, wait until URL is reachable, copy IP
6) replace IPs in `static-nodes.cm.yaml` with the one copied on the previous step
7) replace p2p-host value in node0.yaml and node1.yaml with the load balancer IP
8) `kubectl apply -f static-nodes.cm.yaml`
9) `kubectl apply -f node0.yaml`
10) `kubectl get -o wide`, copy node0 pod IP
11) set copied IP into node0.endpoint.yaml, addresses field
12) `kubectl apply -f node0.endpoint.yaml`
13) `kubectl apply -f node1.yaml`
14) `kubectl get -o wide`, copy node1 pod IP
15) set copied IP into node1.endpoint.yaml, addresses field
16) `kubectl apply -f node1.endpoint.yaml`

// you can also start UDP echo server from echo-udp.yaml file to test UDP connectivity

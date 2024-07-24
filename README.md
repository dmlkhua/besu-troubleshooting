> Reproducing:

1) ensure you are in `dmlkh-besu` namespace
3) `kubectl apply -f genesis.yaml`
4) `kubectl apply -f lb.yaml`
5) `kubectl get svc`, copy value from EXTERNAL_IP column
6) `nslookup {{EXTERNAL_IP}}`, wait until URL is reachable, copy IP
7) replace IPs in `static-nodes.cm.yaml` with the one copied on the previous step
8) replace p2p-host value in node0.yaml and node1.yaml with the load balancer IP
9) `kubectl apply -f static-nodes.cm.yaml`
10) `kubectl apply -f node0.yaml`
11) `kubectl get -o wide`, copy node0 pod IP
12) set copied IP into node0.endpoint.yaml, addresses field
13) `kubectl apply -f node0.endpoint.yaml`
14) `kubectl apply -f node1.yaml`
15) `kubectl get -o wide`, copy node1 pod IP
16) set copied IP into node1.endpoint.yaml, addresses field
17) `kubectl apply -f node1.endpoint.yaml`

// you can also start UDP echo server from echo-udp.yaml file to test UDP connectivity

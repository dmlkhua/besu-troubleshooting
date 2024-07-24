> Reproducing:

1) ensure you are in `dmlkh-besu` namespace
3) `kubectl apply -f genesis.yaml`
4) `kubectl apply -f lb.yaml`
5) wait until load balancers are provisioned
6) find load balancers IPs (use `kubectl get svc` to get the host and then run `nslookup {{EXTERNAL_IP}}` or just find it online)
9) set found IPs to the "static-nodes.cm.yaml" file (IP of "node0" load balancer is for the first entry in the list)
10) set value for "p2p-host" option in node0.yaml and node1.yaml from a node0-lb and node1-lb IPs accordingly
11) `kubectl apply -f static-nodes.cm.yaml`
12) `kubectl apply -f node0.yaml`
13) `kubectl apply -f node1.yaml`

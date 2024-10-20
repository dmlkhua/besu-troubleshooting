
> Env: k8s with https://cert-manager.io and https://traefik.io

- set your relevant cluster host in
  - `static-nodes-configmap.yaml`
  - `node0-ingress.yaml`
  - `node1-ingress.yaml`
  - `node0-certificate.yaml`
  - `node1-certificate.yaml`
- set your relevant storage class in `node0-pvc.yaml` and `node1-pvc.yaml`
- apply certificates:
  ```
  kubectl apply -f issuer.yaml
  kubectl apply -f keystore-pwd.yaml
  kubectl apply -f node0-certificate.yaml
  kubectl apply -f node1-certificate.yaml
  ```
- save node0 ca:
  ```
  k get -o json secret node0-tls | jq -r '.data["ca.crt"]' | base64 --decode -o node0ca.crt
  ```
- save node1 ca:
  ```
  k get -o json secret node1-tls | jq -r '.data["ca.crt"]' | base64 --decode -o node1ca.crt
  ```
- import node0 ca to truststore:
  ```
  keytool -import -file node0ca.crt -alias "node0" -keystore truststore.p12 -storepass abcd1234 -noprompt
  ```
- import node1 ca to truststore:
  ```
  keytool -import -file node1ca.crt -alias "node1" -keystore truststore.p12 -storetype PKCS12 -storepass abcd1234 -noprompt
  ```
- create truststore secret:
  ```
  kubectl create secret generic truststore --from-file=truststore.p12=./truststore.p12 --dry-run=client -o yaml | kubectl apply -f -
  ```
- check correctness of truststore:
  ```
  keytool -list -keystore truststore.p12
  ```
- take a look how besu config TLS is configured: [node1 sts ref](https://github.com/dmlkhua/besu-troubleshooting/blob/main/node1.sts.yaml#L108)
- apply nodes:
  ```
  kubectl apply -f genesis-configmap.yaml
  kubectl apply -f static-nodes-configmap.yaml
  
  kubectl apply -f node0-config.yaml
  kubectl apply -f node1-config.yaml
  
  kubectl apply -f node0-keys.yaml
  kubectl apply -f node1-keys.yaml

  kubectl apply -f node0-pvc.yaml
  kubectl apply -f node1-pvc.yaml
  
  kubectl apply -f node0-service.yaml
  kubectl apply -f node1-service.yaml

  kubectl apply -f node0-ingress.yaml
  kubectl apply -f node1-ingress.yaml
  
  kubectl apply -f node0-sts.yaml
  kubectl apply -f node1-sts.yaml
  ```
- verify that server TLS is configured correctly:
  ```
  openssl s_client -connect node0.your.host.here:443 -servername node0.your.host.here -showcerts
  ```

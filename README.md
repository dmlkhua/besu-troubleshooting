
> Env: k8s

- apply certificates:
  ```
  kubectl apply -f issuer.yaml
  kubectl apply -f keystore-pwd.yaml
  kubectl apply -f node0.certificate.yaml
  kubectl apply -f node1.certificate.yaml
  ```
- save node0 ca:
  ```k get -o json secret node0-tls | jq -r '.data["ca.crt"]' | base64 --decode -o node0ca.crt```
- save node1 ca:
  ```k get -o json secret node1-tls | jq -r '.data["ca.crt"]' | base64 --decode -o node1ca.crt```
- import node0 ca to truststore:
  ```keytool -import -trustcacerts -file node0ca.crt -alias "node0" -keystore truststore.p12 -storepass abcd1234 -noprompt```
- import node1 ca to truststore:
  ```keytool -import -trustcacerts -file node1ca.crt -alias "node1" -keystore truststore.p12 -storepass abcd1234 -noprompt```
- create truststore secret:
  ```kubectl create secret generic truststore --from-file=truststore.p12=./truststore.p12 --dry-run=client -o yaml | kubectl apply -f -```
- check correctness of truststore:
  ```keytool -list -keystore truststore.p12```
- run node with these keys:
  ```shell
  --Xp2p-tls-enabled=true
  --Xp2p-tls-truststore-type=PKCS12
  --Xp2p-tls-truststore-password-file=/etc/besu/keystorepwd/pwd
  --Xp2p-tls-truststore-file=/etc/besu/truststore/truststore.p12
  --Xp2p-tls-keystore-type=PKCS12
  --Xp2p-tls-keystore-password-file=/etc/besu/keystorepwd/pwd
  --Xp2p-tls-keystore-file=/etc/besu/tls/keystore.p12
  ```

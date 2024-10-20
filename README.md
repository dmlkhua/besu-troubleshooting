
> Env: k8s

- apply manifests
- save node0 ca: `k get -o json secret node0-tls | jq -r '.data["ca.crt"]' | base64 --decode -o node0ca.crt`
- save node1 ca: `k get -o json secret node1-tls | jq -r '.data["ca.crt"]' | base64 --decode -o node1ca.crt`
- import node0 ca to truststore: `keytool -import -trustcacerts -file node0ca.crt -alias "node0" -keystore truststore.p12 -storepass abcd1234 -noprompt`
- import node1 ca to truststore:`keytool -import -trustcacerts -file node1ca.crt -alias "node1" -keystore truststore.p12 -storepass abcd1234 -noprompt`
- check correctness of truststore: `keytool -list -keystore truststore.p12`
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

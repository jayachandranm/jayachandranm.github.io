,,openssl req -new -x509 -days <duration> -extensions v3_ca -keyout ca.key -out ca.crt  
openssl genrsa -des3 -out ca.key 2048 (hdbhav)  
openssl req -new -x509 -days 1826 -key ca.key -out ca.crt  
  
,,openssl genrsa -des3 -out server.key 2048   
openssl genrsa -out server.key 2048  
openssl req -new -out server.csr -key server.key {-sha512}  
,,openssl x509 -req -in server.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out server.crt -days <duration>  
openssl x509 -req -in server.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out server.crt -days 360 {-sha512}  
  
,,openssl genrsa -des3 -out client.key 2048  
openssl genrsa -out client.key 2048  
openssl req -new -out client.csr -key client.key  
,,openssl x509 -req -in client.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out client.crt -days <duration>  
openssl x509 -req -in client.csr -CA ca.crt -CAkey ca.key -CAserial ./ca.srl -out client.crt -days 3650 -addtrust clientAuth  
  
openssl x509 -in ca.crt -nameopt multiline -subject -noout  
mosquitto_sub -h localhost -t "#" -v --cafile ca.crt -p 8883 -u "admin" -P "{pass}" --cert client.crt --key client.key  
mosquitto_sub -h localhost -t "#" -v --cafile ca.crt -p 8883 -u "admin" -P "{pass}" (FAIL, no client key)  
mosquitto_sub -h localhost -t "#" -v --cafile ca.crt -p 8883 --cert client.crt --key client.key (FAIL, no user/pass)  
mosquitto_sub -t \$SYS/broker/bytes/\# -v --cafile ca.crt  
  
,,Python  
client1.tls_set(‘c:/python34/steve/MQTT-demos/certs/ca.crt’,tls_version=2)  


CI/CD and test cases.

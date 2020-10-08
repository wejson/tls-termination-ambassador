# tls-termination-ambassador

https://www.getambassador.io/docs/latest/howtos/tls-termination/

create a self signed cert
```shell script
openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -subj '/CN=ambassador-cert' -nodes
```

add a k8s secret
```shell script
kubectl create secret tls tls-cert --cert=cert.pem --key=key.pem
```

deploy ambassador using helm
```shell script
helm install ambassador ./ambassador-chart
kubectl apply -f ./ambassador-chart/tls-tls-config-self-signed.yaml
```

build the docker in mkube
```shell script
eval $(minikube docker-env)
docker build -t http-test ./dn-ms-http-example/.
```
deploy example http service
```shell script
helm install http-test ./http-chart -f ./dn-ms-http-example/values.yaml 
```

mkube tunnel for accessing to LB
```shell script
minikube tunnel
```

curl to test
```shell script
curl -Lk https://127.0.0.1/
```

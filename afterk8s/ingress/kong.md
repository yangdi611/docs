# Kong

## Kong ingress

* NAME: kong LAST 
* DEPLOYED: Fri Dec 13 04:13:38 2019 
* NAMESPACE: default 
* STATUS: deployed 
* REVISION: 1 
* TEST SUITE: None 

> NOTES: 1. Kong Admin can be accessed inside the cluster using: DNS=kong-kong-admin.default.svc.cluster.local PORT=8444

To connect from outside the K8s cluster: 

HOST=$\(kubectl get nodes --namespace default -o jsonpath='{.items\[0\].status.addresses\[0\].address}'\) PORT=$\(kubectl get svc --namespace default kong-kong-admin -o jsonpath='{.spec.ports\[0\].nodePort}'\)

Kong Proxy can be accessed inside the cluster using:

DNS=kong-kong-proxy.default.svc.cluster.localPORT=443To connect from outside the K8s cluster:  HOST=$\(kubectl get nodes --namespace default -o jsonpath='{.items\[0\].status.addresses\[0\].address}'\)  PORT=$\(kubectl get svc --namespace default kong-kong-proxy -o jsonpath='{.spec.ports\[0\].nodePort}'\)


#### This is part 4 of the bootcamp

## another Sidecar container example 
https://github.com/thockin/kubectl-sidecar
**
Example**
Here is a working example which you can use to understand this approach. After applying the manifest you can check it using:

kubectl port-forward deployment/demo-kubectl-sidecar 8080:80
curl http://localhost:8080/this-pod-status.json
curl http://localhost:8080/this-node-status.json

## Pause container 
In Kubernetes when you launch a pod, there is also a pause container that gets spinned up. You can find the pauce.c file [here]()
ctr --namespace k8s.io containers list | grep pause

https://github.com/kubernetes/kubernetes/blob/master/build/pause/linux/pause.c


##PDB
Create deployment
Create pdb
Do rolling update

kubectl set image deployment/nginx-deployment nginx=nginx:1.16.1
kubectl get pods -w

###DownwardAPI
kubectl apply -f downwardapipod.yaml


### QOS 
kubectl get pods nginx-guaranteed -oyaml | grep qos


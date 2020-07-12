# ibm-kubernetes-dashboard

create service account for kubernetes dashboard
```bash
kubectl create serviceaccount dashboard-admin-sa -n kube-system

kubectl create clusterrolebinding dashboard-admin-sa \
  --clusterrole=cluster-admin --serviceaccount=kube-system:dashboard-admin-sa
```

get the token key
```bash
SA_SECRET="$(kubectl get secret -n kube-system \
  --sort-by=.metadata.creationTimestamp | tail -1 | awk '{print $1}')"

kubectl get secret -n kube-system ${SA_SECRET} \
  -o json | jq -r '.data.token' | base64 -d; echo
```

start the proxy
```bash
kubectl proxy
```

Now access Dashboard at:

[`http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/`](
http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/).


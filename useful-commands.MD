# Get everything
kubectl get all

# Get Definition to create a new one:
kubectl get pod PODNAME -o yaml > pod-definition.yaml

# Edit resources:
kubectl edit pod PODNAME
kubectl edit replicaset REPLICASETNAME
    then (for replicaset only)
    kubectl delete --all pods
kubectl edit deployment my-deployment


# Update object:
kubectl replace -f file.yml

# Scale replicaset
kubectl scale --replicas=X -f file.yaml
or
kubectl scale --replicas=X replicaset REPLICASET-NAME

# Changing outputs:
-o {json/name/wide/yaml}

# Getting pods on all namespaces:
kubectl get pods --all-namespaces

# Switching Namespaces:
kubectl config set-context $(kubectl config current-context) --namespace=NEW_NAMESPACE

# Services' DNS structure
SV-service.NAMESPACE.svc.cluster.local

# Test the command without executing it:
--dry-run=client

# Generate a easy yaml file:
kubectl run nginx --image=nginx --labels='key=value' --port=80 --dry-run=client -o yaml
kubectl create deployment --image=nginx nginx --replicas=3 --dry-run -o yaml
kubectl expose pod redis --port=6379 --name redis-service --dry-run=client -o yaml
kubectl create service nodeport nginx --tcp=80:80 --node-port=30080 --dry-run=client -o yaml

# ConfigMaps
kubectl create configmap CONFIG-NAME --from-literal=KEY=VALUE
kubectl create configmap CONFIG-NAME --from-file=FILE
kubectl create -f FILE.yml

# Secrets
kubectl create secret generic SECRET_NAME --from-literal=KEY-VALUE
kubectl create secret generic SECRET_NAME --from-file=KEY-VALUE

# Service Accounts
kubeclt get serviceaccount
kubectl create serviceaccount ACCOUNT_NAME
kubectl describe secret ACCOUNT_NAME TOKEN_NAME

# Taint and Toleration
kubectl taint nodes node-name key=value:taint-effect 
    tainf-effect: NoSchedule | PreferNoSchedule | NoExecute

# Remove Taint from node
kubectl taint nodes node-name TAINT-COMPLETE-NAME-
    ex: kubectl taint nodes controlplane node-role.kubernetes.io/master:NoSchedule-

# Label node
kubectl label nodes NODE-NAME key=value

# Inspect some file inside pod
kubectl -n NAMESPACE exec -it POD -- cat /log/app.log
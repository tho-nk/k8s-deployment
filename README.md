# Setting Up and Managing a Kind Cluster

## 1. Create the Kind Cluster

Run the following command to create your Kind cluster using the configuration file:

```
kind create cluster --config ./clusters/kind/kind-config.yaml --name massy-cluster
```

## 2. Apply Kubernetes Manifests

If you want to apply them to a specific namespace, you can use the --namespace flag:

```
kubectl create namespace massy-rendering
kubectl config set-context --current --namespace=massy-rendering

kubectl delete all --all -n massy-rendering
```



Apply the Kubernetes manifests for your project components. You can organize these manifests into directories, such as deployments, services, etc.

```
kubectl apply -f deployments/nkAnimation/nkAnimation.yaml --namespace massy-rendering
kubectl apply -f deployments/nkApplication/nkApplication.yaml --namespace massy-rendering
kubectl apply -f deployments/nkConnect/nkConnect.yaml --namespace massy-rendering
kubectl apply -f deployments/nkDataProvider/nkDataProvider.yaml --namespace massy-rendering
```

You can apply all the YAML files in your deployment folder at once using the kubectl apply command with the -f option and specifying the directory path. Here's how you can do it:

```
kubectl apply -f deployments/*/*.yaml
```

```
kubectl apply -f deployments/*/*.yaml --namespace massy-rendering
```

## 3. Monitor Cluster and Pods

Check the status of your Kind cluster:

```
kubectl cluster-info --context massy-cluster    
```

List the running pods:

```
kubectl get pods --all-namespaces
```

## 4. Access Services
Access your services using the appropriate service endpoints. For example, if you have a service with a LoadBalancer type, you might need to wait for an external IP to be assigned:

```
kubectl get services --watch
```

Once the external IP is assigned, you can access your services using that IP.

## 5. Delete the Kind Cluster (Optional)
When you're done testing your project, you can delete the Kind cluster:

```
kind delete cluster --name massy-cluster
kubectl delete service nkconnect-service
kubectl delete deployment nkconnect-deployment
kubectl delete pod nkconnect
```
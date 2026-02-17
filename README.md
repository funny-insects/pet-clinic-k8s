# K8s Spring Pet Clinic

## Usage 
* Uncheck `Use containerd for pulling and storing images` then restart Docker Desktop. <https://github.com/kubernetes-sigs/kind/issues/3795>

* Create a KinD cluster that can persist the repo data on the host machine
```bash
kind create cluster --config=kind-config.yaml
```

```bash
kind load docker-image real-spring-petclinic-piepline-runner:latest
```

```bash
kind load docker-image sonatype/nexus3:latest
```

```bash
kubectl create namespace bootcamp
```

```bash
kubectl -n bootcamp create secret generic runner-secret --from-literal=GITHUB_URL="https://github.com/funny-insects/real-spring-petclinic" --from-literal=RUNNER_TOKEN="<TOKEN>"
```

```bash
kubectl apply -f deployment-*.yaml
```

```bash
kubectl apply -f persistent-volume.yaml
```

```bash
kubectl apply -f service-nexus.yaml
```

* Get the password to log in to your Nexus repo
```bash
kubectl exec -n bootcamp $(kubectl get pods -n bootcamp -l app=nexus -o jsonpath='{.items[0].metadata.name}') -- cat /nexus-data/admin.password
```

* Make our nexus repo accessible to our host machine
```bash
kubectl port-forward -n bootcamp svc/nexus 8081:8081
```

* Now in your web browser navigate to http://localhost:8081

* Login with the username `admin` and the randomly generated password we catted out earlier.

* Now you will be prompted to change your password. Change it to `admin`, unless you want to have to update the secrets on the `real-spring-petclinic` repo.

* Create a Maven2 Hosted repository called `spring-petclinic` set Version Policy to Mixed and Redeploy Policy to Allow.

* Try running the workflow on the `real-spring-petclinic` GitHub repo  

* Verify that your bind mount for persistenting the repo data is not empty
```bash
ls /Users/$USER/kind-storage/nexus-data
```


## Useful commands for debuging
List images in Kind  cluster
```bash
docker exec -it my-node-name crictl images
```

```bash
kubectl get pods [-n <namespace>] [--show-labels]
```

```bash
kubectl get rs [-n <namespace>]
```

```bash
kubectl get deployments [-n <namespace>]
```

```bash
kubectl edit secrets runner-secret -n bootcamp
```

```bash
kubectl rollout restart deployment/runner-deployment -n bootcamp
```

```bash
kubectl get pods -n bootcamp -l app=runner
```

```bash
kubectl create secret generic runner-secret \
    --from-literal=GITHUB_URL="https://github.com/funny-insects/real-spring-petclinic" \
    --from-literal=RUNNER_TOKEN="<your-new-token>" \
    -n bootcamp \
    --dry-run=client -o yaml | kubectl apply -f -
```

```bash
kubectl get secret runner-secret -n bootcamp -o jsonpath='{.data.GITHUB_URL}' | base64 -d && echo
```

```bash
kubectl logs -n bootcamp <pod_name> --tail=50
```

```bash
kubectl describe pod -n bootcamp <pod_name>
```

```bash
kubectl get services [-n <namespace>]
```

```bash
kubectl get pv nexus-pv -o yaml
```


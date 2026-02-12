# K8s Spring Pet Clinic

## Usage 
* Uncheck `Use containerd for pulling and storing images` then restart Docker Desktop. <https://github.com/kubernetes-sigs/kind/issues/3795>

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
kubectl apply -f <manifest.yaml>
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
kubectl logs -n bootcamp runner-deployment-5576dbb989-t7kls --tail=50
```

```bash
kubectl describe pod -n bootcamp runner-deployment-5cd789ff5-nwz52
```

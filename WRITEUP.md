### 2. Build the application image and make it available to minikube

```bash
# build image
docker build -t griffin:0.0.1 . --no-cache
# load it into minikube
minikube image load griffin:0.0.1
```

### 4. Encrypt the secret with SOPS

```bash
age-keygen -o age.agekey
cat age.agekey |
    kubectl create secret generic sops-age \
        --namespace=flux-system \
        --from-file=age.agekey=/dev/stdin

sops --encrypt --in-place apps/griffin/secret.yaml
```

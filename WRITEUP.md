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

### 5. Prove connectivity

```bash
❯ kubectl port-forward svc/griffin 8080:80 -n griffin

# in a separate tab
❯ curl localhost:8080
Greetings from deployment.yaml
Secret loaded: yes
```

### 6. Automate the bootstrap with Ansible

```bash
# age.agekey used to encrypt griffin secret must be present
export GITHUB_USER="your-github-user"
export GITHUB_TOKEN="your-github-pat"

ansible-playbook playbook.yml
```

### 2. Build the application image and make it available to minikube

```bash
# build image
docker build -t griffin:0.0.1 . --no-cache
# load it into minikube
minikube image load griffin:0.0.1
```

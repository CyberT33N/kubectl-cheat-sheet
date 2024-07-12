# kubectl-cheat-sheet



<br><br>

## Install
- https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/
```shell
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
kubectl version --client
```

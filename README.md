# kubectl Cheat Sheet



<br><br>

## Install
- https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/
```shell
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
kubectl version --client
```




<br><br>
<br><br>
______________________________________
______________________________________

<br><br>
<br><br>

# Namespace

<br><br>
<br><br>

## Create namespace
```shell
kubectl create namespace dev
```





<br><br>
<br><br>
______________________________________
______________________________________

<br><br>
<br><br>



## Logs

<br><br>

## Get previous logs of pod
```shell
kubectl logs mongodb-data-0 -n test --previous
```








<br><br>
<br><br>


## Check current resource usage
```shell
kubectl top nodes
```

















<br><br>
<br><br>
____________________________________________________________
____________________________________________________________
<br><br>
<br><br>


## Events

<br><br>

### Check event logs
```shell
kubectl get events -n test --sort-by='.metadata.creationTimestamp'
```













<br><br>
<br><br>
____________________________________________________________
____________________________________________________________
<br><br>
<br><br>

## Port Forward
- https://kubernetes.io/docs/reference/kubectl/cheatsheet/#updating-resources
```shell
kubectl port-forward -n "green" deployment/test-manager 9211:80
kubectl port-forward -n "green" service/bitnami-elasticsearch 1335:9200
```

## Kill Port Forward
```shell
sudo kill -9 $(sudo lsof -t -i:9211) & sudo kill -9 $(sudo lsof -t -i:1335)
```






<br><br>
<br><br>
____________________________________________________________
____________________________________________________________
<br><br>
<br><br>

## Service

<br><br>

### Get Service
```shell
kubectl get svc --namespace=dev

NAME          TYPE       CLUSTER-IP     EXTERNAL-IP   PORT(S)           AGE
mongodb-dev   NodePort   10.107.10.66   <none>        27017:30666/TCP   61m
```




















<br><br>
<br><br>
____________________________________________________________
____________________________________________________________
<br><br>
<br><br>

## Pod
- https://kubernetes.io/docs/reference/kubectl/cheatsheet/

<br><br>

### Get Pod Names
```shell
kubectl get pod -n myNamespace
```

<br><br>

### Kill pod
```shell
kubectl delete pod your-pod-xxxxxx-xxxx6 -n myNamespace
```








<br><br>
<br><br>
____________________________________________________________
____________________________________________________________
<br><br>
<br><br>

## Deployment

<br><br>

### Restart deployment
- https://kubernetes.io/docs/reference/kubectl/cheatsheet/#updating-resources
```shell
kubectl rollout restart deployment deploymentName -n myNamespace
```

<br><br>
<br><br>








<br><br>
<br><br>

## change namespace
```bash
kubectl config set-context --current --namespace=namespaceNameHere
```

## change context
```bash
# Show available contexts
kubectl config get-contexts

# Switch context
kubectl config use contextNameHere
```
























<br><br>
<br><br>
____________________________________________________________
____________________________________________________________
<br><br>
<br><br>

# Job

<br><br>
 
### Create job
```bash
kubectl config use-context minikube
kubectl apply -f test/job.yaml -n namespaceNameHere
```
-f means file
- **In order to re-apply the job you must delete it before


<br><br>
<br><br>

 ### Delete job
 ```bash
kubectl config use-context minikube
kubectl delete -f test/job.yaml -n namespaceNameHere
```
-f means file









 
<br><br>
<br><br>
____________________________________________________________
____________________________________________________________
<br><br>
<br><br>

# Cronjob
 
### Create Cronjob
```bash
kubectl apply -f test/cronjob.yaml -n namespaceNameHere
```

```yaml
apiVersion: batch/v1beta1
kind: CronJob
metadata:
  name: mongodb-connection-test
  labels:
    group: test
spec:
  suspend: false
  schedule: "* * * * *"
  jobTemplate:
    metadata:
      name: mongodb-connection-test
      labels:
        group: test
    spec:
      template:
        spec:
          containers:
          - image: nexus.test.local:5000/test:53e89b8
            name: mongodb-connection-test
            env:
            - name: MONGO_OPTION_useNewUrlParser
              value: 'true'
            - name: MONGO_OPTION_useUnifiedTopology
              value: 'true'
            - name: MONGODB_USERNAME
              value: root
            - name: MONGODB_PASSWORD
              valueFrom:
                secretKeyRef:
                  key: mongodb-root-password
                  name: mongodb
            - name: AUTHENTICATION_DATABASE
              value: admin
            - name: MONGODB_ADDRESS
              value: 'mongodb://${MONGODB_USERNAME}:${MONGODB_PASSWORD}@mongodb:27017/test?authSource=${AUTHENTICATION_DATABASE}'
            resources:
              limits:
                cpu: 100m
                memory: 100Mi
              requests:
                cpu: 50m
                memory: 50Mi
          restartPolicy: OnFailure
 ```
 
 
 














 
<br><br>
<br><br>
___________________________________________
___________________________________________

<br><br>
<br><br>
 
## Pod

<br><br>

#### Create Pod
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: envar-demo
  labels:
    purpose: demonstrate-envars
spec:
  containers:
  - name: envar-demo-container
    image: gcr.io/google-samples/node-hello:1.0
    env:
    - name: DEMO_GREETING
      value: "Hello from the environment"
    - name: DEMO_FAREWELL
      value: "Such a sweet sorrow"
```
```bash
kubectl apply -f https://k8s.io/examples/pods/inject/envars.yaml
```







<br><br>
<br><br>
___________________________________________
___________________________________________

<br><br>
<br><br>

## Logs

<br><br>

#### Get logs from pod
```bash
kubectl -n green logs sample-1234abcd
```

<br><br>

#### Get logs from pod with multiple container
```bash
kubectl -n green logs sample-1234abcd -c sample
```

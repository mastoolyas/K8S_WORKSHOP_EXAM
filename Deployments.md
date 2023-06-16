# Deployments
## 1. Create a deployment called webapp with image nginx with 5 replicas
### a. Use the below command to create a yaml file.
`kubectl create deploy webapp --image=nginx --replicas=5 --dry-run=client -o yaml > webapp.yaml`
## 2. Get the deployment rollout status
`kubectl rollout status deploy webapp`\
deployment "webapp" successfully rolled out
## 3. Get the replicaset that created with this deployment
`kubectl get rs -l app=webapp`
## 4. EXPORT the yaml of the replicaset and pods of this deployment
`kubectl get rs -l app=webapp -o yaml >rs.yaml`\
`kubectl get po -l app=webapp -o yaml >pod.yaml`
## 5. Delete the deployment you just created 
`kubectl delete deploy webapp`
### and watch all the pods are also being deleted
`kubectl get po -l app=webapp -w`
## 6. Create a deployment of webapp with image nginx:1.17.1 with container port 80 and verify the image version
`kubectl create deploy webapp --image=nginx:1.17.1 --dry-run=client -o yaml > webapp.yaml`
### b. add the port section (80) and create the deployment
`apiVersion: apps/v1\  
kind: Deployment \ 
metadata:  \
  creationTimestamp: null \ 
  labels:\
    app: webapp \ 
  name: webapp \ 
spec:  \
  replicas: 1 \ 
  selector:\
    matchLabels: \ 
      app: webapp \ 
  strategy: {}  \
  template:  \
    metadata:  \
      creationTimestamp: null \ 
      labels:  \
        app: webapp \
    spec:  \
      containers:  \
      - image: nginx:1.17.1  \
        name: nginx  \
        ports:  \
        - containerPort: 80  \
        resources: {}  \
status: {}`  \
`kubectl create -f webapp.yaml`
## 7. Update the deployment with the image version 1.17.4
`kubectl set image deploy/webapp nginx=nginx:1.17.4`
###  and verify
`kubectl describe deploy webapp | grep Image`
## 8. Check the rollout history and make sure everything is ok after the update
`kubectl rollout history deploy webapp`
## 9. Undo the deployment to the previous version 1.17.1 and verify Image has the previous version
`kubectl rollout undo deploy webapp`
## 10. Update the deployment with the wrong image version 1.100
`kubectl set image deploy/webapp nginx=nginx:1.100`
### and verify something is wrong with the deployment
`kubectl rollout status deploy webapp`
`kubectl get pods`
### b. Undo the deployment with the previous version 
`kubectl rollout undo deploy webapp`
### and verify everything is Ok
`kubectl rollout status deploy webapp`
### c. 
`kubectl rollout history deploy webapp --revision=7`
### d.
`kubectl rollout history deploy webapp`
### e.
`kubectl set image deploy/webapp nginx=nginx:latest`\
`kubectl rollout history deploy webapp`

## 12.
` `
## 13. Clean the cluster by deleting deployment ploy webappand hpa you just created
`kubectl delete deploy webapp'
## 14. 4.Create a job and make it run 10 times one after one
`kubectl create job hello-job --image=busybox --dry-run==client -o yaml -- echo "Hello I am from job" >  > hello-job.yaml` 
### a. Add to the above job completions: 10 inside the yaml
apiVersion: batch/v1\
kind: Job\
metadata:\
  creationTimestamp: null\
  name: hello-job\
spec:\
  completions: 10\
  template:\
    metadata:\
      creationTimestamp: null\
    spec:\
      containers:\
      - command:\
        - echo\
        - Hello I am from job\
        image: busybox\
        name: hello-job\
        resources: {}\
      restartPolicy: Never\
status: {} the yaml
### and create it by:
`kubectl create -f hello-job.yaml`

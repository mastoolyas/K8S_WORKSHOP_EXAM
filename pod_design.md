# Pod Design Questions
## 1. Get the pods with label information
`kubectl get pods --show-labels`
## 2. Create 5 nginx pods in which two of them is labeled env=prod and three of them is labeled env=dev
`kubectl run nginx-dev1 --image=nginx --restart=Never --labels=env=dev`
`kubectl run nginx-dev2 --image=nginx --restart=Never --labels=env=dev`
`kubectl run nginx-dev3 --image=nginx --restart=Never --labels=env=dev`
`kubectl run nginx-prod1 --image=nginx --restart=Never --labels=env=prod`
`kubectl run nginx-prod2 --image=nginx --restart=Never --labels=env=prod`
## 3. Verify all the pods are created with correct labels
`kubeclt get pods --show-labels`
## 4. Get the pods with label env=dev
`kubectl get pods -l env=dev`
## 5. Get the pods with label env=dev and also output the labels
`kubectl get pods -l env=dev --show-labels`
## 6. Get the pods with label env=prod
`kubectl get pods -l env=prod`
## 7.Get the pods with label env=prod and also output the labelsget pods -l env=prod
`kubectl get pods -l env=prod --show-labels`
## 8. Get the pods with label env
`kubectl get pods -L env`
## 9. Get the pods with labels env=dev and env=prod
`kubectl get pods -l 'env in (dev,prod)'`
## 10. Get the pods with labels env=dev and env=prod and output the labels as well
`kubectl get pods -l 'env in (dev,prod)' --show-labels`
## 11. Change the label for one of the pod to env=uat and list all the pods to verify
`kubectl label pod/nginx-dev3 env=uat --overwrite`
`kubectl get pods --show-labels`
## 12. Remove the labels for the pods that we created now and verify all the labels are removed
`kubectl label pod nginx-dev{1..3} env-`
`kubectl label pod nginx-prod{1..2} env-`
`kubectl get po --show-labels`
## 13. Let’s add the label app=nginx for all the pods and verify
`kubectl label pod nginx-dev{1..3} app=nginx`
`kubectl label pod nginx-prod{1..2} app=nginx`
`kubectl get po --show-labels`
## 14. Get all the nodes with labels (if using k3d you would get only master node)
`kubectl get nodes --show-labels`
## 15. Label the worker node nodeName=nginxnode
`kubectl label node <name-of-my-node> nodeName=nginxnode`
## Create a Pod that will be deployed on the worker node 
`kubectl run nginx --image=nginx --restart=Never --dry-run =client -o yaml > pod.yaml`
### Add the nodeSelector to the below and create the pod
`apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: nginx
  name: nginx
spec:
  nodeSelector:
    nodeName: nginxnode
  containers:
  - image: nginx
    name: nginx
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}`
### and then create the pod :
`kubectl create -f pod.yaml`
## 17. Verify the pod that it is scheduled with the node selector on the right node
`kubectl describe po nginx | grep Node-Selectors`



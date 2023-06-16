# Config map
## 1. Create a file called config.txt with two values ​​key1=value1 and key2=value2 and verify the file
`cat >> config.txt << EOF
key1=value1
key2=value2
EOF`

`cat config.txt`
## 2.Create a configmap named keyvalcfgmap and read data from the file config.txt
`kubectl create cm keyvalcfgmap --from-file=config.txt`
### and verify that configmap is created correctly
`kubectl get cm keyvalcfgmap -o yaml`
## 3. Create an nginx pod 
`kubectl run nginx --image=nginx --dry-run=client -o yaml > nginx-pod.yml`
### and load environment values ​​from the above configmap keyvalcfgmap
`    env:
    - name: NAME1
      valueFrom:
        configMapKeyRef:
          name: keyvalcfgmap       
          key: key1
    - name: NAME2
      valueFrom:
        configMapKeyRef:
          name: keyvalcfgmap      
          key: key2
`
### update the pod
kubectl create -f nginx-pod.yml
### and exec into the pod and verify the environment variables and delete the pod
kubectl exec -it nginx -- env


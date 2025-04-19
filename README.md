# synechron_12th

```

Welcome to OpenDev Etherpad!
https://github.com/ramannkhanna2/synechron_12th.git
https://etherpad.opendev.org/p/r.712790ad22fc594b8bb95fdc6283191c




raman khanna


development code >>  testing  >>> package /artefact the code ( build )  >> deploy to server (vm)                                                

vs

development code >>  testing  >>> package /artefact the code ( build )  >>  docker image creation     >>  deploy to container 





docker , podman , rocketd , crio : container runtime tools  : engineer 
-- saves the resources
-- light weight (consuming less space) ---- ( more containers as compared to vm)





why we moved to kubernetes from docker only to docker+kubernetes ?





Containerization ::::
kubernetes : orchestrator :managing the containers  : manager 
     , docker and other tools 
     
     
     opens source 
     
     minikube clsuter : testing clsuter ;
     kubeadm cluster , managed services : eks , aks , gks 
     
     
     
     
     kubeadm cluster : master , two worker nodes ...
     
     3 machines : frame 
     
     
     
     
     
       docker images>> 
       
       kubernetes (manager )>>docker container  (engineer )   >>> containers    >>> my application to be served to our users 
       
       
       ==========================
       
       aws free tier 
       
       
       region : ohio or any region
       ...
       
       3 machines ( one master , 2 workers )
       
       kube-synechron-master  : t2.medium
        kube-synechron-w1 : t2.micro
         kube-synechron-w2 : t2.micro
       
    
     ami : ubuntu
     
     -- create a keypair for all 3 : ohio-key
     
     -- create a sg : all traffic enabled ..
     
     
     storage : 20gb for master , and 8-10 gb fir worker ...
     
     and then launch ....
     ================================================================
     
     
     
     documentation links for refernce :
         
         https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/
         https://kubernetes.io/docs/reference/networking/ports-and-protocols/
         
         =============================================
         
         
         --  all all nodes :
            
         
         ubuntu@ip-172-31-1-135:~$ sudo -i
root@ip-172-31-1-135:~# hostnamectl set-hostname master
root@ip-172-31-1-135:~# bash
root@master:~#


ubuntu@ip-172-31-1-135:~$ sudo -i
root@ip-172-31-1-135:~# hostnamectl set-hostname w1
root@ip-172-31-1-135:~# bash
root@master:~#


ubuntu@ip-172-31-1-135:~$ sudo -i
root@ip-172-31-1-135:~# hostnamectl set-hostname w2
root@ip-172-31-1-135:~# bash
root@master:~#

-------------------------------------------------------------

--- execute the first step ....
     
     
     
     ===========================================
     
      docker ps
   23  kubectl get nodes
   24  kubectl get pods
   25  kubectl get pods -A
   26  clear
   27  kubectl get nodes
   28  cat cluster_initialized.txt
   29  kubectl get nodes
   30  ls
   31  clear
   32  kubectl get nodes
   33  kubectl get pods -A
   34  clear
   35  kubectl get pods -A -o wide

============================================



 docker ps
   69  alias k=kubectl
   70  k get nodes
   71  k get pods -A
   72  clear
   73  k api-resources
   74  clear
   75  k get pods
   76  k get pods --all-namespaces
   77  k get pods -A
   78  clear
   79  k get pods -A
   80  clear
   81  k get pods -A -o wide
   82  kubectl create ns raman
   83  k api-resources
   84  clear
   85  k run raman-app --image httpd
   86  k get po
   87  k get pods -o wide
   88  k describe pod raman-app
   89  clear
   90  k describe node w1
   91  clear
   92  k get pods -A
   93  clear
   94  k get pods -A -o wide
   95  k run raman-app2 --image nginx
   96  k delete pod raman-app2
   97  k run raman-app2 --image nginx -n raman
   98  clear
   99  k get po -A
  100  clear
  101  k get po -A -o wide
  102  clear
  103  k get pods -A | grep -i w1
  104  k get pods -A
  105  k get pods -A -o wide| grep -i w1


=========================================================


root@master:~# cat pod.yml
apiVersion: v1
kind: Pod
metadata:
  name: raman-nginx
spec:
  containers:
  - name: raman-con1
    image: nginx:1.14.2
    ports:
    - containerPort: 80


vi pod.yml
  124  clear
  125  k get pods
  126  ls
  127  cat pod.yml
  128  k create -f pod.yml
  129  k get pods
  130  k create -f pod.yml -n raman
  131  k get pods -A
  132  CLEAR
  133  k get pods
  134  clear
  135  k get pods
  136  k delete pods --all
  137  ls
  138  k create -f pod.yml
  139  k get pods
  140  k get pods -o wide
  141  k delete pod raman-nginx


================================

root@master:~# cat multiconpod.yml
apiVersion: v1
kind: Pod
metadata:
  name: raman-multipod
spec:
  containers:
  - name: raman-con1
    image: nginx:1.14.2
    ports:
    - containerPort: 80
  - name: raman-con2
    image: redis
    ports:
    - containerPort: 6379




 vi multiconpod.yml
  150  k get pods
  151  cat multiconpod.yml
  152  k create -f multiconpod.yml
  153  k get pods
  154  k describe pod
  155  clear
  156  k get pods
  157  k get pods  -o wide
  158  cat multiconpod.yml
  159  k exec -it raman-multipod -c raman-con1 -- /bin/bash
  160  cat multiconpod.yml
  161  k exec -it raman-multipod -c raman-con2 -- /bin/bash
  162  clear
  163  k exec -it raman-multipod -c raman-con2 -- ls
  164  k exec -it raman-multipod -c raman-con2 -- ls /
  165  k exec -it raman-multipod -c raman-con1 -- ls
  166  k exec -it raman-multipod -c raman-con1 -- mkdir ramantest
  167  k exec -it raman-multipod -c raman-con1 -- ls
  168  k exec -it raman-multipod -c raman-con1 -- /bin/bash

================================================================






k create deploy ramandep --replicas 5 --image httpd
  185  k get pods
  186  k delete pod ramandep-676757c4b5-7g26p
  187  k get pods
  188  clear
  189  k get all
  190  k get pods
  191  k delete pods --all
  192  k get pods
  193  clear
  194  k get deploy
  195  k delete deploy ramandep




----------------


root@master:~# cat deploy.yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    project: dev
spec:
  replicas: 10
  selector:
    matchLabels:
      training: synechron
  template:
    metadata:
      labels:
        training: synechron
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80





 k create deploy ramandep --replicas 5 --image httpd
  198  k get pods
  199  k get pods -o wide
  200  history
  201  clear
  202  ls
  203  vi deploy.yml
  204  k create -f deploy.yml
  205  k get pods
  206  k delete deploy ramandep
  207  clear
  208  k get deploy
  209  k get pods
  210  k get rs
  211  k describe pod
  212  clear
  213  k get all
  214  k describe deploy nginx-deployment
  215  clear
  216  k describe rs nginx-deployment-58b7c77b7d
  217  clear
  218  k describe pods | grep -i labels
  219  k describe pods | grep -i labels -A2
  220  clear
  221  cat deploy.yml
  222  k get pods
  223  vi deploy.yml
  224  k apply -f deploy.yml
  225  k get po
  226  clear
  227  k get pods --selector "training: synechron"
  228  k get pods --selector "training=synechron"
  229  k get deploy --selector "training=synechron"
  230  k get deploy --selector "project=dev"
  231  cat deploy.yml
  232  k scale deploy nginx-deployment --replicas 1
  233  k get pods
  234  k logs  k logs
  235  k logs  k nginx-deployment-58b7c77b7d-vxhkq
  236  k logs  nginx-deployment-58b7c77b7d-vxhkq


==================================================




k run ramanapp --image httpd
  247  k get pods
  248  k get pods -o wide
  249  k expose pod ramanapp --type NodePort --target-port 80 --port 80 --name ramanservice1
  250  k get pod -o wide
  251  k get svc -o wide
  252  curl 10.99.85.94
  253  k get po -A
  254  k get po -A -o wide
  255  clear
  256  k get svc
  257  k get po
  258  k run ramanapp2 --image nginx
  259  k expose pod ramanapp2 --type NodePort --target-port 80 --port 80 --name extsvc2
  260  k get svc
  261  k get po -o wide


-------------------------------------------



root@master:~# cat deploy.yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: custom-deployment
  labels:
    project: dev
spec:
  replicas: 1
  selector:
    matchLabels:
      training: synechron
  template:
    metadata:
      labels:
        training: synechron
    spec:
      containers:
      - name: customcon
        image: ramann123/hotstar:latest
        ports:
        - containerPort: 3000
root@master:~# cat service.yml
apiVersion: v1
kind: Service
metadata:
  name: custom-svc
spec:
  type: NodePort
  selector:
    training: synechron
  ports:
    - port: 3000
      # By default and for convenience, the `targetPort` is set to
      # the same value as the `port` field.
      targetPort: 3000
      # Optional field
      # By default and for convenience, the Kubernetes control plane
      # will allocate a port from a range (default: 30000-32767)
      nodePort: 30007



history :
    
    
     clear
  263  history
  264  clear
  265  k get svc
  266  k get pods
  267  k delete pods --all
  268  k get svc
  269  k delete svc --all
  270  ls
  271  vi deploy.yml
  272  k create -f deploy.yml
  273  k get pods
  274  clear
  275  k get pods
  276  k get pods -o wide
  277  curl  192.168.190.84
  278  curl  192.168.190.84:3000
  279  k get pods -o wide
  280  clear
  281  ls
  282  k get al
  283  k get all
  284  vi service.yml
  285  cat deploy.yml
  286  vi service.yml
  287  k create -f service.yml
  288  clear
  289  k get all
  290  k get pods -o wide
  291  k get ep
  292  k get ep my-service
  293  k ep my-service
  294  k get ep my-service
  295  k get ep --all
  296  k describe service my-service
  297  ls
  298  cat service.yml
  299  ls
  300  cat deploy.yml
  301  k get ep
  302  clear
  303  ls
  304  k get svc
  305  k delete svc my-service
  306  vi service.yml
  307  k get pods --selector "training=synechron"
  308  k get pods
  309  k delete deploy --all
  310  clear
  311  ls
  312  k get pods
  313  vi deploy.yml
  314  k get pods
  315  k delete pods --all
  316  clear
  317  k get pods
  318  cat deploy.yml
  319  k create -f deploy.yml
  320  k get pods
  321  k describe pod custom-deployment-6d765cbdd8-gz78z
  322  k get nodes
  323  k describe node w1
  324  k get nodes
  325  ls
  326  cat deploy.yml
  327  cat service.yml
  328  k get nodes
  329  k get pods
  330  k get pods -o wide
  331  cat deploy.yml
  332  vi service.yml
  333  k create -f service.yml
  334  k get svc
  335  k get svc custom-svc
  336  k describe svc custom-svc
  337  k get pods
  338  k get pods -o wide
  339  k get pods
  340  k get svc
  341  k get svc k expose deployment custom-deployment --type Loadbalancer --nameextsvc2 --port 3000 --target-port 3000
  342  k get svc k expose deployment custom-deployment --type Loadbalancer --name extsvc2 --port 3000 --target-port 3000
  343  k get svc k expose deployment custom-deployment --type LoadBalancer --name extsvc2 --port 3000 --target-port 3000
  344  k expose deployment custom-deployment --type LoadBalancer --name extsvc2 --port 3000 --target-port 3000
  345  k get svc

------------------------------------

github link to observe : 
    
                     https://github.com/ramannkhanna2/kubernetes_end2end_front-backend.git
                     


==================================================================


rollbck :
    
    
    
    root@master:~# cat deploy.yml 
apiVersion: apps/v1
kind: Deployment
metadata:
  name: raman-deployment
  labels:
    project: dev
spec:
  replicas: 10
  selector:
    matchLabels:
      training: synechron
  template:
    metadata:
      labels:
        training: synechron
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.1
        ports:
        - containerPort: 80



k apply -f deploy.yml --record=true
   64  k get pods
   65  k get deploy
   66  k get rs


root@master:~# k get pods
NAME                                READY   STATUS    RESTARTS   AGE
raman-deployment-679694d477-2jmmw   1/1     Running   0          19s
raman-deployment-679694d477-7fv94   1/1     Running   0          19s
raman-deployment-679694d477-d244n   1/1     Running   0          19s
raman-deployment-679694d477-dqn4k   1/1     Running   0          19s
raman-deployment-679694d477-dsmbg   1/1     Running   0          19s
raman-deployment-679694d477-gf7fq   1/1     Running   0          19s
raman-deployment-679694d477-jf2qr   1/1     Running   0          19s
raman-deployment-679694d477-kl2ml   1/1     Running   0          19s
raman-deployment-679694d477-p29jr   1/1     Running   0          19s
raman-deployment-679694d477-w65hd   1/1     Running   0          19s
root@master:~# k get deploy
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
raman-deployment   10/10   10           10          26s
root@master:~# k get rs
NAME                          DESIRED   CURRENT   READY   AGE
raman-deployment-679694d477   10        10        10      29s

k get pods
   69  k describe pod raman-deployment-679694d477-2jmmw | grep -i image
   70  k rollout status deploy raman-deployment




# seecond version :
    


root@master:~# cat deploy.yml 
apiVersion: apps/v1
kind: Deployment
metadata:
  name: raman-deployment
  labels:
    project: dev
spec:
  replicas: 10
  selector:
    matchLabels:
      training: synechron
  template:
    metadata:
      labels:
        training: synechron
    spec:
      containers:
      - name: nginx
      # image: nginx:1.14.1
        image: nginx:1.14.2
        ports:
        - containerPort: 80
        
        
        
        k apply -f deploy.yml --record=true
   75  k rollout status deploy raman-deployment
   76  k get pods
   77  k get deploy
   78  k get rs
   
   
   
   
   
   root@master:~# k get all
NAME                                    READY   STATUS    RESTARTS   AGE
pod/raman-deployment-58b7c77b7d-29n5m   1/1     Running   0          2m38s
pod/raman-deployment-58b7c77b7d-422j4   1/1     Running   0          2m46s
pod/raman-deployment-58b7c77b7d-8l8pl   1/1     Running   0          2m40s
pod/raman-deployment-58b7c77b7d-ggfzt   1/1     Running   0          2m46s
pod/raman-deployment-58b7c77b7d-h9dtr   1/1     Running   0          2m39s
pod/raman-deployment-58b7c77b7d-n9wsl   1/1     Running   0          2m46s
pod/raman-deployment-58b7c77b7d-qg94z   1/1     Running   0          2m46s
pod/raman-deployment-58b7c77b7d-thd8d   1/1     Running   0          2m39s
pod/raman-deployment-58b7c77b7d-vhmph   1/1     Running   0          2m39s
pod/raman-deployment-58b7c77b7d-xp2vf   1/1     Running   0          2m46s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   7d16h

NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/raman-deployment   10/10   10           10          12m

NAME                                          DESIRED   CURRENT   READY   AGE
replicaset.apps/raman-deployment-58b7c77b7d   10        10        10      2m46s
replicaset.apps/raman-deployment-679694d477   0         0         0       12m



 k rollout history deployment raman-deployment
   86  kubectl rollout history deployment raman-deployment --revision=1
   87  kubectl rollout history deployment raman-deployment --revision=2
   
   
   
   
   k apply -f deploy.yml --record=true
   94  k rollout status deployment raman-deployment
   95  k get pods
   96  k get deploy
   97  k get deploy raman-deployment -o yaml
   98  vi deploy.yml 
   99  clear
  100  k get pods
  101  k get rs
  102  k rollout history deployment raman-deployment
  103  k rollout history deployment raman-deployment --revision=3
  104  k get pods | grep -i image
  105  k get pods
  106  k describe pods | grep -i image
  107  clear
  108  k rollout undo deployment raman-deployment --revision=2
  109  k rollout undo deployment raman-deployment --to-revision=2
  110  k get pods
  111  k get rs
  112  k describe pods | grep image
  113  k rollout undo deployment raman-deployment --revision=1
  114  k rollout undo deployment raman-deployment --to-revision=1
  115  k get pods
  116  k describe pods | grep image
  
  ===========================================================
  
  
  
  
   helm
  144  curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
  145  chmod 700 get_helm.sh
  146  ./get_helm.sh
  147  helm
  148  clear
  149  k get all
  150  k delete deploy --all
  151  clear
  152  k get all
  153  clear
  154  helm list
  155  helm repo list
  156  k get all
  157  helm list
  158  k get all
  159  helm repo list
  160  helm repo add bitnami https://charts.bitnami.com/bitnami
  161  helm repo list
  162  helm list
  163  helm search repo bitnami
  164  clear
  165  helm search repo bitnami/jenkins
  166  helm repo list
  167  helm show values bitnami/tomcat
  168  clear
  169  helm list
  170  k get all
  171  helm install raman-tomcat bitnami/tomcat
  172  clear
  173  helm list
  174  k get all
  175  k describe pod 
  176  clear
  177  helm list
  178  k get all
  179  k get pv
  180  helm uninstall raman-tomcat
  181  helm list
  182  clear
  183  k get all
  184  helm install raman-tomcat bitnami/tomcat --set service.type="NodePort" --set replicaCount=3 --set persistence.enabled=false
  185  clear
  186  k get all
  187  clear
  188  k get all
  189  k get pods -o wide
  190  helm uninstall raman-tomcat
  191  clear
  192  k get all
  
  ==============================================
  
  HPA :
  
  https://github.com/ramannkhanna2/k8s_metrics_server.git
  
  
  
  k get deploy
  203  k scale deploy raman-dep --replicas 1
  204  k get pods
  205  k autoscale deployment raman-dep --cpu-percent 60 --min 1 --max 5
  206  k get hpa
  207  k get pods
  208  k top pods 
  209  clear
  210  docker stats
  211  clear
  212  k gte hpa
  213  k get hpa
  214  k get pods -o wide
  215  k top nodes
  216  k top pods
  217  git clone https://github.com/ramannkhanna2/k8s_metrics_server.git
  218  ls
  219  cd k8s_metrics_server/
  220  ls
  221  cd deploy/
  222  ls
  223  cd 1.8+/
  224  ls
  225  k apply -f .
  226  clear
  227  k get pods
  228  k get pods -A
  229  k top nodes
  230  k top pods
  231  k get hpa
  232  k get deploy
  233  k get deploy -o yaml > test.yml
  234  ls
  235  rm -rf test.yml 
  236  cd ..
  237  ls
  238  cd ~
  239  k get pods
  240  k get deploy
  241  k get deploy raman-dep -o yaml > test.yml
  242  ls
  243  vi test.yml 
  244  k edit deploy raman-deployment
  245  k edit deploy raman-dep
  246  k get hpa
  247  k edit deploy raman-dep
  
  
  
  
  
  
  --- edit the deployment and update below :
      
  spec:
      containers:
      - image: httpd
        imagePullPolicy: Always
        name: httpd
        resources:    
          requests:
            memory: "64Mi"
            cpu: "100m"
          limits:
            memory: "128Mi"
            cpu: "150m" 
            
            
            
            
            worker ~ 1 vcpu ~ 1000 milicores : t2.micro
            
            
            250 milicores ~ .25 vcpu 
            
            
            ======================================
            
            
            
            pv : cluster level object 
            
            k get pv 
            
            ======================================
            
            
            
as 

apiVersion: v1
kind: PersistentVolume
metadata:
  name: nfs-website
spec:
  capacity:
    storage: 11Gb
  accessModes:
    - ReadWriteMany
  mountOptions:
    - hard
    - nfsvers=4.1
  nfs:
    path: /
    server: 172.31.12.142
    
    
    apiVersion: v1
kind: PersistentVolume
metadata:
  name: nfs-website2
spec:
  capacity:
    storage: 6 gb
  accessModes:
    - ReadWriteMany
  mountOptions:
    - hard
    - nfsvers=4.1
  nfs:
    path: /
    server: 172.31.12.142


as a developer , app needs around 
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: nfs-demo
spec:
  accessModes:
  - ReadWriteMany
  resources:
    requests:
      storage: 5gb
  volumeName: nfs-website
  
  
  

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nfs-raman
spec:
  replicas: 10
  selector:
    matchLabels:
      role: nfs-raman
  template:
    metadata:
      labels:
        role: nfs-raman
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
        volumeMounts:
        - name: nfs
          mountPath: /usr/share/nginx/deploydata
      volumes:
      - name: nfs
        persistentVolumeClaim:
          claimName: nfs-demo
          
          
          =================================================
          
          
          RBAC :


Lab Activity: Enforce Namespace-Specific Permissions and Test Access
Step 1: Create a Namespace
Create a new namespace for testing:

bash
Copy
kubectl create namespace test-namespace
Verify the namespace:

bash
Copy
kubectl get namespaces
Step 2: Create a Role
Create a Role that allows read-only access to Pods in the test-namespace:

Write the following YAML to a file (pod-reader-role.yaml):


apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: test-namespace
  name: pod-reader
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "list", "watch"]
Apply the Role:

bash
Copy
kubectl apply -f pod-reader-role.yaml
Verify the Role:

bash
Copy
kubectl get roles -n test-namespace
Step 3: Create a ServiceAccount
Create a ServiceAccount in the test-namespace:

bash
Copy
kubectl create serviceaccount test-sa -n test-namespace
Verify the ServiceAccount:

bash
Copy
kubectl get serviceaccounts -n test-namespace
Step 4: Create a RoleBinding
Bind the pod-reader Role to the test-sa ServiceAccount:

Write the following YAML to a file (pod-reader-rolebinding.yaml):

yaml
Copy
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: pod-reader-binding
  namespace: test-namespace
subjects:
- kind: ServiceAccount
  name: test-sa
  namespace: test-namespace
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
Apply the RoleBinding:

bash
Copy
kubectl apply -f pod-reader-rolebinding.yaml
Verify the RoleBinding:

bash
Copy
kubectl get rolebindings -n test-namespace
Step 5: Test Access Using kubectl auth can-i
Authenticate as the ServiceAccount:

Use the --as flag to impersonate the test-sa ServiceAccount and test access.

Test Permissions:

Check if the ServiceAccount can list Pods in the test-namespace:

bash
Copy
kubectl auth can-i list pods --namespace test-namespace --as system:serviceaccount:test-namespace:test-sa
Expected Output: yes.

Check if the ServiceAccount can create Pods in the test-namespace:

bash
Copy
kubectl auth can-i create pods --namespace test-namespace --as system:serviceaccount:test-namespace:test-sa
Expected Output: no (since the Role only allows get, list, and watch).

Check if the ServiceAccount can list Pods in the default namespace:

bash
Copy
kubectl auth can-i list pods --namespace default --as system:serviceaccount:test-namespace:test-sa
Expected Output: no (since the Role is namespace-specific).

=====================================

```

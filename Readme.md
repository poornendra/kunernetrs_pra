A full Minikube example with a MySQL database alongside an NGINX app. This setup includes:


minikube addons enable ingress 

kubectl get pods -n kube-system : Displays all the kubenerets details

kubectl get pods -n kube-system
output:
NAME                               READY   STATUS    RESTARTS       AGE
coredns-668d6bf9bc-4psd8           1/1     Running   1 (166m ago)   2d5h        
etcd-minikube                      1/1     Running   1 (166m ago)   2d5h        
kube-apiserver-minikube            1/1     Running   1 (166m ago)   2d5h        
kube-controller-manager-minikube   1/1     Running   2 (166m ago)   2d5h        
kube-proxy-sd9mr                   1/1     Running   1 (166m ago)   2d5h        
kube-scheduler-minikube            1/1     Running   1 (166m ago)   2d5h        
storage-provisioner                1/1     Running   3 (165m ago)   2d5h 

✅ StorageClass for dynamic storage provisioning
✅ Persistent Volume Claim (PVC) for MySQL database storage
✅ MySQL Deployment & Service
✅ ConfigMap (stores DB name & user)
✅ Secret (stores DB password)
✅ NGINX Deployment & Service (connects to MySQL)
✅ Ingress (exposes the app)

Step 1: creating storage class where we will provide details of provisioners and volumebindingmode

The provisioner field determines how storage is dynamically provisioned.
Common provisioners include:

Provisioner	Description
kubernetes.io/aws-ebs	        AWS EBS volumes
kubernetes.io/gce-pd	        Google Cloud Persistent Disk
kubernetes.io/azure-disk	    Azure Disk
kubernetes.io/no-provisioner	No dynamic provisioning (for manual PVs)
csi.rbd.csi.ceph.com	        Ceph RBD (CSI-based)

volumeBindingMode has Immediate and WaitForFirstConsumer

Step 2: Create a ConfigMap (for MySQL)

        here we are adding  MYSQL_DATABASE and MYSQL_USER

step 3:  Create a Secret (for MySQL Password) 
       
       Here we are adding MYSQL_ROOT_PASSWORD ,MYSQL_PASSWORD 
       command to create secret echo -n xxxxx | base64
       here we will use type: opaque and data

Step 4: Create a Persistent Volume Claim (PVC) for MySQL database storage

for only storage class we add api verion as storage.k8s.io and pv , pvc we use v1
ex: apiVersion: v1 # for pv and pvc
apiVersion: storage.k8s.io/v1 # for storage class

step 5: create a my deployment and volumes and env varibles
        here we will create deployment for image mysql:5.7 and create volumemount with name: my-sqlstorage and mountpath=/var/lib/mysql and also we will create env varibles for MYSQL_ROOT_PASSWORD and MYSQL_DATABASE and create volume using persistentVolumeClaim and provide pvc name. 
step 6: Create MySQL Service
        Here we create a service for port 3306 and protcol:tcp

step 7: Create ngnix app deployment to connect to mysql db
        here we will create deployment for image nginx:latest and add env varible like mysql-host and
        MYSQL_USER and MYSQL_password and MYSQL_DATABASE
step 8: we will create internal service for ngnix [Create NGINX Service]
        here we ill specify the port 80
step 9 : create ingress
        here apiVerion will  be networking.k8s.io/v1 we need to give and in metadta we need to add 
        annotations: nginx.ingress.kubernetes.io/rewrite-target: /
        here in spec we need to add rules like host ,http, path, and serviceName and sericePort
        




create a kubernetes deployment for mongo db and mongo expess


commands:

kubectl apply commands in order

kubectl apply -f mongo-secret.yaml
kubectl apply -f mongo.yaml
kubectl apply -f mongo-configmap.yaml 
kubectl apply -f mongo-express.yaml

kubectl get commands

kubectl get pod
kubectl get pod --watch
kubectl get pod -o wide
kubectl get service
kubectl get secret
kubectl get all | grep mongodb
kubectl get deployments mongodb-deployment -o yaml > filename.yml

kubectldebug commands

kubectl describe pod mongodb-deployment-xxxxxx
kubectl describe service mongodb-service
kubectl logs mongo-express-xxxxxx

give url to extrenal

minikube service mongo-express-service




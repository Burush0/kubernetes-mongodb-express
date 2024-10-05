# kubernetes-mongodb-express
Basic setup for a K8s MongoDB with Mongo Express interface with Minikube and Kubectl (per YouTube tutorial by TechWorld with Nana)

## Prerequisites
- [Minikube](https://minikube.sigs.k8s.io/docs/)
- [Kubectl](https://kubernetes.io/docs/reference/kubectl/) (comes with Minikube installation)

## Setup
1. Clone the repo
2. Navigate to the folder
3. Start a Minikube cluster with `minikube start`
4. Initialize all the .yaml files in order:
   ```
   kubectl apply -f mongo-secret.yaml
   kubectl apply -f mongo-configmap.yaml
   kubectl apply -f mongo.yaml
   kubectl apply -f mongo-express.yaml
   ```
5. Once the pods and services have started (you can check with `kubectl get pod` and `kubectl get service` respectively), you need to create an external IP for the Mongo Express service with Minikube. With a regular K8s installation, this step is unneccesary.  
   ```
   minicube service mongo-express-service
   ```
6. For the authorization field that pops up, you can type in the default mongo-express container values to get access to the Mongo Express interface. These values are `admin` and `pass` respectively.

   (I'm not sure why it asks for default values despite specifying the non-default values in the mongo-secret.yaml file, I suppose those values are used to connect the mongo pod to the mongo-express pod, and the web interface still uses the default ones?)

### Why the order matters:
Secret and ConfigMap have to be initialized before the deployments and services, as those are using key-value pairs from there

### Note on mongo-secret.yaml
Normally, a secret file is not provided in GitHub repos as it contains sensitive information, such as credentials. However, since this is only following a tutorial, it only contans "default" values of username and password.

Another intersting thing about creating a Secret in K8s is that you have to write values in there in the Base64 encoding. So, for example, for username "username" I ran the command `echo 'username' | base64` to obtain the string "dXNlcm5hbWU=", which I then put in the corresponding field in the Secret file.

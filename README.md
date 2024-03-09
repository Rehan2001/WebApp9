
# WebApplication
Dockerize a simple web application, deploy it to a Kubernetes cluster using Argo CD, and manage its release process with Argo Rollouts.
## Dockerizing-a-web-app
Create a simple web application, deploy it onto a docker container and uploading the image to docker Hub website
1. Create a working directory
mkdir <working_directory_name>
2. Lets create a html.index , style.css and script.js file that defines a web-app.

[html.index](https://github.com/Rehan2001/WebApp9/blob/main/src/html/index.html)

[style.css](https://github.com/Rehan2001/WebApp9/blob/main/src/html/style.css)

[script.js](https://github.com/Rehan2001/WebApp9/blob/main/src/html/script.js)

3. Lets test the application, click the GoLive in VScode
If you followed the above steps on your system, you will see the same output as test image in github repo:[: http://localhost](http://127.0.0.1:5500/src/html/index.html)

Lets try running the same WebApplication application running on the docker container. To run the application on the docker conatiner we need a docker image.

First, we will create a docker image for the application.

4. Create a Dockerfile
5. Dockerfile should look like this
```
# creates a layer from the nginx:1.10.1-alpine Docker image.
FROM nginx:1.10.1-alpine
# adds files from your Docker client's current directory.
COPY src/html /usr/share/nginx/html

```
6. Building Docker image
    > docker build -t WebApplication .
7. Check the Docker images
    > docker images
8. Run the docker image to create container of WebApplication
    > docker run -it -p 80:80 -d WebApplication
9. Get the container id
    > docker ps
10. Lets know where it is running on
    > docker logs <container_id>
    ```
    output: 
    Example app listening on port 80!

    ```
11.  If you followed the  above steps on your system, you will see the sam output on : [http://localhost:80/](http://localhost:80/)

12. Login to Docker Hub
    > doker Login
13. Tag Your Docker Image
    > docker tag local-image:tag username/repository:tag
    ```
    docker tag webApplication:latest rehna2001

    ```
14. Push the Docker Image to Docker Hub
    > docker push username/repository:tag
    ```
    docker push rehna2001/webapplication

    ```
    sample: [webapplication image](https://github.com/Rehan2001/WebApp9/blob/main/DockerImage.png)
    
    My webapplication on [Docker Hub](https://hub.docker.com/repository/docker/rehna2001/webapplication/general)

15. Check on [Docker Hub](https://hub.docker.com/), go to the Docker Hub website and navigate to your repository to verify that the image has been successfully uploaded.

## Deploy the Web-Application Using Argo CD
Setup and Configuration of kubernetes
- [kubectl install](https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/)
- [minikube install](https://minikube.sigs.k8s.io/docs/start/)
    1. Add Deployment Manifest to the Application Repository
- [Deployment.yaml](https://github.com/Rehan2001/WebApp9/blob/main/deployment.yaml)

Install Argo CD on Your Kubernetes Cluster
- [Install Argo CD](https://argo-cd.readthedocs.io/en/stable/getting_started/)
```
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

```
Argo CD Server
```
minikube service argocd-Server -n argocd

```
COPY the URL and open in the local browser
username = admin
password = decode from followed steps
```
kubectl get secret -n argocd
kubectl edit secret argocd-initial-admin-secret -n argocd
echo [password] | base64 --decode

```
COPY the output password and paste in the argocd dashboard 

### Creating the GitOps Pipeline
Now everything is Set up Argo CD to monitor your repository and automatically deploy changes to your Kubernetes cluster.

    1.  Define a Git Repository in ArgoCD
        - Go to settings.
        - Click on repositories.
        - Click on “Connect Repo”
        - Paste your URL.
        - Click “Connect”
        - Click on Create New app
        Fill the info
        Name = <give any name you want>
        Project Name= default
        Sync Policy= Manual
        Repository URL = <your repo url >
        Branch = master
        Path = <path to your deployment file>
        Cluster url = <just provide the default cluster >
        Namespace = <Provide your namespace or make it as default >
        Click on Create, Now pods are getting created and now your app is deployed.
        In Terminal, run this command to verify that application pod is running
            > kubectl get pods

    2.  Test the Application
        run this command for public URL of the web application
            > minikube service webapplication-deployment --url
        


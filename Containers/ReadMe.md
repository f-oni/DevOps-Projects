# Kubernetes Features
A project to demonstrate some core features of kubernetes. Some of the features to be demonstrated include;

+ Service discovery and load balancing
+ Self healing property
+ Automated rollouts and rollbacks

**Service discovery and load balancing:** In order to ensure even distribution and high performance, network traffic is distributed across multiple containers

**Self healing property:** To ensure application availability, it Restarts, replaces, and reschedules containers that fail, and can automatically redirect traffic away from unhealthy instances.

**Automated rollouts and rollbacks:** It manages the deployment and updates of applications, automatically rolling back to a previous version if something goes wrong

## Project Stack
The following were employed in order to implement the kubernetes features highlighted above:
- Node Js
- Docker
- Docker hub
- kubernetes cluster

## Project Steps
1. Node Js application
2. Building the application image using docker
3. Pushing the application to container repository
4. preparation of manifest file
5. project implementation

 **Node Js application** 
 + A simple node js application to display the hostname was prepared. The `index.js` file is shwown below:
 + The application was tested on the local machine as shown:

 **Building the application image using docker**
 + In order to build the application image, a `Dockerfile` was prepared in the project directory. The content of the file is displayed below:
 + The image was built with the command `docker build . -t folu980/proj-app:0.1`
 + The image was successfully built
   
 **Pushing the application to container repository**
 + The built image was pushed to docker hub with the command `docker push folu980/proj-app:0.1`
 + The image was successfully pushed
   
 **preparation of manifest file**
 + A manifest  file with the name `proj-deploy.yaml` was prepared for the deployment. The file contains the nstructions for the kubernetes resources
 + The content of the file is shown below:
 + The command `kubectl apply -f proj-deply.yaml` was run for the implementation
 
 **project implementatio**
 In order to demonstrate each of the core kubernetes featueres, the project was implemented in phases.

 1. **Service discovery and load balancing:**
    - To increase the number of instances the `replicas` in the manifest file was increased from 1 to 5
    - The containers were reached using the loadbalancer ip and the service port


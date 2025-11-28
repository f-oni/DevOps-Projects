# Application Management with Docker and Kubernetes

Kubernetes is a tool used to automate and manage containerized applications at scale. It is reliable, scalable and portable. The project is meant to simulate how to build an application image with docker and deploy containerized application. It also demonstrates how to apply some of the core features of kubernetes in application management. Some of the features to be demonstrated include:

+ Service discovery and load balancing
+ Self healing property
+ Automated rollouts and rollbacks

**Service discovery and load balancing:** In order to ensure even distribution and high performance, network traffic is distributed across multiple containers

**Self healing property:** To ensure application availability, it Restarts, replaces, and reschedules containers that fail, and can automatically redirect traffic away from unhealthy instances.

**Automated rollouts and rollbacks:** It manages the deployment and updates of applications, automatically rolling back to a previous version if something goes wrong

## Project Stack

The following were employed for the project:

- Node Js
- Docker
- Docker hub
- Minikube

## Project Steps

1. Node Js application
2. Building the application image using docker
3. Pushing the application to container repository
4. preparation of manifest file
5. project implementation

 **Node Js application** 
 
 + A simple node js application to display the hostname was prepared. The `index.js` file is shwown below:
   
    ```
      const express = require('express');
      const os = require('os');
      const app = express();
      const PORT = 3000
      
      app.get('/', (req, res)=>{
          const message = `Hello, message from ${os.hostname}`
          console.log(message)
          res.send (message)
      });
      
      app.listen(PORT, ()=> {
          console.log(`Server runnig on port ${PORT}`);
      });

    ```

 **Building the application image using docker**
 
 + In order to build the application image, a `Dockerfile` was prepared in the project directory. The content of the file is displayed below:
   
    ```
       FROM node:alpine
  
       WORKDIR /app
       
       COPY package.json package-lock.json ./
       
       RUN npm install
       
       COPY . ./
       
       EXPOSE 3000
       
       CMD [ "npm", "start"]
    
    ```
 + The image was built with the command `docker build . -t folu980/proj-app:0.1`
 + The image was successfully built

    <img width="1490" height="548" alt="doc1" src="https://github.com/user-attachments/assets/c2155598-c5c8-4609-bf4f-75e210b71ee3" />

   
 **Pushing the application to container repository**
 
 + The built image was pushed to docker hub with the command `docker push folu980/proj-app:0.1`
 + The image was successfully pushed

   <img width="1571" height="809" alt="repo" src="https://github.com/user-attachments/assets/4d72cdf8-7f31-4a16-addb-e63e7ee7154f" />

 **preparation of manifest file**
 
 + A manifest  file with the name `proj-deploy.yaml` was prepared for the deployment. The file contains the instructions for the kubernetes resources
 + The content of the file is shown below:
   
    ```
     apiVersion: apps/v1
     kind: Deployment
     metadata:
      name: proj-app1
     spec:
      replicas: 1
      selector:
       matchLabels:
        app: proj1
      template:
       metadata:
        labels:
         app: proj1
       spec:
        containers:
         - name: proj-app
           image: folu980/proj-app:0.1
           ports:
            - containerPort: 3000
      
     ---
     apiVersion: v1
     kind: Service
     metadata: 
      name: app-srv
     spec:
      selector:
       app: proj1
      ports:
       - port: 80
         targetPort: 3000
      type: LoadBalancer 
    ```
 + The command `kubectl apply -f proj-deply.yaml` was run for the implementation

   <img width="917" height="471" alt="scaling2" src="https://github.com/user-attachments/assets/3818b6ef-fbe4-4d90-8294-6451198b5df0" />

 
## Project Implementation
 
 In order to demonstrate each of the core kubernetes featueres, the project was implemented in phases.

 1. **Service discovery and load balancing**
    
      - To increase the number of instances the `replicas` in the manifest file was increased from 1 to 5
        
         <img width="930" height="463" alt="scaling5" src="https://github.com/user-attachments/assets/09c5a0a3-6dad-40cd-9d79-c42028c827ef" />

      - The information needed to access the application externally was revealed with the command `minikube service app-srv --url`
        
      - The browser was refreshed at short intervals to simulate traffic distribution across multiple containers.
        
         <img width="1200" height="837" alt="scaling6" src="https://github.com/user-attachments/assets/f210ec68-8350-4128-85e7-591998d3e25c" />

      - The command `curl 172.29.185.92:32638` was also used at intervals on the terminal for traffic distribution simulation
        
         <img width="862" height="713" alt="scaling8" src="https://github.com/user-attachments/assets/c51bff12-abf4-4f6e-9423-d8671c02ba77" />


        **Observation**
        
         The traffic was effectively distributed across the five containers as illustrate above:
        
 2. **Self healing property**
  
    + A pod with the name `-46v94` was deleted with the command `kubectl delete pod proj-app1-9d5664b8c-46v94`
    + The command `kubectl get pods` was run a few seconds later
    + The process was repeated for another pod

      **Observation**
      
      A replacement pod was spurn up instantly and automatically as soon one gets deleted. This helps to always maintain the desired state.

      <img width="839" height="645" alt="rep2" src="https://github.com/user-attachments/assets/64afdd00-07f1-4d25-a104-ca9b34ce2a63" />

      <img width="872" height="634" alt="rep3" src="https://github.com/user-attachments/assets/25a52f11-09f6-4b74-b929-2c7bdb1358bb" />


    
 3. **Automated rollouts and rollbacks**
    
    + The `index.js` file of the node Js application was updated as shown
      
       <img width="968" height="553" alt="img-1" src="https://github.com/user-attachments/assets/f6278e0b-ebce-4398-b14d-82b87b80296a" />

    + A version 2 of the image was built with the command `docker build . -t folu980/proj-app:0.2` and pushed to the image repository on Docker hub

      <img width="1289" height="504" alt="img-2" src="https://github.com/user-attachments/assets/857c23a9-e7bd-48ac-ac5e-6a049412590a" />

      <img width="1913" height="728" alt="img-5" src="https://github.com/user-attachments/assets/2d50a9b7-15aa-4e29-8b89-d9388c4f20fa" />


    + The manifest file was updated to use the newly pushed image
      
       <img width="796" height="908" alt="manifest1" src="https://github.com/user-attachments/assets/27cc0ece-af94-4120-bb39-e627a2c739f5" />

    + A rollout was implemented with the command `kubectl apply -f projh-deploy.yaml`

      <img width="910" height="411" alt="rollout" src="https://github.com/user-attachments/assets/1858dcd9-39bd-4b82-9f3e-b50ce586a699" />

    + The status of the rollout was monitored with the command `kubectl rollout status deployment/proj-app1`
    + A rollback operation was also performed  with the command `kubectl rollout undo deployment/proj-app1`
   
      <img width="993" height="589" alt="rollback2" src="https://github.com/user-attachments/assets/b7586ac5-e741-4dd3-90a1-66392e1d20b7" />


      **Observations**
      
      - The rollout was incrementally done and successful. The version 2 of the application is shown below
        
         <img width="975" height="358" alt="v2-resources" src="https://github.com/user-attachments/assets/dd43ec69-7aed-4a0e-89ea-781818af089d" />

      - The rollback operation was also successfull as the application reverted to version 1

         <img width="760" height="363" alt="rollback" src="https://github.com/user-attachments/assets/f97d3a4d-8e7c-40b1-b008-51b68da4279b" />


     ## Conclusion

    Kubernetes is a very crucial tools for managing containerized applications. Some of its core features were successfully simulated during the project    implementation.
    
    
   


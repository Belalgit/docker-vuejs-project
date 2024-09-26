**Project Structure**

docker-vuejs-project/
│
├── project-directory/
│   ├── app/
│   │   ├── Dockerfile
│   │   ├── package.json
│   │   ├── package-lock.json
│   │   ├── src/  # Vue.js application source code
│   │   ├── public/
│   │   └── vue.config.js
│   ├── nginx/
│   │   ├── Dockerfile
│   │   └── nginx.conf
│   ├── docker-compose.yml

================================================================================================

**Deployment Process**
1. Prepare the EC2 instance:
       Ensure your EC2 instance has Docker and Docker Compose installed:
              sudo apt update
              sudo apt install docker.io docker-compose
              sudo systemctl start docker
              sudo systemctl enable docker

2. Clone or upload the project to the EC2 instance:
       Transfer the docker-vuejs-project to your EC2 instance (or clone a GitHub repo).
3. Build and run the containers using Docker Compose:
       Navigate to your project-directory and run the following command to start the services:
              cd docker-vuejs-project/project-directory
              sudo docker-compose up --build -d
4. Container Information:
       I. App container (vuejs-app):
             Builds a Vue.js app from the app/Dockerfile.
             The Vue.js app is served by Nginx after being built.
             The final build is copied into /usr/share/nginx/html inside the container.
             Volume app-data is mounted at /usr/share/nginx/html to persist data in case of changes.
             Exposed internally to other containers at port 80.
       II. Nginx container (nginx-server):
             Builds a container using the Nginx official image from nginx/Dockerfile.
             Serves the Vue.js app via a reverse proxy configuration from the nginx/nginx.conf file.
             Volume nginx-data is mounted at /etc/nginx, allowing persistent Nginx configuration.
             Exposed externally to port 80 of the host, making the application accessible via the EC2 public IP or domain.
5. Network Configuration:
       Bridge network (app-network):
             The two services, vuejs-app and nginx-server, are connected using an internal bridge network (app-network).
             This ensures that the Nginx server can proxy traffic to the app container internally without exposing the app container's port externally.
6. Volumes:
       **app-data:** Mounted to /usr/share/nginx/html in the vuejs-app container, ensuring the built application files are stored in a persistent volume.
       **nginx-data:** Mounted to /etc/nginx in the nginx-server container to ensure persistent Nginx configuration and logs.


**check the docker images, container, network**
  sudo docker images
  sudo docker ps
  sudo docker network ls


# Open AI setup for AKij group 
#Author: Boni Yeamin
#Date: 2022-02-28
#Version: 1.0
#Description: This script will install Open AI on Ubuntu server

# install dependencies
 Root access

# Install Docker for Ubuntu

Follow these steps to install Docker on an Ubuntu system:


# install docker on ubontu server
$ sudo apt update
$ sudo apt install docker.io
$ sudo systemctl start docker
$ sudo systemctl enable docker



# Create Docker Volume for ollama_data

1. **Create a Docker volume for storing the data:**
    $ docker volume create ollama_data

2. **Verify that the volume was created successfully:**
    $ docker volume ls

3. **Check the status of the volume:**
    $ docker volume inspect ollama_data

4. **Start the container and mount the volume:**

    $ docker run -d -v ollama_data:/root/.ollama -p 11434:11434 --name ollama ollama/ollama:latest
    
5. **Check the status of the container:**
    $ docker ps -a

5. **Access the container's web interface at http://localhost:11434**

# install ollama on ollama Images
$ docker exce ollama ollama run gemma3:4b

# show ollama run gemma3:4b
$ docker exece ollama ollama ps 

# install open web-UI  image
$ docker pull ghcr.io/open-webui/open-webui:main

# Run the Image 
docker run -d -p 3000:8080 -v open-webui:/app/backend/data --name open-webui ghcr.io/open-webui/open-webui:main


# Access the web interface at http://localhost:3000


# install nginx on Ubontu server and config hostname willbe ai.akijgroup.co
$ sudo apt update
$ sudo apt install nginx
$ sudo nano /etc/nginx/sites-available/default

# add the following lines to the server block:
server {
    listen 80;
    server_name ai.akijgroup.co;

    location / {
        proxy_pass http://localhost:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}

# save and exit the file, then restart nginx:

# hostname setup ubontu 
$ sudo hostnamectl set-hostname ai.akijgroup.co

# add the hostname to /etc/hosts file
$ sudo nano /etc/hosts
# add the following line to the file:
127.0.0.1   ai.akijgroup.co     

# restart the server and test the web interface at http://ai.akijgroup.co


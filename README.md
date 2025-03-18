# Open AI Setup for Akij Group

![Akij Group](https://github.com/boniyeamincse/openaisetup/blob/main/image/boni%20Yemain.png)

## Table of Contents
1. [Overview](#overview)
2. [Prerequisites](#prerequisites)
3. [Install Docker](#install-docker)
4. [Docker Compose Configuration](#docker-compose-configuration)
5. [Create Docker Volume](#create-docker-volume)
6. [Install Ollama](#install-ollama)
7. [Install Open Web-UI](#install-open-web-ui)
8. [Install Nginx](#install-nginx)
9. [Set Hostname](#set-hostname)
10. [Setup SSL](#setup-ssl)

## Overview

| **Author**       | Boni Yeamin          |
|------------------|----------------------|
| **Date**         | 2022-02-28           |
| **Version**      | 1.0                  |
| **Description**  | This script will install Open AI on Ubuntu server. |

## Prerequisites
- Root access to the Ubuntu server.

## Install Docker on Ubuntu Server

Follow these steps to install Docker on an Ubuntu system:

```sh
$ sudo apt update
$ sudo apt install docker.io
$ sudo systemctl start docker
$ sudo systemctl enable docker
```

## Docker Compose Configuration

Create a `docker-compose.yml` file with the following content:

```yaml
version: '3.8'

services:
  ollama:
    image: ollama/ollama
    container_name: ollama
    volumes:
      - ollama_data:/root/.ollama
    ports:
      - "11434:11434"
    restart: always

  openwebui:
    image: openwebui/openwebui
    container_name: openwebui
    ports:
      - "8080:8080"
    restart: always

volumes:
  ollama_data:
```

## Create Docker Volume for `ollama_data`

1. **Create a Docker volume for storing the data:**
    ```sh
    $ docker volume create ollama_data
    ```

2. **Verify that the volume was created successfully:**
    ```sh
    $ docker volume ls
    ```

3. **Check the status of the volume:**
    ```sh
    $ docker volume inspect ollama_data
    ```

4. **Start the container and mount the volume:**
    ```sh
    $ docker run -d -v ollama_data:/root/.ollama -p 11434:11434 --name ollama ollama/ollama:latest
    ```

5. **Check the status of the container:**
    ```sh
    $ docker ps -a
    ```

6. **Access the container's web interface at [http://localhost:11434](http://localhost:11434)**

## Install Ollama on Ollama Images

```sh
$ docker exec ollama ollama run gemma3:4b
```

## Show Ollama Running Gemma3:4b

```sh
$ docker exec ollama ollama ps
```

## Install Open Web-UI Image

```sh
$ docker pull ghcr.io/open-webui/open-webui:main
```

## Run the Image

```sh
$ docker run -d -p 3000:8080 -v open-webui:/app/backend/data --name open-webui ghcr.io/open-webui/open-webui:main
```

## Access the Web Interface at [http://localhost:3000](http://localhost:3000)

## Install Nginx on Ubuntu Server and Configure Hostname

1. **Install Nginx:**
    ```sh
    $ sudo apt update
    $ sudo apt install nginx
    ```

2. **Edit the Nginx Configuration File:**
    ```sh
    $ sudo nano /etc/nginx/sites-available/default
    ```

3. **Add the following lines to the server block:**

    ```nginx
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
    ```

4. **Save and exit the file, then restart Nginx:**
    ```sh
    $ sudo systemctl restart nginx
    ```

## Set Hostname on Ubuntu

1. **Set the hostname:**
    ```sh
    $ sudo hostnamectl set-hostname ai.akijgroup.co
    ```

2. **Add the hostname to `/etc/hosts` file:**
    ```sh
    $ sudo nano /etc/hosts
    ```

3. **Add the following line to the file:**
    ```plaintext
    127.0.0.1   ai.akijgroup.co
    ```

4. **Restart the server and test the web interface at [http://ai.akijgroup.co](http://ai.akijgroup.co)**

## Setup SSL

1. **Install Certbot:**
    ```sh
    $ sudo apt update
    $ sudo apt install certbot python3-certbot-nginx
    ```

2. **Obtain an SSL Certificate:**
    ```sh
    $ sudo certbot --nginx -d ai.akijgroup.co
    ```

3. **Follow the instructions to complete the SSL certificate installation.**

4. **Verify the SSL Certificate:**
    ```sh
    $ sudo certbot certificates
    ```

5. **Test SSL Certificate Renewal:**
    ```sh
    $ sudo certbot renew --dry-run
    ```

6. **Access the secure web interface at [https://ai.akijgroup.co](https://ai.akijgroup.co)**

---

This setup guide provides the necessary steps to install and configure Open AI on an Ubuntu server for Akij Group. Follow these instructions carefully to ensure a smooth setup process.

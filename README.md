# Docker-Nodejs-ghost
docker environment using docker-compose, with Node.js:ghost

This guide uses sudo wherever possible. Complete the sections of our Securing Your Server guide to create a standard user account, harden SSH access and remove unnecessary network services.

# Install
The instructions assume that you have already installed [Docker](https://docs.docker.com/installation/) and [Docker Compose](https://docs.docker.com/compose/install/). 

In order to get started be sure to clone this project onto your Docker Host. Create a directory on your host. Please note that the demo webservices will inherit the name from the directory you create. If you create a folder named test. Then the services will all be named test-web, test-redis, test-lb. Also, when you scale your services it will then tack on a number to the end of the service you scale. 

    git clone https://github.com/vegasbrianc/docker-compose-demo.git .

# How to get up and running
Once you've cloned the project to your host we can now start our demo project. Easy! Navigate to the directory in which you cloned the project. Run the following commands from this directory 
    

    docker-compose up -d

The  docker-compose command will pull the images from Docker Hub and then link them together based on the information inside the docker-compose.yml file. This will create ports, links between containers, and configure applications as requrired. After the command completes we can now view the status of our stack

    docker-compose ps

Install GhostPermalink
The Ghost deployment has three components:

* The Ghost service itself;
* database (MySQL) that will store your blog posts;
* A web server (NGINX) that will proxy requests on HTTP and HTTPS to your Ghost service.
These services are listed in a single Docker Compose file.


# Introduction to Docker and Kubernetes Hands-On Workshop

## Working Environment

### Cloud9
We have provisioned a Cloud9 instance and its browser-based IDE and terminal for each of you. This is a web interface to a Linux machine with Docker installed and ready to go. Being Linux, containers run right on these Cloud9 instances when you use the docker commands.

TODO - Add instructions/screenshots to launch the Cloud9

### Kubernetes
We have provisioned a kubernetes cluster based on AWS' EKS with a namespace for each of you and your Cloud9 is set up with the right config and credentials where it is ready to go. Using the `kubectl` command will 'just work' in the examples below. We'll be covering how to set up Kubernetes and how things work under-the-hood in another session soon.

## The Workshop

### Docker Basics
The first part of this workshop will focus on the fundamentals of Docker and how to use it locally within one machine.

1. Type `docker version` to confirm that both the client and server are there and working.
1. Type `docker pull nginx:latest` to pull down the latest nginx trusted image from Docker Hub.
1. Type `docker images` to verify that the image is now on your local machine's Docker cache. If we start it then it won't have to pull it down from Docker Hub first.
1. Type `docker run –d –p 8080:80 --name nginx nginx:latest` to instantiate the nginx image as a background daemon with port 8080 on the host forwarding through to port 80 within the container
1. Type `docker ps` to see that our nginx container is running.
1. Type `curl http://localhost:8080` to use the nginx container and verify it is working with its default `index.html`.
1. Type `docker logs nginx` to see the logs produced by nginx and the container from our loading a page from it.
1. Type `docker exec -it nginx /bin/bash` for an interactive shell into the container's filesystem and constraints
1. Type `cd /usr/share/nginx/html` and `cat index.html` to see the content the nginx is serving which is part of the container.
1. Type `exit` to exit our shell within the container.
1. Type `docker stop nginx` to stop the container.
1. Type `docker ps -a` to see that our container is still there but stopped. At this point it could be restarted with a `docker start nginx` if we wanted.
1. Type `docker rm nginx` to remove the container from our machine
1. Type `docker rmi nginx:latest` to remove the nginx image from our machine's local cache
1. Type `cd docker-kube-intro-workshop` to change into that project.
1. Type `docker build -t nginx:1.0 .` to build nginx from our Dockerfile
1. Type `docker history nginx:1.0`. to see all the steps and base containers that our nginx:1.0 is built on. Note that our change amounts to one new tiny layer on top.
1. Type `docker run -p 8080:80 --name nginx nginx:1.0` to run our new container. Note that we didn't specify the `-d` to make it a daemon which means it holds control of our terminal and outputs the containers logs to there which can be handy in debugging.
1. Type `curl http://localhost:8080` in another terminal tab a few times and see our new content as well as the log lines in our origional terminal. 
1. At this point we could push it to Docker Hub or a private Registry like AWS' ECR for others to pull and run. We won't worry about that yet and will cover it in the next Kubernetes session.
1. Type Ctrl-C to exit the log output. Note that the container is still running though if you do a `docker ps`.
1. Type `sudo docker inspect nginx` to see lots of info about our running container.
1. Type `docker stop nginx` to shut our container down.
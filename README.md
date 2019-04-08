# Introduction to Docker Hands-On Workshop

## Working Environment

### Cloud9
We'll start by setting up a Cloud9 which is an EC2 instance accessible via a browser-based IDE and Terminal. Being backed by a dedicated Linux machine, containers run right on these Cloud9 instances when you use the docker commands.

1. Sign into the AWS Console
1. Choose the Oregon region in the upper right dropdown (Cloud9 is not yet available in Sydney)
1. Click the `Create environment` button
1. Name your environment after your username (e.g. user1) and click the `Next step` button
1. Click the `Next step` button accepting the defaults
1. Click the `Create environment` button
1. When it comes up close the welcome tab and lower work area
1. Then open a new terminal by choosing the `Window` menu then picking `New Terminal`
1. Type `git clone https://github.com/jasonumiker/docker-intro-workshop.git`

### AWS Elastic Container Registry (ECR)

1. Create a new browser tab and go to `https://us-west-2.console.aws.amazon.com`
1. Go to the ECR Service
1. Click the `Get Started` button
1. Enter `nginx` for the `Repository name` and click the `Create repository` button
1. Click the `View push commands` button and then leave this tab open until needed later

## The Workshop

### Docker Basics
The first part of this workshop will focus on the fundamentals of Docker and how to use it locally within one machine.

1. Type `docker version` to confirm that both the client and server are there and working.
1. Type `docker pull nginx:latest` to pull down the latest nginx trusted image from Docker Hub.
1. Type `docker images` to verify that the image is now on your local machine's Docker cache. If we start it then it won't have to pull it down from Docker Hub first.
1. Type `docker run -d -p 8080:80 --name nginx nginx:latest` to instantiate the nginx image as a background daemon with port 8080 on the host forwarding through to port 80 within the container
1. Type `docker ps` to see that our nginx container is running.
1. Type `curl http://localhost:8080` to use the nginx container and verify it is working with its default `index.html`.
1. Type `docker logs nginx` to see the logs produced by nginx and the container from our loading a page from it.
1. Type `docker exec -it nginx /bin/bash` for an interactive shell into the running container's filesystem and constraints
1. Type `cd /usr/share/nginx/html` and `cat index.html` to see the content the nginx is serving which is part of the container.
1. Type `exit` to exit our shell within the container.
1. Type `docker stop nginx` to stop the container.
1. Type `docker ps -a` to see that our container is still there but stopped. At this point it could be restarted with a `docker start nginx` if we wanted.
1. Type `docker rm nginx` to remove the container from our machine
1. Type `docker rmi nginx:latest` to remove the nginx image from our machine's local cache
1. Type `cd docker-intro-workshop` to change into that project.
1. Type `docker build -t nginx:1.0 .` to build nginx from our Dockerfile
1. Type `docker history nginx:1.0`. to see all the steps and base containers that our nginx:1.0 is built on. Note that our change amounts to one new tiny layer on top.
1. Type `docker run -p 8080:80 --name nginx nginx:1.0` to run our new container. Note that we didn't specify the `-d` to make it a daemon which means it holds control of our terminal and outputs the containers logs to there which can be handy in debugging.
1. Open a 2nd Terminal tab
1. Type `curl http://localhost:8080` in the 2nd terminal tab a few times noting our new index.html.
1. Return to the original Terminal tab and see the log lines output there.
1. Type Ctrl-C to exit the contianer.
1. Note that this also stopped the container by typing `docker ps -a`
1. Type `docker start nginx` to start the container again and then `docker ps` to verify it is running.
1. Type `sudo docker inspect nginx` to see lots of info about our running container.
1. Type `docker stop nginx` to shut our container down.
1. Flip to our browser tab with `Push commands for nginx`.
1. Copy and Paste each command into the Cloud9 Terminal -  except for the `build` command (which we've already done) - to push `docker:latest` up to ECR.
1. Repeat the last two steps but using the tag `nginx:1.0` instead of latest (e.g. `docker tag nginx:1.0 575801521278.dkr.ecr.ap-southeast-1.amazonaws.com/nginx:1.0` but with your account string from the Push commands)
1. Click the X to exit the `View push commands` in that broser tab and then click the `nginx` Repository name link.
1. As you'll see we pushed nginx with the `Image tags` `latest` and `1.0` up to ECR. We could pull these down with a `docker pull <Image URI>`.

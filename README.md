## Building Docker Images and Running Applications

We'll be building node, php and python sample web applications packaged into docker images

### Tools and Packages

![Ubuntu](https://img.shields.io/badge/Ubuntu-22.04-E95420?style=for-the-badge&logo=Ubuntu) ![amazon ec2](https://img.shields.io/badge/amazonec2-FF9900?style=for-the-badge&labelColor=black&logo=amazonec2&logoColor=FF9900) ![docker](https://img.shields.io/badge/docker-2496ED?style=for-the-badge&labelColor=black&logo=docker&logoColor=2496ED) 


### Set-up and Installations

1. Start up an EC2 instance on AWS (We'll use Ubuntu on t2.micro)

2. On the Ubuntu server, let's install docker following the installation process here (https://docs.docker.com/engine/install/ubuntu/)

    ```docker --version``` to confirm installation


### PHP Packaging

1. On dockerhub, we need to get a "php" and an "apache" image.

    - We'll be using **php:8-apache** image

2. First, let's create a "helloapp-php" directory on our server. 

    ```
    mkdir helloapp-php
    ```

3. Inside the helloapp-php directory, we'll create an "index.php" file and populate it with the relevant code.

    ```
   vi index.php
    ```

5. Next, we'll create a Dockerfile calling up our image and referencing "index.php" file.

    ```
   vi Dockerfile
    ```

7. Now, we need to build the Dockerfile into an image, tagged "php-helloapp".

    ```
    docker build -t php-helloapp .
    ```

    - The dot "." at the end of the command denotes location of the Dockerfile (that is, current working directory)

8. Use ```docker images``` to see your image

![dk images](https://github.com/uedwinc/Simple-HelloWorld-Apps-on-Docker/blob/main/images/dk%20images.png)

7. Now, we can run the image

    ```
    docker run --name php-helloapp -p 80:80 -d php-helloapp:latest
    ```

    - Since we are using port 80 here, we need to open that up on AWS security group

![dk ps](https://github.com/uedwinc/Simple-HelloWorld-Apps-on-Docker/blob/main/images/dk%20ps.png)

8. Now, we can use ```docker ps``` to check container information, and ```docker logs container-id``` to troubleshoot in case of any error.

9. We can log into our container to confirm index.php location

    ```
    docker exec -it -u -0 container-id bash
    ```

![dk indexphp](https://github.com/uedwinc/Simple-HelloWorld-Apps-on-Docker/blob/main/images/dk%20indexphp.png)

10. We can also check the OS on which our container is running here

    ```
    cat /etc/os-release
    ```

![base img](https://github.com/uedwinc/Simple-HelloWorld-Apps-on-Docker/blob/main/images/base%20img.png)

10. We can also check the versions of php and apache2 on the OS

    ```php -version```
    
    ```apache2 -version```

12. Now, let's try to access the php webapp on a web browser using the ip address (no need for port as 80 is the default port for http)

![php web](https://github.com/uedwinc/Simple-HelloWorld-Apps-on-Docker/blob/main/images/php%20web.png)


### NodeJS Packaging

1. First, let's create a "helloapp-node" directory on our server.

    ```
    mkdir helloapp-node
    ```

2. Inside the helloapp-node directory, we'll create a "server.js" file and populate it with the relevant code. 

    ```
   vi server.js
    ```

    - This is listening on port 8080 as specified in the file, so we need to have that open in our security group

4. Next, we'll create a Dockerfile

    ```
   vi Dockerfile
    ```

6. Now, we need to build the Dockerfile into an image, tagged "node-helloapp".

    ```
    docker buildx build -t node-helloapp .
    ```

7. Now, we can run the image

    ```bash
    docker run -d \
    --name node-helloapp \
    -p 8080:8080 \
    node-helloapp:latest
    ```

    - We can see the node-helloapp container running

![nd ps](https://github.com/uedwinc/Simple-HelloWorld-Apps-on-Docker/blob/main/images/nd%20ps.png)

6. Now, let's try to access the node webapp on a web browser using the ip address and port

![node web](https://github.com/uedwinc/Simple-HelloWorld-Apps-on-Docker/blob/main/images/node%20web.png)


### Python-App Packaging

1. First, we'll create a "helloapp-py" directory on our server.

    ```
   mkdir helloapp-py
    ```

3. Inside the helloapp-py directory, we'll create a "server.py" file and populate it with the relevant code. 
    
    ```
   vi server.js
    ```

5. Next, we'll create a Dockerfile

    ```
   vi Dockerfile
    ```

7. Now, we need to build the Dockerfile into an image, tagged "python-helloapp".

    ```
   docker build -t python-helloapp .
    ```

9. Now, we can run the image

    ```
    docker run -p 8081:8080 -d --name python-helloapp python-helloapp:latest
    ```

    - We can't use port 8080 for another application on our host server as a node app is currently running on it.
    
    - But on the container end, we can have same port serving different applications, so we leave it as 8080.

![dk py](https://github.com/uedwinc/Simple-HelloWorld-Apps-on-Docker/blob/main/images/dk%20py.png)

6. Now, let's try to access the python webapp on a web browser using the ip address and port (8081)

![py web](https://github.com/uedwinc/Simple-HelloWorld-Apps-on-Docker/blob/main/images/py%20web.png)


### Removing Containers and Images

- To stop a running container

    ```
    docker stop container-id
    ```
- To remove a container

    ```
    docker rm container-id
    ``` 
-  To remove an image

    ```
    docker rmi image-name
    ``` 

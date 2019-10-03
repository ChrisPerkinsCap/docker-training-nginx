### docker-training-nginx
### Learning to control Containers
### NGINX CLI --- RUN - PORTS - FLAGS ---
### NGINX CLI --- START - STOP - LS ---
### NGINX CLI --- DETACHED MODE ---

# Docker CLI Reference
[docker Command Line Interface Documentation](https://docs.docker.com/engine/reference/commandline/cli)

[https://docs.docker.com/engine/reference/commandline/cli](https://docs.docker.com/engine/reference/commandline/cli)

# Starting a container
We're going to start an NGINX container and have a look at what it is using the docker Command Line Interface or (CLI).

Nginx is a reverse proxy server, which means that it listens to requests and forwards them on to other applications / endpoints. It's thus able to filter, redirect, limmit and control requests before they are sent on to their final destination. It can also be used as a normal web server to display web-pages and other content directly. This is exactly what it does out of the box. Its' default configuration will display a default home page when it is run in order to show and confirm that it is up and running.

To install Nginx normally on a Debian Linux system we would need to follow the process demonstrated in the following video. Please don't follow these steps. This is just to get an idea of all the work normally involved in setting up and configuring NGINX.

> SKIP THROUGH: \
https://youtu.be/TcKYbNvgg1Q

But some clever Dockerite has already done all the hardwork and captured it permanently in a docker image which we can use as a blueprint for creating containers.

To get Nginx up and running with docker we use the following command:

```bash
docker run -p 80:80 nginx
```

> Now open a browser and see if the Nginx default web page is displayed. \
[http://127.0.0.1](http://127.0.0.1) or [http://localhost](http://localhost) \
For those using play with docker you can hover over the hyperlinked port number [:80](http://ip172-18-0-48-bltrbiockd30008vvtu0-80.direct.labs.play-with-docker.com/) \
next to the ip address of your node instance, right click the link and select copy link address. Then paste the address into a browser.

If we look at our terminal we can see some log output on the screen. This is the default behavior of docker and Nginx together. That is, that docker will print log output sent to STDOUT to the screen by default, so long as the APP is set to send log output to standard out. This is generaly a good idea and a best practice with containerised Apps. Refresh the browser a few times and see what happens.

If we want to return control to the terminal we can use the standard CTR + C. Because the CLI uses the shell to run the container and runs it in the foreground this will actually stop the container not just return control to the termina.

# Listing Running Containers
Open a second terminal window and list all the currently running docker containers with the following command:

```bash
docker container ls
```

> You should see the following output:

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                NAMES
f941b7f8599b        nginx               "nginx -g 'daemon of…"   8 seconds ago       Up 7 seconds        0.0.0.0:80->80/tcp   brave_satoshi
```

# The Docker work-command pattern
In the initial command that we issued we followed a pattern for issuing work to docker.

```code
docker <docker-action> [<flag>] [<flag>, <flag-value>] <docker-object-name>
docker       run                  -p        80:80            nginx
```
Let's look at the port flag:

```bash
|     FLAG     |     Purpose                   |     VALUE     |
      -p         define a port mapping between      80:80
                 the host machine and the
                 container
```

# Mapping properties from host to container
In docker, host to container property mappings, always follow a left to right ordering of:

```bash
HOST : CONTAINER
```
# Separate services by port
Let's run a second Nginx container on a different port.

Take the command we used to start the current Nginx container

```bash
docker run -p 80:80 nginx
```

and adjust it to run an instance of Nginx on port 8080. Place a -d flag in front of the -p flag. 

Run the command in a second terminal and then run the following command in the same terminal to check if it is running.

```bash
docker container ls
```

You should see something similar to the following output:

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                  NAMES
9a435210e4c0        nginx               "nginx -g 'daemon of…"   17 seconds ago      Up 16 seconds       0.0.0.0:8080->80/tcp   intelligent_lamport
9894e335cee7        nginx               "nginx -g 'daemon of…"   2 minutes ago       Up 2 minutes        0.0.0.0:80->80/tcp     cranky_merkle
```

> Now open a browser and see if the Nginx default web page is displayed. \
> http://127.0.0.1:8080 \
> or \
> http://localhost:8080 \
> For those using play with docker you can hover over the hyperlinked port number (:8080) next to the ip address of your node instance, right click the link and select copy link address. Then paste the address into a browser.


# The 'docker-object' command pattern
Often the focus of what we're trying to acomplish with docker is not on the action we're trying to get docker to complete but is on a docker object that we are trying to manipulate. We may be focusing on a particular docker object such as a container.

When we're doing this we use the 'docker-object' command pattern:

```bash
docker <docker-object-type> <docker-object-command> [<docker-object-name>]
```

One good example of this is if we wanted to stop one of the containers that we currently have running. let's say we stop the first one.

```bash
docker container stop cranky_merkle
```

Let's check the list of currently running docker comntainers:

```bash
docker container ls
```

We should now only see one container in the list.

We can get docker to show us all of the running and stopped containers by appending the -a (all) flag to the end of the command.

We should see output similar to the following:

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                      PORTS                  NAMES
9a435210e4c0        nginx               "nginx -g 'daemon of…"   35 minutes ago      Up 35 minutes               0.0.0.0:8080->80/tcp   intelligent_lamport
9894e335cee7        nginx               "nginx -g 'daemon of…"   38 minutes ago      Exited (0) 20 seconds ago                          cranky_merkle
```

We can use the same pattern of command to restart a stopped container:

```bash
docker container start cranky_merkle
```

Lets use the list all containers command again to see what's happend after trying to start a stopped container.

We should see that both containers are back up and running again according to the specification of their original commands.

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                  NAMES
9a435210e4c0        nginx               "nginx -g 'daemon of…"   43 minutes ago      Up 43 minutes       0.0.0.0:8080->80/tcp   intelligent_lamport
9894e335cee7        nginx               "nginx -g 'daemon of…"   45 minutes ago      Up 7 seconds        0.0.0.0:80->80/tcp     cranky_merkle
```

We can also use this pattern to apply acctions to multiple docker-objects of the same type at once by adding multiple docker-object-names to the end of the command.

Try and stop both containers at the same.

Check to see if they have stopped by checking their status with the list all containers command.

If they have both stopped then try to delete both of them with the rm command.

## Detached Mode

To run a docker container in detached mode in the docker CLI we use the following command:

```bash
docker run -d -p 80:80 nginx
```

When we do this the container is run in the background the containers log output is not sent to the terminal window and control is returned to the terminal.

It's not immediately obvious that a container is running without running:

```bash
docker container ls
```

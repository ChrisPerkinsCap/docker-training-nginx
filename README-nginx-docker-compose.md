### docker-training-nginx
### Learning to control Containers
### NGINX docker-compose --- RUN - PORTS - FLAGS ---
### NGINX docker-compose --- UP - DOWN ---

# Docker Compose 3 Reference

[Docker Compose 3 Documentation](https://docs.docker.com/compose/compose-file)

[https://docs.docker.com/compose/compose-file](https://docs.docker.com/compose/compose-file)

# Starting a container
As we've seen previously, with the docker CLI we can spin up an application easily with the following command:

```bash
docker run -p 80:80 nginx
```

Using the CLI is very hands on and can be very useful, especially when we need to get up close and personal and work with a container in a very direct way. We can make it do things, inspect it and even 'login' to the container, explore and manipulate its' filesystem, process and configuration.

There is also another way to manage containers, which presents a different set of advantages. That is using docker-compose. docker-compose allows us to translate docker commands into .yaml files like the following example:

```yaml
version: '3.6'

services:

  web-server:
    image: nginx
    ports:
      - "80:80"
```

The default name for a docker compose file is docker-compose.yaml. If we issue the following command from the the same directory as the compose file it will look for a file called 'docker-compose.yml' and run it:

```bash
docker-compose up
```
If we want to assign a different name to the file or call it from a different location we can but when we want to run the file we need to explicitly provide the file name and / or location as below:

```bash
docker-compose -f ./mydocker/compose-file.yml up
```

## Exercise:

> In the terminal navigate to the directory where the docker-compose.yml file is found and start the container using the compose file. \
Open a browser to: \
http://localhost:80 or http://127.0.0.1:80 \
and verify that nginx is up and running and serving the default webpage. If we look at our terminal we can see some log output on the screen. This is the default behavior of docker-compose.

Here is one of our first subtil differences between the CLI and docker compose although docker-compose still outputs the log entries and error messages to the terminal screen, the container is actually being run in the background and not in the shell.

If we use CTL + c to return contol to the terminal no more log or error entries are output to the screen. But unlike the CLI the container is still running in the background.

Use the following commands to test this out:

```bash
docker-compose up

CTL + c

docker container ls
```

We can still use the CLI to manage a container started with docker-compose.

> Use the docker CLI to list and inspect the running container. \
Try and figure out how to stop the container.

Starting containers with compose is subtilly different to from starting with the CLI. 

We will look at those differences as we move through the exercises. For now given that we are still using the command line to start the container, what advantages if any could be gained by using a compose file?

What advantages could be gained by using docker-compose?

# Detached mode

To run a container in detached mode using docker-compose we also add the -d flag to the start command as follows:

```bash
docker-compose up -d
```

You will notice that no more output is sent to the terminal. The terminal is now free to mange the container and other containers, or equally perform any other task without being attached to the original container.

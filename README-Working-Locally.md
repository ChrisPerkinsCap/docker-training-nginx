# docker-training-nginx
This repository contains Starting point and final implementation examples for teaching docker principles using NGINX

# Running Applications Locally
When running an application directly on an operating system (machine). From the perspective of the machine that the app is running on we consider the application to be running locally.

Each operating system uses a standard ip address reserved especially as a default for running local services. This IP address is 127.0.0.1 and is called the loop-back address. This ia mapped to the more human readable domain name of http://localhost by default.

Multiple applications can be run the localhost or loop-back address 127.0.0.1. Each service will be assigned its' own port over which it will be accessible.

So for example we could run 'nginx' as a web server on the default web traffic port of 80 and access the 'nginx' application by entering the following address into a web browser:

[http://127.0.0.1:80](http://127.0.0.1:80)

or 

[http://localhost:80](http://localhost:80)

While at the same time we could we could run Mongo (Database) at the following address:

[http://127.0.0.1:27017](http://127.0.0.1:27017)

# USING local docker
When using docker locally the applications are run inside a container, which has its' own operating system and its' own loopback address.

When running an application as a docker container the application is run on the localhost or loop-back address (127.0.0.1) of the container and can not be accessed from outside the container unless it is mapped to the host machine.

A container has its' own full set of standard IP addresses and Ports just like any other operating system.

Each application that runs inside a container will be assigned its' own port over which it will be accessible inside the container.

> N.B. Though it is possible to run multiple applications inside one container this is an ANTI-PATTEERN and BAD-PRACTICE. It can also require considerable work to make all applications run persistently and securely. With very few execptions, each application should run inside its' own container. If two or more applications are so tightly coupled that they can not be run in their own container running them on bare metal or decoupling them should be prefered over running them all from one container.  


For example:

The default port for postgres is 5432. So inside the container an instance of postgres with the default configuration will run on 127.0.0.1:5432 where 127.0.0.1 is the loopback address inside the container and is not visible or accessible from outside the container. This means that we can't access it unless it is mapped by docker to the host machine.

Container ports are visible from outside the container, if an external application knows which port on which container to look at.

We can map a container to the host and it will automatically be exposed over the localhost or loopback address on the host, as if it were running directly on that machine. To prevent lots of applications clashing as they all get exposed on the hosts loopback address, each port can only be mapped to one container.

This mapping is achieved by mapping the port that the application runs on in the container to the host port that we want to expose it on.

To make postgress available on the host we need to use dockers port mapping to set the following:

```pretty
<HOST-PORT:CONTAINER-PORT>
      8080:5432
```

> Mappings in docker always follow the left to right pattern of <HOST-PORT:CONTAINER-PORT>

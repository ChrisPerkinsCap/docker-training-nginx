# Using play with docker

Open a terminal a browser and navigate to 
> https://labs.play-with-docker.com \
>https://github.com/play-with-docker/play-with-docker

>Select Login-> docker \
> Enter your dockerhub login credentials \
> Select Start

In the Blue space in the top-left corner you will see a session timer that starts at 4 hours and counts down. After 4 hours it will wipe your session destroy all your work and you need to start a freesh session.

> Select add a new instance on the left and a terminal window will open in the browser.

At the click of a button you have a fully functional Alpine Linux machine with docker fully instaslled. We can verify that with the following commands:

> cat /etc/alpine-release \
> docker -v \
> docker version

Now we can use docker in this window as we would in a terminal on any other machine. To view applications that display in the browser make sure to assign a host to container port mapping with the '-p' flag.

When docker spins up the container a hyperlink of the port number will appear to the right of the IP address. The link targets the newly created application. Open the link in a browser to see the App.

Try the following command to try it out:

> docker run -d --name some-ghost -e url=http://localhost:80 -p 80:2368 ghost

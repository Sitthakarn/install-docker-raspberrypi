# Install docker &amp; docker-compose on raspberry pi
from : Jonathan Meier https://jonathanmeier.io/author/jonathan/

I use Jonathan's setup procedure to install docker & docker-compose. 
It's very easy way to install.

Install Docker and Docker-Compose on your Raspberry Pi
March 6, 2019 jonathan

In this tutorial, we will cover how to install docker and docker-compose on a raspberrypi and run a simple container.

One thing to keep in mind through all of this is that the pi is built on an ARM architecture as opposed to Intel. As a result, you will only be able to run containers built for the ARM architecuture.

Fortunately, Docker Official Images should all support this architecture as per this post

That said, let’s get started.

## 1. First, you can install docker with an easy install script as follows:

```
$ curl -sSL https://get.docker.com | sh
```
Test that the daemon is working with a classic hello world:

```
$ docker run --rm hello-world
```
You should see the following output:

```
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
c1eda109e4da: Pull complete
Digest: sha256:2557e3c07ed1e38f26e389462d03ed943586f744621577a99efb77324b0fe535
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (arm32v7)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.
```

To try something more ambitious, you can run an Ubuntu container with:

```
$ docker run -it ubuntu bash
```

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
If you get a permissions error, be sure to add your user to the docker group:

```
$ usermod -aG docker <your_user>
```

And then logout and login again.

Now that we know docker is working, let’s go and grab docker-compose.

## 2. Install pip:

```
$ curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py && sudo python3 get-pip.py
```

## 3. Install docker-compose
Now install docker-compose with:

```
$ sudo pip3 install docker-compose
```

Finally, test that things are working by creating a docker-compose file:

```
$ vim docker-compose.yml
```
## 4. Install vim
If you don’t have vim installed, you can grab it with:

```
$ sudo apt-get update && sudo apt-get install vim -y
```
## 5. Test docker-compose
Now in docker-compose.yml put:

```
$ vim docker-compose.yml
```
Add this settings:

```
version: '3'
services:
  webapp:
    ports:
      - 5000:8000
    image: python:3.7-alpine
    command: "python -m http.server 8000"
```

Then start the service with:

```
$ docker-compose up -d
```

Now you can curl the service with:
```
$ curl -iv 0.0.0.0:5000
```
and you should get a response similar to the following:

```
* Rebuilt URL to: localhost:5000/
*   Trying ::1...
* TCP_NODELAY set
* Connected to localhost (::1) port 5000 (#0)
> GET / HTTP/1.1
> Host: localhost:5000
> User-Agent: curl/7.52.1
> Accept: */*
>
* HTTP 1.0, assume close after body
< HTTP/1.0 200 OK
HTTP/1.0 200 OK
< Server: SimpleHTTP/0.6 Python/3.7.2
Server: SimpleHTTP/0.6 Python/3.7.2
< Date: Sun, 10 Feb 2019 04:08:24 GMT
Date: Sun, 10 Feb 2019 04:08:24 GMT
< Content-type: text/html; charset=utf-8
Content-type: text/html; charset=utf-8
< Content-Length: 915
Content-Length: 915

<
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8">
<title>Directory listing for /</title>
</head>
<body>
<h1>Directory listing for /</h1>
<hr>
<ul>
<li><a href=".dockerenv">.dockerenv</a></li>
<li><a href="bin/">bin/</a></li>
<li><a href="dev/">dev/</a></li>
<li><a href="etc/">etc/</a></li>
<li><a href="home/">home/</a></li>
<li><a href="lib/">lib/</a></li>
<li><a href="media/">media/</a></li>
<li><a href="mnt/">mnt/</a></li>
<li><a href="opt/">opt/</a></li>
<li><a href="proc/">proc/</a></li>
<li><a href="root/">root/</a></li>
<li><a href="run/">run/</a></li>
<li><a href="sbin/">sbin/</a></li>
<li><a href="srv/">srv/</a></li>
<li><a href="sys/">sys/</a></li>
<li><a href="tmp/">tmp/</a></li>
<li><a href="usr/">usr/</a></li>
<li><a href="var/">var/</a></li>
</ul>
<hr>
</body>
</html>
Curl_http_done: called premature == 0
Closing connection 0
Provided you get a 200 response, your docker-compose service is up and running!
```

Now you can bring the service down with docker-compose down, and you’re all done.

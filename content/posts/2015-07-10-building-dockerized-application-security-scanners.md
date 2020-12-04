---
date: 2015-07-10T14:49:50Z
categories: [docker, docker-compose]
summary: A first attempt at automating web application security scans for dockerized
  web-applications
title: Building Dockerized Application Security Scanners
url: /2015/07/10/building-dockerized-application-security-scanners/
---

I read an [interesting post](https://abhartiya.wordpress.com/2015/07/09/automating-zap-running-against-a-web-application-in-docker-containers/) by [@anshuman_bh](https://twitter.com/anshuman_bh) on automating ZAP scans for dockerized web applications. At the time, I was exploring [docker compose](https://docs.docker.com/compose/) (formerly fig) as a way to "container-ize" a simple Python web application (Flask, Postgres, RabbitMQ). It occurred to me that `docker-compose` would be a good way to accomplish this goal. The advantage of using docker-compose over using docker directly is that you can avoid writing "glue" code by defining links between containers. The example I discuss below is for ZAP, but you could use it for any other web application scanner that can be run in headless mode.

### The Problem
If you haven't read Anshuman's post yet, I suggest that you [check it out](https://abhartiya.wordpress.com/2015/07/09/automating-zap-running-against-a-web-application-in-docker-containers/) to get an understanding of what he's trying to accomplish. The goal is to automate scans against a web application that runs in a docker container. This is useful if you're trying to set-up some sort of CI infrastructure that automatically scans new builds of the web application. We want to ensure that no human interaction is required, and reports generated from scans are saved somewhere on the host machine (outside the containers) to be processed or read.

### The Setup
To accomplish this, I'll use docker-compose instead of plain docker. To follow along, install docker-compose [from here](https://docs.docker.com/compose/#installation-and-set-up). We'll use three containers:

   * The application being tested. For this example, I'll use the famous WebGoat application because, well, goats.
   * The [Zed Attack Proxy](https://www.owasp.org/index.php/OWASP_Zed_Attack_Proxy_Project). You could use any other application scanner that supports automation here.
   * A container that contains custom scripts. We could build on the ZAP container too, but I've found that this is more flexible.

 The idea is to spin up the test application, followed by an instance of ZAP. After these containers are up, we want to connect to the ZAP container and direct it to scan the target and save the results once done.

### Docker-composing All The Things

 Using docker-compose is easy. All you need is one YAML file to define your containers and how they're linked. Here's what it looks like for my setup:
<script src="https://gist.github.com/ritesh/2fcd9fd32995ffeb30f1.js"></script>

To test my setup out, start by [cloning this repo](https://github.com/ritesh/dockerscan). Once you have cloned it, run `docker-compose build`. This will take a while to download images for the first time. Once this is done, run `docker-compose up` to bring up all containers listed in `docker-compose.yml`. After the last container exits, you should have a report from the ZAP scan in `reports/`.

So, what just happened? `docker-compose` looks at the YAML file and figures out what containers need to be started in what sequence. It starts the "target" container, which contains our test application (WebGoat in this case), followed by the ZAP container. The "tooling" container is started last. The `Dockerfile` for tooling is defined in `tools`. It builds on the `python 2.7` image and contains a script to run the ZAP scan. You could add more tools/scripts here that you might want to run against the web application.

Hope this gives you ideas for workflows for automating scans. Send comments/questions/flames to [@rsinha](https://twitter.com/rsinha). 

---
author: aweiteka
comments: true
layout: post
title: Introducing the Open Source Enterprise Registry
date: 2014-09-05 09:22 UTC
tags:
- Registry
- Pulp
- Enterprise
- Docker
- Devops
- Community
categories:
- Blog
---
<img src="http://www.projectatomic.io/images/pulp-logo.png"> As Docker matures it is becoming clear that the registry is a critical part of a devops environment. It is the hand-off point between developer, test and operations. Registry administrators need an interface to control access and manage the lifecycle of repositories and images.

## The Pulp Registry
Long before Docker became popular there was an open-source package management system called Pulp. The [Pulp Project](http://www.pulpproject.org/) has enjoyed heavy contributions from Red Hat devopers, maturing into a robust backend for managing repositories of RPMs, ISOs, Puppet modules and OpenStack VM images via Glance. Recently a Docker plugin was added to support Docker image content. The Pulp team also developed [Crane](https://github.com/pulp/crane), a lightweight, read-only implmentation of the Docker registry protocol to serve `docker pull` requests.

What I like about integrating Pulp into our docker environment is it's a *mature and flexible platform that understands its role in managing content*. Pulp tries to do one thing well. It provides a clean, well-documented interface for integrating into your infrastructure.

Core infrastructure needs to scale and be highly available. Pulp's service-oriented architecture allows for this. Tasks are placed in a message-based distributed task queue so worker services may be scaled out as load increases. The database is MongoDB which can be configured as a highly available replica set. Web services can be placed behind a load balancer.

If you have a large list of things you wish the docker registry could do, check out Pulp. There are many benefits to using Pulp:
* Separation of admin and end-user interfaces
* Role-based access control with LDAP support
* Syncronize content across an organization
* Read-only implementation of the docker registry protocol that can be deployed independently, making it very secure
* List pending and completed tasks
* Manage different content types in one place

## Try it
A Pulp server is typically deployed as a VM but we have also provided a multi-container environment for simple, non-production trial use. To get started with the Pulp registry walk through the [quickstart guide](https://github.com/aweiteka/pulp_packaging/blob/dockerfiles/dockerfiles/docker-quickstart.rst). You don't really *install* the container-packaged Pulp registry. It's a matter of pulling and running the images. Be sure to review the [requirements](https://github.com/aweiteka/pulp_packaging/blob/dockerfiles/dockerfiles/docker-quickstart.rst#requirements) before running the installer script.

```
$ curl -O https://raw.githubusercontent.com/pulp/pulp_packaging/master/centos/install_pulp_server.sh
$ sudo bash install_pulp_server.sh
```

It takes a while to download all the images so be patient!

## Use it
We developed a prototype script to interact with Pulp that is focused on Docker workflows. To keep installation simple the pulp-admin client runs as a container. Get the script:

```
$ curl -O https://raw.githubusercontent.com/pulp/pulp_packaging/master/registry_admin.py
$ chmod +x registry_admin.py
```

When you run the script the first time you will be prompted for configuration information. Try pushing an image to the Pulp registry:

```
$ ./registry-admin.py push my/app
Registry config file not found. Setting up environment.
Creating config file /home/aweiteka/.pulp/admin.conf
Enter registry server hostname: registry.example.com
Verify SSL (requires CA-signed certificate) [False]:
User certificate not found.
Enter registry username [aweiteka]: admin
Enter registry password:

Pulling docker images
Pulling repository pulp/pulp-admin
8a01d78f4c70: Download complete

Repository [my-app] successfully created
[output truncated]
...
```

List repos:
```
$ ./registry-admin.py list repos
my/app
```

List images in a repo:
```
$ ./registry-admin.py list my/app
511136ea3c5a64f264b78b5433614aec563103b4d4702f3ba7d4d2698e22c158
7b23ea3439e3aceaa35bc33529535b3e52c3cf98672da371d9faa09b2969f47c
bcc5d0080e78726615e55c0954156e1be584832284c9a6621436feb027ae7845
c811aee30291a2960fbc5b8c46b8c756b4ad98f0c4d44e79b7c7729f1a35ee20
```

There's lots more you can do. Check out the rest of the [quickstart guide](https://github.com/aweiteka/pulp_packaging/blob/dockerfiles/dockerfiles/docker-quickstart.rst#registry-management) for setting up permissions and managing docker repositories.

## Feedback and Contributions
There's a lot of work to do but we we need to hear from you to make this a great tool. How do you want to use the registry in your organization? Do you have a CI build environment to plug into? Let us know!

**Contact**
* IRC: #pulp on freenode
* Email: <pulp-list@redhat.com>


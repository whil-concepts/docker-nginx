# About this Repo

This repo is forked from the [Git repo](https://github.com/nginxinc/docker-nginx) of the official Docker image for [nginx](https://registry.hub.docker.com/_/nginx/).
See the Hub page for the full readme on how to use the Docker image and for information regarding contributing and issues.

The full readme is generated over in [docker-library/docs](https://github.com/docker-library/docs),
specifically in [docker-library/docs/nginx](https://github.com/docker-library/docs/tree/master/nginx).

# Build

We only use the mainline Alpine Linux container.  
Our config modifications redirect any requests to www.whil.com.
Requests for `/healthcheck` are answered locally, everything else is redirected with the same path.

Build the image with this:

	mainline/alpine> docker build -t=whil/redirect2whilcom

# Deploy

Deploy this container as a Docker Service.
It does not need to be part of the whil-net docker network.
It is configured to work behind an Amazon ELB.
Map host port 8777 to container port 80
Map host port 8888 to container port 443

	docker service create -p 8777:80 -p 8888:443 --log-opt max-size=10m --log-opt max-file=3 --name redirect2whilcom --with-registry-auth whil/redirect2whilcom:latest



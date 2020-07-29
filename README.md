## IBM Edge Application Manager (IEAM) - RTSP server 

### Introduction

RTSP server uses docker image from https://github.com/mpromonet/v4l2rtspserver

#### Build, Publish and Use in a shared IEAM environment

Using a shared single tenant instance creates several challenges when services, patterns and policies need to be published in a common exchange under one org structure without multiple developers clobbering over each other.

#### Issues

- Developers need to identify their service, pattern and policies among many similar assets (concept of owner)
- Developers may have project for demo, dev, test and more ( group the assets)
- Developers need to use their own docker repository (specify own docker account)

The tooling outlined below addresses these concerns and builds on top of existing infrastructure

### Automated Steps

Start with reviewing Makefile for targets.

Build and publish images to docker and services to exchange. Can be executed again and again.

    make
    
Publish deploy-policy

    make deploy-policy

### ENVIRONMENT variables

Must define following ENVIRONMENT variables to build application, add policies and register edge node. Preferebly save in the script directory and use when invoking device registration. 

Enviornment variables EDGE_OWNER, EDGE_DEPLOY provide flexiblity for different developers to use the same exchange without clobering over each other.

    export EDGE_OWNER=<a-two-or-three-letter-distinctive-initial-of-your-name>  # sg.dev  
    export EDGE_DEPLOY=<deploy-target> # e.g: example.rtsp

    export DOCKER_BASE=<docker-base> # e.g. Change this this your docker base 

    export HZN_ORG_ID=mycluster
    export HZN_EXCHANGE_USER_AUTH=iamapikey:<iam-api-key>
    export HZN_EXCHANGE_NODE_AUTH="<UNIQUE-NODE-ANME>:<node-token>"

    hzn exchange node create -n $HZN_EXCHANGE_NODE_AUTH

### Register node - RPI 4 B
Use script to register node

policy

    ./node_register_app.sh -e ENV_TFVISUAL_DEV -r -l


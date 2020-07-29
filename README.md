## IBM Edge Application Manager (IEAM) based RTSP Video Server 

### Introduction
Provides a RTSP Video Server using Raspberry Pi .

### Pre-requisite 

- `hzn` cli is installed on the target `edge node` and verified to be working.
- Raspberry Pi (RPI 4 preferred. Works on RPI 3 as well but will notice lag.)

    - RPI camera 
         - RPI camera is connected to RPI and verified to be working.

    - USB camera    
        - TODO
        
- NUC 
    - TODO
    
### Quick register

1. Create a `node_policy.json` file
```
{
  "properties": [
      { "name": "owner",  "value": "sg.edge" },
      { "name": "rtsp", "value": true },
      { "name": "openhorizon.allowPrivileged", "value": true }
  ],
  "constraints": [
  ]
}
```

3. Setup     
```
export EDGE_OWNER=sg.edge
export EDGE_DEPLOY=example.rtsp
export HZN_ORG_ID=mycluster
export HZN_EXCHANGE_USER_AUTH=iamapikey:<iam-api-key-provided-to-you-for-the-target-ieam-instance>
```
2. Register
```
hzn register --policy=node_policy.json 
```

### Credit

This RTSP server uses docker image from https://github.com/mpromonet/v4l2rtspserver

### Development
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

### ENVIRONMENT variables

Must define following ENVIRONMENT variables to build application, add policies and register edge node. Preferebly save in the script directory and use when invoking device registration. 

Enviornment variables EDGE_OWNER, EDGE_DEPLOY provide flexiblity for different developers to use the same exchange without clobering over each other.

    export EDGE_OWNER=<a-two-or-three-letter-distinctive-initial-of-your-name>  # sg.dev  
    export EDGE_DEPLOY=<deploy-target> # e.g: example.rtsp

    export HZN_ORG_ID=mycluster
    export HZN_EXCHANGE_USER_AUTH=iamapikey:<iam-api-key>

### Register node - RPI 4 B
Use script to register node

    hzn register --policy=node_policy.json


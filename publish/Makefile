#
# Makefile: A RTSP server
#

# Checks required environment variables
-include env.check.mk

export ARCH ?= $(shell hzn architecture)

all: publish-rtsp deploy-policy

publish-rtsp:
	make -C src/rtsp publish-service

deploy-policy:
	make -C src/rtsp deploy-policy





SHELL := /bin/bash
VERSION := $(shell cat ../../version.txt)
IMAGE_NAME_BASE=tfserving-proxy
IMAGE_NAME=seldonio/${IMAGE_NAME_BASE}
KIND_NAME ?= kind

build:
	s2i build \
		-E environment \
		. \
		seldonio/seldon-core-s2i-python37-ubi8:${VERSION} \
		${IMAGE_NAME}:${VERSION}

push:
	docker push ${IMAGE_NAME}:${VERSION}

kind_load: build
	kind load -v 3 docker-image ${IMAGE_NAME}:${VERSION} --name ${KIND_NAME}

scan=ospid-f0a0cf75-6cb0-4f80-a9d8-a7205770e265
redhat-image-scan:
	docker pull ${IMAGE_NAME}:${VERSION}
	source ~/.config/seldon/seldon-core/redhat-image-passwords.sh && \
		echo $${rh_password_tfproxy} | docker login -u unused scan.connect.redhat.com --password-stdin
	docker tag ${IMAGE_NAME}:${VERSION} scan.connect.redhat.com/${scan}/${IMAGE_NAME_BASE}:${VERSION}
	docker push scan.connect.redhat.com/${scan}/${IMAGE_NAME_BASE}:${VERSION}


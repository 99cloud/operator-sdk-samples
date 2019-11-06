.PHONY: install image

IMG ?= memcached-operator
TAG ?= demo
REPO_URL ?= https://github.com/99cloud/operator-sdk-samples
BRANCH ?= animbus-v3.11
COMMIT_REF ?= $(shell git show-ref --heads -s)
OPERATOR_RELEASE_VERSION ?= v0.12.0

export GO111MODULE=on
export GOPROXY=https://goproxy.io

install:
	- curl -LO https://github.com/operator-framework/operator-sdk/releases/download/${OPERATOR_RELEASE_VERSION}/operator-sdk-${OPERATOR_RELEASE_VERSION}-x86_64-linux-gnu
	- chmod +x operator-sdk-${OPERATOR_RELEASE_VERSION}-x86_64-linux-gnu 
	- sudo mkdir -p /usr/local/bin/ 
	- sudo cp operator-sdk-${OPERATOR_RELEASE_VERSION}-x86_64-linux-gnu /usr/local/bin/operator-sdk 
	- rm operator-sdk-${OPERATOR_RELEASE_VERSION}-x86_64-linux-gnu

image:
	- operator-sdk build --image-build-args "--build-arg REPO_URL=$(REPO_URL) --build-arg  BRANCH=$(BRANCH) --build-arg COMMIT_REF=$(COMMIT_REF)" $(IMG):$(TAG)
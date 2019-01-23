SHELL := $(shell which bash)
ENV = /usr/bin/env

.SHELLFLAGS = -c

.SILENT: ;
.ONESHELL: ;
.NOTPARALLEL: ;
.EXPORT_ALL_VARIABLES: ;

.PHONY: all
.DEFAULT: help

APPNAME=argsnenvs
APPVERSION=1.0
BASEIMAGE=alpine
BASEIMAGEVERSION=3.2
BUILD_TIME=`date +%FT%T%z`

ENVIRO1="here and now"

help: ## Show Help
	@grep -E '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}'

build-info: ## Show build info
	echo "$(APPNAME) $(APPVERSION) [$(BASEIMAGE) $(BASEIMAGEVERSION)] $(BUILD_TIME)"

clean: ## Delete image
	docker container rm `docker container ls -aq --filter ancestor=$(APPNAME):$(APPVERSION)`
	docker image rm $(APPNAME):$(APPVERSION)

build: ## Build the docker image
	docker build -t $(APPNAME):$(APPVERSION) --build-arg BASEIMAGE=$(BASEIMAGE) --build-arg BASEIMAGEVERSION=$(BASEIMAGEVERSION) .

run: ## Run the image
	docker run -it --env ENVIRO1=$(ENVIRO1) $(APPNAME):$(APPVERSION) /bin/sh

push: ## Push image to remote repo
	echo "Nothing yet"

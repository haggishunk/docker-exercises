.PHONY: all
.DEFAULT: help

APPNAME=publish-and-expose
APPVERSION=1.0
BUILD_TIME=`date +%FT%T%z`

## PORT_OUTSIDE is the port you choose to have exposed on the host
## PORT_INSIDE is the port that the application in the container listens on
PORT_OUTSIDE=8080
PORT_INSIDE=80

help: ## Show Help
	@grep -E '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}'

build-info: ## Show build info
	echo "$(APPNAME) $(APPVERSION) $(BUILD_TIME)"

clean: ## Stop all running containers, delete the containers and image
	docker stop `docker ps -aq --filter ancestor=$(APPNAME):$(APPVERSION)`
	docker container rm `docker container ls -aq --filter ancestor=$(APPNAME):$(APPVERSION)`
	docker image rm $(APPNAME):$(APPVERSION)

build: ## Build the docker image
	docker build -t $(APPNAME):$(APPVERSION) .

run: ## Run the image without specifying any port publishing
	docker run -d --name $(APPNAME) $(APPNAME):$(APPVERSION)

run-publish: ## Run the image and specifying the port to publish
	docker run -d --name $(APPNAME)-publish -p $(PORT_OUTSIDE):$(PORT_INSIDE) $(APPNAME):$(APPVERSION)

run-automap: ## Run the image and let docker automatically map the host port
	docker run -d --name $(APPNAME)-automap -P $(APPNAME):$(APPVERSION)

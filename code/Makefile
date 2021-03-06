DOCKER_REGISTRY?=docker.io
# Forks may use name/ara for their tests
IMAGE_NAME?=ara
VERSION?=latest

DB_IMAGE=${DOCKER_REGISTRY}/${IMAGE_NAME}-db:${VERSION}
BACK_IMAGE=${DOCKER_REGISTRY}/${IMAGE_NAME}-back:${VERSION}
FRONT_IMAGE=${DOCKER_REGISTRY}/${IMAGE_NAME}-front:${VERSION}

cache-build-back:
	@echo '-- Build java ara-back with local cache --'
	@docker container run -v "$$PWD/back":/root/ara -w /root/ara maven:3-jdk-11 mvn -Pprod -s settings.xml clean package dependency:go-offline

build-db: ## Build ara-db
	@echo '-- Build ${DB_IMAGE} --'
	@docker build -t ${DB_IMAGE} data/instance

build-back: ## Build ara-back
	@echo '-- Build ${BACK_IMAGE} --'
	@docker build -t ${BACK_IMAGE} back

build-front: ## Build ara-front
	@echo '-- Build ${FRONT_IMAGE} --'
	@docker build -t ${FRONT_IMAGE} front

run-front: ## Run front
	@echo '-- Run ara-front --'
	@export VERSION=$(VERSION); docker-compose up ara-front

run-back: ## Run back
	@echo '-- Run ara-back --'
	@export VERSION=$(VERSION); docker-compose up ara-back

run-db: ## Run database
	@echo '-- Run database --'
	@export VERSION=$(VERSION); docker-compose up ara-db

build-full: build-back build-front build-db ## Build ARA

build: ## Build ARA in parallel
	@$(MAKE) build-full -j3 -k

promote-back: ## promote ara back to specified registry
	@echo '-- Push ${BACK_IMAGE} --'
	@echo "${DOCKER_PASSWORD}" | docker login -u "${DOCKER_USERNAME}" --password-stdin ${DOCKER_REGISTRY}
	@docker push ${BACK_IMAGE}
	@docker logout

promote-front: ## promote ara back to specified registry
	@echo '-- Push ${FRONT_IMAGE} --'
	@echo "${DOCKER_PASSWORD}" | docker login -u "${DOCKER_USERNAME}" --password-stdin ${DOCKER_REGISTRY}
	@docker push ${FRONT_IMAGE}
	@docker logout

promote: promote-back promote-front ## Build ARA

start: ## Start ARA
	@echo '-- Start ARA --'
	@export VERSION=$(VERSION); docker-compose up -d

stop: ## Stop ARA
	@echo '-- Stop ARA --'
	@export VERSION=$(VERSION); docker-compose down

helm-tempate:
	@helm template ../charts/stable --debug

helm-uninstall:
	@helm uninstall --namespace ara ara-local

helm-install:
	@helm install --create-namespace --namespace ara ara-local ../charts/stable

helm-upgrade:
	@helm upgrade --namespace ara ara-local ../charts/stable

.PHONY: help
help:
	@grep -E '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}'

.DEFAULT_GOAL := help

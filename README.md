# RedCraft.org Kubernetes infrastructure

## Why

### Why Kubernetes?

Kubernetes is an excellent way of deploying an infrastructure with multiple environments with containers.

### Why are persistant volumes not with the services?

Because you might want to do a `kubectl delete -f <name>-service.yaml` without deleting your data :)

## How to deploy

### Requirements

kubectl

### Create environments

Create namespace environment (production/staging/testing):
`kubectl apply -f namespaces/<environment>.yaml`

## Deploy certificate issuer

Deploy cert-manager:
`kubectl apply -f https://github.com/jetstack/cert-manager/releases/download/v1.5.3/cert-manager.yaml`

⚠️ If you're not deploying a service for RedCraft.org, please replace the email address in cert-manager/cert-manager.yaml ⚠️

Deploy certificate issuer:
`kubectl apply -f cert-manager/cert-manager.yaml`

### Deploy databases

For these examples, you'll need to specify the namespace by replacing `<environment>` for each environment.

#### MariaDB

Create secret:
`kubectl create secret generic mariadb --namespace=<environment> --from-literal=password=<password>`

Create persistent volume:
`kubectl apply -f mariadb/mariadb-pv.yaml --namespace=<environment>`

Deploy service:
`kubectl apply -f mariadb/mariadb-service.yaml --namespace=<environment>`

#### MongoDB

Create persistent volume:
`kubectl apply -f mongodb/mongodb-pv.yaml --namespace=<environment>`

Deploy service:
`kubectl apply -f mongodb/mongodb-service.yaml --namespace=<environment>`

#### Redis

Deploy service:
`kubectl apply -f redis/redis-service.yaml --namespace=<environment>`

### Deploy Bastion

Bastion is used as an SSH relay to access internal services and databases.
We use [bastion](https://github.com/cloudposse/bastion) and [github-authorized-keys](https://github.com/cloudposse/github-authorized-keys) from [Cloud Posse](https://github.com/cloudposse) to do so, and use our GitHub public keys to connect to the cluster.

Create namespace:
`kubectl apply -f namespaces/bastion.yaml`

Create secret:
`kubectl create secret generic bastion --namespace=bastion --from-literal=github_organization=<organization> --from-literal=github_team=<team_name> --from-literal=github_api_token=<api_token>`

Create persistent volume:
`kubectl apply -f bastion/bastion-pv.yaml`

Deploy service:
`kubectl apply -f bastion/bastion-service.yaml`

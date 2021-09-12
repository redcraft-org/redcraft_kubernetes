# redcraft-kubernetes

## Requirements

kubectl

## Secrets that needs to be set

`kubectl create secret generic mariadb-production-root-password --namespace=production --from-literal=password=<password>`

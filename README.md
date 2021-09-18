# redcraft-kubernetes

## Requirements

kubectl

## Secrets that needs to be set

`kubectl create secret generic mariadb-prod --namespace=production --from-literal=password=<password>`
`kubectl create secret generic bastion --namespace=bastion --from-literal=github_organization=<organization> --from-literal=github_team=<team_name> --from-literal=github_api_token=<api_token>`

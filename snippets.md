## POC

### Deploy Sample App

```bash

# Simple pod deploy
kubectl run nginx --image=nginx
kubectl expose pod/nginx --port=80 --target-port=80 --type=LoadBalancer


# Deploy Redis (Helm)
https://bitnami.com/stack/redis/helm 

export REDIS_SERVER='redis-release-master.redis.svc.cluster.local:6379'
export REDIS_PASSWD='set-me-to-something'

helm repo add azure-marketplace https://marketplace.azurecr.io/helm/v1/repo
helm repo update

kubectl create ns redis
helm install redis azure-marketplace/redis \
    --namespace redis \
    --set auth.password=$REDIS_PASSWD \
    --set replica.replicaCount=2

# Deploy Postgres SQL
https://bitnami.com/stack/postgresql/helm

export POSTGRES_ID='reddog'
export POSTGRES_PWD='set-me-to-something'

helm repo add azure-marketplace https://marketplace.azurecr.io/helm/v1/repo
helm repo update

kubectl create ns postgres
helm install postgres azure-marketplace/postgresql \
    --namespace postgres \
    --set global.postgresql.auth.username=$POSTGRES_ID \
    --set global.postgresql.auth.password=$POSTGRES_PWD

# fish-api
# Note: set the password for Redis in the yaml file 

kubectl apply -f fish.yaml


# local debugging
export REDIS_HOST='redis-release-master.redis.svc.cluster.local'
export REDIS_PORT='6379'
export REDIS_PASSWD='set-me-to-something'

redis-cli -h 127.0.0.1 -p 6379 -a ''
az acr build -t chzbrgr71/fish-api:v2 -r reddog .

http://192.168.1.1:3000/fish/atlantic-chub-mackerel
http://192.168.1.1:3000/fish/red-snapper
http://192.168.1.1:3000/fish/crimson-jobfish


# Other sample apps
https://github.com/chzbrgr71/service-tracker
https://medium.com/geekculture/accelerating-your-web-application-with-redis-cache-2ccb60af0562 
https://www.digitalocean.com/community/tutorials/how-to-implement-caching-in-node-js-using-redis
https://www.fishwatch.gov/api/species

```
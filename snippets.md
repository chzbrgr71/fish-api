## POC

### Deploy AKS 



### Deploy Sample App

```bash

# Deploy Redis (Helm)
https://bitnami.com/stack/redis/helm 

export REDIS_PASSWD=''
export REDIS_SERVER='redis-release-master.redis.svc.cluster.local:6379'

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
export POSTGRES_PWD=''

helm repo add azure-marketplace https://marketplace.azurecr.io/helm/v1/repo
helm repo update

kubectl create ns postgres
helm install postgres azure-marketplace/postgresql \
    --namespace postgres \
    --set global.postgresql.auth.username=$POSTGRES_ID \
    --set global.postgresql.auth.password=$POSTGRES_PWD

PostgreSQL can be accessed via port 5432 on the following DNS names from within your cluster:

    postgres-postgresql.postgres.svc.cluster.local

To get the password for "postgres" run:

    export POSTGRES_ADMIN_PASSWORD=$(kubectl get secret --namespace postgres postgres-postgresql -o jsonpath="{.data.postgres-password}" | base64 -d)

    export POSTGRES_PASSWORD=$(kubectl get secret --namespace postgres postgres-postgresql -o jsonpath="{.data.password}" | base64 -d)

To connect to your database run the following command:

    kubectl run postgres-postgresql-client --rm --tty -i --restart='Never' --namespace postgres --image marketplace.azurecr.io/bitnami/postgresql:14.4.0-debian-11-r13 --env="PGPASSWORD=$POSTGRES_PASSWORD" \
      --command -- psql --host postgres-postgresql -U reddog -d postgres -p 5432

    kubectl port-forward --namespace postgres svc/postgres-postgresql 5432:5432 &
    PGPASSWORD="$POSTGRES_PASSWORD" psql --host 127.0.0.1 -U reddog -d postgres -p 5432

# Sample apps
https://github.com/chzbrgr71/service-tracker
https://medium.com/geekculture/accelerating-your-web-application-with-redis-cache-2ccb60af0562 
https://www.digitalocean.com/community/tutorials/how-to-implement-caching-in-node-js-using-redis
https://www.fishwatch.gov/api/species

# sample-api
curl -X POST -H "Content-Type: application/json" -d '{"brian":"Hello k8s"}' 20.127.251.11:8081/api
curl -X POST -H "Content-Type: application/json" -d '{"brian":"Hello k8s"}' localhost:3000/api

curl http://20.127.251.11:8081/api
curl http://localhost:3000/api

export REDIS_HOST='localhost'
export REDIS_HOST='redis-release-master.redis.svc.cluster.local'
export REDIS_PORT='6379'
export REDIS_PASSWORD=''

docker run -d --name redis -p 6379:6379 -e REDIS_PASSWORD=$REDIS_PASSWORD bitnami/redis:latest

redis-cli -h 127.0.0.1 -p 6379 -a ''

http://20.120.57.83:3000/fish/atlantic-chub-mackerel
http://20.120.57.83:3000/fish/red-snapper

http://localhost:3000/fish/atlantic-chub-mackerel
http://localhost:3000/fish/red-snapper

```
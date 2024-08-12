# Running Flask API in Kubernetes (minikube)

This is a small introduction to how you can use the tool [flask-api-SQLAlchemy](https://github.com/nabeelaccount/flask-api-SQLAlchemy) to create a Fask API capable of connecting to a database server.

1. Create the minikube cluster

`minikube start`

2. Create the Postgres Server secrets

```
kubectl create secret generic postgres-server-secrets --from-literal=POSTGRES_PASSWORD='postgres' --from-literal=POSTGRES_USER='postgres' --from-literal=POSTGRES_DB='postgres' --dry-run=client -o yaml > postgres-server-secrets.yaml
```

3. Create a kubernetes secret for the "DATABASE_URL".

Here is an example:

```
kubectl create secret generic flask-app-secrets --from-literal=DATABASE_URL='postgresql://postgres:postgres@postgres-server/postgres' --dry-run=client -o yaml
```

where
DATABASE_URL='postgresql://postgres-server-username:postgres-server-password@postgres-server-endpoint/initial/existing-datase'

3. Run both the flask API app and the Postgres Server

`kubectl apply -f .`

4. Connect to the application service hosted on minikube by running:

`minikube service flask-app`

Expected result:
"http://127.0.0.1:random-minikube-port"


You should now be able to send the POST request through the terminal or using the favourite tool like Postman or Insomnia.
```
curl --header "Content-Type: application/json" \
-X POST http://http://127.0.0.1:random-minikube-port/api/transaction/ \
--data '{"transactionId":"0f7e46df-c685-4df9-9e23-e75e7ac8ba7a",
"amount": "99.99","timestamp":"2009-09-28T19:03:12Z"}'
```
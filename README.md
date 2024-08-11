# Running Flask API in Kubernetes (minikube)

This is a small introduction to how you can use the tool [flask-api-SQLAlchemy](https://github.com/nabeelaccount/flask-api-SQLAlchemy) to create a Fask API capable of connecting to a database server.

1. Create the minikube cluster

`minikube start`

2. Create a kubernetes secret for the "DATABASE_URL".

Here is an example:

`kubectl create secret generic flask-app-secrets --from-literal=DATABASE_URL='postgresql://database-username:database-password@database-endpoint/initial-database-name'`

3. Create the Flask API deployment

`kubectl apply flask-api-deployment.yaml`

4. Connect to the application service hosted on minikube by running:

`minikube service flask-app-service`

Expected result:
"http://127.0.0.1:random-minikube-port"


You should now be able to send the POST request through the terminal or using the favourite tool like Postman or Insomnia.
```
curl --header "Content-Type: application/json" \
-X POST http://http://127.0.0.1:random-minikube-port/api/transaction/ \
--data '{"transactionId":"0f7e46df-c685-4df9-9e23-e75e7ac8ba7a",
"amount": "99.99","timestamp":"2009-09-28T19:03:12Z"}'
```
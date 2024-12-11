# petstore-k8s
To deploy the Swagger Petstore example to Kubernetes with the specified ingress (petstore.axway-aus.com), follow these steps:

1. Create a Kubernetes Deployment for Petstore - deployment.yaml


 
2. Create a Service for Petstore - service.yaml



3. Create an Ingress Resource - ingress.yaml
   

5. Apply the Manifests

Save the above YAMLs into files (e.g., deployment.yaml, service.yaml, ingress.yaml) and apply them to your Kubernetes cluster.

kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
kubectl apply -f ingress.yaml


# petstore-k8s
To deploy the Swagger Petstore example to Kubernetes with the specified ingress (petstore.axway-aus.com), follow these steps:

1. Create a Kubernetes Deployment for Petstore

Define a Kubernetes Deployment manifest to deploy the Petstore application.

apiVersion: apps/v1
kind: Deployment
metadata:
  name: petstore
  labels:
    app: petstore
spec:
  replicas: 1
  selector:
    matchLabels:
      app: petstore
  template:
    metadata:
      labels:
        app: petstore
    spec:
      containers:
      - name: petstore
        image: swaggerapi/petstore
        ports:
        - containerPort: 8080
        env:
        - name: SWAGGER_HOST
          value: "http://petstore.swagger.io"
        - name: SWAGGER_URL
          value: "http://petstore.swagger.io"
        - name: SWAGGER_BASE_PATH
          value: "/v2"
2. Create a Service for Petstore

Define a Service to expose the Deployment to the cluster.
apiVersion: v1
kind: Service
metadata:
  name: petstore
  labels:
    app: petstore
spec:
  ports:
  - port: 80
    targetPort: 8080
  selector:
    app: petstore
  type: ClusterIP

3. Create an Ingress Resource

Set up an Ingress resource to expose the Petstore application externally with the hostname petstore.axway-aus.com.
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: petstore-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
    nginx.ingress.kubernetes.io/ssl-redirect: "true" # Redirect HTTP to HTTPS (optional)
    nginx.ingress.kubernetes.io/backend-protocol: "HTTP"
spec:
  rules:
  - host: petstore.axway-aus.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: petstore
            port:
              number: 80
  tls:
  - hosts:
    - petstore.axway-aus.com
    secretName: petstore-tls # TLS certificate (use cert-manager or manually created secret)

4. Apply the Manifests

Save the above YAMLs into files (e.g., deployment.yaml, service.yaml, ingress.yaml) and apply them to your Kubernetes cluster.

kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
kubectl apply -f ingress.yaml


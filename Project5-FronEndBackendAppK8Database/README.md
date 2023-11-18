Certainly! In a real-world scenario, you often need a database for your backend service. Here's an example with a database in the backend using PostgreSQL as the database. We'll deploy a frontend, a backend API, and a PostgreSQL database.

1. **Frontend Deployment and Service Manifest (frontend.yaml):**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: frontend
  template:
    metadata:
      labels:
        app: frontend
    spec:
      containers:
      - name: frontend-container
        image: nginx:latest
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: frontend-service
spec:
  selector:
    app: frontend
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
```

2. **Backend Deployment and Service Manifest with PostgreSQL Database (backend.yaml):**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: backend-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: backend
  template:
    metadata:
      labels:
        app: backend
    spec:
      containers:
      - name: backend-container
        image: your-backend-image:latest
        ports:
        - containerPort: 8080
---
apiVersion: v1
kind: Service
metadata:
  name: backend-service
spec:
  selector:
    app: backend
  ports:
    - protocol: TCP
      port: 8080
      targetPort: 8080
---
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: postgresql
spec:
  replicas: 1
  selector:
    matchLabels:
      app: postgresql
  serviceName: "postgresql"
  template:
    metadata:
      labels:
        app: postgresql
    spec:
      containers:
        - name: postgresql
          image: postgres:latest
          env:
            - name: POSTGRES_DB
              value: mydb
            - name: POSTGRES_USER
              value: myuser
            - name: POSTGRES_PASSWORD
              value: mypassword
          ports:
            - containerPort: 5432
  volumeClaimTemplates:
    - metadata:
        name: postgres-data
      spec:
        accessModes: ["ReadWriteOnce"]
        resources:
          requests:
            storage: 10Gi
```

3. **Ingress Manifest (ingress.yaml):**

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-ingress
spec:
  rules:
  - host: myapp.example.com  # Replace with your domain
    http:
      paths:
      - path: /frontend
        pathType: Prefix
        backend:
          service:
            name: frontend-service
            port:
              number: 80
      - path: /backend
        pathType: Prefix
        backend:
          service:
            name: backend-service
            port:
              number: 8080
```

4. **Apply the Manifests:**

Apply these manifests using the `kubectl apply -f` command:

```bash
kubectl apply -f frontend.yaml
kubectl apply -f backend.yaml
kubectl apply -f ingress.yaml
```

In this example:

- The frontend is a simple Nginx web server running on port 80.
- The backend is a placeholder for your actual backend service, which communicates with a PostgreSQL database.
- The PostgreSQL database is deployed as a StatefulSet and has a volume claim for data persistence.
- The Ingress resource maps specific paths to the frontend and backend services.

Remember to replace `myapp.example.com` with your actual domain. This example provides a more realistic setup with a backend service that uses a database for data storage.
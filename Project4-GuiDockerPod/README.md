To run a pod that opens a GUI website within a Kubernetes cluster, you need to create a Docker image that contains a web browser and a GUI environment. Typically, this involves running a lightweight Linux distribution, installing a web browser like Chromium or Firefox, and setting up a VNC server for remote desktop access. Here's a simplified example of a Dockerfile for a pod that runs a headless web browser and provides access through VNC:

1. Create a Dockerfile (e.g., `Dockerfile-gui-website`) with the following content:

```Dockerfile
# Use a base image with a minimal Linux distribution
FROM ubuntu:20.04

# Install necessary packages
RUN apt-get update && apt-get install -y \
    xfce4 \
    tightvncserver \
    xfonts-base \
    firefox

# Set a VNC password (change 'password' to your desired password)
RUN echo 'password' | vncpasswd -f > ~/.vnc/passwd
RUN chmod 600 ~/.vnc/passwd

# Expose VNC port
EXPOSE 5901

# Start VNC server with XFCE desktop and Firefox
CMD ["vncserver", ":1", "-localhost", "-geometry", "1280x720", "-depth", "24", "-dpi", "96", "-SecurityTypes", "None", "-fg"]
```

2. Build the Docker image:

```bash
docker build -t gui-website:latest -f Dockerfile-gui-website .
```

3. Create a Kubernetes Deployment to run the pod. You can use a YAML file like the following (`gui-website-deployment.yaml`):

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: gui-website-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: gui-website
  template:
    metadata:
      labels:
        app: gui-website
    spec:
      containers:
      - name: gui-website
        image: gui-website:latest
        ports:
        - containerPort: 5901
```

4. Apply the deployment to your Kubernetes cluster:

```bash
kubectl apply -f gui-website-deployment.yaml
```

5. You can access the GUI website pod using a VNC client. The VNC server is exposed on port 5901 by default. Use your favorite VNC client to connect to the pod's IP address and port 5901, and provide the VNC password you set in the Dockerfile.

Please note that running GUI applications in Kubernetes pods is not a typical use case and may have limitations in a cloud-based cluster. Additionally, this example provides a basic setup and is not suitable for production use. For a more robust and secure solution, consider other approaches like running GUI applications outside the cluster and accessing them remotely.
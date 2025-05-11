# Scalable Web Application on Kubernetes

This repository contains the code and configuration files for a simple web application deployed on DigitalOcean Kubernetes Service (DOKS) with horizontal autoscaling.

## Prerequisites

* DigitalOcean account with `doctl` configured.
* `kubectl` installed and configured to connect to your DOKS cluster.
* Docker installed.

## Deployment Instructions

1.  **Clone this Repository (if you haven't already):**
    ```bash
    git clone https://github.com/shashank-singh-repo/scalable-web-app-kubernetes)
    cd shashank-singh-repo
    ```
  

2.  **Build and Push Docker Image:**
    Ensure you are in the directory containing your `Dockerfile` and application code (`app.py`).
    ```bash
    docker build -t my-python-app .
    docker tag my-python-app [registry.digitalocean.com/shashankcr/my-python-app](https://registry.digitalocean.com/shashankcr/my-python-app)
    docker push [registry.digitalocean.com/shashankcr/my-python-app](https://registry.digitalocean.com/shashankcr/my-python-app)
    ```
   

3.  **Apply Kubernetes Configurations:**
    Apply the Deployment, Service, and HPA configurations:
    ```bash
    kubectl apply -f deployment.yaml
    kubectl apply -f service.yaml
    kubectl apply -f hpa.yaml
    ```

4.  **Get Load Balancer IP:**
    Check the external IP address of the LoadBalancer service to access your application:
    ```bash
    kubectl get svc my-python-app
    ```
    Look for the value in the `EXTERNAL-IP` column for the `my-python-app` service. It might take a few minutes for the IP to become available.

5.  **Access the Application:**
    Open your web browser and navigate to the `EXTERNAL-IP` obtained in the previous step. You should see the "Hello world!" message (or your application's output).

6.  **Check Deployment Status:**
    Verify that the Deployment has the desired number of replicas running:
    ```bash
    kubectl get deployment my-python-app
    ```

7.  **Check Pod Status:**
    Ensure that all pods are in the `Running` state:
    ```bash
    kubectl get pods
    ```

8.  **Check Service Status:**
    Confirm that the LoadBalancer service is correctly configured:
    ```bash
    kubectl get svc my-python-app
    ```

9.  **Check HPA Status:**
    Monitor the Horizontal Pod Autoscaler to see its current state, target CPU utilization, and the number of desired replicas:
    ```bash
    kubectl get hpa my-python-app-hpa
    ```

## Code and Configuration Files

* `app.py`: The Flask application code.
* `Dockerfile`: Instructions for building the Docker image.
* `deployment.yaml`: Kubernetes Deployment definition for the Flask application.
* `service.yaml`: Kubernetes Service definition to expose the application via a LoadBalancer.
* `hpa.yaml`: Kubernetes Horizontal Pod Autoscaler definition to automatically scale the application based on CPU utilization.

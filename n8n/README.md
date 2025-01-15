# N8N on Kubernetes

This project provides the necessary configuration files to deploy [n8n](https://www.n8n.io/) (an extendable workflow automation tool) on a Kubernetes cluster using PostgreSQL as its database. Below are the steps and configurations needed for deployment.

## Prerequisites

- A running Kubernetes cluster.
- `kubectl` configured to interact with your cluster.
- Access to create resources in a specific namespace (`nocode` is used here).
- A PostgreSQL instance accessible from your cluster.

## Files Provided

The following configuration files are included in this repository:

1. **Namespace Configuration**
   - `namespace.yaml`: Defines the Kubernetes namespace where all resources will be deployed.
   
2. **ConfigMap**
   - `configmap.yaml`: Stores non-sensitive configurations such as database type.
   
3. **Secrets Management**
   - `secrets.yaml`: Contains sensitive information like database credentials stored securely in a Kubernetes Secret.

4. **Service Configuration**
   - `service.yaml`: Defines the service that exposes n8n externally using a LoadBalancer.

5. **Deployment Configuration**
   - `deployment.yaml`: Specifies how to deploy n8n with configurations for environment variables and image details.
   
6. **Ingress Resource**
   - `ingress.yaml`: Manages external access to the services in your cluster, typically HTTP.

## Deployment Steps

1. **Create Namespace**

    ```bash
    kubectl apply -f namespace.yaml
    ```

2. **Deploy ConfigMap and Secrets**

    Ensure you have filled out the `secrets.yaml` with actual values before applying it.
    
    ```bash
    kubectl apply -f configmap.yaml
    kubectl apply -f secrets.yaml
    ```

3. **Create Service**

    Deploy the service that exposes n8n.

    ```bash
    kubectl apply -f service.yaml
    ```

4. **Deploy Ingress Resource**

    Apply the ingress configuration to manage access.

    ```bash
    kubectl apply -f ingress.yaml
    ```

5. **Deploy n8n**

    Deploy the n8n application with the necessary configurations.
    
    ```bash
    kubectl apply -f deployment.yaml
    ```

## Accessing n8n

Once deployed, you can access n8n using the external IP provided by your LoadBalancer service. Use `kubectl` to find this IP:

```bash
kubectl get services n8n-service -o=jsonpath='{.status.loadBalancer.ingress[0].ip}'
```

Log into your n8n instance with your credentials, and you should be ready to create workflows.

## Notes

- Ensure that the PostgreSQL database is accessible from within your Kubernetes cluster.
- Update `secrets.yaml` with actual sensitive data before applying it.
- Modify `namespace`, `image`, or other configurations as necessary for your environment.

---

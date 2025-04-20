# 🌐 Services – Exposing Your Application

## 🧠 What is a Service?

A **Service** in Kubernetes is an abstraction that provides a stable network endpoint to expose your application. It allows communication between different components of your application (e.g., between microservices) or between external clients and your application.

### 🧠 Analogy:
Pods are like employees in a company — they come and go. A Service is like the company’s public-facing phone number. It always routes to available employees (pods) that are ready to handle requests.

### Key Features:
1. **Stable IP and DNS**: A Service provides a stable IP address and DNS name for pods, even if the pods are replaced.
2. **Load Balancing**: Services can distribute traffic across multiple pods.
3. **Pod Discovery**: Services use **selectors** to automatically discover and route traffic to matching pods.

---

## 🔑 Types of Services

1. **ClusterIP** (default):
    - Exposes the Service only within the cluster.
    - Use case: Internal communication between microservices.
    - Example: A backend service communicating with a database.

2. **NodePort**:
    - Exposes the Service on a static port on each node in the cluster.
    - Use case: Accessing the application from outside the cluster (not recommended for production).
    - Example: Testing an application locally.

3. **LoadBalancer**:
    - Creates an external load balancer (e.g., AWS ELB, GCP Load Balancer) to expose the Service to the internet.
    - Use case: Exposing a public-facing application.
    - Example: A web application accessible to users.

4. **ExternalName**:
    - Maps the Service to an external DNS name.
    - Use case: Redirecting traffic to an external service (e.g., a third-party API).
    - Example: Mapping `my-service` to `api.example.com`.

---

## 🔧 YAML Examples

### 1. **ClusterIP Service** (Default)
```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: my-app
  ports:
    - port: 80
      targetPort: 8080
  type: ClusterIP
```
- **selector**: Matches pods with the label `app: my-app`.
- **port**: The port exposed by the Service.
- **targetPort**: The port on the pod where the application is running.

### 2. **NodePort Service**
```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: my-app
  ports:
    - port: 80
      targetPort: 8080
      nodePort: 30007
  type: NodePort
```
- **nodePort**: Exposes the Service on port `30007` on each node.

### 3. **LoadBalancer Service**
```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: my-app
  ports:
    - port: 80
      targetPort: 8080
  type: LoadBalancer
```
- **type: LoadBalancer**: Creates an external load balancer to expose the Service.

### 4. **ExternalName Service**
```yaml
apiVersion: v1
kind: Service
metadata:
  name: external-service
spec:
  type: ExternalName
  externalName: api.example.com
```
- **externalName**: Maps the Service to the external DNS `api.example.com`.

---

## 🛠️ Common Commands for Services

1. **List all Services**:
   ```bash
   kubectl get services
   ```

2. **Get detailed information about a Service**:
   ```bash
   kubectl describe service <service-name>
   ```

3. **Expose a Deployment as a Service**:
   ```bash
   kubectl expose deployment <deployment-name> --type=ClusterIP --port=80 --target-port=8080
   ```

4. **Delete a Service**:
   ```bash
   kubectl delete service <service-name>
   ```

5. **Port-forward a Service to access it locally**:
   ```bash
   kubectl port-forward service/<service-name> <local-port>:<service-port>
   ```
   Example:
   ```bash
   kubectl port-forward service/my-service 8080:80
   ```

---

## 🔍 Additional Notes

- **Service Discovery**:
    - Kubernetes automatically creates a DNS entry for each Service.
    - Example: A Service named `my-service` in the `default` namespace can be accessed at `http://my-service.default.svc.cluster.local`.

- **Headless Services**:
    - If you don’t need load balancing, you can create a headless Service by setting `clusterIP: None`.
    - **Use case**: Direct communication with individual pods (e.g., for stateful applications like databases).

```yaml
apiVersion: v1
kind: Service
metadata:
  name: headless-service
spec:
  selector:
    app: my-app
  clusterIP: None
  ports:
    - port: 80
      targetPort: 8080
```

- **Service Selectors**:
    - Services use **selectors** to match pods. If no pods match the selector, the Service will not route traffic.

- **Ingress vs. Service**:
    - A **Service** exposes your application, while an **Ingress** provides advanced routing (e.g., path-based routing, SSL termination) for external traffic.

---

## 🌟 Best Practices

1. **Use ClusterIP for Internal Communication**:
    - Use `ClusterIP` Services for communication between microservices within the cluster.

2. **Avoid NodePort in Production**:
    - `NodePort` is useful for testing but not recommended for production due to limited scalability and security concerns.

3. **Use LoadBalancer for Public-Facing Applications**:
    - Use `LoadBalancer` Services to expose applications to the internet.

4. **Monitor Service Health**:
    - Use tools like Prometheus and Grafana to monitor Service performance and traffic.

5. **Leverage Labels and Selectors**:
    - Use meaningful labels to organize and manage your Services and pods effectively.

---

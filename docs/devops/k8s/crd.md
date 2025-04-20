# 🛠️ Custom Resources (CRD) & Operators

## 🧠 What are CRDs and Operators?

Kubernetes comes with built-in resource types like Pods, Services, and Deployments. **Custom Resource Definitions (CRDs)** allow you to extend Kubernetes by defining your own resource types. **Operators** are controllers that watch these custom resources and automate their management.

### 🧠 Analogy:
- Kubernetes understands built-in resources like Pods and Services. A **CRD** teaches Kubernetes to understand new resource types, like `KafkaCluster` or `RedisFailover`.
- An **Operator** is like a robot that watches these custom resources and takes actions to manage them.

---

## 🎯 Why Use CRDs and Operators?

1. **Extend Kubernetes**:
    - CRDs let you define custom resource types tailored to your application needs.

2. **Automate Complex Workflows**:
    - Operators automate the management of custom resources, such as scaling, backups, and failovers.

3. **Declarative Management**:
    - CRDs and Operators enable declarative management of complex applications, just like Kubernetes manages Pods and Deployments.

4. **Reusable Patterns**:
    - CRDs and Operators provide reusable patterns for managing stateful applications like databases, message queues, and more.

---

## 🔑 Key Concepts

1. **Custom Resource Definition (CRD)**:
    - A CRD is a YAML file that defines a new resource type in Kubernetes.
    - Example: Defining a `KafkaCluster` resource.

2. **Custom Resource (CR)**:
    - A CR is an instance of a CRD, just like a Pod is an instance of a Deployment.

3. **Operator**:
    - An Operator is a controller that watches custom resources and automates their management.

---

## 🔧 Example: Defining a CRD

Here’s an example of a CRD that defines a `MyApp` resource:

    apiVersion: apiextensions.k8s.io/v1
    kind: CustomResourceDefinition
    metadata:
      name: myapps.example.com
    spec:
      group: example.com
      names:
        kind: MyApp
        listKind: MyAppList
        plural: myapps
        singular: myapp
      scope: Namespaced
      versions:
        - name: v1
          served: true
          storage: true
          schema:
            openAPIV3Schema:
              type: object
              properties:
                spec:
                  type: object
                  properties:
                    replicas:
                      type: integer
                    image:
                      type: string

### Explanation:
- **group**: The API group for the resource (e.g., `example.com`).
- **names**: Defines the resource name (`myapps`) and kind (`MyApp`).
- **scope**: Specifies whether the resource is `Namespaced` or `Cluster` scoped.
- **versions**: Lists the supported API versions.
- **schema**: Defines the structure of the resource using OpenAPI v3.

---

## 🔧 Example: Creating a Custom Resource (CR)

Here’s an example of a `MyApp` custom resource:

    apiVersion: example.com/v1
    kind: MyApp
    metadata:
      name: my-app-instance
    spec:
      replicas: 3
      image: nginx:latest

### Explanation:
- **apiVersion**: Matches the group and version defined in the CRD.
- **kind**: Matches the kind defined in the CRD (`MyApp`).
- **spec**: Contains the custom configuration for the resource.

---

## 🔧 Example Commands

1. **Apply a CRD**:
    `kubectl apply -f myapp-crd.yaml`

2. **Create a Custom Resource**:
    `kubectl apply -f myapp-cr.yaml`

3. **List Custom Resources**:
    `kubectl get myapps`

4. **Describe a Custom Resource**:
    `kubectl describe myapp my-app-instance`

5. **Delete a Custom Resource**:
    `kubectl delete myapp my-app-instance`

6. **Delete a CRD**:
    `kubectl delete crd myapps.example.com`

---

## 🌟 Best Practices

1. **Use OpenAPI Validation**:
    - Define schemas in your CRD to validate custom resources.

2. **Namespace Your CRDs**:
    - Use a unique API group (e.g., `example.com`) to avoid conflicts with other CRDs.

3. **Leverage Operators**:
    - Use Operators to automate the management of custom resources.

4. **Monitor Custom Resources**:
    - Use tools like Prometheus and Grafana to monitor the health and performance of custom resources.

5. **Version Your CRDs**:
    - Support multiple versions of your CRD to ensure backward compatibility.

---

## 🔍 Additional Notes

- **Popular Operators**:
    - **cert-manager**: Manages TLS certificates using CRDs like `Certificate`.
    - **Prometheus Operator**: Manages Prometheus instances using CRDs like `Prometheus` and `Alertmanager`.

- **CRD Lifecycle**:
    - CRDs are cluster-scoped resources. Deleting a CRD will delete all associated custom resources.

- **Debugging**:
    - Use `kubectl get events` to debug issues with CRDs or custom resources.

---

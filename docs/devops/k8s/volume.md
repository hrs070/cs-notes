# 💾 Volumes, PV, PVC – Persistent Storage in Kubernetes

## 🧠 What are Volumes, PV, and PVC?

Kubernetes provides a way to manage storage for pods using **Volumes**, **PersistentVolumes (PV)**, and **PersistentVolumeClaims (PVC)**. These abstractions allow pods to use storage that persists beyond the lifecycle of the pod.

### 🧠 Analogy:
- **Pods** are like laptops — they can crash or be replaced.
- A **Volume** is like an external hard drive you plug into the laptop. Even if the laptop dies, the drive can be re-used.
- **PersistentVolume (PV)**: A physical disk in the cluster (e.g., cloud storage, NFS, or local disk).
- **PersistentVolumeClaim (PVC)**: A request for storage made by a pod.

### Simplified Analogy:
- **PV** = A shared drive available in the office.
- **PVC** = Someone asking, “I need 10 GB of space for my work.”
- **Volume** = How the pod plugs into that shared drive.

---

## 🔑 Key Concepts

1. **Volume**:
    - A directory accessible to containers in a pod.
    - Types:
        - **emptyDir**: Temporary storage that exists as long as the pod is running.
        - **hostPath**: Mounts a directory from the host node.
        - **persistentVolumeClaim**: Connects to a PersistentVolume.

2. **PersistentVolume (PV)**:
    - A cluster-wide storage resource.
    - Provisioned by an admin or dynamically created by a StorageClass.

3. **PersistentVolumeClaim (PVC)**:
    - A request for storage by a pod.
    - Binds to a PV that satisfies the request.

---

## 🔧 Example: PersistentVolumeClaim (PVC)

Here’s an example of a PVC requesting 1Gi of storage:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```

### Explanation:
- **accessModes**:
  - `ReadWriteOnce`: The volume can be mounted as read-write by a single node.
  - `ReadOnlyMany`: The volume can be mounted as read-only by many nodes.
  - `ReadWriteMany`: The volume can be mounted as read-write by many nodes.
- **resources.requests.storage**: The amount of storage requested (e.g., 1Gi).

---

## 🔧 Example: PersistentVolume (PV)

Here’s an example of a PV backed by NFS:

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: my-pv
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  nfs:
    path: /path/to/nfs
    server: nfs-server.example.com
```

### Explanation:
- **capacity.storage**: The total storage capacity of the PV.
- **accessModes**: Defines how the volume can be accessed.
- **nfs**: Specifies the NFS server and path.

---

## 🔧 Example: Using PVC in a Pod

Here’s how to use a PVC in a pod:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
    - name: my-container
      image: nginx
      volumeMounts:
        - mountPath: /usr/share/nginx/html
          name: my-volume
  volumes:
    - name: my-volume
      persistentVolumeClaim:
        claimName: my-pvc
```

### Explanation:
- **volumeMounts**: Mounts the volume inside the container at `/usr/share/nginx/html`.
- **volumes.persistentVolumeClaim.claimName**: Refers to the PVC `my-pvc`.

---

## 🛠️ Common Commands for Volumes, PV, and PVC

1. **List all PersistentVolumes (PV)**: `kubectl get pv`

2. **List all PersistentVolumeClaims (PVC)**: `kubectl get pvc`

3. **Describe a PersistentVolume (PV)**: `kubectl describe pv <pv-name>`

4. **Describe a PersistentVolumeClaim (PVC)**: `kubectl describe pvc <pvc-name>`

5. **Delete a PersistentVolumeClaim (PVC)**: `kubectl delete pvc <pvc-name>`

6. **Check which PV is bound to a PVC**: `kubectl get pvc <pvc-name> -o wide`

---

## 🌟 Best Practices

1. **Use Dynamic Provisioning**:
    - Use a **StorageClass** to dynamically provision PVs instead of manually creating them.

2. **Set Resource Requests and Limits**:
    - Define appropriate storage requests in your PVC to avoid over-provisioning.

3. **Monitor Storage Usage**:
    - Use tools like Prometheus and Grafana to monitor storage usage and performance.

4. **Use Access Modes Wisely**:
    - Choose the correct access mode (`ReadWriteOnce`, `ReadOnlyMany`, `ReadWriteMany`) based on your application’s requirements.

5. **Clean Up Unused PVCs**:
    - Delete unused PVCs to free up storage resources.

---

## 🔍 Additional Notes

- **Dynamic Provisioning**:
    - Use a `StorageClass` to automatically provision PVs when a PVC is created.
    - Example:
```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: standard
provisioner: kubernetes.io/aws-ebs
parameters:
  type: gp2
```

- **Reclaim Policy**:
    - PVs have a `reclaimPolicy` that determines what happens when a PVC is deleted:
        - `Retain`: Keeps the PV for manual cleanup.
        - `Delete`: Deletes the PV automatically.
        - `Recycle`: Clears the PV for reuse (deprecated).

---

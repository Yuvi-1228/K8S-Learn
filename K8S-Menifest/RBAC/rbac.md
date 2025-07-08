# RBAC

# Service Level RBAC [ Namespace level / User Base Role ]
# Basic command 
- To check the username 
```
kubectl auth whoami
```
- To check the permission 
```
kubectl auth can-i get pods -n <namespace-name>
```
- To check auth of specific user 
```
kubectl auth can-i get pods -n <namespace-name> --as=<serviceaccount-name: apache-user>
```

# 📚 Common Kubernetes API Groups in RBAC

Kubernetes RBAC mein **apiGroups** specify karte hain ki kaunse API group ke resources pe rules apply honge.

| `apiGroups` Value           | Description / Included Resources |
|----------------------------|---------------------------------|
| `""` (empty string)         | Core API group: `pods`, `services`, `configmaps`, `secrets`, `namespaces`, etc. |
| `apps`                     | Workloads like `deployments`, `statefulsets`, `daemonsets`, `replicasets` |
| `batch`                    | Batch jobs and scheduled jobs: `jobs`, `cronjobs` |
| `extensions`               | (Deprecated) Older APIs for `deployments`, `daemonsets`, etc. |
| `rbac.authorization.k8s.io` | RBAC related resources: `roles`, `rolebindings`, `clusterroles`, `clusterrolebindings` |
| `apiextensions.k8s.io`      | CustomResourceDefinitions (CRDs) management |
| `policy`                   | Pod security policies (deprecated in newer Kubernetes versions) |
| `storage.k8s.io`           | Storage related resources like `storageclasses`, `volumeattachments` |
| `networking.k8s.io`        | Networking resources like `networkpolicies`, `ingresses` |
| `coordination.k8s.io`      | Lease resources for leader election and coordination |
| `*`                        | All API groups (⚠️ use with caution — can grant very broad permissions) |

---

## 🔍 Notes

- Different resources belong to different API groups, so `apiGroups` must match the resource group.
- Always prefer specifying exact `apiGroups` instead of `"*"` for better security.

---

## ✅ Example: Role Rule with Multiple API Groups

```yaml
rules:
- apiGroups: ["", "apps", "batch"]
  resources: ["pods", "deployments", "jobs"]
  verbs: ["get", "list", "watch"]
```

# 🔐 Common Verbs in Kubernetes RBAC

In Kubernetes RBAC, **verbs** define the actions a user, group, or service account can perform on a resource.

Below is a list of commonly used verbs and their meanings:

| Verb               | Description |
|--------------------|-------------|
| `get`              | Single resource ka detail fetch karna |
| `list`             | Multiple resources ki list dekhna |
| `watch`            | Resource changes ka real-time stream dekhna |
| `create`           | Naya resource banana |
| `update`           | Existing resource ko modify karna |
| `patch`            | Partial update (JSON Patch ya Merge Patch) |
| `delete`           | Resource ko delete karna |
| `deletecollection` | Ek saath multiple resources ko delete karna |
| `impersonate`      | Doosre user ya group ke identity mein act karna (mostly for admin use) |
| `escalate`         | Higher privileges assign karna (mostly used with roles) |
| `bind`             | RBAC role bindings ko apply karne ka right |
| `use`              | Resources jise reference kiya ja raha hai (jaise PSP, PodSecurityPolicy) |

---

## 📌 Notes

- `["get", "list", "watch"]` is usually used for **read-only access**.
- `["create", "update", "patch", "delete"]` are used for **write permissions**.
- `["*"]` means **all actions allowed** — use this with caution.

---

## ✅ Example Usage in a Role

```yaml
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "list", "watch"]
```

# 📚 Common Kubernetes RBAC Resources

Kubernetes RBAC mein **resources** se matlab hai wo objects jinpe aap permissions apply karte hain.

| Resource Name          | Description                                  |
|-----------------------|----------------------------------------------|
| `pods`                | Containers ke groups jo cluster mein run hote hain |
| `services`            | Network services jo pods ko expose karte hain |
| `deployments`         | Applications ke declarative updates manage karte hain |
| `statefulsets`        | Stateful applications ke pods manage karte hain |
| `daemonsets`          | Har node par ek pod run karne ke liye use hote hain |
| `replicasets`         | Pods ke multiple copies maintain karte hain |
| `jobs`                | One-time batch jobs run karne ke liye |
| `cronjobs`            | Scheduled jobs ko run karte hain |
| `configmaps`          | Configuration data ko key-value pairs mein store karte hain |
| `secrets`             | Sensitive data jaise passwords aur tokens store karte hain |
| `namespaces`          | Cluster ke logical partitions (isolations) provide karte hain |
| `persistentvolumeclaims` | Storage ko request karne ke liye pods ke dwara use hote hain |
| `ingresses`           | HTTP/HTTPS routing rules manage karte hain |
| `roles`               | Namespace-specific permissions define karte hain |
| `rolebindings`        | Roles ko users/groups ke sath bind karte hain |
| `clusterroles`        | Cluster-wide permissions define karte hain |
| `clusterrolebindings` | ClusterRoles ko users/groups ke sath bind karte hain |

---

## 🔍 Notes

- Har resource ka `apiGroup` alag ho sakta hai (jaise `""`, `apps`, `rbac.authorization.k8s.io`).
- Custom resources (CRDs) ka naam alag hota hai, wo aapke cluster mein installed extensions pe depend karta hai.

---

## ✅ Example: Role mein Resources ka Use

```yaml
rules:
- apiGroups: ["apps"]
  resources: ["deployments", "statefulsets"]
  verbs: ["get", "list", "watch", "create", "update", "delete"]
```


# Cluster base role access [ monitoring, logging]


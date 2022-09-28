# k8s-job-buildkite-plugin
Run a Command Step in a Kubernetes Job using a Pod Spec

## RBAC

You will need to ensure your agent deployment has sufficient privileges to create resources on your kubernetes cluster. An example RBAC policy:

```
kind: ServiceAccount
apiVersion: v1
metadata:
  name: agent-k8s-job
---
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: agent-k8s-job
rules:
  - apiGroups: [""]
    resources: ["pods", "pods/log"]
    verbs: ["get", "watch", "list"]
  - apiGroups: ["batch"]
    resources: ["jobs"]
    verbs: ["get", "list", "watch", "create", "update", "patch", "delete"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: agent-k8s-job
subjects:
  - kind: ServiceAccount
    name: agent-k8s-job
roleRef:
  kind: Role
  name: agent-k8s-job
  apiGroup: rbac.authorization.k8s.io
```

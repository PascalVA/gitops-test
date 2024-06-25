# GitOps Test

A small repository to test gitops with ArgoCD

```yaml
---
project: customers
source:
  repoURL: 'https://github.com/PascalVA/gitops-test.git'
  path: customers
  targetRevision: HEAD
destination:
  server: 'https://kubernetes.default.svc'
  namespace: argocd
```

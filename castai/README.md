# install Cast.ai

> cast.ai

using helm chart <https://github.com/castai/helm-charts/tree/main/charts/castai-agent>

## Pre-install work

manually create namespace and secret with `API_KEY`

key_here=$(echo -n "API_KEY" | base64)

```yaml
---
apiVersion: v1
kind: Namespace
metadata:
  name: castai-agent
---
apiVersion: v1
data:
    API_KEY: <key_here>
kind: Secret
metadata:
    name: castai-agent
    namespace: castai-agent
```

## Default values

- Provider: EKS

## Kustomize patch to your own values

```yaml
patches:
  - target:
      group: argoproj.io
      kind: Application
      name: castai
    patch: |
      - op: add
        path: /spec/sources/2
        value: |
          - ref: values
            repoURL: https://github.com/${YOUR-GIT-REPO}.git
            targetRevision: HEAD
      - op: add
        path: /spec/sources/0/helm/valueFiles/1
        value: $values/${PATH-TO-VALUES}/helm-values.yaml
```

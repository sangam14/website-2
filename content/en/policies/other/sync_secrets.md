---
title: "Sync Secrets"
category: Sample
version: 
policyType: "generate"
description: >
    Secrets like registry credentials often need to exist in multiple namespaces so pods there have access. This sample policy will copy a secret called "regcred" which exists in the default namespace to new namespaces when they are created. It will also push updates to the copied secrets should the source secret be changed.
---

## Policy Definition
<a href="https://github.com/kyverno/policies/raw/main//other/sync_secrets.yaml" target="-blank">/other/sync_secrets.yaml</a>

```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: sync-secrets
  annotations:
    policies.kyverno.io/title: Sync Secrets 
    policies.kyverno.io/category: Sample
    policies.kyverno.io/description: >-
      Secrets like registry credentials often need to exist in multiple
      namespaces so pods there have access. This sample policy will copy a
      secret called "regcred" which exists in the default namespace to
      new namespaces when they are created. It will also push updates to
      the copied secrets should the source secret be changed.
spec:
  background: false
  rules:
  - name: sync-image-pull-secret
    match:
      resources:
        kinds:
        - Namespace
    generate:
      kind: Secret
      name: regcred
      namespace: "{{request.object.metadata.name}}"
      synchronize: true
      clone:
        namespace: default
        name: regcred
```

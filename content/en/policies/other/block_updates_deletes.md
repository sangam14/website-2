---
title: "Block Updates and Deletes"
category: Sample
version: 
policyType: "validate"
description: >
    Kubernetes RBAC allows for controls on kinds of resources or those with specific names. This policy restricts updates and deletes to any resource that contains the label `protected="true"` unless by a cluster-admin and thus functions as a more granular option to Kubernetes RBAC. 
---

## Policy Definition
<a href="https://github.com/kyverno/policies/raw/main//other/block_updates_deletes.yaml" target="-blank">/other/block_updates_deletes.yaml</a>

```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: block-updates-deletes
  annotations:
    policies.kyverno.io/title: Block Updates and Deletes
    policies.kyverno.io/category: Sample
    policies.kyverno.io/description: >-
      Kubernetes RBAC allows for controls on kinds of resources or those
      with specific names. This policy restricts updates and deletes to any
      resource that contains the label `protected="true"` unless by
      a cluster-admin and thus functions as a more granular option to
      Kubernetes RBAC. 
spec:
  validationFailureAction: enforce
  background: false
  rules:
  - name: block-updates-deletes
    match:
      resources:
        selector:
          matchLabels:
            protected: "true"   
    exclude:
      clusterRoles:
      - cluster-admin
    validate:
      message: "This resource is protected and changes are not allowed. Please seek a cluster-admin."
      deny:
        conditions:
          - key: "{{request.operation}}"
            operator: In
            value: 
            - DELETE
            - UPDATE
```

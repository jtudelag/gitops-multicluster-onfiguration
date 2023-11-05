# GitOps configuration for a Multicluster

```bash
├── base
│   ├── api
│   ├── dns
│   ├── identity-providers
│   ├── ingress
│   ├── kustomization.yaml
│   ├── monitoring
│   ├── olm
│   └── kustomization.yaml
└── overlay
    ├── cluster1
    │   ├── kustomization.yaml
    │   └── replicas.patch.yaml
    ├── cluster2
    │   └── kustomization.yaml
    └── local-cluster
        └── kustomization.yaml
```

Sources of inspiration:

* https://github.com/gnunn-gitops/cluster-config/tree/main

---
source_url: https://fluxcd.io/flux/components/kustomize/
fetched: 2026-05-01
---

  1. [Docs](<https://fluxcd.io/flux/>)
  2. [Toolkit Components](<https://fluxcd.io/flux/components/>)
  3. [Kustomize Controller](<https://fluxcd.io/flux/components/kustomize/>)



# Kustomize Controller

The GitOps Toolkit Kustomize Controller documentation.

The kustomize-controller is a Kubernetes operator, specialized in running continuous delivery pipelines for infrastructure and workloads defined with Kubernetes manifests and assembled with Kustomize.

![Kustomize Controller Diagram](/img/kustomize-controller.png)

Features:

  * Reconciles the cluster state from multiple sources (provided by source-controller)
  * Generates manifests with Kustomize (from plain Kubernetes YAMLs or Kustomize overlays)
  * Decrypts Kubernetes secrets with Mozilla SOPS and KMS
  * Validates manifests against Kubernetes API
  * Impersonates service accounts (multi-tenancy RBAC)
  * Health assessment of the deployed workloads
  * Runs pipelines in a specific order (depends-on relationship)
  * Prunes objects removed from source (garbage collection)
  * Reports cluster state changes (alerting provided by notification-controller)



Links:

  * Source code [fluxcd/kustomize-controller](<https://github.com/fluxcd/kustomize-controller>)
  * Specification [docs](<https://github.com/fluxcd/kustomize-controller/tree/main/docs/spec>)



* * *

##### [Controller Options](</flux/components/kustomize/options/>)

Controller command flags and defaults.

##### [Kustomization](</flux/components/kustomize/kustomizations/>)

The GitOps Toolkit Custom Resource Definitions documentation.

##### [API Reference](</flux/components/kustomize/api/>)

Last modified 2023-05-24: [Update docs to Flux RC.4 (08eaeedd)](<https://github.com/fluxcd/website/commit/08eaeedde22f280cfe74697f141c8d469554ef73>)

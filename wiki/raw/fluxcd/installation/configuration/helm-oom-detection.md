---
source_url: https://fluxcd.io/flux/installation/configuration/helm-oom-detection/
fetched: 2026-05-01
---

  1. [Docs](<https://fluxcd.io/flux/>)
  2. [Installation](<https://fluxcd.io/flux/installation/>)
  3. [Configuration](<https://fluxcd.io/flux/installation/configuration/>)
  4. [Helm OOM detection](<https://fluxcd.io/flux/installation/configuration/helm-oom-detection/>)



# Flux near OOM detection for Helm

How to enable Helm near OOM detection

When memory usage of the helm-controller exceeds the configured limit, the controller will forcefully be killed by Kubernetes' OOM killer. This may result in a Helm release being left in a `pending-*` state which causes the HelmRelease to get stuck in an `another operation (install/upgrade/rollback) is in progress` error loop.

To prevent this from happening, the controller offers an OOM watcher which can be enabled with `--feature-gates=OOMWatch=true`. When enabled, the memory usage of the controller will be monitored, and a graceful shutdown will be triggered when it reaches a certain threshold (default 95% utilization).

When gracefully shutting down, running Helm actions may mark the release as `failed`. Because of this, enabling this feature is best combined with thoughtful [remediation strategies](</flux/components/helm/helmreleases/#configuring-failure-handling>).

To enable near OOM detection [during bootstrap](</flux/installation/configuration/bootstrap-customization/>) add the following patches to the flux-system `kustomization.yaml`:
```
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
resources:
  - gotk-components.yaml
  - gotk-sync.yaml
patches:
  - patch: |
      # Enable OOM watch feature
      - op: add
        path: /spec/template/spec/containers/0/args/-
        value: --feature-gates=OOMWatch=true
      # Threshold at which to trigger a graceful shutdown (optional, default 95%)
      - op: add
        path: /spec/template/spec/containers/0/args/-
        value: --oom-watch-memory-threshold=95
      # Interval at which to check memory usage (optional, default 500ms)
      - op: add
        path: /spec/template/spec/containers/0/args/-
        value: --oom-watch-interval=500ms      
    target:
      kind: Deployment
      name: helm-controller
```

Last modified 2025-09-09: [Fix typo: boostrap/bootstrap everywhere (b30bccc4)](<https://github.com/fluxcd/website/commit/b30bccc4d97ee6e2b6686f5ef5aebcf185e91b7e>)

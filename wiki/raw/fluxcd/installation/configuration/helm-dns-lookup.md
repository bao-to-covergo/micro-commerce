---
source_url: https://fluxcd.io/flux/installation/configuration/helm-dns-lookup/
fetched: 2026-05-01
---

  1. [Docs](<https://fluxcd.io/flux/>)
  2. [Installation](<https://fluxcd.io/flux/installation/>)
  3. [Configuration](<https://fluxcd.io/flux/installation/configuration/>)
  4. [Helm DNS lookups](<https://fluxcd.io/flux/installation/configuration/helm-dns-lookup/>)



# Flux DNS lookups for Helm Releases

How to allow Helm DNS lookups

By default, the helm-controller will not perform DNS lookups when rendering Helm templates in clusters because of potential [security implications](<https://github.com/helm/helm/security/advisories/GHSA-pwcw-6f5g-gxf8>).

To enable DNS lookups [during bootstrap](</flux/installation/configuration/bootstrap-customization/>) add the following patches to the flux-system `kustomization.yaml`:
```
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
resources:
  - gotk-components.yaml
  - gotk-sync.yaml
patches:
  - patch: |
      # Allow Helm DNS lookups
      - op: add
        path: /spec/template/spec/containers/0/args/-
        value: --feature-gates=AllowDNSLookups=true      
    target:
      kind: Deployment
      name: helm-controller
```

Last modified 2025-09-09: [Fix typo: boostrap/bootstrap everywhere (b30bccc4)](<https://github.com/fluxcd/website/commit/b30bccc4d97ee6e2b6686f5ef5aebcf185e91b7e>)

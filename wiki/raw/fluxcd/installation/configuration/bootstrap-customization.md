---
source_url: https://fluxcd.io/flux/installation/configuration/bootstrap-customization/
fetched: 2026-05-01
---

  1. [Docs](<https://fluxcd.io/flux/>)
  2. [Installation](<https://fluxcd.io/flux/installation/>)
  3. [Configuration](<https://fluxcd.io/flux/installation/configuration/>)
  4. [Bootstrap customization](<https://fluxcd.io/flux/installation/configuration/bootstrap-customization/>)



# Flux bootstrap customization

How to customize the Flux controllers during bootstrap

To customize the Flux controllers during [bootstrap](</flux/installation/#bootstrap-with-flux-cli>), first you'll need to create a Git repository and clone it locally.

Create the file structure required by bootstrap with:
```
mkdir -p clusters/my-cluster/flux-system
touch clusters/my-cluster/flux-system/gotk-components.yaml \
    clusters/my-cluster/flux-system/gotk-sync.yaml \
    clusters/my-cluster/flux-system/kustomization.yaml
```

The Flux controller deployments, container command arguments, node affinity, etc can be customized using [Kustomize strategic merge patches and JSON patches](<https://github.com/kubernetes-sigs/kustomize/blob/master/examples/patchMultipleObjects.md>).

You can make changes to all controllers using a single patch or target a specific controller:
```
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
resources: # manifests generated during bootstrap
  - gotk-components.yaml
  - gotk-sync.yaml
patches:
  # target all controllers
  - patch: | 
      # strategic merge or JSON patch
    target:
      kind: Deployment
      labelSelector: "app.kubernetes.io/part-of=flux"
  # target controllers by name
  - patch: |
      # strategic merge or JSON patch      
    target:
      kind: Deployment
      name: "(kustomize-controller|helm-controller)"
  # add a command argument to a single controller
  - patch: |
      - op: add
        path: /spec/template/spec/containers/0/args/-
        value: --concurrent=5      
    target:
      kind: Deployment
      name: "source-controller"
```

Push the changes to main branch:
```
git add -A && git commit -m "init flux" && git push
```

And run the bootstrap for `clusters/my-cluster`:
```
flux bootstrap git \
  --url=ssh://git@<host>/<org>/<repository> \
  --branch=main \
  --path=clusters/my-cluster
```

To make further amendments, pull the changes locally, edit the kustomization.yaml file, push the changes upstream and rerun bootstrap.

Last modified 2025-09-09: [Fix typo: boostrap/bootstrap everywhere (b30bccc4)](<https://github.com/fluxcd/website/commit/b30bccc4d97ee6e2b6686f5ef5aebcf185e91b7e>)

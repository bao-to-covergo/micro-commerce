---
source_url: https://fluxcd.io/flux/installation/uninstall/
fetched: 2026-05-01
---

  1. [Docs](<https://fluxcd.io/flux/>)
  2. [Installation](<https://fluxcd.io/flux/installation/>)
  3. [Uninstall](<https://fluxcd.io/flux/installation/uninstall/>)



# Flux uninstall

How to uninstall the Flux controllers

No matter which [installation](</flux/installation/>) method you've used to deploy the Flux controllers on a cluster, you can uninstall them using the Flux CLI.

The uninstallation procedure removes only the Flux components, without touching reconciled namespaces, tenants, cluster addons, workloads, Helm releases, etc.

Please note that uninstalling Flux by any other means e.g. deleting the deployments and namespace with **kubectl is not supported**.

## Uninstall with Flux CLI

You can uninstall the Flux controllers running on a cluster with:
```
flux uninstall
```

The above command performs the following operations:

  * deletes Flux components (deployments and services)
  * deletes Flux network policies
  * deletes Flux RBAC (service accounts, cluster roles and cluster role bindings)
  * removes the Kubernetes finalizers from Flux custom resources
  * deletes Flux custom resource definitions and custom resources
  * deletes the namespace where Flux was installed



If you've installed Flux in a namespace that you wish to preserve, you can skip the namespace deletion with:
```
flux uninstall --namespace=flux-system --keep-namespace
```

#### Reinstall

Note that the `uninstall` command will not remove any Kubernetes objects or Helm releases that were reconciled on the cluster by Flux. It is safe to uninstall Flux and rerun the bootstrap, any existing workloads will not be affected.

Last modified 2023-08-17: [Add install, upgrade, uninstall docs (d2bf0881)](<https://github.com/fluxcd/website/commit/d2bf08811c2e2d6512de73ad10daa7fada5f5ad1>)

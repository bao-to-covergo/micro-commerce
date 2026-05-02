---
source_url: https://fluxcd.io/flux/installation/configuration/optional-components/
fetched: 2026-05-01
---

  1. [Docs](<https://fluxcd.io/flux/>)
  2. [Installation](<https://fluxcd.io/flux/installation/>)
  3. [Configuration](<https://fluxcd.io/flux/installation/configuration/>)
  4. [Optional components](<https://fluxcd.io/flux/installation/configuration/optional-components/>)



# Flux optional components

How to install Flux optional components

The `flux bootstrap` command deploys a series of Kubernetes controllers along with their CRDs, RBAC and network policies in the `flux-system` namespace.

## Default components

The default components are specified with the `--components` flag.
```
flux bootstrap git \
  --components source-controller,kustomize-controller,helm-controller,notification-controller
```

The minimum required components for bootstrapping are `source-controller` and `kustomize-controller`.

When not specifying the `--components` flag, both `flux bootstrap` and `flux install` will deploy the default components.

## Extra components

To enable the Flux [image automation feature](</flux/guides/image-update/>), the extra components can be specified with the `--components-extra` flag.
```
flux bootstrap git \
  --components-extra image-reflector-controller,image-automation-controller
```

To enable the Flux source composition and decomposition features, the [source-watcher](<https://github.com/fluxcd/source-watcher>) component can be added to the extra components.
```
flux bootstrap git \
  --components-extra source-watcher
```

By default, both `flux bootstrap` and `flux install` commands do not include any extra components.

## Network policies

Flux relies on Kubernetes network policies to ensure that only Flux components have direct access to the source artifacts kept in the `source-controller`.

The default network policies block all ingress access to the `flux-system` namespace, except for `notification-controller` webhook receiver.

While not recommend, you can deploy the Flux components without network policies using the `--network-policy` flag.
```
flux bootstrap git \
  --network-policy false
```

## Cluster domain

The Flux components are communicating over HTTP with `source-controller` and `notification-controller`. To reduce the DNS queries performed by each component, the cluster internal domain name is used to compose the FQDN of each service e.g. `http://source-controller.flux-system.svc.cluster.local./`.

The cluster domain name can be set using the `--cluster-domain` flag.
```
flux bootstrap git \
  --cluster-domain cluster.internal
```

When not specified, the cluster domain defaults to `cluster.local`.

Last modified 2025-10-01: [roadmap: Mark the 2.7 milestone as completed (5a605d40)](<https://github.com/fluxcd/website/commit/5a605d40bf532a920bd133fa964038c86864c56d>)

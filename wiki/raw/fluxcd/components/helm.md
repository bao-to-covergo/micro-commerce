---
source_url: https://fluxcd.io/flux/components/helm/
fetched: 2026-05-01
---

  1. [Docs](<https://fluxcd.io/flux/>)
  2. [Toolkit Components](<https://fluxcd.io/flux/components/>)
  3. [Helm Controller](<https://fluxcd.io/flux/components/helm/>)



# Helm Controller

The GitOps Toolkit Helm Controller documentation.

The Helm Controller is a Kubernetes operator, allowing one to declaratively manage Helm chart releases with Kubernetes manifests.

![Helm Controller Diagram](/img/helm-controller.png)

The desired state of a Helm release is described through a Kubernetes Custom Resource named `HelmRelease`. Based on the creation, mutation or removal of a `HelmRelease` resource in the cluster, Helm actions are performed by the controller.

Features:

  * Watches for `HelmRelease` objects and generates `HelmChart` objects
  * Supports `HelmChart` artifacts produced from `HelmRepository` and `GitRepository` sources
  * Fetches artifacts produced by [source-controller](</flux/components/source/>) from `HelmChart` objects
  * Watches `HelmChart` objects for revision changes (including semver ranges for charts from `HelmRepository` sources)
  * Performs automated Helm actions, including Helm tests, rollbacks and uninstalls
  * Offers extensive configuration options for automated remediation (rollback, uninstall, retry) on failed Helm install, upgrade or test actions
  * Runs Helm install/upgrade in a specific order, taking into account the depends-on relationship defined in a set of `HelmRelease` objects
  * Prunes Helm releases removed from cluster (garbage collection)
  * Reports Helm releases statuses (alerting provided by [notification-controller](</flux/components/notification/>))
  * Built-in Kustomize compatible Helm post renderer, providing support for strategic merge, JSON 6902 and images patches



Links:

  * Source code [fluxcd/helm-controller](<https://github.com/fluxcd/helm-controller>)
  * Specification [docs](<https://github.com/fluxcd/helm-controller/tree/main/docs/spec>)



* * *

##### [Controller Options](</flux/components/helm/options/>)

Controller command flags and defaults.

##### [Helm Releases](</flux/components/helm/helmreleases/>)

The GitOps Toolkit Custom Resource Definitions documentation.

##### [API Reference](</flux/components/helm/api/>)

Last modified 2022-08-30: [Move all Flux docs under /flux (a7cd1b87)](<https://github.com/fluxcd/website/commit/a7cd1b872d3ba78e6d129f3278bf41d8fa41b00d>)

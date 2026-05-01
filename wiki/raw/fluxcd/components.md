---
source_url: https://fluxcd.io/flux/components/
fetched: 2026-05-01
---

  1. [Docs](<https://fluxcd.io/flux/>)
  2. [Toolkit Components](<https://fluxcd.io/flux/components/>)



# GitOps Toolkit components

Documentation of the individual GitOps Toolkit components and their APIs.

Flux is constructed with the GitOps Toolkit components, which is a set of

  * specialized tools and Flux Controllers
  * composable APIs
  * reusable Go packages for GitOps under the [fluxcd GitHub organisation](<https://github.com/fluxcd>)



for building Continuous Delivery on top of Kubernetes.

![GitOps Toolkit overview](/img/diagrams/gitops-toolkit.png)

The APIs comprise Kubernetes custom resources, which can be created and updated by a cluster user, or by other automation tooling.

You can use the toolkit to extend Flux, and to build your own systems for continuous delivery. The [source-watcher guide](</flux/gitops-toolkit/source-watcher/>) is a good place to start.

A reference for each component and API type is linked below.

  * [Source Controllers](</flux/components/source/>)
    * [GitRepository CRD](</flux/components/source/gitrepositories/>)
    * [OCIRepository CRD](</flux/components/source/ocirepositories/>)
    * [HelmRepository CRD](</flux/components/source/helmrepositories/>)
    * [HelmChart CRD](</flux/components/source/helmcharts/>)
    * [Bucket CRD](</flux/components/source/buckets/>)
    * [ExternalArtifact CRD](</flux/components/source/externalartifacts/>)
    * [ArtifactGenerator CRD](</flux/components/source/artifactgenerators/>)
  * [Kustomize Controller](</flux/components/kustomize/>)
    * [Kustomization CRD](</flux/components/kustomize/kustomizations/>)
  * [Helm Controller](</flux/components/helm/>)
    * [HelmRelease CRD](</flux/components/helm/helmreleases/>)
  * [Notification Controller](</flux/components/notification/>)
    * [Provider CRD](</flux/components/notification/providers/>)
    * [Alert CRD](</flux/components/notification/alerts/>)
    * [Receiver CRD](</flux/components/notification/receivers/>)
  * [Image automation controllers](</flux/components/image/>)
    * [ImageRepository CRD](</flux/components/image/imagerepositories/>)
    * [ImagePolicy CRD](</flux/components/image/imagepolicies/>)
    * [ImageUpdateAutomation CRD](</flux/components/image/imageupdateautomations/>)



* * *

##### [Source Controllers](</flux/components/source/>)

The GitOps Toolkit Source Controllers documentation.

##### [Kustomize Controller](</flux/components/kustomize/>)

The GitOps Toolkit Kustomize Controller documentation.

##### [Helm Controller](</flux/components/helm/>)

The GitOps Toolkit Helm Controller documentation.

##### [Notification Controller](</flux/components/notification/>)

The GitOps Toolkit Notification Controller documentation.

##### [Image reflector and automation controllers](</flux/components/image/>)

The GitOps Toolkit Image Automation Controllers documentation.

Last modified 2025-10-01: [roadmap: Mark the 2.7 milestone as completed (5a605d40)](<https://github.com/fluxcd/website/commit/5a605d40bf532a920bd133fa964038c86864c56d>)

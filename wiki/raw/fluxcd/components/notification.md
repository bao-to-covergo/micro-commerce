---
source_url: https://fluxcd.io/flux/components/notification/
fetched: 2026-05-01
---

  1. [Docs](<https://fluxcd.io/flux/>)
  2. [Toolkit Components](<https://fluxcd.io/flux/components/>)
  3. [Notification Controller](<https://fluxcd.io/flux/components/notification/>)



# Notification Controller

The GitOps Toolkit Notification Controller documentation.

The Notification Controller is a Kubernetes operator, specialized in handling inbound and outbound events.

![Notification Controller Diagram](/img/notification-controller.png)

The controller handles events coming from external systems (GitHub, GitLab, Bitbucket, Harbor, Jenkins, etc) and notifies the GitOps toolkit controllers about source changes.

The controller handles events emitted by the GitOps toolkit controllers (source, kustomize, helm) and dispatches them to external systems (Slack, Microsoft Teams, Discord, Rocker) based on event severity and involved objects.

Links:

  * Source code [fluxcd/notification-controller](<https://github.com/fluxcd/notification-controller>)
  * Specification [docs](<https://github.com/fluxcd/notification-controller/tree/main/docs/spec>)



* * *

##### [Controller Options](</flux/components/notification/options/>)

Controller command flags and defaults.

##### [Alerts](</flux/components/notification/alerts/>)

The GitOps Toolkit Custom Resource Definitions documentation.

##### [Events](</flux/components/notification/events/>)

The GitOps Toolkit Custom Resource Definitions documentation.

##### [Receivers](</flux/components/notification/receivers/>)

The GitOps Toolkit Custom Resource Definitions documentation.

##### [Providers](</flux/components/notification/providers/>)

The GitOps Toolkit Custom Resource Definitions documentation.

##### [API Reference](</flux/components/notification/api/>)

Last modified 2022-08-30: [Move all Flux docs under /flux (a7cd1b87)](<https://github.com/fluxcd/website/commit/a7cd1b872d3ba78e6d129f3278bf41d8fa41b00d>)

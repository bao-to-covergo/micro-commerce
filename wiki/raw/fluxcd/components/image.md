---
source_url: https://fluxcd.io/flux/components/image/
fetched: 2026-05-01
---

  1. [Docs](<https://fluxcd.io/flux/>)
  2. [Toolkit Components](<https://fluxcd.io/flux/components/>)
  3. [Image Automation Controllers](<https://fluxcd.io/flux/components/image/>)



# Image reflector and automation controllers

The GitOps Toolkit Image Automation Controllers documentation.

The image-reflector-controller and image-automation-controller work together to update a Git repository when new container images are available.

  * The image-reflector-controller scans image repositories and reflects the image metadata in Kubernetes resources.
  * The image-automation-controller updates YAML files based on the latest images scanned, and commits the changes to a given Git repository.



![Image Automation Controller Diagrams](/img/image-update-automation.png)

Links:

  * Reflector source code [fluxcd/image-reflector-controller](<https://github.com/fluxcd/image-reflector-controller>)
  * Reflector [specification docs](<https://github.com/fluxcd/image-reflector-controller/tree/main/docs/spec>)
  * Automation source code [fluxcd/image-automation-controller](<https://github.com/fluxcd/image-automation-controller>)
  * Automation [specification docs](<https://github.com/fluxcd/image-automation-controller/tree/main/docs/spec>)



* * *

##### [Controller Options](</flux/components/image/options/>)

Controller command flags and defaults.

##### [Image Policies](</flux/components/image/imagepolicies/>)

The GitOps Toolkit Custom Resource Definitions documentation.

##### [Image Repositories](</flux/components/image/imagerepositories/>)

The GitOps Toolkit Custom Resource Definitions documentation.

##### [Image Reflector API Reference](</flux/components/image/reflector-api/>)

##### [Image Update Automation API Reference](</flux/components/image/automation-api/>)

##### [Image Update Automations](</flux/components/image/imageupdateautomations/>)

The GitOps Toolkit Custom Resource Definitions documentation.

Last modified 2022-08-30: [Move all Flux docs under /flux (a7cd1b87)](<https://github.com/fluxcd/website/commit/a7cd1b872d3ba78e6d129f3278bf41d8fa41b00d>)

---
source_url: https://fluxcd.io/flux/installation/bootstrap/google-cloud-source/
fetched: 2026-05-01
---

  1. [Docs](<https://fluxcd.io/flux/>)
  2. [Installation](<https://fluxcd.io/flux/installation/>)
  3. [Bootstrap](<https://fluxcd.io/flux/installation/bootstrap/>)
  4. [Google Cloud Source](<https://fluxcd.io/flux/installation/bootstrap/google-cloud-source/>)



# Flux bootstrap for Google Cloud Source

How to bootstrap Flux with Google Cloud Source Repository

To install Flux on a GKE cluster using a Google Cloud Source repository as the source of truth, you can use the [`flux bootstrap git`](</flux/installation/bootstrap/generic-git-server/>) command.

#### Required permissions

To bootstrap Flux, the person running the command must have **cluster admin rights** for the target Kubernetes cluster. It is also required that the person running the command to have **pull and push rights** for the Google Cloud Source repository.

## Bootstrap over SSH

First create a new repository to hold your Flux install and other Kubernetes resources. Then generate a SSH key and add the SSH public key to your personal SSH keys on Google Cloud.

Run bootstrap using the SSH private key and passphrase:
```
flux bootstrap git \
  --url=ssh://<user>s@source.developers.google.com:2022/p/<project-name>/r/<repo-name> \
  --branch=<my-branch> \
  --private-key-file=<path/to/ssh/private.key> \
  --password=<key-passphrase> \
  --path=clusters/my-cluster
```

You can also pipe the passphrase e.g. `echo key-passphrase | flux bootstrap git`.

The SSH private key and the known hosts keys are stored in the cluster as a Kubernetes secret named `flux-system` inside the `flux-system` namespace.

#### SSH Key rotation

To rotate the SSH public key, delete the `flux-system` secret from the cluster and re-run the bootstrap command using a new SSH private key.

Last modified 2023-08-17: [Add bootstrap guides (f81dc813)](<https://github.com/fluxcd/website/commit/f81dc813af4f31e9acd20c2f8647f4ce371cf3b7>)

---
source_url: https://fluxcd.io/flux/installation/configuration/air-gapped/
fetched: 2026-05-01
---

  1. [Docs](<https://fluxcd.io/flux/>)
  2. [Installation](<https://fluxcd.io/flux/installation/>)
  3. [Configuration](<https://fluxcd.io/flux/installation/configuration/>)
  4. [Air-Gapped installation](<https://fluxcd.io/flux/installation/configuration/air-gapped/>)



# Flux air-gapped installation

How to configure Flux for air-gapped clusters

Flux can be installed on air-gapped environments where the Kubernetes cluster, the container registry and the Git server are not connected to the internet.

## Copy the container images

On a machine with access to `github.com` and `ghcr.io`, download the Flux CLI from [GitHub releases page](<https://github.com/fluxcd/flux2/releases>).

List the Flux container images with:
```
flux install --export | grep ghcr.io
```

Copy each controller images to your private container registry using [crane](<https://github.com/google/go-containerregistry/blob/main/cmd/crane/README.md>):
```
FLUX_CONTROLLERS=(
"source-controller"
"kustomize-controller"
"helm-controller"
"notification-controller"
"image-reflector-controller"
"image-automation-controller"
)

for controller in "${FLUX_CONTROLLERS[@]}"; do
 crane copy --all-tags ghcr.io/fluxcd/$controller  <your-registry>/$controller
done
```

## Bootstrap Flux

Copy the Flux CLI binary to the machine inside the air-gapped network and run bootstrap using the images from your private registry:
```
flux bootstrap git \
  --registry=registry.internal/fluxcd \
  --url=ssh://git@<host>/<org>/<repository> \
  --branch=<my-branch> \
  --private-key-file=<path/to/private.key> \
  --password=<key-passphrase> \
  --path=clusters/my-cluster
```

**Note** that you must generate a SSH private key and set the public key as the deploy key on your Git server in advance.

For more information on how to use the `flux bootstrap git` command, please see the generic Git server [documentation](</flux/installation/bootstrap/generic-git-server/>).

## Bootstrap Flux and authenticate to a private container registry

If the private container registry requires authentication, you can pass the credentials to the Flux CLI using the `--registry-creds` flag. Replace `username:password` with your credentials and run:
```
flux bootstrap git \
  --registry-creds=username:password \
  --image-pull-secret=regcred \
  --registry=registry.internal/fluxcd \
  --url=ssh://git@<host>/<org>/<repository> \
  --branch=<my-branch> \
  --private-key-file=<path/to/private.key> \
  --password=<key-passphrase> \
  --path=clusters/my-cluster
```

The Flux CLI creates the image pull secret specified with `--image-pull-secret` in the `flux-system` namespace and configures the controllers pods to use it.

To update the credentials in the image pull secret, you can re-run the bootstrap command with the new credentials or update the secret directly with:
```
flux -n flux-system create secret oci regcred \
  --username=username \
  --password=password
```

Last modified 2024-04-08: [Improve the air-gapped with `flux bootstrap --registry-creds` (7f962411)](<https://github.com/fluxcd/website/commit/7f962411b1288621c08dfa8d19651628ba45e242>)

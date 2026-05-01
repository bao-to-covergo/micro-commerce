---
source_url: https://fluxcd.io/flux/installation/configuration/deploy-key-rotation/
fetched: 2026-05-01
---

  1. [Docs](<https://fluxcd.io/flux/>)
  2. [Installation](<https://fluxcd.io/flux/installation/>)
  3. [Configuration](<https://fluxcd.io/flux/installation/configuration/>)
  4. [Deploy key rotation](<https://fluxcd.io/flux/installation/configuration/deploy-key-rotation/>)



# Flux deploy key rotation

How to rotate the deploy key generated at bootstrap

There are several reasons you may want to rotate the deploy key:

  * The token used to generate the key has expired.
  * The key has been compromised.
  * You want to change the scope of the key, e.g. to allow write access using the `--read-write-key` flag to `flux bootstrap`.



While you can run `flux bootstrap` repeatedly, be aware that the `flux-system` Kubernetes Secret is never overwritten. You need to manually rotate the key as described here.

To rotate the SSH key generated at bootstrap, first delete the secret from the cluster with:
```
kubectl -n flux-system delete secret flux-system
```

Then you have two alternatives to generate a new key:

  1. Generate a new secret with
```
flux create secret git flux-system \
       --url=ssh://git@<host>/<org>/<repository>
```

The above command will print the SSH public key, once you set it as the deploy key, Flux will resume all operations.

  2. Run `flux bootstrap ...` again. This will generate a new key pair and, depending on which Git provider you use, print the SSH public key that you then set as deploy key or automatically set the deploy key (e.g. with GitHub).




Last modified 2023-08-17: [Add bootstrap configuration tasks (3ac6e663)](<https://github.com/fluxcd/website/commit/3ac6e663f508fb6362855cce7ed7b7ce1fa8123c>)

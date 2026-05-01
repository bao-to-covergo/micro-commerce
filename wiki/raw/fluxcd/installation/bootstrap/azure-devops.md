---
source_url: https://fluxcd.io/flux/installation/bootstrap/azure-devops/
fetched: 2026-05-01
---

  1. [Docs](<https://fluxcd.io/flux/>)
  2. [Installation](<https://fluxcd.io/flux/installation/>)
  3. [Bootstrap](<https://fluxcd.io/flux/installation/bootstrap/>)
  4. [Azure DevOps](<https://fluxcd.io/flux/installation/bootstrap/azure-devops/>)



# Flux bootstrap for Azure DevOps

How to bootstrap Flux with Azure DevOps

To install Flux on an AKS cluster using an Azure DevOps Git repository as the source of truth, you can use the [`flux bootstrap git`](</flux/installation/bootstrap/generic-git-server/>) command.

#### Required permissions

To bootstrap Flux, the person running the command must have **cluster admin rights** for the target Kubernetes cluster. It is also required that the person running the command to have **pull and push rights** for the Azure DevOps Git repository.

## Azure DevOps PAT

For accessing the Azure API, the bootstrap command requires an Azure DevOps personal access token (PAT) with pull and push permissions for Git repositories.

Generate an [Azure DevOps PAT](<https://docs.microsoft.com/en-us/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate?view=azure-devops&tabs=preview-page>) and create a new repository to hold your Flux install and other Kubernetes resources.

The Azure DevOps PAT can be exported as an environment variable:
```
export GIT_PASSWORD=<az-token>
```

If the `GIT_PASSWORD` env var is not set, the bootstrap command will prompt you to type the token.

You can also supply the token using a pipe e.g. `echo "<az-token>" | flux bootstrap git`.

## Bootstrap using a DevOps PAT

Run the bootstrap for a repository using token-based authentication:
```
flux bootstrap git \
  --token-auth=true \
  --url=https://dev.azure.com/<org>/<project>/_git/<repository> \
  --branch=main \
  --path=clusters/my-cluster
```

When using `--token-auth`, the CLI and the Flux controllers running on the cluster will use the Azure DevOps PAT to access the Git repository over HTTPS.

Note that the Azure DevOps PAT is stored in the cluster as a **Kubernetes Secret** named `flux-system` inside the `flux-system` namespace.

#### Token rotation

Note that Azure DevOps PAT have an expiry date. To rotate the token before it expires, delete the `flux-system` secret from the cluster and create a new one with the new PAT:
```
flux create secret git flux-system \
   --url=https://dev.azure.com/<org>/<project>/_git/<repository> \
   --username=git \
   --password=<az-token>
```

## Bootstrap using SSH keys

Azure DevOps SSH works only with RSA SHA-2 keys.

To configure Flux with RSA SHA-2 keys, you need to clone the DevOps locally, then create the file structure required by bootstrap with:
```
mkdir -p clusters/my-cluster/flux-system
touch clusters/my-cluster/flux-system/gotk-components.yaml \
    clusters/my-cluster/flux-system/gotk-sync.yaml \
    clusters/my-cluster/flux-system/kustomization.yaml
```

Edit the `kustomization.yaml` file to include the following patches:
```
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
resources:
  - gotk-components.yaml
  - gotk-sync.yaml
patches:
  - patch: |
      - op: add
        path: /spec/template/spec/containers/0/args/-
        value: --ssh-hostkey-algos=rsa-sha2-512,rsa-sha2-256            
    target:
      kind: Deployment
      name: (source-controller|image-automation-controller)
```

Commit and push the changes to upstream with:
```
git add -A && git commit -m "init flux" && git push
```

To generate an SSH key pair compatible with Azure DevOps, you'll need to use `ssh-keygen` with the `rsa-sha2-512` algorithm:
```
ssh-keygen -t rsa-sha2-512
```

Upload the SSH public key to Azure DevOps. For more information, see the [Azure DevOps documentation](<https://learn.microsoft.com/en-us/azure/devops/repos/git/use-ssh-keys-to-authenticate?view=azure-devops#step-2-add-the-public-key-to-azure-devops>).

Run bootstrap using the SSH URL of the Azure DevOps repository and the RSA SHA-2 private key:
```
flux bootstrap git \
  --url=ssh://git@ssh.dev.azure.com/v3/<org>/<project>/<repository>
  --branch=<my-branch> \
  --ssh-hostkey-algos=rsa-sha2-512,rsa-sha2-256 \
  --private-key-file=<path/to/ssh/private.key> \
  --password=<key-passphrase> \
  --path=clusters/my-cluster
```

For more information on how to use the `flux bootstrap git` command, please see the generic Git server [documentation](</flux/installation/bootstrap/generic-git-server/>).

Last modified 2026-02-08: [docs: fix typo (f407d182)](<https://github.com/fluxcd/website/commit/f407d182765a60639fe125d2be2a01d8252ad0f8>)

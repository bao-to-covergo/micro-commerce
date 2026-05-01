---
source_url: https://fluxcd.io/flux/installation/bootstrap/oracle-vbs-git-repositories/
fetched: 2026-05-01
---

  1. [Docs](<https://fluxcd.io/flux/>)
  2. [Installation](<https://fluxcd.io/flux/installation/>)
  3. [Bootstrap](<https://fluxcd.io/flux/installation/bootstrap/>)
  4. [Oracle VBS Git Repositories](<https://fluxcd.io/flux/installation/bootstrap/oracle-vbs-git-repositories/>)



# Flux bootstrap for Oracle VBS Git Repositories

How to bootstrap Flux with Oracle VBS Git Repositories

To install Flux on an [OKE](<https://www.oracle.com/cloud/cloud-native/container-engine-kubernetes>) cluster using an Oracle VBS Git repository as the source of truth, you can use the [`flux bootstrap git`](</flux/installation/bootstrap/generic-git-server/>) command.

#### Required permissions

To bootstrap Flux, the person running the command must have **cluster admin rights** for the target Kubernetes cluster. It is also required that the person running the command to have **pull and push rights** for the Oracle VBS Git repositories.

## Oracle VBS PAT

For accessing the Oracle VBS, the bootstrap command requires an Oracle VBS personal access token (PAT) with pull and push permissions for Git repositories.

Generate an [Oracle VBS Access Token](<https://docs.public.oneportal.content.oci.oraclecloud.com/en-us/iaas/Content/Identity/usersettings/generate-personal-access-tokens.htm>). And create a new repository to hold your Flux install and other Kubernetes resources.

The Oracle VBS PAT can be exported as an environment variable:
```
export GIT_PASSWORD=<vbs-token>
```

If the `GIT_PASSWORD` env var is not set, the bootstrap command will prompt you to type the token.

You can also supply the token using a pipe e.g. `echo "<vbs-token>" | flux bootstrap git`.

## Bootstrap using an Oracle VBS PAT

Run the bootstrap for a repository using token-based authentication:
```
flux bootstrap git \
  --with-bearer-token=true \
  --url=https://<vbs-repository-url> \
  --branch=my-branch \
  --path=clusters/my-cluster
```

When using `--with-bearer-token`, the CLI and the Flux controllers running on the cluster will use the Oracle VBS PAT to access the Git repository over HTTPS.

Note that the Oracle VBS PAT is stored in the cluster as a **Kubernetes Secret** named `flux-system` inside the `flux-system` namespace.

#### Token rotation

Note that Oracle VBS PAT may have an expiry date if it was configured to have one. To rotate the token before it expires, delete the `flux-system` secret from the cluster and recreate it with the new PAT:
```
flux create secret git flux-system \
   --url=https://<vbs-repository-url> \
   --bearer-token=<vbs-token>
```

Last modified 2026-02-08: [docs: fix typo (f407d182)](<https://github.com/fluxcd/website/commit/f407d182765a60639fe125d2be2a01d8252ad0f8>)

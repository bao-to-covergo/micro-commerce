---
source_url: https://fluxcd.io/flux/installation/bootstrap/bitbucket/
fetched: 2026-05-01
---

  1. [Docs](<https://fluxcd.io/flux/>)
  2. [Installation](<https://fluxcd.io/flux/installation/>)
  3. [Bootstrap](<https://fluxcd.io/flux/installation/bootstrap/>)
  4. [Bitbucket](<https://fluxcd.io/flux/installation/bootstrap/bitbucket/>)



# Flux bootstrap for Bitbucket

How to bootstrap Flux with Bitbucket Server and Data Center

The [flux bootstrap bitbucket-server](</flux/cmd/flux_bootstrap_bitbucket-server/>) command deploys the Flux controllers on a Kubernetes cluster and configures the controllers to sync the cluster state from a Bitbucket project. Besides installing the controllers, the bootstrap command pushes the Flux manifests to the Bitbucket project and configures Flux to update itself from Git.

After running the bootstrap command, any operation on the cluster (including Flux upgrades) can be done via Git push, without the need to connect to the Kubernetes cluster.

#### Required permissions

To bootstrap Flux, the person running the command must have **cluster admin rights** for the target Kubernetes cluster. It is also required that the person running the command to be the **owner** of the Bitbucket project, or to have admin rights of a Bitbucket group.

#### Bitbucket versions

This bootstrap command works only with Bitbucket Server and Data Center. For Bitbucket Cloud, please use the [generic bootstrap](</flux/installation/bootstrap/generic-git-server/>) procedure.

## Bitbucket HTTP Access Token

For accessing the Bitbucket API, the bootstrap command requires a [Bitbucket HTTP Access Token](<https://confluence.atlassian.com/bitbucketserver/http-access-tokens-939515499.html>) with administration permissions.

The Bitbucket HTTP access token can be exported as an environment variable:
```
export BITBUCKET_TOKEN=<bb-token>
```

If the `BITBUCKET_TOKEN` env var is not set, the bootstrap command will prompt you to type the token. You can also supply the token using a pipe e.g. `echo "<bb-token>" | flux bootstrap bitbucket-server`.

## Bitbucket Personal Account

Run the bootstrap for a repository on your personal Bitbucket Server account:
```
flux bootstrap bitbucket-server \
  --token-auth \
  --hostname=my-bitbucket-server.com \
  --owner=my-bitbucket-username \
  --repository=my-repository \
  --branch=main \
  --path=clusters/my-cluster \
  --personal
```

If the specified repository does not exist, Flux will create it for you as private. If you wish to create a public repository, set `--private=false`.

When using `--token-auth`, the CLI and the Flux controllers running on the cluster will use the Bitbucket token to access the Git repository over HTTPS.

#### PAT secret

Note that the Bitbucket token is stored in the cluster as a **Kubernetes Secret** named `flux-system` inside the `flux-system` namespace. If you want to avoid storing your token in the cluster, please see how to configure Bitbucket SSH access keys.

## Bitbucket Personal Project

Run the bootstrap for a repository owned by a Bitbucket Server project:
```
flux bootstrap bitbucket-server \
  --token-auth \
  --hostname=my-bitbucket-server.com \
  --owner=my-bitbucket-project \
  --username=my-bitbucket-username \
  --repository=my-repository \
  --branch=main \
  --path=clusters/my-cluster \
  --group=group-name 
```

When you specify a list of groups, those teams will be granted write access to the repository.

**Note:** The `username` is mandatory for `project` owned repositories. The specified user must own the `BITBUCKET_TOKEN` and have sufficient rights on the target `project` to create repositories.

## Bitbucket SSH Access Keys

If you want to bootstrap Flux using SSH instead of HTTP/S, you can set `--token-auth=false` and the Flux CLI will use the Bitbucket token to set a SSH access key for your repository.

When using SSH, the bootstrap command will generate a SSH private key. The private key is stored in the cluster as a Kubernetes secret named `flux-system` inside the `flux-system` namespace.

The generated SSH key defaults to `ECDSA P-384`, to change the format use `--ssh-key-algorithm` and `--ssh-ecdsa-curve`.

By default, the SSH key is set to read-only access. If you're using Flux image automation, you must give it write access with `--read-write-key=true`.

To run the bootstrap for Bitbucket server with a custom SSH hostname and port:
```
flux bootstrap bitbucket-server \
  --hostname=my-bitbucket-server.com \
  --ssh-hostname=my-bitbucket-server.com:7999 \
  --owner=my-bitbucket-project \
  --username=my-bitbucket-username \
  --repository=my-repository \
  --branch=main \
  --path=clusters/my-cluster
```

## Bootstrap without a Bitbucket token

For existing Bitbucket repositories, you can bootstrap Flux over SSH without using a Bitbucket token.

To use a SSH key instead of a Bitbucket token, the command changes to `flux bootstrap git`:
```
flux bootstrap git \
  --url=ssh://git@<host>/<org>/<repository> \
  --branch=<my-branch> \
  --private-key-file=<path/to/ssh/private.key> \
  --password=<key-passphrase> \
  --path=clusters/my-cluster
```

**Note** that you must generate a SSH private key and set the public key as the access key on Bitbucket in advance.

For more information on how to use the `flux bootstrap git` command, please see the generic Git server [documentation](</flux/installation/bootstrap/generic-git-server/>).

Last modified 2026-02-08: [docs: fix typo (f407d182)](<https://github.com/fluxcd/website/commit/f407d182765a60639fe125d2be2a01d8252ad0f8>)

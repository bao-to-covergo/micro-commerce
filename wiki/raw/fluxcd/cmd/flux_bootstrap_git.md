---
source_url: https://fluxcd.io/flux/cmd/flux_bootstrap_git/
fetched: 2026-05-01
---

  1. [Docs](<https://fluxcd.io/flux/>)
  2. [Flux CLI](<https://fluxcd.io/flux/cmd/>)
  3. [flux bootstrap git](<https://fluxcd.io/flux/cmd/flux_bootstrap_git/>)



# flux bootstrap git

## flux bootstrap git

Deploy Flux on a cluster connected to a Git repository

### Synopsis

The bootstrap git command commits the Flux manifests to the branch of a Git repository. And then it configures the target cluster to synchronize with that repository. If the Flux components are present on the cluster, the bootstrap command will perform an upgrade if needed.
```
flux bootstrap git [flags]
```

### Examples
```
  # Run bootstrap for a Git repository and authenticate with your SSH agent
  flux bootstrap git --url=ssh://git@example.com/repository.git --path=clusters/my-cluster

  # Run bootstrap for a Git repository and authenticate using a password
  flux bootstrap git --url=https://example.com/repository.git --password=<password> --path=clusters/my-cluster

  # Run bootstrap for a Git repository and authenticate using a password from environment variable
  GIT_PASSWORD=<password> && flux bootstrap git --url=https://example.com/repository.git --path=clusters/my-cluster

  # Run bootstrap for a Git repository with a passwordless private key
  flux bootstrap git --url=ssh://git@example.com/repository.git --private-key-file=<path/to/private.key> --path=clusters/my-cluster

  # Run bootstrap for a Git repository with a private key and password
  flux bootstrap git --url=ssh://git@example.com/repository.git --private-key-file=<path/to/private.key> --password=<password> --path=clusters/my-cluster

  # Run bootstrap for a Git repository on AWS CodeCommit
  flux bootstrap git --url=ssh://<SSH-Key-ID>@git-codecommit.<region>.amazonaws.com/v1/repos/<repository> --private-key-file=<path/to/private.key> --password=<SSH-passphrase> --path=clusters/my-cluster

  # Run bootstrap for a Git repository on Azure Devops
  flux bootstrap git --url=ssh://git@ssh.dev.azure.com/v3/<org>/<project>/<repository> --private-key-file=<path/to/rsa-sha2-private.key> --ssh-hostkey-algos=rsa-sha2-512,rsa-sha2-256 --path=clusters/my-cluster

  # Run bootstrap for a Git repository on Oracle VBS
  flux bootstrap git --url=https://repository_url.git --with-bearer-token=true --password=<PAT> --path=clusters/my-cluster
```

### Options
```
      --allow-insecure-http     allows insecure HTTP connections
  -h, --help                    help for git
      --interval duration       sync interval (default 1m0s)
  -p, --password string         basic authentication password
      --path safeRelativePath   path relative to the repository root, when specified the cluster sync will be scoped to this path
  -s, --silent                  assumes the deploy key is already setup, skips confirmation
      --url string              Git repository URL
  -u, --username string         basic authentication username (default "git")
      --with-bearer-token       use password as bearer token for Authorization header
```

### Options inherited from parent commands
```
      --as string                             Username to impersonate for the operation. User could be a regular user or a service account in a namespace.
      --as-group stringArray                  Group to impersonate for the operation, this flag can be repeated to specify multiple groups.
      --as-uid string                         UID to impersonate for the operation.
      --as-user-extra stringArray             User extras to impersonate for the operation, this flag can be repeated to specify multiple values for the same key.
      --author-email string                   author email for Git commits
      --author-name string                    author name for Git commits (default "Flux")
      --branch string                         Git branch (default "main")
      --ca-file string                        path to TLS CA file used for validating self-signed certificates
      --cache-dir string                      Default cache directory (default "/opt/buildhome/.kube/cache")
      --certificate-authority string          Path to a cert file for the certificate authority to authenticate the Kubernetes API server
      --client-certificate string             Path to a client certificate file for TLS authentication to the Kubernetes API server
      --client-key string                     Path to a client key file for TLS authentication to the Kubernetes API server
      --cluster string                        The name of the kubeconfig cluster to use
      --cluster-domain string                 internal cluster domain (default "cluster.local")
      --commit-message-appendix string        string to add to the commit messages, e.g. '[ci skip]'
      --components strings                    list of components, accepts comma-separated values (default [source-controller,kustomize-controller,helm-controller,notification-controller])
      --components-extra strings              list of components in addition to those supplied or defaulted, accepts values such as 'image-reflector-controller,image-automation-controller,source-watcher'
      --context string                        The name of the kubeconfig context to use
      --disable-compression                   If true, opt-out of response compression for all requests to the server
      --force                                 override existing Flux installation if it's managed by a different tool such as Helm
      --gpg-key-id string                     key id for selecting a particular key
      --gpg-key-ring string                   path to GPG key ring for signing commits
      --gpg-passphrase string                 passphrase for decrypting GPG private key
      --image-pull-secret string              Kubernetes secret name used for pulling the controller images from a private registry
      --insecure-skip-tls-verify              If true, the Kubernetes API server's certificate will not be checked for validity. This will make your HTTPS connections insecure
      --kube-api-burst int                    The maximum burst queries-per-second of requests sent to the Kubernetes API. (default 300)
      --kube-api-qps float32                  The maximum queries-per-second of requests sent to the Kubernetes API. (default 50)
      --kubeconfig string                     Path to the kubeconfig file to use for CLI requests.
      --log-level debug|info|error            log level (default info)
  -n, --namespace string                      If present, the namespace scope for this CLI request (default "flux-system")
      --network-policy                        setup Kubernetes network policies to deny ingress access to the Flux controllers from other namespaces (default true)
      --private-key-file string               path to a private key file used for authenticating to the Git SSH server
      --recurse-submodules                    when enabled, configures the GitRepository source to initialize and include Git submodules in the artifact it produces
      --registry string                       container registry where the Flux controller images are published (default "ghcr.io/fluxcd")
      --registry-creds string                 container registry credentials in the format 'user:password', requires --image-pull-secret to be set
      --secret-name string                    name of the secret the sync credentials can be found in or stored to (default "flux-system")
      --server string                         The address and port of the Kubernetes API server
      --ssh-ecdsa-curve p256|p384|p521        SSH ECDSA public key curve (default p384)
      --ssh-hostkey-algos strings             list of host key algorithms to be used by the CLI for SSH connections
      --ssh-hostname string                   SSH hostname, to be used when the SSH host differs from the HTTPS one
      --ssh-key-algorithm rsa|ecdsa|ed25519   SSH public key algorithm (default ecdsa)
      --ssh-rsa-bits rsaKeyBits               SSH RSA public key bit size (multiplies of 8, min 1024) (default 2048)
      --timeout duration                      timeout for this operation (default 5m0s)
      --tls-server-name string                Server name to use for server certificate validation. If it is not provided, the hostname used to contact the server is used
      --token string                          Bearer token for authentication to the API server
      --token-auth                            when enabled, the personal access token will be used instead of the SSH deploy key
      --toleration-keys strings               list of toleration keys used to schedule the controller pods onto nodes with matching taints
      --user string                           The name of the kubeconfig user to use
      --verbose                               print generated objects
  -v, --version string                        toolkit version, when specified the manifests are downloaded from https://github.com/fluxcd/flux2/releases
      --watch-all-namespaces                  watch for custom resources in all namespaces, if set to false it will only watch the namespace where the Flux controllers are installed (default true)
```

### SEE ALSO

  * [flux bootstrap](<../flux_bootstrap/>) \- Deploy Flux on a cluster the GitOps way.



---
source_url: https://fluxcd.io/flux/cmd/flux_create_source_git/
fetched: 2026-05-01
---

  1. [Docs](<https://fluxcd.io/flux/>)
  2. [Flux CLI](<https://fluxcd.io/flux/cmd/>)
  3. [flux create source git](<https://fluxcd.io/flux/cmd/flux_create_source_git/>)



# flux create source git

## flux create source git

Create or update a GitRepository source

### Synopsis

The create source git command generates a GitRepository resource and waits for it to sync. For Git over SSH, host and SSH keys are automatically generated and stored in a Kubernetes secret. For private Git repositories, the basic authentication credentials are stored in a Kubernetes secret.
```
flux create source git [name] [flags]
```

### Examples
```
  # Create a source from a public Git repository master branch
  flux create source git podinfo \
    --url=https://github.com/stefanprodan/podinfo \
    --branch=master

  # Create a source for a Git repository pinned to specific git tag
  flux create source git podinfo \
    --url=https://github.com/stefanprodan/podinfo \
    --tag="3.2.3"

  # Create a source from a public Git repository tag that matches a semver range
  flux create source git podinfo \
    --url=https://github.com/stefanprodan/podinfo \
    --tag-semver=">=3.2.0 <3.3.0"

  # Create a source for a Git repository using SSH authentication
  flux create source git podinfo \
    --url=ssh://git@github.com/stefanprodan/podinfo \
    --branch=master

  # Create a source for a Git repository using SSH authentication and an
  # ECDSA P-521 curve public key
  flux create source git podinfo \
    --url=ssh://git@github.com/stefanprodan/podinfo \
    --branch=master \
    --ssh-key-algorithm=ecdsa \
    --ssh-ecdsa-curve=p521

  # Create a source for a Git repository using SSH authentication and a
  #	passwordless private key from file
  # The public SSH host key will still be gathered from the host
  flux create source git podinfo \
    --url=ssh://git@github.com/stefanprodan/podinfo \
    --branch=master \
    --private-key-file=./private.key

  # Create a source for a Git repository using SSH authentication and a
  # private key with a password from file
  # The public SSH host key will still be gathered from the host
  flux create source git podinfo \
    --url=ssh://git@github.com/stefanprodan/podinfo \
    --branch=master \
    --private-key-file=./private.key \
    --password=<password>

  # Create a source for a Git repository using basic authentication
  flux create source git podinfo \
    --url=https://github.com/stefanprodan/podinfo \
    --branch=master \
    --username=username \
    --password=password

  # Create a source for a Git repository using azure provider
  flux create source git podinfo \
    --url=https://dev.azure.com/foo/bar/_git/podinfo \
    --branch=master \
    --provider=azure
```

### Options
```
      --branch string                         git branch
      --ca-file string                        path to TLS CA file used for validating self-signed certificates
      --commit string                         git commit
  -h, --help                                  help for git
      --ignore-paths strings                  set paths to ignore in git resource (can specify multiple paths with commas: path1,path2)
  -p, --password string                       basic authentication password
      --private-key-file string               path to a passwordless private key file used for authenticating to the Git SSH server
      --provider generic|azure|github         the Git provider name
      --proxy-secret-ref string               the name of an existing secret containing the proxy address and credentials
      --recurse-submodules                    when enabled, configures the GitRepository source to initialize and include Git submodules in the artifact it produces
      --ref-name string                       git reference name
      --secret-ref string                     the name of an existing secret containing SSH or basic credentials or github app authentication
  -s, --silent                                assumes the deploy key is already setup, skips confirmation
      --sparse-checkout-paths strings         set paths to sparse checkout in git resource (can specify multiple paths with commas: path1,path2)
      --ssh-ecdsa-curve p256|p384|p521        SSH ECDSA public key curve (default p384)
      --ssh-key-algorithm rsa|ecdsa|ed25519   SSH public key algorithm (default ecdsa)
      --ssh-rsa-bits rsaKeyBits               SSH RSA public key bit size (multiplies of 8, min 1024) (default 2048)
      --tag string                            git tag
      --tag-semver string                     git tag semver range
      --url string                            git address, e.g. ssh://git@host/org/repository
  -u, --username string                       basic authentication username
```

### Options inherited from parent commands
```
      --as string                      Username to impersonate for the operation. User could be a regular user or a service account in a namespace.
      --as-group stringArray           Group to impersonate for the operation, this flag can be repeated to specify multiple groups.
      --as-uid string                  UID to impersonate for the operation.
      --as-user-extra stringArray      User extras to impersonate for the operation, this flag can be repeated to specify multiple values for the same key.
      --cache-dir string               Default cache directory (default "/opt/buildhome/.kube/cache")
      --certificate-authority string   Path to a cert file for the certificate authority to authenticate the Kubernetes API server
      --client-certificate string      Path to a client certificate file for TLS authentication to the Kubernetes API server
      --client-key string              Path to a client key file for TLS authentication to the Kubernetes API server
      --cluster string                 The name of the kubeconfig cluster to use
      --context string                 The name of the kubeconfig context to use
      --disable-compression            If true, opt-out of response compression for all requests to the server
      --export                         export in YAML format to stdout
      --fetch-timeout duration         set a timeout for fetch operations performed by source-controller (e.g. 'git clone' or 'helm repo update')
      --insecure-skip-tls-verify       If true, the Kubernetes API server's certificate will not be checked for validity. This will make your HTTPS connections insecure
      --interval duration              source sync interval (default 1m0s)
      --kube-api-burst int             The maximum burst queries-per-second of requests sent to the Kubernetes API. (default 300)
      --kube-api-qps float32           The maximum queries-per-second of requests sent to the Kubernetes API. (default 50)
      --kubeconfig string              Path to the kubeconfig file to use for CLI requests.
      --label strings                  set labels on the resource (can specify multiple labels with commas: label1=value1,label2=value2)
  -n, --namespace string               If present, the namespace scope for this CLI request (default "flux-system")
      --server string                  The address and port of the Kubernetes API server
      --timeout duration               timeout for this operation (default 5m0s)
      --tls-server-name string         Server name to use for server certificate validation. If it is not provided, the hostname used to contact the server is used
      --token string                   Bearer token for authentication to the API server
      --user string                    The name of the kubeconfig user to use
      --verbose                        print generated objects
```

### SEE ALSO

  * [flux create source](<../flux_create_source/>) \- Create or update sources



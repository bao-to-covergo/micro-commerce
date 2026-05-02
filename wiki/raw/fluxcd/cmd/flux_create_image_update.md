---
source_url: https://fluxcd.io/flux/cmd/flux_create_image_update/
fetched: 2026-05-01
---

  1. [Docs](<https://fluxcd.io/flux/>)
  2. [Flux CLI](<https://fluxcd.io/flux/cmd/>)
  3. [flux create image update](<https://fluxcd.io/flux/cmd/flux_create_image_update/>)



# flux create image update

## flux create image update

Create or update an ImageUpdateAutomation object

### Synopsis

The create image update command generates an ImageUpdateAutomation resource. An ImageUpdateAutomation object specifies an automated update to images mentioned in YAMLs in a git repository.
```
flux create image update [name] [flags]
```

### Examples
```
  # Configure image updates for the main repository created by flux bootstrap
  flux create image update flux-system \
    --git-repo-ref=flux-system \
    --git-repo-path="./clusters/my-cluster" \
    --checkout-branch=main \
    --author-name=flux \
    --author-email=flux@example.com \
    --commit-template="{{range .Updated.Images}}{{println .}}{{end}}"

  # Configure image updates to push changes to a different branch, if the branch doesn't exists it will be created
  flux create image update flux-system \
    --git-repo-ref=flux-system \
    --git-repo-path="./clusters/my-cluster" \
    --checkout-branch=main \
    --push-branch=image-updates \
    --author-name=flux \
    --author-email=flux@example.com \
    --commit-template="{{range .Updated.Images}}{{println .}}{{end}}"

  # Configure image updates for a Git repository in a different namespace
  flux create image update apps \
    --namespace=apps \
    --git-repo-ref=flux-system \
    --git-repo-namespace=flux-system \
    --git-repo-path="./clusters/my-cluster" \
    --checkout-branch=main \
    --push-branch=image-updates \
    --author-name=flux \
    --author-email=flux@example.com \
    --commit-template="{{range .Updated.Images}}{{println .}}{{end}}"
```

### Options
```
      --author-email string         the email to use for commit author
      --author-name string          the name to use for commit author
      --checkout-branch string      the branch to checkout
      --commit-template string      a template for commit messages
      --git-repo-namespace string   the namespace of the GitRepository resource, defaults to the ImageUpdateAutomation namespace
      --git-repo-path string        path to the directory containing the manifests to be updated, defaults to the repository root
      --git-repo-ref string         the name of a GitRepository resource with details of the upstream Git repository
  -h, --help                        help for update
      --push-branch string          the branch to push commits to, defaults to the checkout branch if not specified
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

  * [flux create image](<../flux_create_image/>) \- Create or update resources dealing with image automation



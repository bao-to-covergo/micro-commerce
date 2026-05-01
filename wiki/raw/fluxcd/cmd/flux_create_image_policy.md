---
source_url: https://fluxcd.io/flux/cmd/flux_create_image_policy/
fetched: 2026-05-01
---

  1. [Docs](<https://fluxcd.io/flux/>)
  2. [Flux CLI](<https://fluxcd.io/flux/cmd/>)
  3. [flux create image policy](<https://fluxcd.io/flux/cmd/flux_create_image_policy/>)



# flux create image policy

## flux create image policy

Create or update an ImagePolicy object

### Synopsis

The create image policy command generates an ImagePolicy resource. An ImagePolicy object calculates a "latest image" given an image repository and a policy, e.g., semver.

The image that sorts highest according to the policy is recorded in the status of the object.
```
flux create image policy [name] [flags]
```

### Examples
```
  # Create an ImagePolicy to select the latest stable release
  flux create image policy podinfo \
    --image-ref=podinfo \
    --select-semver=">=1.0.0"

  # Create an ImagePolicy to select the latest main branch build tagged as "${GIT_BRANCH}-${GIT_SHA:0:7}-$(date +%s)"
  flux create image policy podinfo \
    --image-ref=podinfo \
    --select-numeric=asc \
	--filter-regex='^main-[a-f0-9]+-(?P<ts>[0-9]+)' \
	--filter-extract='$ts'
```

### Options
```
      --filter-extract string   replacement pattern (using capture groups from --filter-regex) to use for sorting
      --filter-regex string     regular expression pattern used to filter the image tags
  -h, --help                    help for policy
      --image-ref string        the name of an image repository object
      --interval duration       the interval at which to check for new image digests when the policy is set to 'Always'
      --reflect-digest string   the digest reflection policy to use when observing latest image tags (one of 'Never', 'IfNotPresent', 'Never')
      --select-alpha string     use alphabetical sorting to select image; either "asc" meaning select the last, or "desc" meaning select the first
      --select-numeric string   use numeric sorting to select image; either "asc" meaning select the last, or "desc" meaning select the first
      --select-semver string    a semver range to apply to tags; e.g., '1.x'
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



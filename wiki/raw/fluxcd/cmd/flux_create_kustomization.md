---
source_url: https://fluxcd.io/flux/cmd/flux_create_kustomization/
fetched: 2026-05-01
---

  1. [Docs](<https://fluxcd.io/flux/>)
  2. [Flux CLI](<https://fluxcd.io/flux/cmd/>)
  3. [flux create kustomization](<https://fluxcd.io/flux/cmd/flux_create_kustomization/>)



# flux create kustomization

## flux create kustomization

Create or update a Kustomization resource

### Synopsis

The create command generates a Kustomization resource for a given source.
```
flux create kustomization [name] [flags]
```

### Examples
```
  # Create a Kustomization resource from a source at a given path
  flux create kustomization kyverno \
    --source=GitRepository/kyverno \
    --path="./config/release" \
    --prune=true \
    --interval=60m \
    --wait=true \
    --health-check-timeout=3m

  # Create a Kustomization resource that depends on the previous one
  flux create kustomization kyverno-policies \
    --depends-on=kyverno \
    --source=GitRepository/kyverno-policies \
    --path="./policies/flux" \
    --prune=true \
    --interval=5m

  # Create a Kustomization using a source from a different namespace
  flux create kustomization podinfo \
    --namespace=default \
    --source=GitRepository/podinfo.flux-system \
    --path="./kustomize" \
    --prune=true \
    --interval=5m

  # Create a Kustomization resource that references an OCIRepository
  flux create kustomization podinfo \
    --source=OCIRepository/podinfo \
    --target-namespace=default \
    --prune=true \
    --interval=5m

  # Create a Kustomization resource that references a Bucket
  flux create kustomization secrets \
    --source=Bucket/secrets \
    --prune=true \
    --interval=5m
```

### Options
```
      --decryption-provider decryptionProvider   decryption provider, available options are: (sops)
      --decryption-secret string                 set the Kubernetes secret name that contains the OpenPGP private keys used for sops decryption
      --depends-on strings                       Kustomization that must be ready before this Kustomization can be applied, supported formats '<name>' and '<namespace>/<name>', also accepts comma-separated values
      --health-check strings                     workload to be included in the health assessment, in the format '<kind>/<name>.<namespace>'
      --health-check-timeout duration            timeout of health checking operations (default 2m0s)
  -h, --help                                     help for kustomization
      --kubeconfig-secret-ref string             the name of the Kubernetes Secret that contains a key with the kubeconfig file for connecting to a remote cluster
      --path safeRelativePath                    path to the directory containing a kustomization.yaml file (default ./)
      --prune                                    enable garbage collection
      --retry-interval duration                  the interval at which to retry a previously failed reconciliation
      --service-account string                   the name of the service account to impersonate when reconciling this Kustomization
      --source string                            source that contains the Kubernetes manifests in the format '[<kind>/]<name>.<namespace>', where kind must be one of: (OCIRepository, GitRepository, Bucket), if kind is not specified it defaults to GitRepository
      --target-namespace string                  overrides the namespace of all Kustomization objects reconciled by this Kustomization
      --wait                                     enable health checking of all the applied resources
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

  * [flux create](<../flux_create/>) \- Create or update sources and resources



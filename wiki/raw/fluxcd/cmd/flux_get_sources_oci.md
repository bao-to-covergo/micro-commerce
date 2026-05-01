---
source_url: https://fluxcd.io/flux/cmd/flux_get_sources_oci/
fetched: 2026-05-01
---

  1. [Docs](<https://fluxcd.io/flux/>)
  2. [Flux CLI](<https://fluxcd.io/flux/cmd/>)
  3. [flux get sources oci](<https://fluxcd.io/flux/cmd/flux_get_sources_oci/>)



# flux get sources oci

## flux get sources oci

Get OCIRepository status

### Synopsis

The get sources oci command prints the status of the OCIRepository sources.

⚠️ Please note that this command is in preview and under development. While we try our best to not introduce breaking changes, they may occur when we adapt to new features and/or find better ways to facilitate what it does.
```
flux get sources oci [flags]
```

### Examples
```
  # List all OCIRepositories and their status
  flux get sources oci

  # List OCIRepositories from all namespaces
  flux get sources oci --all-namespaces
```

### Options
```
  -h, --help   help for oci
```

### Options inherited from parent commands
```
  -A, --all-namespaces                 list the requested object(s) across all namespaces
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
      --insecure-skip-tls-verify       If true, the Kubernetes API server's certificate will not be checked for validity. This will make your HTTPS connections insecure
      --kube-api-burst int             The maximum burst queries-per-second of requests sent to the Kubernetes API. (default 300)
      --kube-api-qps float32           The maximum queries-per-second of requests sent to the Kubernetes API. (default 50)
      --kubeconfig string              Path to the kubeconfig file to use for CLI requests.
  -l, --label-selector string          filter objects by label selector
  -n, --namespace string               If present, the namespace scope for this CLI request (default "flux-system")
      --no-header                      skip the header when printing the results
      --server string                  The address and port of the Kubernetes API server
      --status-selector string         specify the status condition name and the desired state to filter the get result, e.g. ready=false
      --timeout duration               timeout for this operation (default 5m0s)
      --tls-server-name string         Server name to use for server certificate validation. If it is not provided, the hostname used to contact the server is used
      --token string                   Bearer token for authentication to the API server
      --user string                    The name of the kubeconfig user to use
      --verbose                        print generated objects
  -w, --watch                          After listing/getting the requested object, watch for changes.
```

### SEE ALSO

  * [flux get sources](<../flux_get_sources/>) \- Get source statuses



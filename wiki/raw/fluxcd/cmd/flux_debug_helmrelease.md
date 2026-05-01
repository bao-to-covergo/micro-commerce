---
source_url: https://fluxcd.io/flux/cmd/flux_debug_helmrelease/
fetched: 2026-05-01
---

  1. [Docs](<https://fluxcd.io/flux/>)
  2. [Flux CLI](<https://fluxcd.io/flux/cmd/>)
  3. [flux debug helmrelease](<https://fluxcd.io/flux/cmd/flux_debug_helmrelease/>)



# flux debug helmrelease

## flux debug helmrelease

Debug a HelmRelease resource

### Synopsis

The debug helmrelease command can be used to troubleshoot failing Helm release reconciliations. WARNING: This command will print sensitive information if Kubernetes Secrets are referenced in the HelmRelease .spec.valuesFrom field.

⚠️ Please note that this command is in preview and under development. While we try our best to not introduce breaking changes, they may occur when we adapt to new features and/or find better ways to facilitate what it does.
```
flux debug helmrelease [name] [flags]
```

### Examples
```
  # Print the status of a Helm release
  flux debug hr podinfo --show-status

  # Export the final values of a Helm release composed from referred ConfigMaps and Secrets
  flux debug hr podinfo --show-values > values.yaml

  # Print the reconciliation history of a Helm release
  flux debug hr podinfo --show-history
```

### Options
```
  -h, --help           help for helmrelease
      --show-history   print the reconciliation history of the Helm release
      --show-status    print the status of the Helm release
      --show-values    print the final values of the Helm release
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
      --insecure-skip-tls-verify       If true, the Kubernetes API server's certificate will not be checked for validity. This will make your HTTPS connections insecure
      --kube-api-burst int             The maximum burst queries-per-second of requests sent to the Kubernetes API. (default 300)
      --kube-api-qps float32           The maximum queries-per-second of requests sent to the Kubernetes API. (default 50)
      --kubeconfig string              Path to the kubeconfig file to use for CLI requests.
  -n, --namespace string               If present, the namespace scope for this CLI request (default "flux-system")
      --server string                  The address and port of the Kubernetes API server
      --timeout duration               timeout for this operation (default 5m0s)
      --tls-server-name string         Server name to use for server certificate validation. If it is not provided, the hostname used to contact the server is used
      --token string                   Bearer token for authentication to the API server
      --user string                    The name of the kubeconfig user to use
      --verbose                        print generated objects
```

### SEE ALSO

  * [flux debug](<../flux_debug/>) \- Debug a flux resource



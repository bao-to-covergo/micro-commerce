---
source_url: https://fluxcd.io/flux/cmd/flux_create_secret_notation/
fetched: 2026-05-01
---

  1. [Docs](<https://fluxcd.io/flux/>)
  2. [Flux CLI](<https://fluxcd.io/flux/cmd/>)
  3. [flux create secret notation](<https://fluxcd.io/flux/cmd/flux_create_secret_notation/>)



# flux create secret notation

## flux create secret notation

Create or update a Kubernetes secret for verifications of artifacts signed by Notation

### Synopsis

The create secret notation command generates a Kubernetes secret with root ca certificates and trust policy.

⚠️ Please note that this command is in preview and under development. While we try our best to not introduce breaking changes, they may occur when we adapt to new features and/or find better ways to facilitate what it does.
```
flux create secret notation [name] [flags]
```

### Examples
```
 # Create a Notation configuration secret on disk and encrypt it with Mozilla SOPS
  flux create secret notation my-notation-cert \
    --namespace=my-namespace \
    --trust-policy-file=./my-trust-policy.json \
    --ca-cert-file=./my-cert.crt \
    --export > my-notation-cert.yaml

  sops --encrypt --encrypted-regex '^(data|stringData)$' \
    --in-place my-notation-cert.yaml
```

### Options
```
      --ca-cert-file strings       root ca cert file path
  -h, --help                       help for notation
      --trust-policy-file string   notation trust policy file path
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

  * [flux create secret](<../flux_create_secret/>) \- Create or update Kubernetes secrets



---
source_url: https://fluxcd.io/flux/cmd/flux_create_source_helm/
fetched: 2026-05-01
---

  1. [Docs](<https://fluxcd.io/flux/>)
  2. [Flux CLI](<https://fluxcd.io/flux/cmd/>)
  3. [flux create source helm](<https://fluxcd.io/flux/cmd/flux_create_source_helm/>)



# flux create source helm

## flux create source helm

Create or update a HelmRepository source

### Synopsis

The create source helm command generates a HelmRepository resource and waits for it to fetch the index. For private Helm repositories, the basic authentication credentials are stored in a Kubernetes secret.
```
flux create source helm [name] [flags]
```

### Examples
```
  # Create a source for an HTTPS public Helm repository
  flux create source helm podinfo \
    --url=https://stefanprodan.github.io/podinfo \
    --interval=10m

  # Create a source for an HTTPS Helm repository using basic authentication
  flux create source helm podinfo \
    --url=https://stefanprodan.github.io/podinfo \
    --username=username \
    --password=password

  # Create a source for an HTTPS Helm repository using TLS authentication
  flux create source helm podinfo \
    --url=https://stefanprodan.github.io/podinfo \
    --cert-file=./cert.crt \
    --key-file=./key.crt \
    --ca-file=./ca.crt

  # Create a source for an OCI Helm repository
  flux create source helm podinfo \
    --url=oci://ghcr.io/stefanprodan/charts/podinfo \
    --username=username \
    --password=password

  # Create a source for an OCI Helm repository using an existing secret with basic auth or dockerconfig credentials
  flux create source helm podinfo \
    --url=oci://ghcr.io/stefanprodan/charts/podinfo \
    --secret-ref=docker-config
```

### Options
```
      --ca-file string        TLS authentication CA file path
      --cert-file string      TLS authentication cert file path
  -h, --help                  help for helm
      --key-file string       TLS authentication key file path
      --oci-provider string   OCI provider for authentication
      --pass-credentials      pass credentials to all domains
  -p, --password string       basic authentication password
      --secret-ref string     the name of an existing secret containing TLS, basic auth or docker-config credentials
      --url string            Helm repository address
  -u, --username string       basic authentication username
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



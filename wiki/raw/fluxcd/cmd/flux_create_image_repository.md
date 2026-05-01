---
source_url: https://fluxcd.io/flux/cmd/flux_create_image_repository/
fetched: 2026-05-01
---

  1. [Docs](<https://fluxcd.io/flux/>)
  2. [Flux CLI](<https://fluxcd.io/flux/cmd/>)
  3. [flux create image repository](<https://fluxcd.io/flux/cmd/flux_create_image_repository/>)



# flux create image repository

## flux create image repository

Create or update an ImageRepository object

### Synopsis

The create image repository command generates an ImageRepository resource. An ImageRepository object specifies an image repository to scan.
```
flux create image repository [name] [flags]
```

### Examples
```
  # Create an ImageRepository object to scan the alpine image repository:
  flux create image repository alpine-repo --image alpine --interval 20m

  # Create an image repository that uses an image pull secret (assumed to
  # have been created already):
  flux create image repository myapp-repo \
    --secret-ref image-pull \
    --image ghcr.io/example.com/myapp --interval 5m

  # Create a TLS secret for a local image registry using a self-signed
  # host certificate, and use it to scan an image. ca.pem is a file
  # containing the CA certificate used to sign the host certificate.
  flux create secret tls local-registry-cert --ca-file ./ca.pem
  flux create image repository app-repo \
    --cert-secret-ref local-registry-cert \
    --image local-registry:5000/app --interval 5m

  # Create a TLS secret with a client certificate and key, and use it
  # to scan a private image registry.
  flux create secret tls client-cert \
    --cert-file client.crt --key-file client.key
  flux create image repository app-repo \
    --cert-secret-ref client-cert \
    --image registry.example.com/private/app --interval 5m
```

### Options
```
      --cert-ref string         the name of a secret to use for TLS certificates
  -h, --help                    help for repository
      --image string            the image repository to scan; e.g., library/alpine
      --scan-timeout duration   a timeout for scanning; this defaults to the interval if not set
      --secret-ref string       the name of a docker-registry secret to use for credentials
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



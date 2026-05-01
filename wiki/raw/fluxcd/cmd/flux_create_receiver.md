---
source_url: https://fluxcd.io/flux/cmd/flux_create_receiver/
fetched: 2026-05-01
---

  1. [Docs](<https://fluxcd.io/flux/>)
  2. [Flux CLI](<https://fluxcd.io/flux/cmd/>)
  3. [flux create receiver](<https://fluxcd.io/flux/cmd/flux_create_receiver/>)



# flux create receiver

## flux create receiver

Create or update a Receiver resource

### Synopsis

The create receiver command generates a Receiver resource.
```
flux create receiver [name] [flags]
```

### Examples
```
  # Create a Receiver
  flux create receiver github-receiver \
	--type github \
	--event ping \
	--event push \
	--secret-ref webhook-token \
	--resource GitRepository/webapp \
	--resource HelmRepository/webapp
```

### Options
```
      --event strings                                                                                    also accepts comma-separated values
  -h, --help                                                                                             help for receiver
      --resource strings                                                                                 also accepts comma-separated values
      --secret-ref string                                                                                
      --type generic|generic-hmac|github|gitlab|bitbucket|harbor|dockerhub|quay|gcr|nexus|acr|cdevents   the receiver type
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



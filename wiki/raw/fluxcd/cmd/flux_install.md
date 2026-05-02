---
source_url: https://fluxcd.io/flux/cmd/flux_install/
fetched: 2026-05-01
---

  1. [Docs](<https://fluxcd.io/flux/>)
  2. [Flux CLI](<https://fluxcd.io/flux/cmd/>)
  3. [flux install](<https://fluxcd.io/flux/cmd/flux_install/>)



# flux install

## flux install

Install or upgrade Flux

### Synopsis

The install command deploys Flux in the specified namespace. If a previous version is installed, then an in-place upgrade will be performed.
```
flux install [flags]
```

### Examples
```
  # Install the latest version in the flux-system namespace
  flux install --namespace=flux-system

  # Install a specific series of components
  flux install --components="source-controller,kustomize-controller"

  # Install all components including the image automation ones
  flux install --components-extra="image-reflector-controller,image-automation-controller"

  # Install Flux onto tainted Kubernetes nodes
  flux install --toleration-keys=node.kubernetes.io/dedicated-to-flux

  # Dry-run install
  flux install --export | kubectl apply --dry-run=client -f- 

  # Write install manifests to file
  flux install --export > flux-system.yaml
```

### Options
```
      --cluster-domain string        internal cluster domain (default "cluster.local")
      --components strings           list of components, accepts comma-separated values (default [source-controller,kustomize-controller,helm-controller,notification-controller])
      --components-extra strings     list of components in addition to those supplied or defaulted, accepts values such as 'image-reflector-controller,image-automation-controller,source-watcher'
      --export                       write the install manifests to stdout and exit
      --force                        override existing Flux installation if it's managed by a different tool such as Helm
  -h, --help                         help for install
      --image-pull-secret string     Kubernetes secret name used for pulling the toolkit images from a private registry
      --log-level debug|info|error   log level (default info)
      --network-policy               deny ingress access to the toolkit controllers from other namespaces using network policies (default true)
      --registry string              container registry where the toolkit images are published (default "ghcr.io/fluxcd")
      --registry-creds string        container registry credentials in the format 'user:password', requires --image-pull-secret to be set
      --toleration-keys strings      list of toleration keys used to schedule the components pods onto nodes with matching taints
  -v, --version string               toolkit version, when specified the manifests are downloaded from https://github.com/fluxcd/flux2/releases
      --watch-all-namespaces         watch for custom resources in all namespaces, if set to false it will only watch the namespace where the toolkit is installed (default true)
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

  * [flux](<../flux/>) \- Command line utility for assembling Kubernetes CD pipelines



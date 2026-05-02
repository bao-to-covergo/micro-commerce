---
source_url: https://fluxcd.io/flux/cmd/flux_build_kustomization/
fetched: 2026-05-01
---

  1. [Docs](<https://fluxcd.io/flux/>)
  2. [Flux CLI](<https://fluxcd.io/flux/cmd/>)
  3. [flux build kustomization](<https://fluxcd.io/flux/cmd/flux_build_kustomization/>)



# flux build kustomization

## flux build kustomization

Build Kustomization

### Synopsis

The build command queries the Kubernetes API and fetches the specified Flux Kustomization. It then uses the fetched in cluster flux kustomization to perform needed transformation on the local kustomization.yaml pointed at by -path. The local kustomization.yaml is generated if it does not exist. Finally it builds the overlays using the local kustomization.yaml, and write the resulting multi-doc YAML to stdout.

It is possible to specify a Flux kustomization file using -kustomization-file.
```
flux build kustomization [flags]
```

### Examples
```
# Build the local manifests as they were built on the cluster
flux build kustomization my-app --path ./path/to/local/manifests

# Build using a local flux kustomization file
flux build kustomization my-app --path ./path/to/local/manifests --kustomization-file ./path/to/local/my-app.yaml

# Build in dry-run mode without connecting to the cluster.
# Note that variable substitutions from Secrets and ConfigMaps are skipped in dry-run mode.
flux build kustomization my-app --path ./path/to/local/manifests \
	--kustomization-file ./path/to/local/my-app.yaml \
	--dry-run

# Exclude files by providing a comma separated list of entries that follow the .gitignore pattern fromat.
flux build kustomization my-app --path ./path/to/local/manifests \
	--kustomization-file ./path/to/local/my-app.yaml \
	--ignore-paths "/to_ignore/**/*.yaml,ignore.yaml"

# Run recursively on all encountered Kustomizations
flux build kustomization my-app --path ./path/to/local/manifests \
	--recursive \
	--local-sources GitRepository/flux-system/my-repo=./path/to/local/git
```

### Options
```
      --dry-run                        Dry run mode.
  -h, --help                           help for kustomization
      --ignore-paths strings           set paths to ignore in .gitignore format
      --kustomization-file string      Path to the Flux Kustomization YAML file.
      --local-sources stringToString   Comma-separated list of repositories in format: Kind/namespace/name=path (default [])
      --path string                    Path to the manifests location.
  -r, --recursive                      Recursively build Kustomizations
      --strict-substitute              When enabled, the post build substitutions will fail if a var without a default value is declared in files but is missing from the input vars.
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

  * [flux build](<../flux_build/>) \- Build a flux resource



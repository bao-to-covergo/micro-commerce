---
source_url: https://fluxcd.io/flux/cmd/flux_diff_kustomization/
fetched: 2026-05-01
---

  1. [Docs](<https://fluxcd.io/flux/>)
  2. [Flux CLI](<https://fluxcd.io/flux/cmd/>)
  3. [flux diff kustomization](<https://fluxcd.io/flux/cmd/flux_diff_kustomization/>)



# flux diff kustomization

## flux diff kustomization

Diff Kustomization

### Synopsis

The diff command does a build, then it performs a server-side dry-run and prints the diff. Exit status: 0 No differences were found. 1 Differences were found. >1 diff failed with an error.
```
flux diff kustomization [flags]
```

### Examples
```
# Preview local changes as they were applied on the cluster
flux diff kustomization my-app --path ./path/to/local/manifests

# Preview using a local flux kustomization file
flux diff kustomization my-app --path ./path/to/local/manifests \
	--kustomization-file ./path/to/local/my-app.yaml

# Exclude files by providing a comma separated list of entries that follow the .gitignore pattern fromat.
flux diff kustomization my-app --path ./path/to/local/manifests \
	--kustomization-file ./path/to/local/my-app.yaml \
	--ignore-paths "/to_ignore/**/*.yaml,ignore.yaml"

# Run recursively on all encountered Kustomizations
flux diff kustomization my-app --path ./path/to/local/manifests \
    --recursive \
    --local-sources GitRepository/flux-system/my-repo=./path/to/local/git
```

### Options
```
  -h, --help                           help for kustomization
      --ignore-paths strings           set paths to ignore in .gitignore format
      --kustomization-file string      Path to the Flux Kustomization YAML file.
      --local-sources stringToString   Comma-separated list of repositories in format: Kind/namespace/name=path (default [])
      --path string                    Path to a local directory that matches the specified Kustomization.spec.path.
      --progress-bar                   Boolean to set the progress bar. The default value is true. (default true)
  -r, --recursive                      Recursively diff Kustomizations
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

  * [flux diff](<../flux_diff/>) \- Diff a flux resource



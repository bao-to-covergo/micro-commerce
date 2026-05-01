---
source_url: https://fluxcd.io/flux/cmd/flux_migrate/
fetched: 2026-05-01
---

  1. [Docs](<https://fluxcd.io/flux/>)
  2. [Flux CLI](<https://fluxcd.io/flux/cmd/>)
  3. [flux migrate](<https://fluxcd.io/flux/cmd/flux_migrate/>)



# flux migrate

## flux migrate

Migrate the Flux custom resources to their latest API version

### Synopsis

The migrate command must be run before a Flux minor version upgrade.

The command has two modes of operation:

  * Cluster mode (default): migrates all the Flux custom resources stored in Kubernetes etcd to their latest API version.
  * File system mode (-f): migrates the Flux custom resources defined in the manifests located in the specified path.


```
flux migrate [flags]
```

### Examples
```
  # Migrate all the Flux custom resources in the cluster.
  # This uses the current kubeconfig context and requires cluster-admin permissions.
  flux migrate

  # Migrate all the Flux custom resources in a Git repository
  # checked out in the current working directory.
  flux migrate -f .

  # Migrate all Flux custom resources defined in YAML and Helm YAML template files.
  flux migrate -f . --extensions=.yml,.yaml,.tpl

  # Migrate the Flux custom resources to the latest API versions of Flux 2.6.
  flux migrate -f . --version=2.6

  # Migrate the Flux custom resources defined in a multi-document YAML manifest file.
  flux migrate -f path/to/manifest.yaml

  # Simulate the migration without making any changes.
  flux migrate -f . --dry-run

  # Run the migration skipping confirmation prompts.
  flux migrate -f . --yes
```

### Options
```
      --dry-run              simulate the migration of manifests without making any changes, only applicable with --path
  -e, --extensions strings   the file extensions to consider when migrating manifests, only applicable with --path (default [.yaml,.yml])
  -h, --help                 help for migrate
  -f, --path string          the path to the directory containing the manifests to migrate
  -v, --version string       the target Flux minor version to migrate manifests to, only applicable with --path (defaults to the version of the CLI)
  -y, --yes                  skip confirmation prompts when migrating manifests, only applicable with --path
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



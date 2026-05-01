---
source_url: https://fluxcd.io/flux/cmd/flux_create_source_chart/
fetched: 2026-05-01
---

  1. [Docs](<https://fluxcd.io/flux/>)
  2. [Flux CLI](<https://fluxcd.io/flux/cmd/>)
  3. [flux create source chart](<https://fluxcd.io/flux/cmd/flux_create_source_chart/>)



# flux create source chart

## flux create source chart

Create or update a HelmChart source

### Synopsis

The create source chart command generates a HelmChart resource and waits for the chart to be available.
```
flux create source chart [name] [flags]
```

### Examples
```
  # Create a source for a chart residing in a HelmRepository
  flux create source chart podinfo \
    --source=HelmRepository/podinfo \
    --chart=podinfo \
    --chart-version=6.x

  # Create a source for a chart residing in a Git repository
  flux create source chart podinfo \
    --source=GitRepository/podinfo \
    --chart=./charts/podinfo

  # Create a source for a chart residing in a S3 Bucket
  flux create source chart podinfo \
    --source=Bucket/podinfo \
    --chart=./charts/podinfo

  # Create a source for a chart from OCI and verify its signature
  flux create source chart podinfo \
    --source HelmRepository/podinfo \
    --chart podinfo \
    --chart-version=6.6.2 \
    --verify-provider=cosign \
    --verify-issuer=https://token.actions.githubusercontent.com \
    --verify-subject=https://github.com/stefanprodan/podinfo/.github/workflows/release.yml@refs/tags/6.6.2
```

### Options
```
      --chart string                              Helm chart name or path
      --chart-version string                      Helm chart version, accepts a semver range (ignored for charts from GitRepository sources)
  -h, --help                                      help for chart
      --reconcile-strategy string                 the reconcile strategy for helm chart (accepted values: Revision and ChartRevision) (default "ChartVersion")
      --source helmChartSource                    source that contains the chart in the format '<kind>/<name>', where kind must be one of: (HelmRepository, GitRepository, Bucket)
      --verify-issuer string                      regular expression to use for the OIDC issuer during signature verification
      --verify-provider sourceOCIVerifyProvider   the OCI verify provider name to use for signature verification, available options are: (cosign)
      --verify-secret-ref string                  the name of a secret to use for signature verification
      --verify-subject string                     regular expression to use for the OIDC subject during signature verification
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



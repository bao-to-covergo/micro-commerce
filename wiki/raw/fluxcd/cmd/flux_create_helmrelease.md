---
source_url: https://fluxcd.io/flux/cmd/flux_create_helmrelease/
fetched: 2026-05-01
---

  1. [Docs](<https://fluxcd.io/flux/>)
  2. [Flux CLI](<https://fluxcd.io/flux/cmd/>)
  3. [flux create helmrelease](<https://fluxcd.io/flux/cmd/flux_create_helmrelease/>)



# flux create helmrelease

## flux create helmrelease

Create or update a HelmRelease resource

### Synopsis

The helmrelease create command generates a HelmRelease resource for a given HelmRepository source.
```
flux create helmrelease [name] [flags]
```

### Examples
```
  # Create a HelmRelease with a chart from a HelmRepository source
  flux create hr podinfo \
    --interval=10m \
    --source=HelmRepository/podinfo \
    --chart=podinfo \
    --chart-version=">4.0.0"

  # Create a HelmRelease with a chart from a GitRepository source
  flux create hr podinfo \
    --interval=10m \
    --source=GitRepository/podinfo \
    --chart=./charts/podinfo

  # Create a HelmRelease with a chart from a Bucket source
  flux create hr podinfo \
    --interval=10m \
    --source=Bucket/podinfo \
    --chart=./charts/podinfo

  # Create a HelmRelease with values from local YAML files
  flux create hr podinfo \
    --source=HelmRepository/podinfo \
    --chart=podinfo \
    --values=./my-values1.yaml \
    --values=./my-values2.yaml

  # Create a HelmRelease with values from a Kubernetes secret
  kubectl -n app create secret generic my-secret-values \
	--from-file=values.yaml=/path/to/my-secret-values.yaml
  flux -n app create hr podinfo \
    --source=HelmRepository/podinfo \
    --chart=podinfo \
    --values-from=Secret/my-secret-values

  # Create a HelmRelease with a custom release name
  flux create hr podinfo \
    --release-name=podinfo-dev \
    --source=HelmRepository/podinfo \
    --chart=podinfo

  # Create a HelmRelease targeting another namespace than the resource
  flux create hr podinfo \
    --target-namespace=test \
    --create-target-namespace=true \
    --source=HelmRepository/podinfo \
    --chart=podinfo

  # Create a HelmRelease with custom storage namespace for hub-and-spoke model
  flux create hr podinfo \
    --target-namespace=production \
    --storage-namespace=fluxcd-system \
    --source=HelmRepository/podinfo \
    --chart=podinfo

  # Create a HelmRelease using a source from a different namespace
  flux create hr podinfo \
    --namespace=default \
    --source=HelmRepository/podinfo.flux-system \
    --chart=podinfo

  # Create a HelmRelease definition on disk without applying it on the cluster
  flux create hr podinfo \
    --source=HelmRepository/podinfo \
    --chart=podinfo \
    --values=./values.yaml \
    --export > podinfo-release.yaml
		
  # Create a HelmRelease using a chart from a HelmChart resource
  flux create hr podinfo \
    --namespace=default \
    --chart-ref=HelmChart/podinfo.flux-system \

  # Create a HelmRelease using a chart from an OCIRepository resource
  flux create hr podinfo \
    --namespace=default \
    --chart-ref=OCIRepository/podinfo.flux-system
```

### Options
```
      --chart string                     Helm chart name or path
      --chart-interval duration          the interval of which to check for new chart versions
      --chart-ref string                 the name of the HelmChart resource to use as source for the HelmRelease, in the format '<kind>/<name>.<namespace>', where kind must be one of: (OCIRepository,HelmChart)
      --chart-version string             Helm chart version, accepts a semver range (ignored for charts from GitRepository sources)
      --crds Skip|Create|CreateReplace   upgrade CRDs policy
      --create-target-namespace          create the target namespace if it does not exist
      --depends-on strings               HelmReleases that must be ready before this release can be installed, supported formats '<name>' and '<namespace>/<name>'
  -h, --help                             help for helmrelease
      --kubeconfig-secret-ref string     the name of the Kubernetes Secret that contains a key with the kubeconfig file for connecting to a remote cluster
      --reconcile-strategy string        the reconcile strategy for helm chart created by the helm release(accepted values: Revision and ChartRevision) (default "ChartVersion")
      --release-name string              name used for the Helm release, defaults to a composition of '[<target-namespace>-]<HelmRelease-name>'
      --service-account string           the name of the service account to impersonate when reconciling this HelmRelease
      --source string                    source that contains the chart in the format '<kind>/<name>.<namespace>', where kind must be one of: (HelmRepository, GitRepository, Bucket)
      --storage-namespace string         namespace to store the Helm release, defaults to the target namespace
      --target-namespace string          namespace to install this release, defaults to the HelmRelease namespace
      --values strings                   local path to values.yaml files, also accepts comma-separated values
      --values-from strings              a Kubernetes object reference that contains the values.yaml data key in the format '<kind>/<name>', where kind must be one of: (Secret,ConfigMap)
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



---
source_url: https://fluxcd.io/flux/cmd/flux_push_artifact/
fetched: 2026-05-01
---

  1. [Docs](<https://fluxcd.io/flux/>)
  2. [Flux CLI](<https://fluxcd.io/flux/cmd/>)
  3. [flux push artifact](<https://fluxcd.io/flux/cmd/flux_push_artifact/>)



# flux push artifact

## flux push artifact

Push artifact

### Synopsis

The push artifact command creates a tarball from the given directory or the single file and uploads the artifact to an OCI repository. The command can read the credentials from '~/.docker/config.json' but they can also be passed with -creds. It can also login to a supported provider with the -provider flag.
```
flux push artifact [flags]
```

### Examples
```
  # Push manifests to GHCR using the short Git SHA as the OCI artifact tag
  echo $GITHUB_PAT | docker login ghcr.io --username flux --password-stdin
  flux push artifact oci://ghcr.io/org/config/app:$(git rev-parse --short HEAD) \
	--path="./path/to/local/manifests" \
	--source="$(git config --get remote.origin.url)" \
	--revision="$(git branch --show-current)@sha1:$(git rev-parse HEAD)"

  # Push and sign artifact with cosign
  digest_url = $(flux push artifact \
	oci://ghcr.io/org/config/app:$(git rev-parse --short HEAD) \
	--source="$(git config --get remote.origin.url)" \
	--revision="$(git branch --show-current)@sha1:$(git rev-parse HEAD)" \
	--path="./path/to/local/manifest.yaml" \
	--output json | \
	jq -r '. | .repository + "@" + .digest')
  cosign sign $digest_url

  # Push manifests passed into stdin to GHCR and set custom OCI annotations
  kustomize build . | flux push artifact oci://ghcr.io/org/config/app:$(git rev-parse --short HEAD) -f - \ 
    --source="$(git config --get remote.origin.url)" \
    --revision="$(git branch --show-current)@sha1:$(git rev-parse HEAD)" \
    --annotations='org.opencontainers.image.licenses=Apache-2.0' \
    --annotations='org.opencontainers.image.documentation=https://app.org/docs' \
    --annotations='org.opencontainers.image.description=Production config.'

  # Push single manifest file to GHCR using the short Git SHA as the OCI artifact tag
  echo $GITHUB_PAT | docker login ghcr.io --username flux --password-stdin
  flux push artifact oci://ghcr.io/org/config/app:$(git rev-parse --short HEAD) \
	--path="./path/to/local/manifest.yaml" \
	--source="$(git config --get remote.origin.url)" \
	--revision="$(git branch --show-current)@sha1:$(git rev-parse HEAD)"

  # Push manifests to Docker Hub using the Git tag as the OCI artifact tag
  echo $DOCKER_PAT | docker login --username flux --password-stdin
  flux push artifact oci://docker.io/org/app-config:$(git tag --points-at HEAD) \
	--path="./path/to/local/manifests" \
	--source="$(git config --get remote.origin.url)" \
	--revision="$(git tag --points-at HEAD)@sha1:$(git rev-parse HEAD)"

  # Login directly to the registry provider
  # You might need to export the following variable if you use local config files for AWS:
  # export AWS_SDK_LOAD_CONFIG=1
  flux push artifact oci://<account>.dkr.ecr.<region>.amazonaws.com/app-config:$(git tag --points-at HEAD) \
	--path="./path/to/local/manifests" \
	--source="$(git config --get remote.origin.url)" \
	--revision="$(git tag --points-at HEAD)@sha1:$(git rev-parse HEAD)" \
	--provider aws

  # Login by passing credentials directly
  flux push artifact oci://docker.io/org/app-config:$(git tag --points-at HEAD) \
	--path="./path/to/local/manifests" \
	--source="$(git config --get remote.origin.url)" \
	--revision="$(git tag --points-at HEAD)@sha1:$(git rev-parse HEAD)" \
	--creds flux:$DOCKER_PAT
```

### Options
```
  -a, --annotations stringArray          Set custom OCI annotations in the format '<key>=<value>'
      --creds string                     credentials for OCI registry in the format <username>[:<password>] if --provider is generic
      --debug                            display logs from underlying library
  -h, --help                             help for artifact
      --ignore-paths strings             set paths to ignore in .gitignore format (default [.git/,.gitignore,.gitmodules,.gitattributes,*.jpg,*.jpeg,*.gif,*.png,*.wmv,*.flv,*.tar.gz,*.zip])
      --insecure-registry                allows artifacts to be pushed without TLS
  -o, --output string                    the format in which the artifact digest should be printed, can be 'json' or 'yaml'
  -f, --path string                      path to the directory where the Kubernetes manifests are located
      --provider generic|aws|azure|gcp   the OCI provider name (default generic)
      --reproducible                     ensure reproducible image digests by setting the created timestamp to '1970-01-01T00:00:00Z'
      --revision string                  the source revision in the format '<branch|tag>@sha1:<commit-sha>'
      --source string                    the source address, e.g. the Git URL
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

  * [flux push](<../flux_push/>) \- Push artifacts



---
source_url: https://fluxcd.io/flux/cmd/flux_create_source_bucket/
fetched: 2026-05-01
---

  1. [Docs](<https://fluxcd.io/flux/>)
  2. [Flux CLI](<https://fluxcd.io/flux/cmd/>)
  3. [flux create source bucket](<https://fluxcd.io/flux/cmd/flux_create_source_bucket/>)



# flux create source bucket

## flux create source bucket

Create or update a Bucket source

### Synopsis

The create source bucket command generates a Bucket resource and waits for it to be downloaded. For Buckets with static authentication, the credentials are stored in a Kubernetes secret.
```
flux create source bucket [name] [flags]
```

### Examples
```
  # Create a source for a Bucket using static authentication
  flux create source bucket podinfo \
	--bucket-name=podinfo \
    --endpoint=minio.minio.svc.cluster.local:9000 \
	--insecure=true \
	--access-key=myaccesskey \
	--secret-key=mysecretkey \
    --interval=10m

  # Create a source for an Amazon S3 Bucket using IAM authentication
  flux create source bucket podinfo \
	--bucket-name=podinfo \
	--provider=aws \
    --endpoint=s3.amazonaws.com \
	--region=us-east-1 \
    --interval=10m
```

### Options
```
      --access-key string                the bucket access key
      --bucket-name string               the bucket name
      --endpoint string                  the bucket endpoint address
  -h, --help                             help for bucket
      --ignore-paths strings             set paths to ignore in bucket resource (can specify multiple paths with commas: path1,path2)
      --insecure                         for when connecting to a non-TLS S3 HTTP endpoint
      --provider generic|aws|azure|gcp   the S3 compatible storage provider name (default generic)
      --proxy-secret-ref string          the name of an existing secret containing the proxy address and credentials
      --region string                    the bucket region
      --secret-key string                the bucket secret key
      --secret-ref string                the name of an existing secret containing credentials
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



---
source_url: https://fluxcd.io/flux/cmd/flux_create_secret_githubapp/
fetched: 2026-05-01
---

  1. [Docs](<https://fluxcd.io/flux/>)
  2. [Flux CLI](<https://fluxcd.io/flux/cmd/>)
  3. [flux create secret githubapp](<https://fluxcd.io/flux/cmd/flux_create_secret_githubapp/>)



# flux create secret githubapp

## flux create secret githubapp

Create or update a github app secret

### Synopsis

The create secret githubapp command generates a Kubernetes secret that can be used for GitRepository authentication with github app

⚠️ Please note that this command is in preview and under development. While we try our best to not introduce breaking changes, they may occur when we adapt to new features and/or find better ways to facilitate what it does.
```
flux create secret githubapp [name] [flags]
```

### Examples
```
  # Create a githubapp authentication secret on disk and encrypt it with Mozilla SOPS
  flux create secret githubapp podinfo-auth \
    --app-id="1" \
    --app-installation-id="2" \
    --app-private-key=./private-key-file.pem \
    --export > githubapp-auth.yaml

  sops --encrypt --encrypted-regex '^(data|stringData)$' \
    --in-place githubapp-auth.yaml
```

### Options
```
      --app-base-url string             github app base URL
      --app-id string                   github app ID
      --app-installation-id string      github app installation ID
      --app-installation-owner string   github app installation owner (user or organization)
      --app-private-key string          github app private key file path
  -h, --help                            help for githubapp
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

  * [flux create secret](<../flux_create_secret/>) \- Create or update Kubernetes secrets



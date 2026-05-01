---
source_url: https://fluxcd.io/flux/installation/configuration/vertical-scaling/
fetched: 2026-05-01
---

  1. [Docs](<https://fluxcd.io/flux/>)
  2. [Installation](<https://fluxcd.io/flux/installation/>)
  3. [Configuration](<https://fluxcd.io/flux/installation/configuration/>)
  4. [Vertical scaling](<https://fluxcd.io/flux/installation/configuration/vertical-scaling/>)



# Flux vertical scaling

How to configure vertical scaling for Flux controllers

When Flux is managing hundreds of applications that are deployed multiple times per day, cluster admins can fine tune the Flux controller at [bootstrap time](</flux/installation/configuration/bootstrap-customization/>) to run at scale by:

  * Increasing the number of workers and resource limits
  * Enabling in-memory kustomize builds
  * Enabling Helm repository caching to reduce memory usage
  * Enabling persistent storage for internal artifacts
  * Running the Flux controllers on dedicated nodes



#### Horizontal scaling

When vertical scaling is not an option, you can use sharding to horizontally scale the Flux controllers. For more details please see the [sharding guide](</flux/installation/configuration/sharding/>).

## Increase the number of workers and limits

If Flux is managing hundreds of applications, it is advised to increase the number of reconciliations that can be performed in parallel and to bump the resources limits:
```
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
resources:
  - gotk-components.yaml
  - gotk-sync.yaml
patches:
  - patch: |
      - op: add
        path: /spec/template/spec/containers/0/args/-
        value: --concurrent=10
      - op: add
        path: /spec/template/spec/containers/0/args/-
        value: --requeue-dependency=5s      
    target:
      kind: Deployment
      name: "(kustomize-controller|helm-controller|source-controller)"
  - patch: |
      apiVersion: apps/v1
      kind: Deployment
      metadata:
        name: all
      spec:
        template:
          spec:
            containers:
              - name: manager
                resources:
                  limits:
                    cpu: 2000m
                    memory: 2Gi      
    target:
      kind: Deployment
      name: "(kustomize-controller|helm-controller|source-controller)"
```

#### I/O Device Contention

Note that further increasing the number of concurrent reconciliations for kustomize-controller, without using a RAM-backed filesystem, may not have the desired effect due to IO contention on the Kubernetes node disk.

## Enable in-memory kustomize builds

When increasing the number of concurrent reconciliations, it is advised to use tmpfs for the `/tmp` filesystem to speed up the Flux kustomize build operations:
```
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
resources:
  - gotk-components.yaml
  - gotk-sync.yaml
patches:
  - patch: |
      - op: add
        path: /spec/template/spec/containers/0/args/-
        value: --concurrent=20
      - op: replace
        path: /spec/template/spec/volumes/0
        value:
          name: temp
          emptyDir:
            medium: Memory      
    target:
      kind: Deployment
      name: kustomize-controller
```

#### Memory usage

Note that tmpfs will count against the controller's pod memory usage, so it is advised to make use of the `GitRepository` [ignore feature](</flux/components/source/gitrepositories/#ignore-spec>) to exclude non-YAML files when Flux syncs app repos.

## Enable Helm repositories caching

If Flux connects to Helm repositories hosting hundreds of Helm charts, it is advised to enable caching to reduce the memory footprint of source-controller:
```
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
resources:
  - gotk-components.yaml
  - gotk-sync.yaml
patches:
  - patch: |
      - op: add
        path: /spec/template/spec/containers/0/args/-
        value: --helm-cache-max-size=10
      - op: add
        path: /spec/template/spec/containers/0/args/-
        value: --helm-cache-ttl=60m
      - op: add
        path: /spec/template/spec/containers/0/args/-
        value: --helm-cache-purge-interval=5m      
    target:
      kind: Deployment
      name: source-controller
```

When `helm-cache-max-size` is reached, an error is logged and the index is instead read from file. Cache hits are exposed via the `gotk_cache_events_total` Prometheus metrics. Use this data to fine-tune the configuration flags.

## Persistent storage for Flux internal artifacts

Flux maintains a local cache of artifacts acquired from external sources. By default, the cache is stored in an `EmptyDir` volume, which means that after a restart, Flux has to restore the local cache by fetching the content of all Git repositories, Buckets, Helm charts and OCI artifacts. To avoid losing the cached artifacts, you can configure source-controller with a persistent volume.

Create a Kubernetes PVC definition named `gotk-pvc.yaml` and place it in your `flux-system` directory:
```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: gotk-pvc
  namespace: flux-system
spec:
  accessModes:
    - ReadWriteOnce
  storageClassName: standard
  resources:
    requests:
      storage: 10Gi
```

Add the PVC file to the `kustomization.yaml` resources and patch the source-controller volumes:
```
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
resources:
  - gotk-components.yaml
  - gotk-sync.yaml
  - gotk-pvc.yaml
patches:
  - patch: |
      - op: add
        path: '/spec/template/spec/volumes/-'
        value:
          name: persistent-data
          persistentVolumeClaim:
            claimName: gotk-pvc
      - op: replace
        path: '/spec/template/spec/containers/0/volumeMounts/0'
        value:
          name: persistent-data
          mountPath: /data      
    target:
      kind: Deployment
      name: source-controller
```

## Node affinity and tolerations

Pin the Flux controller pods to specific nodes and allow the cluster autoscaler to evict them:
```
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
resources:
  - gotk-components.yaml
  - gotk-sync.yaml
patches:
  - patch: |
      apiVersion: apps/v1
      kind: Deployment
      metadata:
        name: all
      spec:
        template:
          metadata:
            annotations:
              cluster-autoscaler.kubernetes.io/safe-to-evict: "true"
          spec:
            affinity:
              nodeAffinity:
                requiredDuringSchedulingIgnoredDuringExecution:
                  nodeSelectorTerms:
                    - matchExpressions:
                        - key: role
                          operator: In
                          values:
                            - flux
            tolerations:
              - effect: NoSchedule
                key: role
                operator: Equal
                value: flux      
    target:
      kind: Deployment
      labelSelector: app.kubernetes.io/part-of=flux
```

The above configuration pins Flux to nodes tainted and labeled with:
```
kubectl taint nodes <my-node> role=flux:NoSchedule
kubectl label nodes <my-node> role=flux
```

Last modified 2025-09-09: [Fix typo: boostrap/bootstrap everywhere (b30bccc4)](<https://github.com/fluxcd/website/commit/b30bccc4d97ee6e2b6686f5ef5aebcf185e91b7e>)

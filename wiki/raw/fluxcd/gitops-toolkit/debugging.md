---
source_url: https://fluxcd.io/flux/gitops-toolkit/debugging/
fetched: 2026-05-01
---

  1. [Docs](<https://fluxcd.io/flux/>)
  2. [Toolkit Dev Guides](<https://fluxcd.io/flux/gitops-toolkit/>)
  3. [Advanced debugging](<https://fluxcd.io/flux/gitops-toolkit/debugging/>)



# Advanced debugging

Configure Go profiling for the GitOps Toolkit controllers.

This guide covers more advanced debugging topics such as collecting runtime profiling data from GitOps Toolkit components.

As a user, this page normally should be a last resort, but you may be asked by a maintainer to share a collected profile to debug e.g. performance issues.

## Pprof

The [GitOps Toolkit components](</flux/components/>) serve [`pprof`](<https://golang.org/pkg/net/http/pprof/>) runtime profiling data on their metrics HTTP server (default `:8080`).

### Endpoints

Endpoint| Path  
---|---  
Index| `/debug/pprof/`  
CPU profile| `/debug/pprof/profile`  
Symbol| `/debug/pprof/symbol`  
Trace| `/debug/pprof/trace`  
  
### Collecting a profile

To collect a profile, port-forward to the component's metrics endpoint and collect the data from the endpoint of choice:
```
$ kubectl port-forward -n <namespace> deploy/<component> 8080
$ curl -Sk -v http://localhost:8080/debug/pprof/heap > heap.out
```

The collected profile [can be analyzed using `go`](<https://blog.golang.org/pprof>), or shared with one of the maintainers.

## Resource usage

As `kubectl top` gives a limited (and at times inaccurate) overview of resource usage, it is often better to make use of the Grafana metrics to gather insights. See [Flux Prometheus metrics](</flux/monitoring/metrics/>) for a guide on how to visualize this data with a Grafana dashboard.

Last modified 2024-05-14: [Update dev guides to Flux v2.3 (e48ef4d3)](<https://github.com/fluxcd/website/commit/e48ef4d3808bdb9b917d8b9ee5269c57b07dad19>)

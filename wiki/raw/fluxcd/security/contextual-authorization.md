---
source_url: https://fluxcd.io/flux/security/contextual-authorization/
fetched: 2026-05-01
---

  1. [Docs](<https://fluxcd.io/flux/>)
  2. [Security](<https://fluxcd.io/flux/security/>)
  3. [Contextual Authorization](<https://fluxcd.io/flux/security/contextual-authorization/>)



# Contextual Authorization

Contextual Authorization for securing Flux deployments.

## Introduction

Most cloud providers support context-based authorization, enabling applications to benefit from strong access controls applied to a given context (e.g. Virtual Machine), without the need of managing authentication tokens and credentials.

For example, by granting a given Virtual Machine (or principal that such machine operates under) access to AWS S3, applications running inside that machine can request a token on-demand, which would grant them access to the AWS S3 buckets without having to store long lived credentials anywhere.

By leveraging such capability, Flux users can focus on the big picture, which is access controls enforcement with a least-privileged approach, whilst not having to do deal with security hygiene topics such as encrypting authentication secrets, and ensure they are being rotated regularly. All that is taken care of automatically by the cloud providers, as the tokens provided are context- and time-bound.

## Current Support

Below is a list of Flux features that support this functionality and their documentation:

Status| Component| Feature| Provider| Ref  
---|---|---|---|---  
Supported| Source Controller| GitRepository Authentication| Azure| [Guide](</flux/integrations/azure/>)  
Supported| Source Controller| Bucket Authentication| AWS| [Guide](</flux/integrations/aws/>)  
Supported| Source Controller| Bucket Authentication| Azure| [Guide](</flux/integrations/azure/>)  
Supported| Source Controller| Bucket Authentication| GCP| [Guide](</flux/integrations/gcp/>)  
Supported| Source Controller| OCIRepository Authentication| AWS| [Guide](</flux/integrations/aws/>)  
Supported| Source Controller| OCIRepository Authentication| Azure| [Guide](</flux/integrations/azure/>)  
Supported| Source Controller| OCIRepository Authentication| GCP| [Guide](</flux/integrations/gcp/>)  
Supported| Source Controller| `oci` HelmRepository Authentication| AWS| [Guide](</flux/integrations/aws/>)  
Supported| Source Controller| `oci` HelmRepository Authentication| Azure| [Guide](</flux/integrations/azure/>)  
Supported| Source Controller| `oci` HelmRepository Authentication| GCP| [Guide](</flux/integrations/gcp/>)  
Supported| Kustomize Controller| SOPS Integration with KMS| AWS| [Guide](</flux/integrations/aws/>)  
Supported| Kustomize Controller| SOPS Integration with Key Vault| Azure| [Guide](</flux/integrations/azure/>)  
Supported| Kustomize Controller| SOPS Integration with KMS| GCP| [Guide](</flux/integrations/gcp/>)  
Supported| Kustomize Controller| Remote EKS Cluster Authentication| AWS| [Guide](</flux/integrations/aws/>)  
Supported| Kustomize Controller| Remote AKS Cluster Authentication| Azure| [Guide](</flux/integrations/azure/>)  
Supported| Kustomize Controller| Remote GKE Cluster Authentication| GCP| [Guide](</flux/integrations/gcp/>)  
Supported| Helm Controller| Remote EKS Cluster Authentication| AWS| [Guide](</flux/integrations/aws/>)  
Supported| Helm Controller| Remote AKS Cluster Authentication| Azure| [Guide](</flux/integrations/azure/>)  
Supported| Helm Controller| Remote GKE Cluster Authentication| GCP| [Guide](</flux/integrations/gcp/>)  
Supported| Notification Controller| Azure DevOps Commit Status Updates| Azure| [Guide](</flux/integrations/azure/>)  
Supported| Notification Controller| Azure Event Hubs| Azure| [Guide](</flux/integrations/azure/>)  
Supported| Notification Controller| Google Cloud Pub/Sub| GCP| [Guide](</flux/integrations/gcp/>)  
Supported| Image Reflector Controller| ImageRepository Authentication| AWS| [Guide](</flux/integrations/aws/>)  
Supported| Image Reflector Controller| ImageRepository Authentication| Azure| [Guide](</flux/integrations/azure/>)  
Supported| Image Reflector Controller| ImageRepository Authentication| GCP| [Guide](</flux/integrations/gcp/>)  
Supported| Image Automation Controller| GitRepository Authentication| Azure| [Guide](</flux/integrations/azure/>)  
  
Last modified 2025-09-02: [[RFC-0010] Complete implementation (0dde458c)](<https://github.com/fluxcd/website/commit/0dde458c92985840811f9f359016c79914af09e9>)

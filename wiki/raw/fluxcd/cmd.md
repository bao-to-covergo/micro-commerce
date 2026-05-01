---
source_url: https://fluxcd.io/flux/cmd/
fetched: 2026-05-01
---

  1. [Docs](<https://fluxcd.io/flux/>)
  2. [Flux CLI](<https://fluxcd.io/flux/cmd/>)



# Flux CLI

The Flux Command-Line Interface documentation.

The Flux CLI is available as a binary executable for all major platforms, the binaries can be downloaded from GitHub [releases page](<https://github.com/fluxcd/flux2/releases>).

## Install using package management

  * macOS
  * Linux
  * Windows



With [Homebrew](<https://brew.sh>):
```
brew install fluxcd/tap/flux
```

With [Homebrew](<https://brew.sh>):
```
brew install fluxcd/tap/flux
```

With [yay](<https://github.com/Jguer/yay>):
```
yay -S flux-bin
```

With [nix-env](<https://nixos.org/manual/nix/unstable/command-ref/nix-env.html>):
```
nix-env -i fluxcd
```

With [Chocolatey](<https://chocolatey.org/>):
```
choco install flux
```

With [winget](<https://github.com/microsoft/winget-cli>):
```
winget install -e --id FluxCD.Flux
```

## Install using Bash

To install the latest release on Linux, macOS or Windows WSL:
```
curl -s https://fluxcd.io/install.sh | sudo bash
```

The [install script](<https://raw.githubusercontent.com/fluxcd/flux2/main/install/flux.sh>) does the following:

  * attempts to detect your OS
  * downloads, verifies and unpacks the release tar file in a temporary directory
  * copies the `flux` binary to `/usr/local/bin`
  * removes the temporary directory



You can also install to a custom directory (e.g., `~/.local/bin`):
```
curl -s https://fluxcd.io/install.sh | bash -s ~/.local/bin
```

Please make sure that this directory is part of your `$PATH` environment variable.

To install a specific version, you can set the `FLUX_VERSION` environment variable (e.g., `2.7.0`):
```
curl -s https://fluxcd.io/install.sh | FLUX_VERSION=2.7.0 bash -s ~/.local/bin
```

## Install using Docker

A container image with `kubectl` and `flux` is available on DockerHub and GitHub:

  * `docker.io/fluxcd/flux-cli:<version>`
  * `ghcr.io/fluxcd/flux-cli:<version>`



Example usage:
```
$ docker run -it --entrypoint=sh -v ~/.kube/config:/kubeconfig ghcr.io/fluxcd/flux-cli:v2.7.0
/ # flux check --kubeconfig=kubeconfig
```

## Enable shell autocompletion

To configure your shell to load `flux` [bash completions](</flux/cmd/flux_completion_bash/>) add to your profile:
```
. <(flux completion bash)
```

[`zsh`](</flux/cmd/flux_completion_zsh/>), [`fish`](</flux/cmd/flux_completion_fish/>), and [`powershell`](</flux/cmd/flux_completion_powershell/>) are also supported with their own sub-commands.

## Install using GitHub Actions

To install the latest release on Linux, macOS or Windows GitHub runners:
```
steps:
  - name: Setup Flux CLI
    uses: fluxcd/flux2/action@main
    with:
      version: 'latest'
  - name: Run Flux CLI
    run: flux version --client
```

For more information please see the [Flux GitHub Action documentation](</flux/flux-gh-action/>).

* * *

##### [flux](</flux/cmd/flux/>)

##### [flux bootstrap](</flux/cmd/flux_bootstrap/>)

##### [flux bootstrap bitbucket-server](</flux/cmd/flux_bootstrap_bitbucket-server/>)

##### [flux bootstrap git](</flux/cmd/flux_bootstrap_git/>)

##### [flux bootstrap gitea](</flux/cmd/flux_bootstrap_gitea/>)

##### [flux bootstrap github](</flux/cmd/flux_bootstrap_github/>)

##### [flux bootstrap gitlab](</flux/cmd/flux_bootstrap_gitlab/>)

##### [flux build](</flux/cmd/flux_build/>)

##### [flux build artifact](</flux/cmd/flux_build_artifact/>)

##### [flux build kustomization](</flux/cmd/flux_build_kustomization/>)

##### [flux check](</flux/cmd/flux_check/>)

##### [flux completion](</flux/cmd/flux_completion/>)

##### [flux completion bash](</flux/cmd/flux_completion_bash/>)

##### [flux completion fish](</flux/cmd/flux_completion_fish/>)

##### [flux completion powershell](</flux/cmd/flux_completion_powershell/>)

##### [flux completion zsh](</flux/cmd/flux_completion_zsh/>)

##### [flux create](</flux/cmd/flux_create/>)

##### [flux create alert](</flux/cmd/flux_create_alert/>)

##### [flux create alert-provider](</flux/cmd/flux_create_alert-provider/>)

##### [flux create helmrelease](</flux/cmd/flux_create_helmrelease/>)

##### [flux create image](</flux/cmd/flux_create_image/>)

##### [flux create image policy](</flux/cmd/flux_create_image_policy/>)

##### [flux create image repository](</flux/cmd/flux_create_image_repository/>)

##### [flux create image update](</flux/cmd/flux_create_image_update/>)

##### [flux create kustomization](</flux/cmd/flux_create_kustomization/>)

##### [flux create receiver](</flux/cmd/flux_create_receiver/>)

##### [flux create secret](</flux/cmd/flux_create_secret/>)

##### [flux create secret git](</flux/cmd/flux_create_secret_git/>)

##### [flux create secret githubapp](</flux/cmd/flux_create_secret_githubapp/>)

##### [flux create secret helm](</flux/cmd/flux_create_secret_helm/>)

##### [flux create secret notation](</flux/cmd/flux_create_secret_notation/>)

##### [flux create secret oci](</flux/cmd/flux_create_secret_oci/>)

##### [flux create secret proxy](</flux/cmd/flux_create_secret_proxy/>)

##### [flux create secret receiver](</flux/cmd/flux_create_secret_receiver/>)

##### [flux create secret tls](</flux/cmd/flux_create_secret_tls/>)

##### [flux create source](</flux/cmd/flux_create_source/>)

##### [flux create source bucket](</flux/cmd/flux_create_source_bucket/>)

##### [flux create source chart](</flux/cmd/flux_create_source_chart/>)

##### [flux create source git](</flux/cmd/flux_create_source_git/>)

##### [flux create source helm](</flux/cmd/flux_create_source_helm/>)

##### [flux create source oci](</flux/cmd/flux_create_source_oci/>)

##### [flux create tenant](</flux/cmd/flux_create_tenant/>)

##### [flux debug](</flux/cmd/flux_debug/>)

##### [flux debug helmrelease](</flux/cmd/flux_debug_helmrelease/>)

##### [flux debug kustomization](</flux/cmd/flux_debug_kustomization/>)

##### [flux delete](</flux/cmd/flux_delete/>)

##### [flux delete alert](</flux/cmd/flux_delete_alert/>)

##### [flux delete alert-provider](</flux/cmd/flux_delete_alert-provider/>)

##### [flux delete helmrelease](</flux/cmd/flux_delete_helmrelease/>)

##### [flux delete image](</flux/cmd/flux_delete_image/>)

##### [flux delete image policy](</flux/cmd/flux_delete_image_policy/>)

##### [flux delete image repository](</flux/cmd/flux_delete_image_repository/>)

##### [flux delete image update](</flux/cmd/flux_delete_image_update/>)

##### [flux delete kustomization](</flux/cmd/flux_delete_kustomization/>)

##### [flux delete receiver](</flux/cmd/flux_delete_receiver/>)

##### [flux delete source](</flux/cmd/flux_delete_source/>)

##### [flux delete source bucket](</flux/cmd/flux_delete_source_bucket/>)

##### [flux delete source chart](</flux/cmd/flux_delete_source_chart/>)

##### [flux delete source git](</flux/cmd/flux_delete_source_git/>)

##### [flux delete source helm](</flux/cmd/flux_delete_source_helm/>)

##### [flux delete source oci](</flux/cmd/flux_delete_source_oci/>)

##### [flux diff](</flux/cmd/flux_diff/>)

##### [flux diff artifact](</flux/cmd/flux_diff_artifact/>)

##### [flux diff kustomization](</flux/cmd/flux_diff_kustomization/>)

##### [flux envsubst](</flux/cmd/flux_envsubst/>)

##### [flux events](</flux/cmd/flux_events/>)

##### [flux export](</flux/cmd/flux_export/>)

##### [flux export alert](</flux/cmd/flux_export_alert/>)

##### [flux export alert-provider](</flux/cmd/flux_export_alert-provider/>)

##### [flux export artifact](</flux/cmd/flux_export_artifact/>)

##### [flux export artifact generator](</flux/cmd/flux_export_artifact_generator/>)

##### [flux export helmrelease](</flux/cmd/flux_export_helmrelease/>)

##### [flux export image](</flux/cmd/flux_export_image/>)

##### [flux export image policy](</flux/cmd/flux_export_image_policy/>)

##### [flux export image repository](</flux/cmd/flux_export_image_repository/>)

##### [flux export image update](</flux/cmd/flux_export_image_update/>)

##### [flux export kustomization](</flux/cmd/flux_export_kustomization/>)

##### [flux export receiver](</flux/cmd/flux_export_receiver/>)

##### [flux export source](</flux/cmd/flux_export_source/>)

##### [flux export source bucket](</flux/cmd/flux_export_source_bucket/>)

##### [flux export source chart](</flux/cmd/flux_export_source_chart/>)

##### [flux export source external](</flux/cmd/flux_export_source_external/>)

##### [flux export source git](</flux/cmd/flux_export_source_git/>)

##### [flux export source helm](</flux/cmd/flux_export_source_helm/>)

##### [flux export source oci](</flux/cmd/flux_export_source_oci/>)

##### [flux get](</flux/cmd/flux_get/>)

##### [flux get alert-providers](</flux/cmd/flux_get_alert-providers/>)

##### [flux get alerts](</flux/cmd/flux_get_alerts/>)

##### [flux get all](</flux/cmd/flux_get_all/>)

##### [flux get artifacts](</flux/cmd/flux_get_artifacts/>)

##### [flux get artifacts generators](</flux/cmd/flux_get_artifacts_generators/>)

##### [flux get helmreleases](</flux/cmd/flux_get_helmreleases/>)

##### [flux get images](</flux/cmd/flux_get_images/>)

##### [flux get images all](</flux/cmd/flux_get_images_all/>)

##### [flux get images policy](</flux/cmd/flux_get_images_policy/>)

##### [flux get images repository](</flux/cmd/flux_get_images_repository/>)

##### [flux get images update](</flux/cmd/flux_get_images_update/>)

##### [flux get kustomizations](</flux/cmd/flux_get_kustomizations/>)

##### [flux get receivers](</flux/cmd/flux_get_receivers/>)

##### [flux get sources](</flux/cmd/flux_get_sources/>)

##### [flux get sources all](</flux/cmd/flux_get_sources_all/>)

##### [flux get sources bucket](</flux/cmd/flux_get_sources_bucket/>)

##### [flux get sources chart](</flux/cmd/flux_get_sources_chart/>)

##### [flux get sources external](</flux/cmd/flux_get_sources_external/>)

##### [flux get sources git](</flux/cmd/flux_get_sources_git/>)

##### [flux get sources helm](</flux/cmd/flux_get_sources_helm/>)

##### [flux get sources oci](</flux/cmd/flux_get_sources_oci/>)

##### [flux install](</flux/cmd/flux_install/>)

##### [flux list](</flux/cmd/flux_list/>)

##### [flux list artifacts](</flux/cmd/flux_list_artifacts/>)

##### [flux logs](</flux/cmd/flux_logs/>)

##### [flux migrate](</flux/cmd/flux_migrate/>)

##### [flux pull](</flux/cmd/flux_pull/>)

##### [flux pull artifact](</flux/cmd/flux_pull_artifact/>)

##### [flux push](</flux/cmd/flux_push/>)

##### [flux push artifact](</flux/cmd/flux_push_artifact/>)

##### [flux reconcile](</flux/cmd/flux_reconcile/>)

##### [flux reconcile helmrelease](</flux/cmd/flux_reconcile_helmrelease/>)

##### [flux reconcile image](</flux/cmd/flux_reconcile_image/>)

##### [flux reconcile image policy](</flux/cmd/flux_reconcile_image_policy/>)

##### [flux reconcile image repository](</flux/cmd/flux_reconcile_image_repository/>)

##### [flux reconcile image update](</flux/cmd/flux_reconcile_image_update/>)

##### [flux reconcile kustomization](</flux/cmd/flux_reconcile_kustomization/>)

##### [flux reconcile receiver](</flux/cmd/flux_reconcile_receiver/>)

##### [flux reconcile source](</flux/cmd/flux_reconcile_source/>)

##### [flux reconcile source bucket](</flux/cmd/flux_reconcile_source_bucket/>)

##### [flux reconcile source chart](</flux/cmd/flux_reconcile_source_chart/>)

##### [flux reconcile source git](</flux/cmd/flux_reconcile_source_git/>)

##### [flux reconcile source helm](</flux/cmd/flux_reconcile_source_helm/>)

##### [flux reconcile source oci](</flux/cmd/flux_reconcile_source_oci/>)

##### [flux resume](</flux/cmd/flux_resume/>)

##### [flux resume alert](</flux/cmd/flux_resume_alert/>)

##### [flux resume alert-provider](</flux/cmd/flux_resume_alert-provider/>)

##### [flux resume helmrelease](</flux/cmd/flux_resume_helmrelease/>)

##### [flux resume image](</flux/cmd/flux_resume_image/>)

##### [flux resume image policy](</flux/cmd/flux_resume_image_policy/>)

##### [flux resume image repository](</flux/cmd/flux_resume_image_repository/>)

##### [flux resume image update](</flux/cmd/flux_resume_image_update/>)

##### [flux resume kustomization](</flux/cmd/flux_resume_kustomization/>)

##### [flux resume receiver](</flux/cmd/flux_resume_receiver/>)

##### [flux resume source](</flux/cmd/flux_resume_source/>)

##### [flux resume source bucket](</flux/cmd/flux_resume_source_bucket/>)

##### [flux resume source chart](</flux/cmd/flux_resume_source_chart/>)

##### [flux resume source git](</flux/cmd/flux_resume_source_git/>)

##### [flux resume source helm](</flux/cmd/flux_resume_source_helm/>)

##### [flux resume source oci](</flux/cmd/flux_resume_source_oci/>)

##### [flux stats](</flux/cmd/flux_stats/>)

##### [flux suspend](</flux/cmd/flux_suspend/>)

##### [flux suspend alert](</flux/cmd/flux_suspend_alert/>)

##### [flux suspend alert-provider](</flux/cmd/flux_suspend_alert-provider/>)

##### [flux suspend helmrelease](</flux/cmd/flux_suspend_helmrelease/>)

##### [flux suspend image](</flux/cmd/flux_suspend_image/>)

##### [flux suspend image policy](</flux/cmd/flux_suspend_image_policy/>)

##### [flux suspend image repository](</flux/cmd/flux_suspend_image_repository/>)

##### [flux suspend image update](</flux/cmd/flux_suspend_image_update/>)

##### [flux suspend kustomization](</flux/cmd/flux_suspend_kustomization/>)

##### [flux suspend receiver](</flux/cmd/flux_suspend_receiver/>)

##### [flux suspend source](</flux/cmd/flux_suspend_source/>)

##### [flux suspend source bucket](</flux/cmd/flux_suspend_source_bucket/>)

##### [flux suspend source chart](</flux/cmd/flux_suspend_source_chart/>)

##### [flux suspend source git](</flux/cmd/flux_suspend_source_git/>)

##### [flux suspend source helm](</flux/cmd/flux_suspend_source_helm/>)

##### [flux suspend source oci](</flux/cmd/flux_suspend_source_oci/>)

##### [flux tag](</flux/cmd/flux_tag/>)

##### [flux tag artifact](</flux/cmd/flux_tag_artifact/>)

##### [flux trace](</flux/cmd/flux_trace/>)

##### [flux tree](</flux/cmd/flux_tree/>)

##### [flux tree artifact](</flux/cmd/flux_tree_artifact/>)

##### [flux tree artifact generator](</flux/cmd/flux_tree_artifact_generator/>)

##### [flux tree kustomization](</flux/cmd/flux_tree_kustomization/>)

##### [flux uninstall](</flux/cmd/flux_uninstall/>)

##### [flux version](</flux/cmd/flux_version/>)

Last modified 2025-10-01: [Update version in the Flux CLI install doc (4d5558a1)](<https://github.com/fluxcd/website/commit/4d5558a129059fea698d3a3ead7901768dfa8618>)

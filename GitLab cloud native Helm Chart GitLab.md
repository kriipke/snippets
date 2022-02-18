# GitLab cloud native Helm Chart | GitLab
This is the official, recommended, and supported method to install GitLab on a cloud native environment.

## Introduction[](#introduction "Permalink")

The `gitlab/gitlab` chart is the best way to operate GitLab on Kubernetes. This chart contains all the required components to get started, and can scale to large deployments.

This chart includes all the components for a complete experience, but each part can be installed separately.

-   Core GitLab components:
    -   [NGINX Ingress](https://docs.gitlab.com/charts/charts/nginx/index.html)
    -   [Registry](https://docs.gitlab.com/charts/charts/registry/index.html)
    -   GitLab/[Gitaly](https://docs.gitlab.com/charts/charts/gitlab/gitaly/index.html)
    -   GitLab/[GitLab Exporter](https://docs.gitlab.com/charts/charts/gitlab/gitlab-exporter/index.html)
    -   GitLab/[GitLab Grafana](https://docs.gitlab.com/charts/charts/gitlab/gitlab-grafana/index.html)
    -   GitLab/[GitLab Pages](https://docs.gitlab.com/charts/charts/gitlab/gitlab-pages/index.html)
    -   GitLab/[GitLab Shell](https://docs.gitlab.com/charts/charts/gitlab/gitlab-shell/index.html)
    -   GitLab/[Mailroom](https://docs.gitlab.com/charts/charts/gitlab/mailroom/index.html)
    -   GitLab/[Migrations](https://docs.gitlab.com/charts/charts/gitlab/migrations/index.html)
    -   GitLab/[Sidekiq](https://docs.gitlab.com/charts/charts/gitlab/sidekiq/index.html)
    -   GitLab/[Toolbox](https://docs.gitlab.com/charts/charts/gitlab/toolbox/index.html)
    -   GitLab/[Webservice](https://docs.gitlab.com/charts/charts/gitlab/webservice/index.html)
-   Optional dependencies:
    -   [PostgreSQL](https://artifacthub.io/packages/helm/bitnami/postgresql)
    -   [Redis](https://artifacthub.io/packages/helm/bitnami/redis)
    -   [MinIO](https://docs.gitlab.com/charts/charts/minio/index.html)
-   Optional additions:
    -   [Prometheus](https://artifacthub.io/packages/helm/prometheus-community/prometheus)
    -   [Grafana](https://artifacthub.io/packages/helm/grafana/grafana)
    -   [_Unprivileged_](https://docs.gitlab.com/runner/install/kubernetes.html#running-docker-in-docker-containers-with-gitlab-runner) [GitLab Runner](https://docs.gitlab.com/runner/) using the Kubernetes executor
    -   Automatically provisioned SSL via [Let’s Encrypt](https://letsencrypt.org/), using [Jetstack](https://www.jetstack.io/)’s [cert-manager](https://cert-manager.io/docs/)
    -   GitLab/[Praefect](https://docs.gitlab.com/charts/charts/gitlab/praefect/index.html)
    -   GitLab/[GitLab Agent Server (KAS)](https://docs.gitlab.com/charts/charts/gitlab/kas/index.html)

## GitLab Helm chart quick start guide[](#gitlab-helm-chart-quick-start-guide "Permalink")

For those looking to get up and running with these charts as fast as possible, in a _non-production_ use case, we provide a [Quick Start Guide](https://docs.gitlab.com/charts/quickstart/index.html) for Proof of Concept (PoC) deployments.

This guide walks the user through deploying these charts with default values & features, but _does not_ meet production ready requirements. If you wish to deploy these charts into production under sustained load, you should follow the complete [Installation guide](#installation) below.

## Troubleshooting[](#troubleshooting "Permalink")

We’ve done our best to make these charts as seamless as possible, but occasionally troubles do surface outside of our control. We’ve collected tips and tricks for troubleshooting common issues. Please examine these first before raising an [Issue](https://gitlab.com/gitlab-org/charts/gitlab/-/issues), and feel free to add to them by raising a [Merge Request](https://gitlab.com/gitlab-org/charts/gitlab/-/merge_requests)!

See [Troubleshooting](https://docs.gitlab.com/charts/troubleshooting/index.html).

## Installation[](#installation "Permalink")

The `gitlab/gitlab` chart contains all required dependencies. In production, you may want to enable optional features or [advanced configuration](#advanced-configuration). This guide walks all of the options and features of these charts in great depth.

If you are just looking to deploy a Proof of Concept for testing, we strongly suggest you follow our [Quick Start](https://docs.gitlab.com/charts/quickstart/index.html) for your first iteration.

1.  [Preparation](https://docs.gitlab.com/charts/installation/index.html)
2.  [Deployment](https://docs.gitlab.com/charts/installation/deployment.html)

### Global settings[](#global-settings "Permalink")

The complexity of these charts lends themselves to the use of global properties. There are many common global settings that apply to multiple charts. See the [Globals documentation](https://docs.gitlab.com/charts/charts/globals.html) for details on the different global configuration values and their application.

### Complete properties list[](#complete-properties-list "Permalink")

We’re often asked to put a table of all possible properties directly into this index. These charts are _massive_ in scale, and as such the number of properties exceeds the amount of context we’re comfortable placing here. Please see our (nearly) [comprehensive list of properties and defaults](https://docs.gitlab.com/charts/installation/command-line-options.html).

## Upgrading[](#upgrading "Permalink")

Please see [Upgrading](https://docs.gitlab.com/charts/installation/upgrade.html).

## Uninstall[](#uninstall "Permalink")

To uninstall the GitLab Chart, run the following:

For the purposes of continuity, these charts have some Kubernetes objects that are not removed when performing `helm uninstall`. These are items we require you to _consciously_ remove them, as they affect re-deployment should you choose to.

-   PVCs for stateful data, which you must _consciously_ remove
    -   Gitaly: This is your repository data.
    -   PostgreSQL (if internal): This is your metadata.
    -   Redis (if internal): This is cache & job queues, which can be safely removed.
-   Secrets, if generated by our shared-secrets Job. These charts are designed to never generate Kubernetes Secrets via Helm directly. As such, Helm can’t remove them. They contain passwords, encryption secrets, and so on. They should not be callously destroyed.
-   ConfigMaps
    -   `ingress-controller-leader-RELEASE-nginx`: This is generated by the NGINX Ingress controller itself, and is outside the control of our chart. It can be safely removed.

The PVCs and Secrets have the `release` label set, so you can find these with:

    kubectl get pvc,secret -lrelease=gitlab 

## Advanced[](#advanced "Permalink")

Beyond the basic deployments of GitLab in cloud native environments, more complex configuration is possible. This section serves a guide point for those tasks which require further planning, such as large scale deployments or migrating from the Omnibus GitLab.

### Advanced Configuration[](#advanced-configuration "Permalink")

Advanced and large scale deployments have the ability to make use of external services, extended functionality, and alternate providers.

Examples of advanced configurations:

-   GitLab Geo
-   External object storage providers
-   External PostgreSQL, Redis, Gitaly
-   External Ingress providers

See [Advanced Configuration](https://docs.gitlab.com/charts/advanced/index.html).

### Migrate from Omnibus GitLab to Kubernetes[](#migrate-from-omnibus-gitlab-to-kubernetes "Permalink")

It is possible to migrate from [Omnibus GitLab](https://docs.gitlab.com/omnibus/) to these charts. Doing so generally requires migrating existing data to object storage, and thus is an [Advanced Configuration](https://docs.gitlab.com/charts/advanced/index.html).

To migrate your existing Omnibus GitLab instance to these charts, follow the [migration documentation](https://docs.gitlab.com/charts/installation/migration/index.html).

## Architecture[](#architecture "Permalink")

These charts are complex, as they coordinate the deployment of a complete application suite. We provide [documentation](https://docs.gitlab.com/charts/architecture/index.html) of the goals, structure, design decisions, and resource consumption.

## Development[](#development "Permalink")

For those interested in contributing to these charts, we provide development guidelines covering the gamut of working with this project. They can be found under [development](https://docs.gitlab.com/charts/development/index.html).

### GitLab version mappings[](#gitlab-version-mappings "Permalink")

The GitLab chart doesn’t have the same version number as GitLab itself. Breaking changes are anticipated that may have to be introduced to the chart that would warrant a major version bump, and the requirement for these changes could completely block other development on these charts until completed.

To quickly see the full list of the `gitlab` chart versions and the GitLab version they map to, issue the following command with [Helm](https://docs.gitlab.com/charts/installation/tools.html#helm):

    helm repo add gitlab https://charts.gitlab.io/
    helm search repo -l gitlab/gitlab 

For more information, visit the [version mappings docs](https://docs.gitlab.com/charts/installation/version_mappings.html).

### Contributing[](#contributing "Permalink")

See the [developer documentation](https://docs.gitlab.com/charts/development/index.html) to learn how to contribute to the GitLab charts, in addition to our [Contributing Guidelines](https://gitlab.com/gitlab-org/charts/gitlab/tree/master/CONTRIBUTING.md). 
 [https://docs.gitlab.com/charts/#installation](https://docs.gitlab.com/charts/#installation)

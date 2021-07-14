# With ArgoCD \(Recommended\)

## Introduction

We recommend using [ArgoCD ](https://argoproj.github.io/argo-cd/)to deploy the Granary platform. For this purpose we provide a sample that bundles the Granary components and provides easy integration into an ArgoCD based gitops pipeline. 

## Additional Prerequisites

* An ArgoCD instance is configured to be able to deploy to your cluster
* Credentials to access [https://hub.syncier.cloud/chartrepo/grnry](./#helm-chart-repositories) are registered in ArgoCD
* A git repository for the gitops process is set up and ArgoCD has access rights.
* Grnry Docker registry and Artifactory credentials have been provided to you 

## Installation Steps

* Copy the 1.0 installation sample from [https://github.com/syncier/grnry-samples/tree/master/installations/argocd-1.0](https://github.com/syncier/grnry-samples/tree/master/installations/argocd-1.0) to your gitops repository.
* Add a "Application" to your ArgoCD that points to the `argo_project` subdirectory of the copied directory.
* Add the docker registry and artifactory credentials as a secret to the secrets subdirectory or deploy it manually
  * default expected names are `grnry-dockerconfig` and `artifactory-release-credentials` ; this can be changed but values files of components have to be modified accordingly
* Configure the [Granary base deployment](with-helm/granary-base-deployment.md) values \(`granary/grnryBaseValues.yaml`, see the Helm chart's README for detailed instructions\)
  * Additionally you need to adjust a `namespace` value according to your environment in
    * `granary/grafanaValues.yaml`
    * `granary/grafanaDashboards.yaml`
  * Set a [Keycloak client secret](https://www.keycloak.org/docs/latest/securing_apps/#_client_authentication_adapter) to be used for authentication by Granary components
* Add a postgres user and the database name as a secret to the secrets subdirectory or deploy it manually
* Adjust the values files of all components according to your ingress provider and FQDN.
* Commit these changes and wait for ArgoCD to deploy the components in order.

## Maintenance and advanced Configuration

For ongoing maintenance and more control over the configuration of your granary installation, you should check out `granary/values.yaml`. It contains a `components` section where all the Helm chart sources and versions of the different components are defined. These can be adjusted to perform upgrades to individual cluster components if necessary.

For more advanced configuration of an individual component, you should have a look at the respective Helm chart. It describes in greater detail which options can be set to fine tune behaviour or performance.


# With ArgoCD \(Recommended\)

## Introduction

We recommend using [ArgoCD ](https://argoproj.github.io/argo-cd/)to deploy the granary-platform. For this purpose we provide a sample that bundles the granary components and provides easy integration into a ArgoCD based gitops pipeline. 

## Additional Prerequisites

* An ArgoCD instance is configured to be able to deploy to your cluster
* Credentials to access https://hub.syncier.cloud/chartrepo/grnry are registered in ArgoCD
* A git repository for the gitops process is set up and ArgoCD has access rights.
* Grnry Docker registry and Artifactory credentials have been provided to you 

## Installation Steps

* Copy the 0.9 installation sample from [https://github.com/syncier/grnry-samples/tree/master/installations/argocd-0.9](https://github.com/syncier/grnry-samples/tree/master/installations/argocd-0.9) to your gitops repository.
* Add a "Application" to your ArgoCD that points to the `argo_project` subdirectory of the copied directory.
* Add the docker registry and artifactory credentials as a secret to the secrets subdirectory.
  * default expected names are `grnry-dockerconfig` and `artifactory-release-credentials` ; this can be changed but values files of components have to be modified accordingly
* Configure the granary base deployment values \(granary/grnryBaseValues.yaml , the helm chart README for detailed instructions\)
  * Additionaly you need to adjust a `namespace` value according to your environment en
    * `granary/citusValues.yaml`
    * `granary/grafanaValues.yaml`
    * `granary/grafanaDashboards.yaml`
  * Set a keycloak clientSecret to be used for authentication by components
* Adjust the values files of the components according to your ingress and FQDN .
* Commit these changes and wait for ArgoCD to deploy the components in order.

## Maintenance and advanced Configuration

For ongoing maintenance and more control over the configuration of your granary installation you should check the `granary/values.yaml` . It contains a `components` section where all the sources and versions of the different components are defined. These can be adjusted to perform upgrades to individual cluster components if necessary.

For more advanced configuration of an individual component you should have a look at the respective helm chart. It will describe in greater detail which options can be set to fine tune behaviour or performance.


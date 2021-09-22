# LeanIX Kubernetes Run Action

This action provides a standard way for the Krane run command. The run process uses azure-cli to access the cluster and
kubernetes-shopify (Krane) to run a task. All configuration for the deployment is provided using action input
variables. The podtemplate for the task needs to be defined in the target namespace.

https://github.com/Shopify/krane#prerequisites-1
https://github.com/leanix/k8s-deploy-action

## Usage

This action will use the injected secrets within the GitHub workflow environment to access a kubernetes cluster for deployment.
Use in advance the provided [leanix/secrets-action](https://github.com/leanix/secrets-action) to inject the required secrets.

A simple run definition would look like:
```yaml
- name: Use k8s run action
  uses: leanix/k8s-run-action@master
  with:
    namespace: myapp1
    environment: test
    template: mypodtemplate
```

| input | required | default | description |
|-------|----------|---------|-------------|
|namespace|yes|-|The kubernetes namespace to use.|
|environment|yes|test|The cluster environment to use (e.g. test or prod)|
|template_directory|yes|k8s-deployment|Directory that contains the manifests to deploy. [see](#kubernetes-manifests)|
|vault_secret_keys|no|-|List of Azure Key Vault secret names to inject as k8s secrets. [see](#kubernetes-secrets-from-azure-key-vault)|
|dry_run|no|-|Set to true for dry run mode: Only validate and render the templates, not actually deploying to any cluster|
|disable_validation|no|-|Set to true for disabling the validate and render the templates option during the deployment.|

### Configure deployment target

Normally you must only define the namespace, that was provisioned for you and the action will find out, which clusters
your namespace is registered for:

* `namespace`            Namespace to use on that cluster

Sometimes it may be necessary to add one of the following optional filters to only deploy to a subset of clusters:

* `environment`          Only deploy to a specific environment (e.g. prod or test)
* `region`               Only deploy to a specific region (e.g. westeurope)
* `cluster`              Only deploy to a specific cluster (e.g. aks-westeurope-cluster-name)
* `cluster_tag`          Only deploy to a cluster with a specific tag (e.g. router)

## Requires
This action requires following GitHub actions in advance:
- [leanix/secrets-action@master](https://github.com/leanix/secrets-action)

## See also
This action uses [leanix-k8s-deploy](https://github.com/leanix/leanix-k8s-deploy).

## Copyright and license

Copyright 2020 LeanIX GmbH under the [Unlicense license](LICENSE).

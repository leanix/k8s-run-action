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
    template: mypodtemplate
```

| input | required | default | description |
|-------|----------|---------|-------------|
|namespace|yes|-|The kubernetes namespace to use.|
|template|yes|-|Name of the PodTemplate to run.|
|timeout|yes|5m|Global timeout for the krane run command. ie. 30m, 1h, ...|
|environment|yes|test|The cluster environment to use (e.g. test or prod)|
|region|no|-|The region defining the cluster to deploy to, e.g. westeurope or australiaeast. Leave empty to deploy to all regions.|
|environmentVariables|no|-|Comma separated list of variables to pass to krane run. ie: VAR=val,FOO=bar|

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

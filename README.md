# Istio multicluster & multiprimary with Sail Operator - OpenShift
> :mag: These use cases have been tested on OpenShift 4.14 with MetalLB.

Use cases:

1. [Istio multicluster \& multiprimary](#scenario-1-istio-multicluster--multiprimary-on-openshift-cluster-domain-set-as-clusterlocal)
2. [Istio multicluster \& multiprimary with different cluster domain](#scenario-2-istio-multicluster--multiprimary-on-openshift-cluster-domain-different-per-cluster-exposing-all-services-automatically)
3. [Istio multicluster \& multiprimary. Custom service discovery instead of automatic service discovery](#scenario-3-istio-multicluster--multiprimary-on-openshift-cluster-domain-different-per-cluster-adding-custom-services-instead-of-automatic-service-discovery)


## Scenario 1: Istio multicluster & multiprimary on OpenShift. Cluster domain set as _cluster.local_

In this scenario, Istio is installed via [Sail Operator](https://github.com/maistra/istio-operator/blob/maistra-3.0/bundle/README.md). The deployment model is [Multi-Primary on different networks](https://istio.io/latest/docs/setup/install/multicluster/multi-primary_multi-network/).

Follow the specific [README](./docs/multicluster-multiprimary-sail-metallb-scenario-1.md) for this scenario.

## Scenario 2: Istio multicluster & multiprimary on OpenShift. Cluster domain different per cluster. Exposing all services automatically

In this scenario, Istio is installed via [Sail Operator](https://github.com/maistra/istio-operator/blob/maistra-3.0/bundle/README.md). The deployment model is [Multi-Primary on different networks](https://istio.io/latest/docs/setup/install/multicluster/multi-primary_multi-network/).

In this scenario, the cluster domain is different per cluster:

- **cluster1** domain: cluster1.local
- **cluster2** domain: cluster2.local

> :warning: **With this setup, both cluster domains should be considered the same as the trustdomain**: You can **not** differentiate the cluster domain when using _spiffe_. For instance, by applying an AuthorizationPolicy, you can not trust only a cluster, both are trusted. See the following [issue](https://github.com/istio/istio/issues/39204) for more information.

Follow the specific [README](./docs/multicluster-multiprimary-sail-metallb-scenario-2.md) for this scenario.

## Scenario 3: Istio multicluster & multiprimary on OpenShift. Cluster domain different per cluster. Adding custom services instead of automatic service discovery

In this scenario, Istio is installed via [Sail Operator](https://github.com/maistra/istio-operator/blob/maistra-3.0/bundle/README.md). The deployment model is [Multi-Primary on different networks](https://istio.io/latest/docs/setup/install/multicluster/multi-primary_multi-network/).

In this scenario, the cluster domain is different per cluster:

- **cluster1** domain: cluster1.local
- **cluster2** domain: cluster2.local

Each custom service is added to the cluster by using the following Istio resources:
* [Service Entry](https://istio.io/latest/docs/reference/config/networking/service-entry/)
* [Workload Entry](https://istio.io/latest/docs/reference/config/networking/workload-entry/)
* [Gateway](https://istio.io/latest/docs/reference/config/networking/gateway/)

> :warning: **With this setup, only the _spiffe id_ used in the Istio resources is trusted**: In this use case, you can differentiate the cluster domain when using _spiffe_.

Follow the specific [README](./docs/multicluster-multiprimary-sail-metallb-scenario-3.md) for this scenario.

## Author

Fran Perea Rodríguez @RedHat
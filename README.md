# Istio multicluster & multiprimary with Sail Operator - OpenShift
:mag: These use cases have been tested on OpenShift with MetalLB.

Use cases:

1. [Istio multicluster \& multiprimary](#scenario-1-istio-multicluster--multiprimary-on-openshift-cluster-domain-set-as-clusterlocal)
2. [Istio multicluster \& multiprimary with different cluster domain](#scenario-2-istio-multicluster--multiprimary-on-openshift-cluster-domain-different-per-cluster)
3. [Istio multicluster \& multiprimary. Custom service discovery instead of automatic service discovery](#scenario-3-istio-multicluster--multiprimary-on-openshift-cluster-domain-different-per-cluster-adding-custom-services-instead-of-automatic-service-discovery)


## Scenario 1: Istio multicluster & multiprimary on OpenShift. Cluster domain set as _cluster.local_

In this scenario, Istio is installed via [Sail Operator](https://github.com/maistra/istio-operator/blob/maistra-3.0/bundle/README.md). The deployment model is [Multi-Primary on different networks](https://istio.io/latest/docs/setup/install/multicluster/multi-primary_multi-network/).

Follow the specific [README](./docs/multicluster-multiprimary-sail-metallb.md) for this scenario.

## Scenario 2: Istio multicluster & multiprimary on OpenShift. Cluster domain different per cluster.

In this scenario, Istio is installed via [Sail Operator](https://github.com/maistra/istio-operator/blob/maistra-3.0/bundle/README.md). The deployment model is [Multi-Primary on different networks](https://istio.io/latest/docs/setup/install/multicluster/multi-primary_multi-network/).

In this scenario, the cluster domain is different per cluster:

- **cluster1** domain: cluster1.local
- **cluster2** domain: cluster2.local

Follow the specific [README](./docs/multicluster-multiprimary-sail-metallb-different-domain.md) for this scenario.

## Scenario 3: Istio multicluster & multiprimary on OpenShift. Cluster domain different per cluster. Adding custom services instead of automatic service discovery.

In this scenario, Istio is installed via [Sail Operator](https://github.com/maistra/istio-operator/blob/maistra-3.0/bundle/README.md). The deployment model is [Multi-Primary on different networks](https://istio.io/latest/docs/setup/install/multicluster/multi-primary_multi-network/).

In this scenario, the cluster domain is different per cluster:

- **cluster1** domain: cluster1.local
- **cluster2** domain: cluster2.local

Each custom service is added to the cluster by using the following Istio resources:
* [Service Entry](https://istio.io/latest/docs/reference/config/networking/service-entry/)
* [Workload Entry](https://istio.io/latest/docs/reference/config/networking/workload-entry/)
* [Gateway](https://istio.io/latest/docs/reference/config/networking/gateway/)

Follow the specific [README](./docs/multicluster-multiprimary-sail-metallb-custom-service.md) for this scenario.

## Author

Fran Perea Rodríguez @RedHat
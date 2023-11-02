# Istio multicluster & multiprimary with Sail Operator - OpenShift

## Scenario 1: Istio multicluster & multiprimary on OpenShift (vSphere) with MetalLB. Cluster domain set as _cluster.local_

In this scenario, Istio is installed via [Sail Operator](https://github.com/maistra/istio-operator/blob/maistra-3.0/bundle/README.md). The deployment model is [Multi-Primary on different networks](https://istio.io/latest/docs/setup/install/multicluster/multi-primary_multi-network/).

Follow the specific [README](./docs/multicluster-multiprimary-sail-metallb.md) for this scenario.

## Scenario 2: Istio multicluster & multiprimary on OpenShift (vSphere) with MetalLB. Cluster domain different per cluster.

In this scenario, Istio is installed via [Sail Operator](https://github.com/maistra/istio-operator/blob/maistra-3.0/bundle/README.md). The deployment model is [Multi-Primary on different networks](https://istio.io/latest/docs/setup/install/multicluster/multi-primary_multi-network/).

In the [Scenario 1](#scenario-1-istio-multicluster--multiprimary-on-openshift-vsphere-with-metallb-cluster-domain-set-as-clusterlocal), the cluster domain used is the default _cluster.local_. In this scenario, the cluster domain is different per cluster:

- **cluster1** domain: cluster1.local
- **cluster2** domain: cluster2.local

Follow the specific [README](./docs/multicluster-multiprimary-sail-metallb-different-domain.md) for this scenario.
# Istio multicluster & multiprimary with Sail Operator - OpenShift

## Sail Operator

How install Sail Operator.

## Installing Istio

Create OCP project:
```bash
oc new-project istio-system
oc new-project istio-gateways
```

- Cluster1
```bash
oc label namespace istio-system topology.istio.io/network=cluster1-network
```

- Cluster2
```bash
oc label namespace istio-system topology.istio.io/network=cluster2-network
```

Create CA and certificates.Open a new terminal, clone the Istio [repository](https://github.com/istio/istio) and go to _istio_ folder (new cloned repo). The steps under [_Create certificates_](#create-certificates) section must be executed from _istio_ folder.
```bash
mkdir certs
pushd certs
make -f ../tools/certs/Makefile.selfsigned.mk root-ca
make -f ../tools/certs/Makefile.selfsigned.mk cluster1-cacerts
make -f ../tools/certs/Makefile.selfsigned.mk cluster2-cacerts
```

Create the Istio secret in both clusters:

- Cluster1
```bash
oc create secret generic cacerts -n istio-system \
      --from-file=cluster1/ca-cert.pem \
      --from-file=cluster1/ca-key.pem \
      --from-file=cluster1/root-cert.pem \
      --from-file=cluster1/cert-chain.pem
```

- Cluster2
```bash
oc create secret generic cacerts -n istio-system \
      --from-file=cluster2/ca-cert.pem \
      --from-file=cluster2/ca-key.pem \
      --from-file=cluster2/root-cert.pem \
      --from-file=cluster2/cert-chain.pem
```

```bash
popd
```


Install Istio by executing:

- Cluster1
```bash
oc -n istio-system apply -f 0-istio-setup/istio-cluster1.yaml
```

- Cluster2
```bash
oc -n istio-system apply -f 0-istio-setup/istio-cluster2.yaml
```

## Deploy East-West gateways in both clusters

- Cluster1:
```bash
helm install istio-eastwestgateway istio/gateway --set networkGateway=cluster1-network -n istio-gateways  -f 1-multicluster/openshift-values.yaml
```

- Cluster2:
```bash
helm install istio-eastwestgateway istio/gateway --set networkGateway=cluster2-network -n istio-gateways  -f 1-multicluster/openshift-values.yaml
```

### Configure multicluster-multiprimary

Both clusters:
```bash
oc apply -f 1-multicluster/gw.yaml
```

- Cluster1:
```bash
istioctl x create-remote-secret --name=cluster1 > 1-multicluster/remote-secret-cluster2.yaml
```

- Cluster2:
```bash
istioctl x create-remote-secret --name=cluster2 > 1-multicluster/remote-secret-cluster1.yaml
```

- Cluster1:
```bash
oc apply -f 1-multicluster/remote-secret-cluster1.yaml
```

- Cluster2:
```bash
oc apply -f 1-multicluster/remote-secret-cluster2.yaml
```

## Deploying the helloworld sample application

Create the _my-awesome-project_ OCP project:
```bash
oc new-project my-awesome-project
```

Since we are using [Automatic Sidecar Injection](https://istio.io/latest/docs/setup/additional-setup/sidecar-injection/#automatic-sidecar-injection), label the _my-awesome-project_ OCP project:

```bash
oc label namespace my-awesome-project istio-injection=enabled
```

Deploy the _helloworld_ application:
```bash
oc apply -f https://raw.githubusercontent.com/istio/istio/master/samples/helloworld/helloworld.yaml -l version=v1 -n my-awesome-project
oc apply -f https://raw.githubusercontent.com/istio/istio/master/samples/helloworld/helloworld.yaml -l version=v2 -n my-awesome-project
oc apply -f https://raw.githubusercontent.com/istio/istio/master/samples/helloworld/helloworld.yaml -l service=helloworld -n my-awesome-project
```

Deploy the _sleep_ application:
```bash
oc apply -f 2-data-plane/sleep-app/deploy.yaml
```

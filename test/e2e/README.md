# End-to-End (E2E) Tests

This directory contains the E2E test suite for the Coraza Kubernetes Operator.

Unlike the `/test/integration` suite, which typically focuses purely on the Operator's control-plane behavior (e.g. checking that ConfigMap updates cause RuleSets to reconcile, or asserting that Engine Gateways are found), the E2E tests explicitly validate the **data plane**.

The tests programmatically deploy `Gateways`, Coraza `Engines`, `RuleSets`, and backend dummy applications, and send real HTTP traffic using port-forwards to verify that the WAF extensions drop malicious traffic and allow benign traffic.

## Running the E2E Tests

To run the E2E suite, you must have an active cluster with Gateway API CRDs, your target Gateway controller (e.g., Istio or Sail Operator), and the Coraza Operator already deployed.

### 1. Set your Context
Make sure your active `KUBECONFIG` context points to the target cluster where the operator is installed. The framework will automatically read your `~/.kube/config` or `$KUBECONFIG`.

### 2. Run tests via Makefile
The `Makefile` at the root of the repository provides the `test.e2e` target.

#### Running on OpenShift
If you used `hack/ocp_cluster.py` to deploy the cluster, the GatewayClass name defaults to `openshift-default`. You must specify `GATEWAY_CLASS` to override the default `istio` class:
```bash
GATEWAY_CLASS=openshift-default make test.e2e
```

#### Running on Kind
If you used `make cluster.kind` (or `hack/kind_cluster.py`) to deploy the cluster, the test framework needs to explicitly grab the config from kind. Set the `KIND_CLUSTER_NAME`:
```bash
make test.e2e KIND_CLUSTER_NAME=coraza-kubernetes-operator-integration ISTIO_VERSION=1.28.2
```


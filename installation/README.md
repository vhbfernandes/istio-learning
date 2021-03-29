# Demo installation how-to

installing is quite simple for a test cluster using istioctl. Be mindful that it will take a lot of resources using demo mode, as it collects 100% of requests, not a sample. Most configs can be changed with `--set` as in a helm chart.

```bash
istioctl install --set profile=demo
```

## Production grade install

istioctl provides a simple install of a default profile that claims to be fine on production grade systems, with a single command
```bash
istioctl install
```
# BEWARE
Installing istio this way WILL NOT ALLOW YOU TO SAFELY UPDATE the mesh components. It is fine if you work with sort of immutable-clusters that can be re-created at will. If not, suggested method of installing and preparing for canary updates as follow:

```bash
istioctl install --set revision=1-9
```

This will allow the cluster to support several istiod deployments that can safely change versions by tagging the namespace.

Correct namespace tagging will not be `istio-injection=enabled` anymore, but `istio.io/rev=1-9`
More on this on [UPGRADING.md](UPGRADING.md)

Other than this, installing might require some teaks on the envoy proxy default resources, as it will take up a lot of base resources.


## Generating YAML manifests
I promised some example yaml on every section, but this one I'd rather avoid. Although istioctl can be used to generate static yaml files that can then be put into version control, installing it on a cluster should be done using the appropriate CLI and tweaked at the current version. To gernerate the operator manifests to `raw-istio.yaml` the command is

```bash
istioctl profile dump $PROFILE_NAME > raw-istio.yaml
```

The files then can be tweaked and applied using istioctl. It is not possible to apply it using kubectl since this yaml requires the installation of the istio operator.

This file can be used to generate raw kubernetes yaml that can be applied using kubectl, using the command 

```bash
istioctl manifest generate -f raw-istio.yaml > raw-kubernetes.yaml
```

## multi-cluster install
(TBD)

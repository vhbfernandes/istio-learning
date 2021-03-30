# mTLS

## Installing:
![its free](https://www.tibiabr.com/wp-content/uploads/2017/09/its-free-meme-600px.jpg)

Just as telemetry, mutual TLS is also free around all the mesh, just by adding the proxies. This was changed a few versions ago

## Wtf is this?

mutual TLS is the way istio upgrades the whole mesh to work in a zero trust network fashion. 

## Why do I neeed it?

A simple example using AWS and an EKS cluster created following the best practices, you should have more than one availability zone. Inside your network, another AZ is simply translated, but digging deeper, you'll find out that if you send intra-cluster traffic to another AZ, it will move around using the internet and not plain, docker networking. With that in mind, if you don't encrypt pod-to-pod traffic, you'll just send plain http content over the internet and that might be open to attacks and interception.


## Strict vs permissive modes

To ensure all traffic inside the network is working with mTLS you can enable STRICT mode, this WILL FAIL requests that are not coming from inside the mesh and therefore cannot be simply upgraded by the envoy proxies. The example yaml is here:

```yaml
apiVersion: "security.istio.io/v1beta1"
kind: "PeerAuthentication"
metadata:
  name: "default"
  namespace: "istio-system"
spec:
  mtls:
    mode: STRICT # default mode is PERMISSIVE
```
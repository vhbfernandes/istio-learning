# Routing

Istio traffic routing comprises of two components that can work together or standalone, basically decoupling targets and rules into bare networking fundamentals. These new components, VirtualServices and DestinationRules work on the data plane (the envoy proxies running as sidecars to all of the mesh-enabled pods) and allow for several traffic management possibilities, some of which are described in the yaml files of this folder.

You can roughly think of this two components as a L7 load balancer decoupled for input/output. Some traffic management can be used using one or both of these components combined.

## VirtualServices

Not to be confused with kubernetes services, as they don't really have that much in common. VirtualServices are mostly L7 load balancing rule (remember, envoy is a proxy after all) that can be created to route traffic to destinations based on any parameter you can collect from the request's origin or headers. 

Some cool applications of VirtualServices are like A/B tests (which use two target services routed based on a consistent hash header for example), Internal Releases (you can send `beta` users straight to some new version of some application based on header traffic), traffic can be mirrored or you can weight traffic on more than one subset of the same application (AKA Canary releases).

VirtualServices apply proxying on the OUTGOING layer, so as your application sends traffic to `something`, it is evaluated first what `something` destination means in the virtualservice's conditions.

### IMPORTANT

Rules on VirtualServices are evaluated in order.

## DestinationRules

DestinationRules are the counterpart of the VirtualServices, and used to evaluate traffic on the destination service itself. 

In the DestinationRule, the targets can be named/versioned on subsets, destination load balancing options can be applied (like round robin or least requests). One simple feature example using only a DestinationRule is circuit breaking. It is essential in canaries, as it is responsible for creating the target subsets.
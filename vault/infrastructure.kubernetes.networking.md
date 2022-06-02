---
id: A49UF6F6j25U1k79vXh4y
title: Networking
desc: ''
updated: 1643484063430
created: 1643483751470
---

## Ingress Controller

The ingress controller is a bunch of reverse proxies, that will be configured to redirect traffic onto desired services (defined by `Ingress` objects).

When used on an 

See : [ingress-nginx](https://kubernetes.github.io/ingress-nginx/)


## LoadBalancer

To expose the ingress controller to the internet, we need some kind of ~~glorified reverse proxy~~ load balancer.

On promising solution is [MetalLB](https://metallb.universe.tf/), which enables native `LoadBalancer` kubernetes services.  
However, it needs an allocable IP pool, which I don't have because I'm cheap.

So we're going to use a basic HAProxy to redirect all traffic to the ingress controller.  
The latter will be exposed on a `NodePort` service instead of a `LoadBalancer`.


## Links

[Bare-metal Kubernetes with Kubeadm, NGINX ingress controller and HAProxy](https://itnext.io/bare-metal-kubernetes-with-kubeadm-nginx-ingress-controller-and-haproxy-bb0a7ef29d4e)
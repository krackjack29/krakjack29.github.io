---
title: 'Kubernetes TLS secrets watcher'
layout: single
comments: true
date : 2020/06/29
categories:
    - Kubernetes
tags:
    - kubernetes
    - containers
    - secrets
    - tls
    - prometheus
---

We have been managing a multi-tenant kuberentes cluster with more than 100 nodes, hosting multiple ingress services exposed to the public domain. I will cover how we acheived multi tenancy in another blog post. 

The stack on the production cluster contains the following
1. [Istio](https://istio.io) for ServiceMesh
2. [Prometheus](https://github.com/coreos/prometheus-operator) and [Grafana](https://grafana.com/) for Monitoring.

Its been running smoothly, since last one year without any hiccups. Recently we had an issue of connecting from the global load balancer to the istio-ingressgateway (End to end SSL connectivity), when dug deeper found out that the one of the certificates has expired. 

These certificates are loaded as kubernetes secrets of type ```kubernetes.io/tls``` , now that this cluster is a multi-tenant cluster there are more than few certificates and to keep track of all certs expiry was a challenge.

Now that we already had Prometheus alert manager configured, but the metrics to export the expiry dates for the tls secrets were missing. So, I ended up writing a new one.

## k8s-tls-watcher

Code is available here in github [https://github.com/krackjack29/k8s-tls-watcher](https://github.com/krackjack29/k8s-tls-watcher)

I have created a small utlity pod which scans the secrets of type ```kubernetes.io/tls``` in all namespaces and checks for the expiry days left on each certificates and presents that data as Prometheus metrics for Prometheus to scrape at regular interval (by default 300s).

To deploy the pod on your cluster just run the following command, from the root directory once you have cloned the source code.
```bash
kubectl apply -k deploy
```

Once you have the pod deployed in the cluster, you can query using promql for metrics ```tls_certs_expiry_in_days``` which provides the name of the secret, domain and the days left for certificate expiry.

I have chosen .NetCore and C# as the application language and framework just to prove that these tools can be used to write applications which are close to kubernetes api. 

I have used [KubernetesClient](https://github.com/kubernetes-client/csharp) library to connect with the k8s cluster with a kubernetes service account credentials and [Prometheus-net](https://github.com/prometheus-net/prometheus-net) library to emit the prometheus metrics on port 3031.

Below is the screenshot for the metrics displayed in the prometheus

![Promql](/assets/images/k8stls/promql.png)

![Alert](/assets/images/k8stls/alert.png)
## Projects

### First Level
+ [kubernetes](https://github.com/kubernetes/kubernetes)
+ [istio](https://github.com/istio/istio)
+ [cilium](https://github.com/cilium/cilium)
+ [containerd](https://github.com/containerd/containerd)

### Second Level
+ [karmada-io/karmada](https://github.com/karmada-io/karmada)
+ [goharbor/harbor](https://github.com/goharbor/harbor)
+ Helm
+ vmware-tanzu/velero
+ open-policy-agent/gatekeeper
+ Kyverno
+ kubernetes-sigs/cluster-api
+ kubernetes-sigs/kubespray
+ kubernetes/kubeadm
+ Moby
+ etcd-io/*
+ projectcalico/calico
+ k8snetworkplumbingwg/multus-cni
+ Metallb
+ kubernetes/ingress-nginx
+ projectcontour/contour
+ Coredns
+ submariner-io/submariner
+ LINBIT/drbd
+ Minio
+ Prometheus
+ VictoriaMetrics
+ open-telemetry/opentelemetry-collector
+ open-telemetry/opentelemetry-collector-contrib
+ jaegertracing/jaeger
+ tektoncd/triggers
+ tektoncd/pipeline
+ argoproj/argo-cd
+ Kubeedge
+ envoyproxy/envoy
+ kubernetes-sigs/scheduler-plugins
+ kubernetes-sigs/kueue
+ alibaba/Sentinel
+ kubernetes-sigs/kwok

## Change Tips

1. remove `io/ioutil`
  + https://github.com/kubernetes-sigs/descheduler/pull/1030/files

2. remove `github.com/pkg/errors`
  + https://github.com/kubernetes/kubernetes/pull/113672/files
 
3. comparison to bool
  + https://github.com/cilium/cilium/pull/22588/files

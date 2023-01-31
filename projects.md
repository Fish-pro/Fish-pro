## Projects

### First Level
+ [kubernetes](https://github.com/kubernetes/kubernetes)
+ [istio](https://github.com/istio/istio)
+ [cilium](https://github.com/cilium/cilium)
+ [containerd](https://github.com/containerd/containerd)

### Second Level
+ [karmada-io/karmada](https://github.com/karmada-io/karmada)
+ [goharbor/harbor](https://github.com/goharbor/harbor)
+ [helm/helm](https://github.com/helm/helm)
+ [vmware-tanzu/velero](https://github.com/vmware-tanzu/velero)
+ [open-policy-agent/gatekeeper](https://github.com/open-policy-agent/gatekeeper)
+ [Kyverno](https://github.com/kyverno/kyverno)
+ [kubernetes-sigs/cluster-api](https://github.com/kubernetes-sigs/cluster-api)
+ [kubernetes-sigs/kubespray](https://github.com/kubernetes-sigs/kubespray)
+ [kubernetes/kubeadm](https://github.com/kubernetes/kubeadm)
+ [Moby](https://github.com/moby/moby)
+ [etcd-io/*](https://github.com/etcd-io)
+ [projectcalico/calico](https://github.com/projectcalico/calico)
+ [k8snetworkplumbingwg/multus-cni](https://github.com/k8snetworkplumbingwg/multus-cni)
+ [Metallb](https://github.com/metallb/metallb)
+ [kubernetes/ingress-nginx](https://github.com/kubernetes/ingress-nginx)
+ [projectcontour/contour](https://github.com/projectcontour/contour)
+ [Coredns](https://github.com/coredns/coredns)
+ [submariner-io/submariner](https://github.com/submariner-io/submariner)
+ [LINBIT/drbd](https://github.com/LINBIT/drbd)
+ [Minio](https://github.com/minio/minio)
+ [Prometheus](https://github.com/prometheus/prometheus)
+ [VictoriaMetrics](https://github.com/VictoriaMetrics/VictoriaMetrics)
+ [open-telemetry/opentelemetry-collector](https://github.com/open-telemetry/opentelemetry-collector)
+ [open-telemetry/opentelemetry-collector-contrib](https://github.com/open-telemetry/opentelemetry-collector-contrib)
+ [jaegertracing/jaeger](https://github.com/jaegertracing/jaeger)
+ [tektoncd/triggers](https://github.com/tektoncd/triggers)
+ [tektoncd/pipeline](https://github.com/tektoncd/pipeline)
+ [argoproj/argo-cd](https://github.com/argoproj/argo-cd)
+ [Kubeedge](https://github.com/kubeedge/kubeedge)
+ [envoyproxy/envoy](https://github.com/envoyproxy/envoy)
+ [kubernetes-sigs/scheduler-plugins](https://github.com/kubernetes-sigs/scheduler-plugins)
+ [kubernetes-sigs/kueue](https://github.com/kubernetes-sigs/kueue)
+ [alibaba/Sentinel](https://github.com/alibaba/Sentinel)
+ [kubernetes-sigs/kwok](https://github.com/kubernetes-sigs/kwok)

## Change Tips

1. remove `io/ioutil`
  + https://github.com/kubernetes-sigs/descheduler/pull/1030

2. remove `github.com/pkg/errors`
  + https://github.com/kubernetes/kubernetes/pull/113672
 
3. comparison to bool
  + https://github.com/cilium/cilium/pull/22588

4. docs
  + https://github.com/kubernetes/kubeadm/pull/2810

5. reformat table
  + https://github.com/kubernetes/kubeadm/pull/2811

6. errors.Is()
  + https://github.com/open-policy-agent/gatekeeper/pull/2491

7. deployment apiversion `extensions/v1beta1` change to `apps/v1`
  + https://github.com/istio/istio/pull/42631

8. klog format error
  + https://github.com/karmada-io/karmada/pull/3003

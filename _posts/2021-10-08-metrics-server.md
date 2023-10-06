---
layout: post
title: "Metrics Server"
date: 2021-10-08T19:23:37+02:00
tags: ["lke"]
---

```shell
$ kubectl top nodes
W1008 19:23:53.157850   69063 top_node.go:119] Using json format to get metrics. Next release will switch to protocol-buffers, switch early by passing --use-protocol-buffers flag
error: Metrics API not available
```

See: [How Can I Deploy the Kubernetes-Metrics Server on LKE?](https://www.linode.com/community/questions/19756/how-can-i-deploy-the-kubernetes-metrics-server-on-lke)



## file: `metrics-server.yaml`

```
kind: ClusterRole
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: system:aggregated-metrics-reader
  labels:
    rbac.authorization.k8s.io/aggregate-to-view: "true"
    rbac.authorization.k8s.io/aggregate-to-edit: "true"
    rbac.authorization.k8s.io/aggregate-to-admin: "true"
rules:
- apiGroups: ["metrics.k8s.io"]
  resources: ["pods", "nodes"]
  verbs: ["get", "list", "watch"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: metrics-server:system:auth-delegator
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: system:auth-delegator
subjects:
- kind: ServiceAccount
  name: metrics-server
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: metrics-server-auth-reader
  namespace: kube-system
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: extension-apiserver-authentication-reader
subjects:
- kind: ServiceAccount
  name: metrics-server
  namespace: kube-system
---
apiVersion: apiregistration.k8s.io/v1
kind: APIService
metadata:
  name: v1beta1.metrics.k8s.io
spec:
  service:
    name: metrics-server
    namespace: kube-system
  group: metrics.k8s.io
  version: v1beta1
  insecureSkipTLSVerify: true
  groupPriorityMinimum: 100
  versionPriority: 100
---
apiVersion: v1
kind: ServiceAccount
metadata:
  name: metrics-server
  namespace: kube-system
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: metrics-server
  namespace: kube-system
  labels:
    k8s-app: metrics-server
spec:
  selector:
    matchLabels:
      k8s-app: metrics-server
  template:
    metadata:
      name: metrics-server
      labels:
        k8s-app: metrics-server
    spec:
      serviceAccountName: metrics-server
      volumes:
      # Mount in tmp so we can safely use from-scratch images and/or read-only containers
      - name: tmp-dir
        emptyDir: {}
      containers:
      - name: metrics-server
        image: k8s.gcr.io/metrics-server-amd64:v0.3.6
        imagePullPolicy: Always
        command:
          - /metrics-server
          # * metrics-server must reach kubelets by Node private IP
          - --kubelet-preferred-address-types=InternalIP
          # * metrics-server connects to each Node's kubelet
          # * each Node's kubelet presents a CA certificate
          #     echo | \
          #       openssl s_client -showcerts -connect localhost:10250 2>/dev/null | \
          #       openssl x509 -inform pem -noout -text
          # * The Common Name and Subject Alternative Names do not include the Node's private IP
          #     Subject: CN = lke3578-4746-5e97658362af@1586980323
          #     X509v3 extensions:
          #       X509v3 Subject Alternative Name:
          #       DNS:lke3578-4746-5e97658362af
          # * This certificate is generated frequently and dynamically by kubelet
          # * There is not currently a way to add Node private IP as a SAN
          - --kubelet-insecure-tls
        volumeMounts:
        - name: tmp-dir
          mountPath: /tmp
---
apiVersion: v1
kind: Service
metadata:
  name: metrics-server
  namespace: kube-system
  labels:
    kubernetes.io/name: "Metrics-server"
    kubernetes.io/cluster-service: "true"
spec:
  selector:
    k8s-app: metrics-server
  ports:
  - port: 443
    protocol: TCP
    targetPort: 443
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: system:metrics-server
rules:
- apiGroups:
  - ""
  resources:
  - pods
  - nodes
  - nodes/stats
  - namespaces
  verbs:
  - get
  - list
  - watch
- apiGroups:
  - "extensions"
  resources:
  - deployments
  verbs:
  - get
  - list
  - watch
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: system:metrics-server
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: system:metrics-server
subjects:
- kind: ServiceAccount
  name: metrics-server
  namespace: kube-system
```

I've updated the APIService `apiVersion`. 

I've had no luck with a newer version:
`image: k8s.gcr.io/metrics-server-amd64:v0.5.1`.



## Apply

```shell
$ kubectl apply -f metrics-server.yaml
clusterrole.rbac.authorization.k8s.io/system:aggregated-metrics-reader created
clusterrolebinding.rbac.authorization.k8s.io/metrics-server:system:auth-delegator created
rolebinding.rbac.authorization.k8s.io/metrics-server-auth-reader created
apiservice.apiregistration.k8s.io/v1beta1.metrics.k8s.io created
serviceaccount/metrics-server created
deployment.apps/metrics-server created
service/metrics-server created
clusterrole.rbac.authorization.k8s.io/system:metrics-server created
clusterrolebinding.rbac.authorization.k8s.io/system:metrics-server created
```


## Top Node

```shell
$ kubectl top node
W1008 19:36:11.335849   69582 top_node.go:119] Using json format to get metrics. Next release will switch to protocol-buffers, switch early by passing --use-protocol-buffers flag
NAME                          CPU(cores)   CPU%   MEMORY(bytes)   MEMORY%
lke39473-64379-615ec4bfcab8   212m         21%    1171Mi          62%
lke39473-64379-615ec4c02cea   116m         11%    1034Mi          54%
```

```shell
$ kubectl top node --use-protocol-buffers
NAME                          CPU(cores)   CPU%   MEMORY(bytes)   MEMORY%
lke39473-64379-615ec4bfcab8   178m         17%    1171Mi          62%
lke39473-64379-615ec4c02cea   117m         11%    1034Mi          54%
```


## Top Pod

```shell
$ kubectl top pod -A
W1008 19:36:45.178974   69605 top_pod.go:140] Using json format to get metrics. Next release will switch to protocol-buffers, switch early by passing --use-protocol-buffers flag
NAMESPACE      NAME                                        CPU(cores)   MEMORY(bytes)
cert-manager   cert-manager-7998c69865-qf6xf               2m           35Mi
cert-manager   cert-manager-cainjector-7b744d56fb-ldrsn    3m           47Mi
cert-manager   cert-manager-webhook-97f8b47bc-tl9z7        2m           27Mi
default        ingress-nginx-controller-5cc57bf6c7-75lxg   2m           71Mi
default        static-site-deployment-79f595f9b-27gf6      0m           3Mi
default        static-site-deployment-79f595f9b-gtmgz      0m           2Mi
kube-system    calico-kube-controllers-75f955b89b-vwczx    4m           25Mi
kube-system    calico-node-glrdw                           27m          103Mi
kube-system    calico-node-r99kr                           44m          99Mi
kube-system    coredns-65648f44c6-7qfsl                    2m           19Mi
kube-system    coredns-65648f44c6-vl22s                    2m           20Mi
kube-system    csi-linode-controller-0                     2m           49Mi
kube-system    csi-linode-node-4trm4                       1m           21Mi
kube-system    csi-linode-node-sgsql                       1m           20Mi
kube-system    kube-proxy-62nnr                            1m           22Mi
kube-system    kube-proxy-x6ggg                            1m           22Mi
kube-system    metrics-server-7f976b7d7d-zj8pd             2m           12Mi
```

```shell
$ kubectl top pod -A --use-protocol-buffers
NAMESPACE      NAME                                        CPU(cores)   MEMORY(bytes)
cert-manager   cert-manager-7998c69865-qf6xf               2m           35Mi
cert-manager   cert-manager-cainjector-7b744d56fb-ldrsn    5m           47Mi
cert-manager   cert-manager-webhook-97f8b47bc-tl9z7        2m           27Mi
default        ingress-nginx-controller-5cc57bf6c7-75lxg   2m           71Mi
default        static-site-deployment-79f595f9b-27gf6      0m           3Mi
default        static-site-deployment-79f595f9b-gtmgz      0m           2Mi
kube-system    calico-kube-controllers-75f955b89b-vwczx    4m           25Mi
kube-system    calico-node-glrdw                           38m          103Mi
kube-system    calico-node-r99kr                           39m          109Mi
kube-system    coredns-65648f44c6-7qfsl                    2m           19Mi
kube-system    coredns-65648f44c6-vl22s                    2m           20Mi
kube-system    csi-linode-controller-0                     1m           49Mi
kube-system    csi-linode-node-4trm4                       1m           21Mi
kube-system    csi-linode-node-sgsql                       1m           20Mi
kube-system    kube-proxy-62nnr                            1m           22Mi
kube-system    kube-proxy-x6ggg                            1m           22Mi
kube-system    metrics-server-7f976b7d7d-zj8pd             1m           10Mi
```

```shell
$ kubectl top pod --use-protocol-buffers
NAME                                        CPU(cores)   MEMORY(bytes)
ingress-nginx-controller-5cc57bf6c7-75lxg   2m           71Mi
static-site-deployment-79f595f9b-27gf6      0m           3Mi
static-site-deployment-79f595f9b-gtmgz      0m           2Mi
```
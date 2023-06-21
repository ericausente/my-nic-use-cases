```
% kubectl get deployment| grep ingress 
ingress-plus-nginx-ingress-controller    1/1     1            1           25d

% kubectl get deployment ingress-plus-nginx-ingress-controller -o yaml | grep config   
        - -nginx-configmaps=$(POD_NAMESPACE)/ingress-plus-nginx-ingress
```     

Make sure you have the resolvers configured: 

```
% kubectl get cm ingress-plus-nginx-ingress -o yaml
apiVersion: v1
data:
  resolver-addresses: 8.8.8.8 1.1.1.1 2.2.2.2
kind: ConfigMap
metadata:
  annotations:
    meta.helm.sh/release-name: ingress-plus
    meta.helm.sh/release-namespace: default
  creationTimestamp: "2023-05-27T13:11:24Z"
  labels:
    app.kubernetes.io/instance: ingress-plus
    app.kubernetes.io/managed-by: Helm
    app.kubernetes.io/name: nginx-ingress
    app.kubernetes.io/version: 3.1.1
    helm.sh/chart: nginx-ingress-0.17.1
  name: ingress-plus-nginx-ingress
  namespace: default
  resourceVersion: "16086655"
  uid: 63fdc08a-8da4-4fff-93f8-f239e1f53f58

```

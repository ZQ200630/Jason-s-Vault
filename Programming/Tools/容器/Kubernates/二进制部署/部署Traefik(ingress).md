---
tags: Kubernates
---
# 部署Traefik（ingress）

```bash
[[HDSS7-200]]
docker pull traefik:v1.7.2-alpine

[[traefik-crd]].yaml
apiVersion: apiextensions.k8s.io/v1beta1
kind: CustomResourceDefinition
metadata:
  name: ingressroutes.traefik.containo.us

spec:
  group: traefik.containo.us
  version: v1alpha1
  names:
    kind: IngressRoute
    plural: ingressroutes
    singular: ingressroute
  scope: Namespaced

---
apiVersion: apiextensions.k8s.io/v1beta1
kind: CustomResourceDefinition
metadata:
  name: ingressroutetcps.traefik.containo.us

spec:
  group: traefik.containo.us
  version: v1alpha1
  names:
    kind: IngressRouteTCP
    plural: ingressroutetcps
    singular: ingressroutetcp
  scope: Namespaced

---
apiVersion: apiextensions.k8s.io/v1beta1
kind: CustomResourceDefinition
metadata:
  name: middlewares.traefik.containo.us

spec:
  group: traefik.containo.us
  version: v1alpha1
  names:
    kind: Middleware
    plural: middlewares
    singular: middleware
  scope: Namespaced

---
apiVersion: apiextensions.k8s.io/v1beta1
kind: CustomResourceDefinition
metadata:
  name: tlsoptions.traefik.containo.us

spec:
  group: traefik.containo.us
  version: v1alpha1
  names:
    kind: TLSOption
    plural: tlsoptions
    singular: tlsoption
  scope: Namespaced

---
kind: ClusterRole
apiVersion: rbac.authorization.k8s.io/v1beta1
metadata:
  name: traefik-ingress-controller

rules:
  - apiGroups:
      - ""
    resources:
      - services
      - endpoints
      - secrets
    verbs:
      - get
      - list
      - watch
  - apiGroups:
      - extensions
    resources:
      - ingresses
    verbs:
      - get
      - list
      - watch
  - apiGroups:
      - extensions
    resources:
      - ingresses/status
    verbs:
      - update
  - apiGroups:
      - traefik.containo.us
    resources:
      - middlewares
    verbs:
      - get
      - list
      - watch
  - apiGroups:
      - traefik.containo.us
    resources:
      - ingressroutes
    verbs:
      - get
      - list
      - watch
  - apiGroups:
      - traefik.containo.us
    resources:
      - ingressroutetcps
    verbs:
      - get
      - list
      - watch
  - apiGroups:
      - traefik.containo.us
    resources:
      - tlsoptions
    verbs:
      - get
      - list
      - watch

---
kind: ClusterRoleBinding
apiVersion: rbac.authorization.k8s.io/v1beta1
metadata:
  name: traefik-ingress-controller

roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: traefik-ingress-controller
subjects:
  - kind: ServiceAccount
    name: traefik-ingress-controller
    namespace: default

[[traefik-svc]].yaml
apiVersion: v1
kind: Service
metadata:
  name: traefik

spec:
  type: NodePort
  ports:
    - protocol: TCP
      name: web
      port: 8000
      nodePort: 20000
    - protocol: TCP
      name: admin
      port: 8080
      nodePort: 20001
    - protocol: TCP
      name: websecure
      port: 4443
      nodePort: 20002
  selector:
    app: traefik

[[traefik-deploy]].yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  namespace: default
  name: traefik-ingress-controller

---
kind: Deployment
apiVersion: apps/v1
metadata:
  namespace: default
  name: traefik
  labels:
    app: traefik

spec:
  replicas: 2
  selector:
    matchLabels:
      app: traefik
  template:
    metadata:
      labels:
        app: traefik
    spec:
      serviceAccountName: traefik-ingress-controller
      containers:
        - name: traefik
          image: harbor.od.com/public/traefik:v2.0.2
          args:
            - --api.insecure
            - --accesslog
            - --entrypoints.web.Address=:8000
            - --entrypoints.websecure.Address=:4443
            - --providers.kubernetescrd
            - --certificatesresolvers.default.acme.tlschallenge
            - --certificatesresolvers.default.acme.email=foo@you.com
            - --certificatesresolvers.default.acme.storage=acme.json
            # Please note that this is the staging Let's Encrypt server.
            # Once you get things working, you should remove that whole line altogether.
            - --certificatesresolvers.default.acme.caserver=https://acme-staging-v02.api.letsencrypt.org/directory
          ports:
            - name: web
              containerPort: 8000
            - name: websecure
              containerPort: 4443
            - name: admin
              containerPort: 8080

[[HDSS7-21]]
kubectl create -f http://k8s-yaml.od.com/traefik/traefik-crd.yaml 
kubectl create -f http://k8s-yaml.od.com/traefik/traefik-deploy.yaml 
kubectl create -f http://k8s-yaml.od.com/traefik/traefik-svc.yaml 

[[ingress]]
[[ingress]].yaml
apiVersion: traefik.containo.us/v1alpha1
kind: IngressRoute
metadata:
  name: pro
  namespace: default
spec:
  entryPoints:
    - web
  routes:
  - match: Host(`traefik.od.com`) && PathPrefix(`/`)
    kind: Rule
    services:
    - name: test-server
      port: 8080
          
          
[[vi]] /etc/nginx/conf.d/od.com.conf
upstream default_backend_traefik {
	server 10.4.7.21:20000 max_fails=3 fail_timeout=10s;
	server 10.4.7.22:20000 max_fails=3 fail_timeout=10s;
}
server {
	server_name *.od.com;
	location / {
		proxy_pass http://default_backend_traefik;
		proxy_set_header Host	$http_host;
		proxy_set_header x_forwarded_for $proxy_add_x_forwarded_for;
	}
}
```

```bash
wget  http://raw.githubusercontent.com/kubernetes/ingress-nginx/nginx-0.18.0/deploy/mandatory.yaml
在
serviceAccountName: nginx-ingress-serviceaccount 下加一句
      hostNetwork: true
      
[[ingress]].yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: https-web
  annotations:
    kuberenetes.io/ingress.class: "nginx"
spec:
  rules:
  - host: traefik.od.com
    http:
      paths:
      - path: /
        backend:
          serviceName: nginx
          servicePort: 80
```
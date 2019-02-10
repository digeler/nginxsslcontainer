# how to deploy :
kubectl run sslnginx --image=dinorg/sslnginx --port=443
kubectl expose deployment sslnginx --port=8124 --target-port=443

then the ingress :

apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  annotations:
    nginx.ingress.kubernetes.io/ssl-passthrough: "true"

  name: tls-example-ingress
  namespace: default

spec:
  rules:
  - host: testhttps.com
    http:
      paths:
      - backend:
          serviceName: sslnginx
          servicePort: 8124
        path: /
  tls:
  - hosts:
    - testhttps.com



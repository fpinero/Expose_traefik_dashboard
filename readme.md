Expose traefik dashboard

.- Create service

traefik-dashboard-service.yaml:

apiVersion: v1
kind: Service
metadata:
  name: traefik-dashboard
  namespace: kube-system
  labels:
    app.kubernetes.io/instance: traefik
    app.kubernetes.io/name: traefik-dashboard
spec:
  type: ClusterIP
  ports:
  - name: traefik
    port: 9000
    targetPort: traefik
    protocol: TCP
  selector:
    app.kubernetes.io/instance: traefik
    app.kubernetes.io/name: traefik
---

cat traefik-dashboard-service.yaml | envsubst | kubectl apply -n kube-system -f -

---

.- Create ingress

traefik-dashboard-ingress.yaml:

apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: traefik-ingress
  namespace: kube-system
  annotations:
    kubernetes.io/ingress.class: traefik
spec:
  rules:
    - host: dok8s.n0-reply.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: traefik-dashboard
                port:
                  number: 9000

---

cat traefik-dashboard-ingress.yaml | envsubst | kubectl apply -n kube-system -f -

---

http://dok8s.n0-reply.com/dashboard/ 

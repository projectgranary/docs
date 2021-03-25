# Nginx-ingress rewrite-rule Migration to version 0.22.0 and above

With nginx-ingress version 0.22 a breaking change to the handling of rewrite-rules was introduced. It is now required to explicitly define capture groups for every substring that needs to be passed to the rewrite-rule. 

### Example belt-api ingress configuration:

#### Before 0.22.0:

```text
ingress:
  contextPath: "/belts"
  hosts: []
  annotations:
    kubernetes.io/ingress.class: nginx
    nginx.ingress.kubernetes.io/rewrite-target: /belts
```

#### 0.22.0 and beyond:

```text
ingress:
  contextPath: "/belts(.*)"
  annotations:
    kubernetes.io/ingress.class: nginx
    nginx.ingress.kubernetes.io/rewrite-target: /belts$1
```

The default ingress values of granary 1.0 charts are compatible with nginx-ingress versions beyond 0.22.0.

See [https://kubernetes.github.io/ingress-nginx/examples/rewrite/\#rewrite-target](https://kubernetes.github.io/ingress-nginx/examples/rewrite/#rewrite-target) for detailed documentation.


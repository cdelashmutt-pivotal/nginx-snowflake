You can use the manifests in this directory to create a custom domain name for the app using ytt to fill in the template values.

# Basics
```
ytt -f . --data-value ingress_domain=YOUR_INGRESS_DOMAIN --data-value workload.namespace=WORKLOAD_NAMESPACE --data-value workload.name=WORKLOAD_NAME | kubectl apply -f -
```

# TLS
If you want a specialized certificate for the workload, you'll needed to generate one that covers the workload's FQDN, and then reference it in the ytt templating.

```
ytt -f . --data-value ingress_domain=YOUR_INGRESS_DOMAIN --data-value workload.namespace=WORKLOAD_NAMESPACE --data-value workload.name=WORKLOAD_NAME --data-value tls_secret=SECRET_NAME_IN_WORKLOAD_NAMESPACE | kubectl apply -f -
```

## Certificate request using cert-manager
```
cat <<EOF | kubectl apply -f-
apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
  name: snowflake-tls
spec:
  secretName: snowflake-tls

  duration: 2160h
  renewBefore: 360h
  subject:
    organizations:
      - example
  commonName: special-snowflake.workshop.amer.end2end.link
  isCA: false
  privateKey:
    algorithm: RSA
    encoding: PKCS1
    size: 2048
  usages:
    - server auth
    - client auth
  dnsNames:
    - special-snowflake.workshop.amer.end2end.link
  issuerRef:
    name: letsencrypt-contour-cluster-issuer
    kind: ClusterIssuer
EOF
```
# Seerr (Overseerr)

Single-replica Seerr deployment in the `default` namespace, pinned to
`k3s-worker-02` for the requested outbound internet access. Application data
is stored at `/app/config` on `synology-k8s-pv02` under `seer/config`; back up
the Synology data along with the application configuration.

The image is promoted from `ghcr.io/seerr-team/seerr` and pinned by immutable
digest. The pod pulls from the private registry using the namespace secret
`image-registry-credentials`. Do not add internet access elsewhere as an image
pull workaround; promotion is handled separately.

## Access

- Direct HTTP: `http://k3s-worker-02.home:30555`
- Kubernetes Service: `seer.default.svc:5055`

The NodePort is the backend/fallback. Route an HTTPS hostname through HAProxy
if desired; no hostname was specified for this deployment.

## Operations

```sh
kubectl apply -k .
kubectl rollout status deployment/seer --timeout=180s
kubectl get pods,svc,endpoints -l app=seer -o wide
```

This application is explicitly scheduled only on worker-02, which has scoped
TCP/80,443 egress. Its native metrics endpoint was not verified, so no
ServiceMonitor is configured.

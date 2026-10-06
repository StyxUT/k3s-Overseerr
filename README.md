# Seerr

Seerr runs as a single replica in the `default` namespace on
`k3s-worker-02`, where the approved internet egress is available. The image is
pinned by immutable digest and pulled from the private registry.

## Persistence

`/app/config` uses the existing `default/synology-k8s-pv02` ReadWriteMany claim
under `seerr/config` on the Synology `/volume2/k8s-pv02` export. Back up the
Seerr configuration along with the NAS data.

## Access

- Direct HTTP: `http://k3s-worker-02.home:30555`
- Kubernetes Service: `default/seer`, NodePort `30555`, target port `5055`

The Service resource retains its historical `seer` name to avoid breaking
existing callers; its selector targets the current `app=seerr` Deployment.

## Deployment

```sh
kubectl apply -k .
kubectl rollout status deployment/seerr --timeout=180s
kubectl get pods,svc,endpoints -l app=seerr -o wide
```

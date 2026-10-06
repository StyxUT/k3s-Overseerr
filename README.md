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
- Kubernetes Service: `default/seerr`, NodePort `30555`, target port `5055`

The Service selects the `app=seerr` Deployment.

## Deployment

```sh
kubectl apply -k .
kubectl rollout status deployment/seerr --timeout=180s
kubectl get pods -l app=seerr -o wide
kubectl get service/endpoints seerr -o wide
```

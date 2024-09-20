# Guide for boostraping flux in a k8s cluster


1. Create a kind k8s cluster

```
kind create cluster --name fluxcd --image kindest/node:v1.30.4
```

2. Run a container to work in

This is an optional step, it will allow you to work in a container instead of your local machine.

```
docker run -it --rm -v ${HOME}:/root/ -v ${PWD}:/work -w /work --net host alpine sh
```
3. Export githu token (fine grained PAT)

export GITHUB_TOKEN=<your-token>

4. Bootstrap flush for github

```
flux bootstrap github \
  --token-auth \
  --owner=garfiny \
  --repository=flux-bootstrap \
  --path=kubernetes/fluxcd/repositories/infra-repo/clusters/dev-cluster \ // where flux stores the yaml files
  --personal \
  --branch fluxcd-local
```
Note: if there's local proxy configured, kind cluster might not be able pull flux images, so you need to do step 5.

5. Load images to Kind cluster (Optional)

- Docker pull image to local

    ```
    docker pull ghcr.io/fluxcd/source-controller:v1.3.0
    docker pull ghcr.io/fluxcd/notification-controller:v1.3.0
    docker pull ghcr.io/fluxcd/kustomize-controller:v1.3.0
    docker pull ghcr.io/fluxcd/helm-controller:v1.0.1
    ```

- Load image to kind cluster
    ```
        kind load docker-image ghcr.io/fluxcd/source-controller:v1.3.0 --name fluxcd
        kind load docker-image ghcr.io/fluxcd/notification-controller:v1.3.0 --name fluxcd
        kind load docker-image ghcr.io/fluxcd/kustomize-controller:v1.3.0 --name fluxcd
        kind load docker-image ghcr.io/fluxcd/helm-controller:v1.0.1 --name fluxcd
    ```

# Container images

The Helm chart does not pin a public image. These are the Dockerfiles used to build the images it expects, so
you can build and push your own to any registry.

## Open5GS core

`docker/open5gs/<version>/Dockerfile` builds Open5GS from source against the upstream repository and installs it
under `/open5gs/install`. The chart invokes the binaries from `/open5gs/install/bin`, so keep that prefix if you
write your own.

| Version | Notes |
|---|---|
| `2.4.8`, `2.4.9`, `2.4.12` | Ubuntu 20.04 base, build args for repo and tag |
| `2.5.6` | Ubuntu 20.04 base |
| `2.5.9` | Adds `meson configure -Dmetrics_impl=prometheus` — **required for the AMF/SMF metrics endpoints** |
| `2.6.1` | Default for this chart; matches `Chart.yaml` `appVersion` |
| `2.6.6` | Newer core, same layout |
| `latest` | Tracks upstream `master`, unpinned — for experiments only |

```bash
docker build -t <registry>/open5gs:2.6.1 docker/open5gs/2.6.1
docker push  <registry>/open5gs:2.6.1
```

> Versions before 2.5.9 build without the Prometheus metrics implementation. If you enable
> `amf.configuration.metrics` or `smf.configuration.metrics` on an older image, the ServiceMonitors are created
> but the endpoints return nothing.

## Open5GS WebUI

`docker/open5gs-webui/Dockerfile` builds the Node.js subscriber management UI.

```bash
docker build -t <registry>/open5gs-webui:latest docker/open5gs-webui
```

> This image is dated: it pins `node:12.14.1-alpine3.9` and clones upstream `master` unpinned, so two builds a
> month apart can produce different WebUIs. Fine for a lab, worth modernising before anyone relies on it.

## Subscriber import

`docker/subscribers/Dockerfile` provides `curl` and `mongoimport`, the two tools the post-install hook in
`add_subs_auto.helm_post_hook` needs. It installs the MongoDB database tools explicitly rather than relying on
the official `mongo` image, which stopped shipping `mongoimport` after 4.4.

```bash
docker build -t <registry>/open5gs-subscribers:0.0.2 docker/subscribers
```

## UERANSIM

`docker/ueransim/Dockerfile` builds [UERANSIM](https://github.com/aligungr/UERANSIM) v3.2.6 — a 5G UE and gNodeB
simulator — as a multi-stage build, shipping `nr-gnb`, `nr-ue`, `nr-cli` and the `nr-binder` helper on a slim
Ubuntu 20.04 runtime. It is not deployed by this chart; it is what you attach to the core to generate traffic.

```bash
docker build -t <registry>/ueransim:3.2.6 docker/ueransim
```

Mount your gNB/UE configuration at `/ueransim/config` and run `./nr-gnb -c config/gnb.yaml`.

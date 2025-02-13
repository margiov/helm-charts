# Helm chart for OpenProject

This is the chart for OpenProject itself.

## Installation

Please see our [installation guide on our website](https://www.openproject.org/docs/installation-and-operations/installation/helm-chart/).

## Development

To install or update from this directory run the following command.

```bash
bin/install-dev
```

This will install the chart with `--set develop=true` which is recommended
on local clusters such as **minikube** or **kind**.

This will also set `OPENPROJECT_HTTPS` to false so no TLS certificate is required
to access it.

You can set other options just like when installing via `--set`
(e.g. `bin/install-dev --set persistence.enabled=false`).

### Debugging

Changes to the chart can be debugged using the following.

```bash
bin/debug
```

This will try to render the templates and show any errors.
You can set values just like when installing via `--set`
(e.g. `bin/debug --set persistence.enabled=false`).

## TLS

Create a TLS certificate, e.g. using [mkcert](https://github.com/FiloSottile/mkcert).

```
mkcert helm-example.openproject-dev.com
```

Create the tls secret in kubernetes.

```
kubectl -n openproject create secret tls openproject-tls \
  --key="helm-example.openproject-dev.com-key.pem" \
  --cert="helm-example.openproject-dev.com.pem"
```

Set the tls secret value during installation or an upgrade by adding the following.

```
--set ingress.tls.enabled=true --set tls.secretName=openproject-tls
```

### Root CA

If you want to add your own root CA for outgoing TLS connection, do the following.

1. Put the certificate into a config map.

```
kubectl -n openproject-dev create configmap ca-pemstore --from-file=/path/to/rootCA.pem
```

To make OpenProject use this CA for outgoing TLS connection, set the following options.

```
  --set egress.tls.rootCA.configMap=ca-pemstore \
  --set egress.tls.rootCA.fileName=rootCA.pem
```

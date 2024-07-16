# Setting resources for a Helm sidecar

The sidecar deployment is composed of a single container, which can have
[resource requirements and limits](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/) attached to it.

## Setting default requests and limits to the sidecar container

To set the default resources for the sidecar container, add the following lines
to your `values.yaml` file.

```yaml
resources:
  limits:
    cpu: 400m
    memory: 8096Mi
  requests:
    cpu: 100m
    memory: 4096Mi
```

**NOTE:** The above are the recommended settings for the sidecar.

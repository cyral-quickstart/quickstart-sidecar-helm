# Setting resources for a Helm sidecar

The sidecar deployment is composed of many containers, and all of these containers can have
[resource requirements and limits](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/) attached to them.

## Setting default requests and limits to all containers on the sidecar

To set the default resources for all containers on the sidecar, add the following lines
to your `values.yaml` file.

```yaml
resources:
  limits:
    cpu: 100m
    memory: 20Mi
  requests:
    cpu: 50m
    memory: 10Mi
```

**NOTE:** The above are the recommended settings for the sidecar. Remember that these are for
**each** container in the sidecar, and not for the whole sidecar.

## Setting resources for a single container
To set up resources for a single container, go to the `container` section of the values file
and add these lines to it:

```yaml
<repo>:
  resources:
    limits:
      cpu: 100m
      memory: 20Mi
    requests:
      cpu: 50m
      memory: 10Mi
```

You can set the limits and requests to whatever matches your infrastructure capabilities.
For example, if you want to have the `mysql` container require 1 cpu and 100MB of memory,
set this on your `values.yaml`:

```yaml
mysql:
  resources:
    requests:
      cpu: 1
      memory: 100Mi
```

**NOTE:** Setting the resources for a single container overrides the default resources
for that container.






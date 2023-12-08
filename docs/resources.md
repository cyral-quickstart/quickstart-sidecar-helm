# Setting resources for a Helm sidecar

The sidecar deployment is composed of many containers, and all of these containers can have
[resource requirements and limits](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/) attached to them.

## Setting default requests and limits to all containers on the sidecar

To set the default resources for all containers on the sidecar, add the following lines
to your `values.yaml` file.

> [!TIP]
> Setting CPU limits is not advised! Please see [this blog post](https://home.robusta.dev/blog/stop-using-cpu-limits) for more information.

```yaml
resources:
  limits:
    #cpu: 200m
    memory: 20Mi
  requests:
    cpu: 10m
    memory: 10Mi
```

> [!IMPORTANT]
> Keep in mind that this setting applies to each container within the sidecar, rather than the entire sidecar itself.
> It is recommended to assign specific values for each wire used when implementing these guidelines.

## Setting resources for a single container

> [!NOTE]
> Setting the resources for a single container overrides the default resources
> for that container.

To set up resources for a single container, go to the `container` section of the values file
and add these lines to it:

```yaml
<repo>:
  resources:
    limits:
      #cpu: 100m
      memory: 20Mi
    requests:
      cpu: 10m
      memory: 100Mi
```

You can set the limits and requests to whatever matches your infrastructure capabilities.
For example, if you want to have the `mysql` container require 1 cpu and 1GB of memory,
set this on your `values.yaml`:

```yaml
mysql:
  resources:
    requests:
      cpu: 1
      memory: 1Gi
```

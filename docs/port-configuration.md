# Restricting ports where users connect to repositories

A single Cyral sidecar cluster usually monitors and protects many
repositories of different types. To make it easy for data users to
connect to these repositories using the port numbers they're
accustomed to, the sidecar cluster exposes multiple ports.
You can restrict or increase the set of exposed ports by changing
the exposed ports in the `values.yaml` file.

## Changing a single repository exposed ports

On the `values.yaml` file, you can disable a specific repository, which will make its
defined ports not get exposed in the main service. For example, by default,
the `mysql` section of the `values.yaml` file contains this definition:

```yaml
containerPorts:
  mysql: 3306
  pg: 5432
```

This means that the ports `3306` and `5432` will be exposed in the
sidecar service. To increase or decrease the ports exposed, you can
add or remove ports from the `containerPorts` section of the `values.yaml` file.

## Overriding all exposed ports

If you only need to expose a few ports and don't want to go through the trouble of
changing all repository ports, you can use the `service.ports` section of the
`values.yaml` file, which will make the chart ignore repo-specific port definitions
and only expose the ports defined in that section.

```yaml
service:
  ...
  ports:
    - 5432
    - 3306
```

The above example makes no port but `3306` and `5432` be exposed on the service.

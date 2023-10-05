# Restricting ports where users connect to repositories

A single Cyral sidecar cluster usually monitors and protects many
repositories of different types. To make it easy for data users to
connect to these repositories using the port numbers they're
accustomed to, the sidecar cluster exposes multiple ports, which
are neatly organized in each repo's section of the provided values
file.

You can restrict or increase the set of exposed ports by either changing
the exposed ports for each repository or by overriding all ports on the main
service configuration of the `values.yaml` file.

## Changing a single repository exposed ports

On the `values.yaml` file, you can disable a specific repository, which will make its
defined ports not get exposed in the main service. For example, by default,
the `mysql` section of the `values.yaml` file contains this definition:

```yaml
mysql:
  enabled: true
  ...
  ports:
    ...
    # ports that are added to the sidecar service
    sidecar:
      - 3306
      - 3307
      - 3308
 ...
```

This means that the ports `3306`, `3307` and `3308` will be exposed in the
sidecar service. To increase or decrease the ports exposed for `mysql`, you can
add or remove ports from the `mysql.sidecar.ports` section of the `values.yaml` file.
This is analogous for all repositories, that is, you can change the `<repo>.sidecar.ports`
section to add or remove specific ports from the exposed list.

**NOTE:** If a repository is disabled, that is, the `<repo>.enabled` key is set to `false`,
none of its ports will be exposed in the service.

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

# fluentd-helm

This Helm chart deploys Fluentd as a DaemonSet for New Relic Logging

## Deploying the Helm Chart

## Chart Details

## Configuration

- Clone this repo
- Deploy the chart using your New Relic license key: `helm install --set licenseKey=(your-license-key) ./helm/fluentd-helm`
- Check the New Relic Logs for your logs and AWS EC2 tags

See [values.yaml](values.yaml) for the default values

| Parameter                                      | Description                                                                                                                                                                                                                                       | Default                     |
| ---------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------- |
| `global.licenseKey` - `licenseKey`             | The [license key](https://docs.newrelic.com/docs/accounts/install-new-relic/account-setup/license-key) for your New Relic Account. This will be the preferred configuration option if both `licenseKey` and `customSecret*` values are specified. |                             |
| `global.customSecretName` - `customSecretName` | Name of the Secret object where the license key is stored                                                                                                                                                                                         |                             |
| `global.customSecretKey` - `customSecretKey`   | Key in the Secret object where the license key is stored.                                                                                                                                                                                         |                             |
| `rbac.create`                                  | Enable Role-based authentication                                                                                                                                                                                                                  | `true`                      |
| `image.repository`                             | The container to pull.                                                                                                                                                                                                                            | `newrelic/fluentd`          |
| `image.pullPolicy`                             | The pull policy.                                                                                                                                                                                                                                  | `IfNotPresent`              |
| `image.tag`                                    | The version of the container to pull.                                                                                                                                                                                                             | See value in [values.yaml]` |
| `resources`                                    | Any resources you wish to assign to the pod.                                                                                                                                                                                                      | See Resources below         |
| `priorityClassName`                            | Scheduling priority of the pod                                                                                                                                                                                                                    | `nil`                       |
| `nodeSelector`                                 | Node label to use for scheduling                                                                                                                                                                                                                  | `nil`                       |
| `tolerations`                                  | List of node taints to tolerate (requires Kubernetes >= 1.6)                                                                                                                                                                                      | See Tolerations below       |
| `updateStrategy`                               | Strategy for DaemonSet updates (requires Kubernetes >= 1.6)                                                                                                                                                                                       | `RollingUpdate`             |
| `serviveAccount.create`                        | If true, a service account would be created and assigned to the deployment                                                                                                                                                                        | true                        |
| `serviveAccount.name`                          | The service account to assign to the deployment. If `serviveAccount.create` is true then this name will be used when creating the service account                                                                                                 |                             |

## Example

```sh
helm install ./helm/fluentd-helm \
--set licenseKey=(your-license-key)
```

## Resources

The default set of resources assigned to the pods is shown below:

```yaml
resources:
  limits:
    cpu: 500m
    memory: 128Mi
  requests:
    cpu: 250m
    memory: 64Mi
```

## Tolerations

The default set of tolerations assigned to our daemonset is shown below:

```yaml
tolerations:
  - operator: "Exists"
    effect: "NoSchedule"
  - operator: "Exists"
    effect: "NoExecute"
```

## Legal

This project is provided AS-IS WITHOUT WARRANTY OR SUPPORT, although you can report issues and contribute to the project here on GitHub.

# fluentd-helm

This Helm chart deploys Fluentd as a DaemonSet for New Relic Logging. This chart was created specifically for customers who need to capture logs and Amazon EC2 tags from Amazon EKS deployments

## Deploying the Helm Chart

- Clone this repo
- Make sure you have the latest version of helm: `helm repo update`
- Export your license key: `export NEW_RELIC_INSIGHTS_INSERT_KEY="<INSERT YOUR NEW RELIC INSIGHTS API KEY>"`
- Deploy the chart using your New Relic license key: `helm install --set nrInsightsInsertKey=$NEW_RELIC_INSIGHTS_INSERT_KEY --name <INSERT NAME> .`
- Check the New Relic Logs for your logs and AWS EC2 tags

## Chart Details

See [values.yaml](../../blob/master/newrelic-logging/values.yaml) for the default values

| Parameter               | Description                                                                                                                                       | Default                                                                                                                                                                                                                                           |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| UPDATE AND FIX THIS     | `global.licenseKey` - `licenseKey`                                                                                                                | The [license key](https://docs.newrelic.com/docs/accounts/install-new-relic/account-setup/license-key) for your New Relic Account. This will be the preferred configuration option if both `licenseKey` and `customSecret*` values are specified. |  |
| `rbac.create`           | Enable Role-based authentication                                                                                                                  | `true`                                                                                                                                                                                                                                            |
| `image.repository`      | The container to pull.                                                                                                                            | `newrelic/fluentd`                                                                                                                                                                                                                                |
| `image.pullPolicy`      | The pull policy.                                                                                                                                  | `IfNotPresent`                                                                                                                                                                                                                                    |
| `image.tag`             | The version of the container to pull.                                                                                                             | See value in [values.yaml]`                                                                                                                                                                                                                       |
| `resources`             | Any resources you wish to assign to the pod.                                                                                                      | See Resources below                                                                                                                                                                                                                               |
| `priorityClassName`     | Scheduling priority of the pod                                                                                                                    | `nil`                                                                                                                                                                                                                                             |
| `nodeSelector`          | Node label to use for scheduling                                                                                                                  | `nil`                                                                                                                                                                                                                                             |
| `tolerations`           | List of node taints to tolerate (requires Kubernetes >= 1.6)                                                                                      | See Tolerations below                                                                                                                                                                                                                             |
| `updateStrategy`        | Strategy for DaemonSet updates (requires Kubernetes >= 1.6)                                                                                       | `RollingUpdate`                                                                                                                                                                                                                                   |
| `serviceAccount.create` | If true, a service account would be created and assigned to the deployment                                                                        | true                                                                                                                                                                                                                                              |
| `serviceAccount.name`   | The service account to assign to the deployment. If `serviceAccount.create` is true then this name will be used when creating the service account |                                                                                                                                                                                                                                                   |

## Examples

### Image

The default image and repository:

```yaml
image:
  repository: newrelic/fluentd
  tag: 1.0
  pullPolicy: IfNotPresent
```

### Tolerations

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

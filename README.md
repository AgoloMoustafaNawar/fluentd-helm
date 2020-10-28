![Archived header](https://github.com/newrelic/opensource-website/raw/master/src/images/categories/Archived.png)](https://opensource.newrelic.com/oss-category/#archived)

# fluentd-helm

This Helm chart deploys Fluentd as a DaemonSet for New Relic Logging. This chart was created specifically for customers who use fluentd to capture logs and Amazon EC2 tags from their Kubernetes environments

## Deploying the Helm Chart

- Clone this repo
- Edit the [configmap.yaml](../../blob/master/newrelic-logging/templates/configmap.yaml) to define the EC2 tags you want to capture as well as the log configurations such as log source, patterns, etc ... that you want to use
- Export your license key: `export NEW_RELIC_LICENSE_KEY="<INSERT YOUR NEW RELIC LICENSE KEY>"`
- Deploy the chart using your New Relic license key (assumes helm v2.x):  
  `helm install --set licenseKey=$NEW_RELIC_LICENSE_KEY --name fluentd .`
- If your are using Helm v3.x you don't need to use the --name flag:  
  `helm install --set licenseKey=$NEW_RELIC_LICENSE_KEY fluentd .`
- Check New Relic Logs for your logs and AWS EC2 tags

## Chart Details

See [values.yaml](../../blob/master/newrelic-logging/values.yaml) for the default values

| Parameter               | Description                                                                                                                                       | Default                     |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------- |
| `global.licenseKey`     | [license key](https://docs.newrelic.com/docs/accounts/install-new-relic/account-setup/license-key) for your New Relic Account.                    |
| `rbac.create`           | Enable Role-based authentication                                                                                                                  | `true`                      |
| `image.repository`      | The container to pull.                                                                                                                            | `newrelic/fluentd`          |
| `image.pullPolicy`      | The pull policy.                                                                                                                                  | `IfNotPresent`              |
| `image.tag`             | The version of the container to pull.                                                                                                             | See value in [values.yaml]` |
| `resources`             | Any resources you wish to assign to the pod.                                                                                                      | See Resources below         |
| `priorityClassName`     | Scheduling priority of the pod                                                                                                                    | `nil`                       |
| `nodeSelector`          | Node label to use for scheduling                                                                                                                  | `nil`                       |
| `tolerations`           | List of node taints to tolerate (requires Kubernetes >= 1.6)                                                                                      | See Tolerations below       |
| `updateStrategy`        | Strategy for DaemonSet updates (requires Kubernetes >= 1.6)                                                                                       | `RollingUpdate`             |
| `serviceAccount.create` | If true, a service account would be created and assigned to the deployment                                                                        | true                        |
| `serviceAccount.name`   | The service account to assign to the deployment. If `serviceAccount.create` is true then this name will be used when creating the service account |                             |

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

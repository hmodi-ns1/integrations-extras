# Agent Check: Aqua

## Overview

This check monitors [Aqua][1].

The Aqua check will alert the user if total high-severity vulnerability is reached, or if a container is running inside a host not registered by Aqua. Aqua will also send data alerts regarding blocked events in runtime, and it is possible to trigger a webhook to scale infrastructure if more Aqua scanners are required.

## Setup

The Aqua check is not included in the [Datadog Agent][2] package, so you will
need to install it yourself.

### Installation

To install the Aqua check on your host:

1. [Download the Datadog Agent][2].
2. Download the [`aqua.py` file][8] for Aqua.
3. Place it in the Agent's `checks.d` directory.

### Configuration


1. Edit the `aqua.d/conf.yaml` file in the `conf.d/` folder at the root of your [Agent's configuration directory](https://docs.datadoghq.com/agent/faq/agent-configuration-files/#agent-configuration-directory) to start collecting your Aqua [metrics](#metric-collection) and [logs](#log-collection).
  See the [sample conf.yaml][3] for all available configuration options.
  
#### Metric Collection

1. Add this configuration block to your `aqua.d/conf.yaml` file to start gathering your [Aqua Metrics](#metrics):

```
instances:
  - url: http://your-aqua-instance.com
    api_user: <api_username>
    password: <api_user_password>
```    

Change the `api_user` and `password` parameter values and configure them for your environment.

[Restart the Agent][4].

#### Log Collection

There are two types of logs generated by Aqua: 

* Aqua audit logs
* Aqua enforcer logs

To collect Aqua audit logs:

1. Connect to your Aqua account
2. Go to the `Log Management` Section of the `Integration` Page
3. Activate the Webhook integration
4. Enable it and add the following endpoint: `https://http-intake.logs.datadoghq.com/v1/input/<DATADOG_API_KEY>?ddsource=aqua`

* Replace `<DATADOG_API_KEY>` by your [Datadog Api Key](https://app.datadoghq.com/account/settings#api).
* *Note*: For the EU region, replace `.com` by `.eu` in the endpoint.

For the Aqua Enforcer logs: **Available for Agent >6.0**

* Collecting logs is disabled by default in the Datadog Agent. Enable it in your [daemonset configuration](https://docs.datadoghq.com/agent/kubernetes/daemonset_setup/#log-collection):

```
(...)
  env:
    (...)
    - name: DD_LOGS_ENABLED
        value: "true"
    - name: DD_LOGS_CONFIG_CONTAINER_COLLECT_ALL
        value: "true"
(...)
```

* Make sure that the Docker socket is mounted to the Datadog Agent as done in [this manifest](https://docs.datadoghq.com/agent/kubernetes/daemonset_setup/#create-manifest).

* [Restart the Agent][4].


### Validation

[Run the Agent's `status` subcommand][5] and look for `aqua` under the Checks section.

## Data Collected

### Metrics

See [metadata.csv][6] for a list of metrics provided by this integration.

### Service Checks

**aqua.can_connect**:

Returns CRITICAL if the Agent cannot connect to Aqua to collect metrics. Returns OK otherwise.

### Events

Aqua does not include any events.

## Troubleshooting

Need help? Contact [Datadog support][7].

[1]: https://www.aquasec.com
[2]: https://app.datadoghq.com/account/settings#agent
[3]: https://github.com/DataDog/integrations-extras/blob/master/aqua/datadog_checks/aqua/data/conf.yaml.example
[4]: https://docs.datadoghq.com/agent/faq/agent-commands/#start-stop-restart-the-agent
[5]: https://docs.datadoghq.com/agent/faq/agent-commands/#agent-status-and-information
[6]: https://github.com/DataDog/integrations-extras/blob/master/aqua/metadata.csv
[7]: https://docs.datadoghq.com/help/
[8]: https://github.com/DataDog/integrations-extras/blob/master/aqua/datadog_checks/aqua/aqua.py
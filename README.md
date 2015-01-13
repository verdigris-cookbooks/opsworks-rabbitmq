# opsworks_rabbitmq

This is a wrapper cookbook which utilizes the [rabbitmq cookbook](https://github.com/jjasghar/rabbitmq)
for installing [RabbitMQ](https://www.rabbitmq.com/) on AWS OpsWorks with
support for automatic clustering from layer instances.

The recipes included in this cookbook is based on the recipes provided by
the community [rabbitmq cookbook](https://github.com/jjasghar/rabbitmq) with
modifications which provide better recipe granularity for AWS OpsWorks lifecycle
events.

## Requirements

* **erlang** (≥ 0.9)
* **rabbitmq** (≥ 3.7)

## Attributes

| Key                                      |  Type  | Description                                                       | Default    |
|:-----------------------------------------|:------:|:------------------------------------------------------------------|:-----------|
| `['rabbitmq']['opsworks']['layer_name']` | String | OpsWorks stack's layer shortname that contains RabbitMQ instances | `rabbitmq` |

## Recipes

Most of the recipes included by this cookbook used to be in `rabbitmq::default`
on the original [rabbitmq cookbook](https://github.com/jjasghar/rabbitmq). They
have been mostly broken up into two recipes to be executed during AWS OpsWorks
instance lifecycle events.

### opsworks_rabbitmq::install

Installs **[RabbitMQ](https://www.rabbitmq.com)**.

Configure your stack layer to run `opsworks_rabbitmq::install` during **Setup**
lifecycle event in your layer instances.

### opsworks_rabbitmq::configure

Sets up directory permissions and generate configuration files from templates.
If clustering is enabled, it will also run `opsworks_rabbitmq::cluster` recipe.

Configure your stack layer to run `opsworks_rabbitmq::configure` during
**Configure** lifecycle event in your layer instances. See [below](#opsworks_rabbitmqcluster)
for more information on why.

> **NOTE:** When clustering is *disabled*, it should not matter whether this
recipe is configured to run during **Setup** or **Configure** lifecycle event
as long as it runs after `rabbitmq::install` recipe.

### opsworks_rabbitmq::cluster

Automatically gathers all instances in your stack layer and automatically
generates the `node['rabbitmq']['cluster_disk_nodes']` list required for
clustering setup in `opsworks_rabbitmq::configure`.

Because the list of nodes needs to be updated when a layer instance goes online
or shuts down, this recipe must run during **Configure** lifecycle event in
your layer instances.

## License

This cookbook is license and distributed under the Simplified BSD license.
See [LICENSE](https://github.com/verdigris-cookbooks/opsworks-rabbitmq/blob/master/LICENSE)
for more details.

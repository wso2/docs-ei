# Tuning Inbound Endpoints

## Tuning the HTTP Inbound

By default inbound endpoints share the PassThrough transport worker pool to handle incoming requests. If you need a separate worker pool for the inbound endpoint to increase the performance, you need to configure the [HTTP worker pool parameters](../../../references/synapse-properties/inbound-endpoint-properties/#httphttps-worker-pool-configuration-properties).

## Tuning the Kafka inbound

Open the deployment.toml file, and change the inbound thread pool size based on your use case. Recommended values are specified below.

```toml
[[mediation]]
inbound.threads.core = 200 
inbound.threads.max = 1000   
```
See the [descriptions of the parameters](../../../references/config-catalog/#mediation-process).

When you create your inbound endpoint, set the [sequential](../../../references/synapse-properties/inbound-endpoint-properties/#kafka-inbound-required-properties) parameter to `false` to use the Kafka inbound in a non-sequential mode as it allows better performance than the sequential mode.

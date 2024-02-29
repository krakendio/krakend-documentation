---
lastmod: 2024-01-29
date: 2024-01-22
notoc: false
linktitle: OpenTelemetry - Layers and Metrics
title: Understanding OpenTelemetry layers and metrics
description: Learn about the metrics available when activating OpenTelemetry on KrakenD API Gateway, enabling real-time visibility and analysis of API performance
weight: 21
#images:
#- /images/documentation/available-exporters.png
menu:
  community_current:
    parent: "160 Monitoring, Logs, and Analytics"
---
You can add several `exporters` to your [OpenTelemetry configuration](/docs/telemetry/opentelemetry/#opentelemetry-configuration) (the more, the hungrier the gateway will be), and KrakenD will send data to all the declared exporters and layers by default.

While `exporters` define **where** you will have the metrics, the `layers` define **which** metrics you want to have. The layers contain the traces and metrics for a subset of the execution flow. These are the layers you can use:

![Diagram showing global, proxy, and backend sequence](/images/documentation/diagrams/opentelemetry-layers.mmd.svg)

- `global`: The global layer contains everything that KrakenD saw in and out. It includes the total timings for a request hitting the service until it is delivered to the client.
- `proxy`: The proxy layer starts acting after processing the HTTP request and comprehends the internal work of dealing with the endpoint, including spawning multiple requests to the backends, aggregating their results, and manipulating the final response.
- `backend`: The backend layer monitors the activity of a single backend request and response with any additional components in its level. It contains mainly the timings between KrakenD and your service. It is the richest layer of all.

**It is vital to see that `backend` is a subset of `proxy`, and `proxy` is a subset of `global.`**. Do not generate metrics for the layers you will not use.

{{< note title="CPU and memory consumption" type="warning" >}}
The more layers and details you add, the more resources KrakenD will consume and the more storage you need to save the metrics and traces. Choose carefully a balance between observability power and resource consumption to save money!
{{< /note >}}

Once the OpenTelemetry component is properly configured, KrakenD will start reporting metrics when there is activity. The section below describes the different metrics you have in each layer with a short explanation of their meaning.

{{% otel_metrics %}}
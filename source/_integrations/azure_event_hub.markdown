---
title: Azure Event Hub
description: Setup for Azure Event Hub integration
logo: azure_event_hub.svg
ha_category:
  - History
ha_release: 0.94
ha_codeowners:
  - '@eavanvalkenburg'
---

The `Azure Event Hub` integration allows you to hook into the Home Assistant event bus and send events to [Azure Event Hub](https://azure.microsoft.com/en-us/services/event-hubs/) or to a [Azure IoT Hub](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-messages-read-builtin). 

## First time setup

This assumes you already have a Azure account. Otherwise create a Free account [here](https://azure.microsoft.com/en-us/free/).

You need to create a Event Hub namespace and a Event Hub in that namespace, you can follow [this guide](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-create). Alternatively you can directly deploy an ARM template with the namespace and the Event Hub [from here](https://github.com/Azure/azure-quickstart-templates/tree/master/201-event-hubs-create-event-hub-and-consumer-group/).

You must then create a Shared Access Policy for the Event Hub with 'Send' claims or use the RootManageAccessKey from your namespace (this key has additional claims, including managing the event hub and listening, which are not needed for this purpose), for more details on the security of Event Hubs [go here](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-authentication-and-security-model-overview).

Once you have the name of your namespace, instance, Shared Access Policy and the key for that policy, you can setup the integration itself.

## Configuration

Add the following lines to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
azure_event_hub:
  event_hub_namespace: NAMESPACE_NAME
  event_hub_instance_name: EVENT_HUB_INSTANCE_NAME
  event_hub_sas_policy: SAS_POLICY_NAME
  event_hub_sas_key: SAS_KEY
  filter:
    include_domains:
    - homeassistant
    - light
    - media_player
```

{% configuration %}
event_hub_namespace:
  description: The name of your Event Hub namespace.
  required: true
  type: string
event_hub_instance_name:
  description: The name of your Event Hub instance.
  required: true
  type: string
event_hub_sas_policy:
  description: The name of your Shared Access Policy.
  required: true
  type: string
event_hub_sas_key:
  description: The key for the Shared Access Policy.
  required: true
  type: string
filter:
  description: Filter domains and entities for Event Hub.
  required: true
  type: map
  default: Includes all entities from all domains
  keys:
    include_domains:
      description: List of domains to include (e.g., `light`).
      required: false
      type: list
    exclude_domains:
      description: List of domains to exclude (e.g., `light`).
      required: false
      type: list
    include_entities:
      description: List of entities to include (e.g., `light.attic`).
      required: false
      type: list
    exclude_entities:
      description: List of entities to include (e.g., `light.attic`).
      required: false
      type: list
{% endconfiguration %}

<div class='note warning'>
Not filtering domains or entities will send every event to Azure Event Hub, thus taking up a lot of space. 
</div>

<div class='note warning'>
Event Hubs have a retention time of at most 7 days, if you do not capture or use the events they are deleted automatically from the Event Hub, the default retention is 1 day.
</div>

### Using the data in Azure

There are a number of ways to stream the data that comes into the Event Hub into storages in Azure, the easiest way is to use the built-in Capture function and this allows you to capture the data in Azure Blob Storage or Azure Data Lake store, [details here](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-capture-overview).

Other storages in Azure (and outside) are possible with a [Azure Stream Analytics job](https://docs.microsoft.com/en-us/azure/stream-analytics/stream-analytics-define-inputs#stream-data-from-event-hubs), for instance for [Cosmos DB](https://docs.microsoft.com/en-us/azure/stream-analytics/stream-analytics-documentdb-output), [Azure SQL DB](https://docs.microsoft.com/en-us/azure/stream-analytics/stream-analytics-sql-output-perf), [Azure Table Storage](https://docs.microsoft.com/en-us/azure/stream-analytics/stream-analytics-define-outputs#table-storage), custom writing to [Azure Blob Storage](https://docs.microsoft.com/en-us/azure/stream-analytics/stream-analytics-custom-path-patterns-blob-storage-output) and [Topic and Queues](https://docs.microsoft.com/en-us/azure/stream-analytics/stream-analytics-quick-create-portal#configure-job-output).

On the analytical side, Event Hub can be directly fed into [Azure Databricks Spark](https://docs.microsoft.com/en-us/azure/azure-databricks/databricks-stream-from-eventhubs?toc=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure%2Fevent-hubs%2FTOC.json&bc=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure%2Fbread%2Ftoc.json), [Azure Time Series Insights](https://docs.microsoft.com/en-us/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-eventhub) and [Microsoft Power BI](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-tutorial-visualize-anomalies).

The final way to use the data in Azure is to connect a Azure Function to the Event Hub using the [Event Hub trigger binding](https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-event-hubs).

# Azure IoT Operations through Polyglot Notebooks

This repository contains a set of Polyglot Notebooks that demonstrate how to use Azure IoT Operations using Codespaces.

## What is Azure IoT Operations?

> **IMPORTANT:** Azure IoT Operations Preview – enabled by Azure Arc is currently in PREVIEW.
You shouldn't use this preview software in production environments.
>
> See the [Supplemental Terms of Use for Microsoft Azure Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) for legal terms that apply to Azure features that are in beta, preview, or otherwise not yet released into general availability.

_Azure IoT Operations Preview_ is a unified data plane for the edge. It's composed of a set of modular, scalable, and highly available data services that run on Azure Arc-enabled edge Kubernetes clusters. It enables data capture from various different systems and integrates with data modeling applications such as Microsoft Fabric to help organizations deploy the industrial metaverse.

Azure IoT Operations:

* Is built from ground up by using Kubernetes native applications.
* Includes an industrial-grade, edge-native MQTT broker that powers event-driven architectures.
* Is highly extensible, scalable, resilient, and secure.
* Lets you manage all edge services from the cloud by using Azure Arc.
* Can integrate customer workloads into the platform to create a unified solution.
* Supports GitOps configuration as code for deployment and updates.
* Natively integrates with [Azure Event Hubs](/azure/event-hubs/azure-event-hubs-kafka-overview), [Azure Event Grid's MQTT broker](/azure/event-grid/mqtt-overview), and [Microsoft Fabric](/fabric/) in the cloud.

## Features

This project framework provides the following features:

* **Azure IoT Operations Preview**. The set of data services that run on Azure Arc-enabled edge Kubernetes clusters. It includes the following services:
* **Azure IoT Data Processor Preview** - a configurable data processing service that can manage the complexities and diversity of industrial data. Use Data Processor to make data from disparate sources more understandable, usable, and valuable.
* **Azure IoT MQ Preview** - an edge-native MQTT broker that powers event-driven architectures.
* **Azure IoT OPC UA Broker Preview** - an OPC UA broker that handles the complexities of OPC UA communication with OPC UA servers and other leaf devices.
* **Azure IoT Operations Experience Preview portal**. This web UI provides a unified experience for operational technologists to manage assets and Data Processor pipelines in an Azure IoT Operations deployment.

## Getting Started

### Prerequisites

- Azure Subscription
- Codespaces

### Quickstart

1. Create codespace from [https://github.com/Azure-Samples/azure-edge-extensions-polyglotnotebook-aio]
1. git checkout -b <mybranch>

## Resources

(Any additional resources or related projects)

- [Azure IoT Operations](https://docs.microsoft.com/azure/iot-operations/overview-iot-operations)
- [Azure IoT Operations CLI](https://docs.microsoft.com/cli/azure/ext/azure-iot-ops/iot/ops?view=azure-cli-latest)

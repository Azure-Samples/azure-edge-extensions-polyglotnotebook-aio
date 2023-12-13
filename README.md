# Azure IoT Operations through Polyglot Notebooks

This repository contains a set of [Polyglot Notebooks](./notebooks/) that demonstrate how to use Azure IoT Operations using Codespaces.

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

* An Azure subscription. If you don't have an Azure subscription, [create one for free](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.

* A [GitHub](https://github.com) account.

### Quickstart

Use GitHub Codespaces to try Azure IoT Operations on a Kubernetes cluster without installing anything on your local machine. Use the **explore-iot-operations** codespace that is preconfigured with:

- [K3s](https://k3s.io/) running in [K3d](https://k3d.io/) for a lightweight Kubernetes cluster
- [Azure CLI](/cli/azure/install-azure-cli)
- [Kubectl](https://kubernetes.io/docs/tasks/tools/) for managing Kubernetes resources
- Other useful tools like [Helm](https://helm.sh/) and [k9s](https://k9scli.io/)

> [!IMPORTANT]
> Codespaces are easy to set up quickly and tear down later, but they're not suitable for performance evaluation or scale testing. Use GitHub Codespaces for exploration only.

To get started with your codespace:

1. Create the codespace, entering your Azure details to store them as environment variables for the terminal.

   [![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://codespaces.new/Azure-Samples/azure-edge-extensions-polyglotnotebook-aio?quickstart=1)

1. Your codespace should open automatically. If not, open it from the **Codespaces** tab in GitHub.

## Next Steps
1. Go to the [Quickstart Deploy notebook](./notebooks/get-started/quickstart-virtual-deploy.ipynb) to get started.

## Resources

(Any additional resources or related projects)

- [Azure IoT Operations](https://docs.microsoft.com/azure/iot-operations/overview-iot-operations)
- [Azure IoT Operations CLI](https://docs.microsoft.com/cli/azure/ext/azure-iot-ops/iot/ops?view=azure-cli-latest)

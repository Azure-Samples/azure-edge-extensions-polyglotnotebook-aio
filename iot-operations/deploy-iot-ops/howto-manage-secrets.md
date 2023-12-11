---
title: Manage secrets - Azure IoT Operations
description: Create, update, and manage secrets that are required to give your Arc-connected cluster access to Azure resources.
author: kgremban
ms.author: kgremban
# ms.subservice: orchestrator
ms.topic: how-to
ms.date: 12/06/2023
ms.custom:
  - ignite-2023

#CustomerIntent: As an IT professional, I want prepare an Azure-Arc enabled Kubernetes cluster with Key Vault secrets so that I can deploy Azure IoT Operations to it.
---

# Manage secrets for your Azure IoT Operations deployment

Secrets management in Azure IoT Operations uses Azure Key Vault as the managed vault solution on the cloud and uses the secrets store CSI driver to pull secrets down from the cloud and store them on the edge.

## Prerequisites

* An Arc-enabled Kubernetes cluster. For more information, see [Prepare your cluster](./howto-prepare-cluster.md).

## Configure a secret store on your cluster

Azure IoT Operations supports Azure Key Vault for storing secrets and certificates. The `az iot ops init` Azure CLI command automates the steps to create a key vault, set up a service principal to give access to the key vault, and configure the secrets that you need for running Azure IoT Operations.

For more information, see [Deploy Azure IoT Operations extensions](./howto-deploy-iot-operations.md?tabs=cli).

## Add a secret to an Azure IoT Operations component

Once you have the secret store set up on your cluster, you can create and add Azure Key Vault secrets.

1. Create your secret in Key Vault with whatever name and value you need. You can create a secret by using the [Azure portal](https://portal.azure.com) or the [az keyvault secret set](/cli/azure/keyvault/secret#az-keyvault-secret-set) command.

1. On your cluster, identify the secret provider class (SPC) for the component that you want to add the secret to. For example, `aio-default-spc`.

1. Open the file in your preferred text editor. If you use k9s, type `e` to edit.

1. Add the secret object to the list under `spec.parameters.objects.array`. For example:

   ```yml
   spec:
     parameters:
       keyvaultName: my-key-vault
       objects: |
         array:
           - |
             objectName: PlaceholderSecret
             objectType: secret
             objectVersion: ""
   ```

1. Save your changes and apply them to your cluster. If you use k9s, your changes are automatically applied.

The CSI driver updates secrets according to a polling interval, so a new secret won't be updated on the pods until the next polling interval. If you want the secrets to be updated immediately, update the pods for that component. For example, for the Azure IoT Data Processor component, update the `aio-dp-reader-worker-0` and `aio-dp-runner-worker-0` pods.

## Azure IoT MQ secrets

The steps to manage secrets with Azure Key Vault for Azure IoT MQ are different. For more information, see [Manage Azure IoT MQ secrets using Azure Key Vault](../manage-mqtt-connectivity/howto-manage-secrets.md).

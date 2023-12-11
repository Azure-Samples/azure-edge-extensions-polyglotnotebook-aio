---
title: "Known issues: Azure IoT Operations"
description: A list of known issues for Azure IoT Operations.
author: dominicbetts
ms.author: dobett
ms.topic: troubleshooting-known-issue
ms.custom:
  - ignite-2023
ms.date: 11/15/2023
---

# Known issues: Azure IoT Operations

[!INCLUDE [public-preview-note](../includes/public-preview-note.md)]

This article contains known issues for Azure IoT Operations Preview.

## Azure IoT Operations

- You must use the Azure CLI interactive login `az login`. If you don't, you might see an error such as _ERROR: AADSTS530003: Your device is required to be managed to access this resource_.

- Uninstalling K3s: When you uninstall k3s on Ubuntu by using the `/usr/local/bin/k3s-uninstall.sh` script, you may encounter an issue where the script gets stuck on unmounting the NFS pod. A workaround for this issue is to run the following command before you run the uninstall script: `sudo systemctl stop k3s`.

## Azure IoT MQ (preview)

- You can only access the default deployment by using the cluster IP, TLS, and a service account token. Clients outside the cluster need extra configuration before they can connect.

- You can't update the Broker custom resource after the initial deployment. You can't make configuration changes to cardinality, memory profile, or disk buffer.

- You can't configure the size of a disk-backed buffer unless your chosen storage class supports it.

- Resource synchronization isn't currently supported: updating a custom resource in the cluster doesn't reflect to Azure.

- QoS2 isn't currently available.

- Full persistence support isn't currently available.

- There are known intermittent issues with IoT MQ's MQTT bridge connecting to Azure Event Grid.

- It's possible for an IoT MQ pod to fail to reconnect if it loses connection to other pods in the cluster. You might also see errors such as `invalid sat: [invalid bearer token, service account token has expired]`. If you notice this happening, run the following command, to manually restart the affected pods:

    ```bash
    kubectl -n azure-iot-operations delete pods <pod-name>
    ```

- Even though IoT MQ's [diagnostic service](../monitor/howto-configure-diagnostics.md) produces telemetry on its own topic, you might still get messages from the self-test when you subscribe to `#` topic.

- You can't currently access these [observability metrics](../reference/observability-metrics-mq.md) for IoT MQ.

    - aio_mq_backend_replicas
    - aio_mq_backend_replicas_current
    - aio_mq_frontend_replicas
    - aio_mq_frontend_replicas_current

## Azure IoT Data Processor (preview)

If edits you make to a pipeline aren't applied to messages, run the following commands to propagate the changes:

```bash
kubectl rollout restart deployment aio-dp-operator -n azure-iot-operations 

kubectl rollout restart statefulset aio-dp-runner-worker -n azure-iot-operations 

kubectl rollout restart statefulset aio-dp-reader-worker -n azure-iot-operations
```

It's possible a momentary loss of communication with IoT MQ broker pods can pause the processing of data pipelines. You might also see errors such as `service account token expired`. If you notice this happening, run the following commands:

```bash
kubectl rollout restart statefulset aio-dp-runner-worker -n azure-iot-operations
kubectl rollout restart statefulset aio-dp-reader-worker -n azure-iot-operations
```

## Layered Network Management (preview)

- If the Layered Network Management service isn't getting an IP address while running K3S on Ubuntu host, reinstall K3S without _trafeik ingress controller_ by using the `--disable=traefik` option.

    ```bash
    curl -sfL https://get.k3s.io | sh -s - --disable=traefik --write-kubeconfig-mode 644
    ```

    For more information, see [Networking | K3s](https://docs.k3s.io/networking#traefik-ingress-controller).

- If DNS queries aren't getting resolved to expected IP address while using [CoreDNS](../manage-layered-network/howto-configure-layered-network.md#configure-coredns) service running on child network level, upgrade to Ubuntu 22.04 and reinstall K3S.

## OPC UA Broker (preview)

- Users are unable to add assets when there's more than one _AssetEndpointProfile_ for the OPC UA Connector. Resolution: You need to recreate the cluster.
- If more than one transport authentication certificate is available, the OPC UA Broker might exhibit random behavior. Resolution: Provide only one certificate for transport authentication.
- When you adjust the publishing interval in the Digital Operations Experience portal, the OPC UA Broker continues to use the default settings.
- When you delete an asset in the Digital Operations Experience portal, the asset isn't removed from the cluster. Resolution: manually delete the retained messages associated with the asset from MQ and then restart OPC UA Connector pod (opc.tcp-1).
- Configuration of OPC UA user authentication with an X.509 user certificate isn't currently supported.
- Configuration of OPC UA issuer and trust lists isn't yet supported.
- Support for QoS1 isn't available.

## OPC PLC simulator

If you create an asset endpoint for the OPC PLC simulator, but the OPC PLC simulator isn't sending data to the IoT MQ broker, try the following command:

- Patch the asset endpoint with `autoAcceptUntrustedServerCertificates=true`:

```bash
ENDPOINT_NAME=<name-of-you-endpoint-here>
kubectl patch AssetEndpointProfile $ENDPOINT_NAME \
-n azure-iot-operations \
--type=merge \
-p '{"spec":{"additionalConfiguration":"{\"applicationName\":\"'"$ENDPOINT_NAME"'\",\"security\":{\"autoAcceptUntrustedServerCertificates\":true}}"}}'
```

You can also patch all your asset endpoints with the following command:

```bash
ENDPOINTS=$(kubectl get AssetEndpointProfile -n azure-iot-operations --no-headers -o custom-columns=":metadata.name")
for ENDPOINT_NAME in `echo "$ENDPOINTS"`; do \
kubectl patch AssetEndpointProfile $ENDPOINT_NAME \
   -n azure-iot-operations \
   --type=merge \
   -p '{"spec":{"additionalConfiguration":"{\"applicationName\":\"'"$ENDPOINT_NAME"'\",\"security\":{\"autoAcceptUntrustedServerCertificates\":true}}"}}'; \
done
```

> [!WARNING]
> Don't use untrusted certificates in production environments.

If the OPC PLC simulator isn't sending data to the IoT MQ broker after you create a new asset, restart the OPC PLC simulator pod. The pod name looks like `aio-opc-opc.tcp-1-f95d76c54-w9v9c`. To restart the pod, use the `k9s` tool to kill the pod, or run the following command:

```bash
kubectl delete pod aio-opc-opc.tcp-1-f95d76c54-w9v9c -n azure-iot-operations
```

## Azure IoT Operations (preview) portal

To sign in to the Azure IoT Operations portal, you need a Microsoft Entra ID with at least contributor permissions for the resource group that contains your **Kubernetes - Azure Arc** instance. You can't sign in with a Microsoft account (MSA). To create an Entra ID in your Azure tenant:

1. Sign in to the [Azure portal](https://portal.azure.com/) with the same tenant and user name that you used to deploy Azure IoT Operations.
1. In the Azure portal, navigate to the **Microsoft Entra ID** section, select **Users > +New user > Create new user**. Create a new user and make a note of the password, you need it to sign in later.
1. In the Azure portal, navigate to the resource group that contains your **Kubernetes - Azure Arc** instance. On the **Access control (IAM)** page, select **+Add > Add role assignment**.
1. On the **Add role assignment page**, select **Privileged administrator roles**. Then select **Contributor** and then select **Next**.
1. On the **Members** page, add your new user to the role.
1. Select **Review and assign** to complete setting up the new user.

You can now use the new user account to sign in to the [Azure IoT Operations portal](https://iotoperations.azure.com).

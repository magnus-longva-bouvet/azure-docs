---
title: Manage protocols and ciphers in Azure API Management | Microsoft Learn
description: Learn how to manage transport layer security (TLS) protocols and cipher suites in Azure API Management.
services: api-management
author: dlepow

ms.service: api-management
ms.topic: how-to
ms.date: 09/22/2022
ms.author: danlep
---

# Manage protocols and ciphers in Azure API Management

Azure API Management supports multiple versions of Transport Layer Security (TLS) protocol to secure API traffic for:
* Client side
* Backend side

API Management also supports multiple cipher suites used by the API gateway.

By default, API Management enables TLS 1.2 for client and backend connectivity and several supported cipher suites. This guide shows you how to manage protocols and ciphers configuration for an Azure API Management instance.

:::image type="content" source="media/api-management-howto-manage-protocols-ciphers/api-management-protocols-ciphers.png" alt-text="Screenshot of managing protocols and ciphers in the Azure portal.":::


> [!NOTE]
> * If you're using the self-hosted gateway, see [self-hosted gateway security](self-hosted-gateway-overview.md#security) to manage TLS protocols and cipher suites.
> * Currently, API Management doesn't support TLS 1.3.
> * The Consumption tier doesn't support changes to the default cipher configuration. 

## Prerequisites

* An API Management instance. [Create one if you haven't already](get-started-create-service-instance.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## How to manage TLS protocols cipher suites

1. In the left navigation of your API Management instance, under **Security**, select **Protocols + ciphers**.  
1. Enable or disable desired protocols or ciphers.
1. Select **Save**. Changes are applied within an hour.  

> [!NOTE]
> Some protocols or cipher suites (such as backend-side TLS 1.2) can't be enabled or disabled from the Azure portal. Instead, you'll need to apply the REST API call. Use the `properties.customProperties` structure in the [Create/Update API Management Service](/rest/api/apimanagement/current-ga/api-management-service/create-or-update) REST API.

## Next steps

* For recommendations on securing your API Management instance, see [Azure security baseline for API Management](/security/benchmark/azure/baselines/api-management-security-baseline).
* Learn about security considerations in the API Management [landing zone accelerator](/azure/cloud-adoption-framework/scenarios/app-platform/api-management/security).
* Learn more about [TLS](/dotnet/framework/network-programming/tls).

---
title: Indoor Maps wayfinding service
titleSuffix: Microsoft Azure Maps Creator
description: How to use the wayfinding service to plot and display routes for indoor maps in Microsoft Azure Maps Creator
author: stevemunk
ms.author: v-munksteve
ms.date: 10/25/2022
ms.topic: how-to
ms.service: azure-maps
services: azure-maps
---

# Indoor maps wayfinding service (preview)

The Azure Maps Creator [wayfinding service][wayfinding service] allows you to navigate from place to place anywhere within your indoor map. The service utilizes stairs and elevators to navigate between floors and provides guidance to help you navigate around physical obstructions. This article describes how to generate a path from a starting point to a destination point in a sample indoor map.

## Prerequisites

- Understanding of [Creator concepts](creator-indoor-maps.md).
- An Azure Maps Creator [dataset][dataset] and [tileset][tileset]. If you have never used Azure Maps Creator to create an indoor map, you might find the [Use Creator to create indoor maps](tutorial-creator-indoor-maps.md) tutorial helpful.

>[!IMPORTANT]
>
> - This article uses the `us.atlas.microsoft.com` geographical URL. If your Creator service wasn't created in the United States, you must use a different geographical URL. For more information, see [Access to Creator Services][how to manage access to creator services].
> - In the URL examples in this article you will need to:
>   - Replace `{Your-Azure-Maps-Subscription-key}` with your Azure Maps subscription key.
>   - Replace `{datasetId`} with your `datasetId`. For more information, see the [Check the dataset creation status][check dataset creation status] section of the *Use Creator to create indoor maps* tutorial.

## Create a routeset

A [routeset][routeset] is a collection of indoor map data that is used by the wayfinding service.

A routeset is created from a dataset, but is independent from that dataset. This means that if the dataset is deleted, the routeset continues to exist.

Once you've created a routeset, you can then use the wayfinding API to get a path from the starting point to the destination point within the facility.

To create a routeset:

1. Execute the following **HTTP POST request**:

    ```http
    https://us.atlas.microsoft.com/routesets?api-version=2022-09-01-preview&datasetID={datasetId}&subscription-key={Your-Azure-Maps-Subscription-key} 
    
    ```

1. Copy the value of the **Operation-Location** key from the response header.

This is the status URL that you'll use to check the status of the routeset creation in the next section.

### Check the routeset creation status and retrieve the routesetId

To check the status of the routeset creation process and retrieve the routesetId:

1. Execute the following **HTTP GET request**:

    ```http
    https://us.atlas.microsoft.com/routesets/operations/{operationId}?api-version=2022-09-01-preview&subscription-key={Your-Azure-Maps-Subscription-key} 
 
    ```

    > [!NOTE]
    > Get the `operationId` from the Operation-Location key in the response header when creating a new routeset.

1. Copy the value of the **Resource-Location** key from the responses header. This is the resource location URL and contains the `routesetId`, as shown below:

   > https://us.atlas.microsoft.com/routesets/**675ce646-f405-03be-302e-0d22bcfe17e8**?api-version=2022-09-01-preview

Make a note of the `routesetId`, it will be required parameter in all [wayfinding](#get-a-wayfinding-path) requests, and when your [Get the facility ID](#get-the-facility-id).

### Get the facility ID

The `facilityId`, a property of the routeset, is a required parameter when searching for a wayfinding path. Get the `facilityId` by querying the routeset.

1. Execute the following **HTTP GET request**:

    ```http
    https://us.atlas.microsoft.com/routesets/{routesetId}?api-version=2022-09-01-preview&subscription-key={Your-Azure-Maps-Subscription-key} 
 
    ```

1. The `facilityId` is a property of the `facilityDetails` object, which you can find in the response body of the routeset request, which is `FCL43` in the following example:

```json
{
    "routeSetId": "675ce646-f405-03be-302e-0d22bcfe17e8",
    "dataSetId": "eec3825c-620f-13e1-b469-85d2767c8a41",
    "created": "10/10/2022 6:58:32 PM +00:00",
    "facilityDetails": [
        {
            "facilityId": "FCL43",
            "levelOrdinals": [
                0,
                1
            ]
        }
    ],
    "creationMode": "Wall",
    "ontology": "facility-2.0"
}
```

## Get a wayfinding path

In this section, you’ll use the [wayfinding API][wayfinding API] to generate a path from the routeset you created in the previous section. The wayfinding API requires a query that contains start and end points in an indoor map, along with floor level ordinal numbers. For more information about Creator wayfinding, see [wayfinding][wayfinding] in the concepts article.

To create a wayfinding query:

1. Execute the following **HTTP GET request** (replace {routesetId} with the routesetId obtained in the [Check the routeset creation status](#check-the-routeset-creation-status-and-retrieve-the-routesetid) section and the {facilityId} with the facilityId obtained in the [Get the facility ID](#get-the-facility-id) section):

    ```http
    https://us.atlas.microsoft.com/wayfinding/path?api-version=2022-09-01-preview&subscription-key={Your-Azure-Maps-Subscription-key}&routesetid={routeset-ID}&facilityid={facility-ID}&fromPoint={lat,lon}&fromLevel={from-level}&toPoint={lat,lon}&toLevel={to-level}&minWidth={minimun-width}
    ```

    > [!TIP]
    > The `AvoidFeatures` parameter can be used to specify something for the wayfinding service to avoid when determining the path, such as elevators or stairs.

1. The details of the path and legs are displayed in the Body of the response.

The summary displays the estimated travel time in seconds for the total journey. In addition, the estimated time for each section of the journey is displayed at the beginning of each leg.

The wayfinding service calculates the path through specific intervening points. Each point is displayed, along with its latitude and longitude details.

<!-- TODO: ## Implement the wayfinding service in your map   (Refer to sample app once completed)  -->

[dataset]: creator-indoor-maps.md#datasets
[tileset]: creator-indoor-maps.md#tilesets
[routeset]: /rest/api/maps/v20220901preview/routeset
[wayfinding]: creator-indoor-maps.md#wayfinding-preview
[wayfinding API]: /rest/api/maps/v20220901preview/wayfinding
[how to manage access to creator services]: how-to-manage-creator.md#access-to-creator-services
[check dataset creation status]: tutorial-creator-indoor-maps.md#check-the-dataset-creation-status
[wayfinding service]: creator-indoor-maps.md#wayfinding-preview

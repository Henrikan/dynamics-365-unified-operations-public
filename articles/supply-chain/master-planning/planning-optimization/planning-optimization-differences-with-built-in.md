---
title: Differences between built-in master planning and Planning Optimization
description: This article lists features that Planning Optimization doesn't yet support and that aren't listed on the Planning Optimization fit analysis page.
author: t-benebo
ms.date: 07/30/2021
ms.topic: article
audience: Application User
ms.reviewer: kamaybac
ms.search.region: Global
ms.author: benebotg
ms.search.validFrom: 2021-07-30
ms.dyn365.ops.version: 10.0.21
---

# Differences between built-in master planning and Planning Optimization

[!include [banner](../../includes/banner.md)]

Planning Optimization results might differ from results from the built-in master planning engine. The differences can be caused by pending features. This article lists features that Planning Optimization doesn't yet support and that aren't listed on the **[Planning Optimization fit analysis](planning-optimization-fit-analysis.md)** page].

| Feature | Current behavior in Planning Optimization |
|---|---|
| Catch weight products | Catch-weight products are considered usual products.|
| Extensible dimensions | Extensible dimensions are not supported by Planning Optimization. When you use Planning Optimization, extensible dimensions are empty on planned orders, even when the **Coverage plan by dimension** checkbox is selected on the **Storage dimension groups** or **Tracking dimension groups** page. |
| Filtered production runs | For details, see [Production planning - Filters](production-planning.md#filters). |
| Forecast planning | Forecast planning isn't supported. We recommend that you use master planning where a forecast model is assigned to the master plan. |
| Number sequences for planned orders | Number sequences for planned orders aren't supported. Planned order numbers are generated on the service side. The planned order number is normally shown with 10 digits, but the sequence is actually built on 20 characters, with 10 digits allocated for the planning run count and the other 10 digits for the planned orders count. |
| Plan copy, delete plan, and plan version cleanup | <p>The following items are disabled under **Master planning \> Master planning \> Maintain plans** in the navigation pane:</p><ul><li>Plan copy</li><li>Delete plan</li><li>Plan version cleanup</li></ul> |
| Return orders | Return orders aren't considered. |
| Scheduling related features | For details, see [Scheduling with infinite capacity](infinite-capacity-planning.md#limitations). |
| Safety stock fulfillment | Planning Optimization always uses the *Today's date + procurement time* option for the **Fulfill minimum** field on the **Item coverage** page. This helps to prevent unwanted planned orders and other issues because if the procurement time isn't included for safety stock, planned orders that are created for low on-hand inventory would always be delayed because of the lead time. |
| Safety stock pegging and net requirements | The *Safety stock* requirement type isn't included and isn't displayed on the **Net requirements** page. Safety stock doesn't represent demand and doesn't have a requirement date associated with it. Instead, it sets a constraint on how much inventory must be present at all times. However, the **Minimum** field value is still taken into account when calculating planned orders during master planning. We suggest that you inspect the **Accumulated quantity** column on the **Net requirements** page to see that this value was considered. Because the pegging is different, different actions may be suggested. |
| Transport calendars | The value in the **Transport calendar** column on the **Modes of delivery** page is ignored. |
| Min/max coverage code with no values| With the built-in planning engine, when you use a min/max coverage code where no minimum or maximum values are set, the planning engine treats the coverage code as a requirement and creates one order for each requirement. With Planning Optimization, the system will create one order per day to cover the full amount for that day.  |
| Net requirements and manually created planned orders | With the built-in planning engine, manually created supply orders for an item automatically appear among the net requirements for that item. For example, when creating a purchase order from a sales order, the purchase order appears on the **Net requirements** page without requiring any prior actions. This is because the built-in planning engine logs inventory transactions in the `inventLogTTS` table and shows changes on the **Net requirements** page for dynamic plans. However, with Planning Optimization, manually created orders won't appear among the net requirements of an item until Planning Optimization is run (using a plan that includes the item), or until you select **Update \> Master planning** on the Action Pane on the **Net requirements** page, which will run master planning for the item. For more information about how to work with the **Net requirements** page, see [Net requirements and pegging information with Planning Optimization](net-requirements.md). |
| Resource assignment | When working with infinite capacity, the built-in master planning engine assigns all planned orders to the same resource on a given resource group. Planning Optimization improves on this by selecting resources at random so different production orders can use different resources. If you want to use the same resource for all planned orders, then you must specify that resource in the route. |
| Extended data types (EDTs) | Planning Optimization doesn't support changes to the precision of EDTs. For example, if you extend the product quantity precision from two decimal places (default) to four, Planning Optimization will still use just two decimal places. |

## Additional resources

- [Planning Optimization fit analysis](planning-optimization-fit-analysis.md)
- [Parameters not used by Planning Optimization](not-used-parameters.md)
- [Date and time parameters used by Planning Optimization](date-time-used.md)

[!INCLUDE[footer-include](../../../includes/footer-banner.md)]

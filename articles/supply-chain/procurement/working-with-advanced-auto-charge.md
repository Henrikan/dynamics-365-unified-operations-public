---
# required metadata

title: Working with advanced auto charges
description: The charges feature in Microsoft Dynamics 365 Supply Chain Management has been extended with additional capabilties covering the ability to do compounding for sales quotation and sales order header charges and apply a charge category type 'specific unit' and 'specific unit match' to calculate a line charge to sales and purchase line charges. 
author: Henrik Andersen
ms.date: 10/04/2023
ms.topic: article
ms.prod: 
ms.technology: 

# optional metadata

# ms.search.form:  [Operations AOT form name to tie this article to]
audience: Application User
# ms.devlang: 
ms.reviewer: kamaybac
# ms.tgt_pltfrm: 
# ms.custom: [used by loc for articles migrated from the wiki]
ms.search.region: Global
# ms.search.industry: [leave blank for most, retail, public sector]
ms.author: henrikan
ms.search.validFrom: 2023-10-01
ms.dyn365.ops.version: 10.0.38
---

# Sequence and compound 
[!include [banner](../includes/banner.md)]

Based on the customer that you're working with or the item that you're selling, you might want to apply specific additional charges for order headers and order lines. For sales quotations and sales orders it is possible to compound header charges. 
 
An auto charge of level header can be set to compound which controls the value base upon which the auto charge is calculated. All header level charges (where customer is debit or credit) with a sequence equal to or lower than the sequence for the compound charge is added to the sum of line net amounts. This sum is referred to as 'Value base'. The value base can be either "Sum of line net amounts only" or "Sum including charge amounts". Compound is applied when the charge category is precentage only.  

In order to be able to calculate a compounded header charge, a charge sequence is required. The sequence on a charge, which can only exist on level header, determines the sequence in which header charges are sequentially calculated building up an interim header charge total. More interim totals can exist. Depending on the 'value base for header charges' setting, An interim header charge total is applied on top of the sum of line net amounts, including all line charges, and sales taxes ("Sum including charge amounts") or the sum of line net amounts only ("Sum of line net amounts only") to constitute a value base upon which subsequent compound charges are calculated. The sequence exists on a header charge for a sales order and a sales quotation. A sequence is set on the auto charge setup for header level and is inherited to the header charge on a sales quotation and sales order. Sequence is a prerequisite for compound. Sequence only has an impact when you have one or more charges set to compound. 

In cases where more header charges with the same sequence is applied, a position on the header charges on a sales quotation and sales order controls the order. The position determines the order in which the header charges are summed within the context of compound. The position is added runtime and is applied to the header charges on sales quotation and sales order header. Position only has an impact when you have one or more charges set to compound in auto charges. 

In order to leverage the ability to compound header charges, enable the feature 'Sequence and compound for customer charges' in feature management. When this feature is enabled, new settings are availble from accounts receivable parameters to control the compounding and colums sequence, compound, and position are visible on header charges on sales quotation and sales order header.  

Go to **Accounts receivable \> Setup \> Accounts receivable parameters**.
1. Open the **Prices** tab.
1. On the **Prices** FastTab, make the following settings:
    - **Find auto charges for header** – Set to 'Yes'
1. On the **Charges** fasttab, make the following settings:
    - **Re-search on posting** – Specify whether header auto charges on sales orders and sales quotations should be re-searched when documents are confirmed or posted. Other manually added charges won't be affected during the re-search process. Set this to 'Yes' to ensure that the header charges setup in auto charges are always applied. 
    - **combine charges on combined invoices** – The parameter controls how auto charges are calculated when multiple sales orders are combined into a single invoice using the "summary update" function. Set this to "Yes" to first calculate the grand total of all sales orders included in the combined invoice and then apply the auto charge to that total. Set this to "No" to calculate a separate auto charge for each sales order included in the combined invoice. 
    - **value base for header charges** – 	This parameter determines the value base for calculating percentage-based header charges. You can select the option "Sum of line net amounts only" to calculate charges based on the sum of line amounts only. Alternatively, choose the option "Sum including charge amounts" to calculate charges based on the sum of line amounts, including line charges. 
With the "Sum including charge amounts" option, users can also include tax amounts in the value base by specifying tax codes that should be used to determine the tax amount. This can be defined on an individual auto charge line by clicking the button "Include taxes in value base," which will automatically appear when this option is enabled. Set this to "Sum of line net amounts only" to ensure compatibilty with charges calculation prior to enabling the feature 'Sequence and compound for customer charges'. 

## Working with sequence and compound 
To apply a charge set as compound, you must first setup the auto charge.

Follow these steps:

1. Go to **Accounts receivable \> setup \> Charges \> Auto charges**.
2. Select Level 'Header'
3. On the Action Pane, select **New** to create a charges.
4. In the header of the new record, set the following fields:
    - **Account code** – All
    - **Item code** – All
    - **Mode of delivery code** – All
    - **Charge description** – Enter the apropriate description.

5. On the Action Pane, select **Save**.
6. In the Lines grid Action ribbon, select **Add** to create the following charge line.

    - **Sequence** – 1
    - **Compound** – unchecked
    - **Currency** – USD
    - **Charges code** – Freight
    - **Category** – Fixed
    - **Charges value** – 100
    - **From amount** – Empty.
    - **To amount** – Empty
    - **Sales tax group** – Empty
    - **Site** and **Warehouse** – Empty
    - **Keep** – unchecked

7. In the Lines grid Action ribbon, select **Add** to create the following charge line.

    - **Sequence** – 2
    - **Currency** – USD
    - **Charges code** – Handling
    - **Category** – Percentage
    - **Compound** – checked
    - **Charges value** – 2
    - **From amount** – Empty.
    - **To amount** – Empty
    - **Sales tax group** – Empty
    - **Site** and **Warehouse** – Empty
    - **Keep** – unchecked

These charges can now be applied automatically onto a sales quotation or sales order. To apply the charges to a sales order, follow these steps:

1. Go to **Sales and Marketing \>  \> Sales orders \> All sales orders**.
2. On the Action Pane, select **New** to create a new sales order

Create the sales order for customer US-004, or any customer using USD as currency. 

3. In the header of the new sales order, select Header tab **Sell** and **Maintain charges** in the **Charges** group.

You will have two charge lines:
First line: Position=1, Sequence=1, Compound=unchecked, Charges code=Freight, Category=Fixed, Charges value = 100
Second line: Position=2, Sequence=2, Compound=checked, Charges code=Handling, Category=Percentage, Charges value = 2

Note, that the position equals the sequence from the auto charges setup. The position will differ from sequence in cases where there is a overlap in sequence numbering. it is the position which is used in the calcuation of the value base used for the charge set to compound. 

In this particular case, 2% charge os applied on top of the 100 USD fixed charge equalling 102 USD as total header charges (100+2). 

It is possible to change the assigned position, sequence, and compound values for a header charge applied from auto charges. It should however be done with caution as it will impact the the calcuation of the value base used for the charge set to compound. 

Consider a user doing the following changes to the below: 
Auto created first line: Position=1, Sequence=1, Compound=unchecked, Charges code=Freight, Category=Fixed, Charges value = 100
Autocreated second line: Position=2, Sequence=2, Compound=checked, Charges code=Handling, Category=Percentage, Charges value = 2

Change first line: Position=2, Sequence=1, Compound=unchecked, Charges code=Freight, Category=Fixed, Charges value = 100
Change second line: Position=1, Sequence=2, Compound=checked, Charges code=Handling, Category=Percentage, Charges value = 2

In this particular case, first 2% charge is applied. As the value base is 0, 2% results in a zero charge. Then the 100 USD fixed charge is added equalling 100 USD as total header charges (0+100). 

Alternatively do the following:

Change first line: Position=1, Sequence=1, Compound=unchecked, Charges code=Freight, Category=Fixed, Charges value = 100
Change second line: Position=2, Sequence=2, Compound=unchecked, Charges code=Handling, Category=Percentage, Charges value = 2

In this particular case first the 100 USD fixed charge is applied. As the 2% is not set to compound, they will calculated only based on the sum of the line amounts. Since the line amount is 0, 2% will equal a zero charge. Total sum of header charges will be 100 USD (100+0). 

In the case where a manipulation of the header charges automatically added from auto charge setup has been done and should be reverted, do the following: 

4. In the header of the new sales order, select Header tab **Header auto charges** in the **Calculate** group.

This action will restore the auto charges on the sales order header automatically added from auto charges to the values from the auto charge setup. Any manually added charges to the sales order header to will not be affected by this action. This action will not restore any order line charges. 
   
         > [!NOTE]
        > It is possible to have overlaps both position and sequence when deleting charges applied from auto charges, manually adding charges, followed by restoring the charges through the **Header auto charges** action. In such a case it will be the rec id of the charge record that determines the position when calculating the value base. It is recommended to avoid such scenarioes, as it will not be transparent to the user how the calculation of the value base is performed. 
 > [!NOTE]
        > It is possible to set a manually added header charge to compound. This will however have no effect, as compounding will only be performed for header charges added automatically from auto charges. 
        

   **STOP**


1. The **Posting** FastTab includes **Debit** and **Credit** sections. Set the following fields, depending on the ledger that you want to post the charges to:

    - **Type** – Select the type of account that you're posting to (*Ledger*, *Customer*, or *Item*).
    - **Posting** – Select the type of postings to create (such as *Broker fee* or *Customer settlement*).
    - **Account** – Select the account to post the charge for.

1. On the Action Pane, select **Save**.


Re-search on posting works both with and without compounding. The following procedure shows how to work with re-search on posting. Before you start this procedure, you should already have a header-level charge of the *fixed amount* type and an order where that charge is applied. Additionally, the order should already include at least one line item.

1. Open the purchase order or charge order.
1. On the Action Pane, follow one of these steps:

    - For purchase orders: On the **Purchase** tab, in the **Charges** group, select **Allocate charges**.
    - For sales orders: On the **Sell** tab, in the **Charges** group, select **Allocate charges**.

 

## Set up compound
An auto charge of level header can be set to compound which controls the value base upon which the auto charge is calculated. All header level charges (where customer is debit or credit) with a sequence equal to or lower than the sequence for the compound charge is added to the sum of line net amounts (including all line charges).  
Compound is only of relevance when the charge category is % (percent). 




Automatic charges, or auto charges, are automatically applied when you create a sales order or a purchase order. You can define auto charges for specific vendors, customers, groups of vendors, or items. You can also define auto charges that apply to all vendors, customers, or items.

## Set up parameters

The **Procurement and sourcing parameters** page has a few settings that are especially relevant when you want to apply charges automatically. To complete this setup, follow these steps.

1. Go to **Procurement and sourcing \> Setup \> Procurement and sourcing parameters**.
1. Open the **Prices** tab.
1. On the **Prices** FastTab, make the following settings:
    - **Find auto charges for header** – Specifies whether charges should automatically be applied for purchase order headers. Set this to *Yes* to use automatic application of charges.
    - **Find auto charges for line** – Specifies whether charges should automatically be applied for purchase order lines. Set this to *Yes* to use automatic application of charges.

The **Sales and marketing parameters** page has a few settings that are especially relevant when you want to apply charges automatically. To complete this setup, follow these steps.

1. Go to **Sales and marketing \> Setup \> Sales and marketing parameters**.
1. Open the **Prices** tab.
1. On the **Prices** FastTab, make the following settings:
    - **Find auto charges for header** – Specifies whether charges should automatically be applied for sales quotation and sales order headers. Set this to *Yes* to use automatic application of charges.
    - **Find auto charges for line** – Specifies whether charges should automatically be applied forsales quotation and sales order lines. Set this to *Yes* to use automatic application of charges.


## Set up charges codes

To apply charges, you must first define charges codes.

1. Follow one of these steps:

    - For purchase orders: Go to **Procurement and sourcing \> setup \> Charges \> Charges code**.
    - For sales orders: Go to **Accounts receivable \> setup \> Charges \> Charges code**.

1. On the Action Pane, select **New** to create a charges code.
1. In the header of the new record, set the following fields:

    - **Charges code** – Enter a code for the charges.
    - **Description** – Enter a description of the charges.
    - **Item sales tax group** – Select an item sales tax group, if applicable.
    - **Prorate** – Set this option to *Yes* if you want to prorate your charges. This option is available only for sales orders.
    - **Maximum amount** – Enter the maximum amount that is allowed for the charges code. This field is used to validate charges for vendor invoices. It's available only for purchase orders.

        > [!NOTE]
        > To turn on the functionality for validating charges for purchase orders, go to **Accounts payable \> Setup \> Accounts payable parameters**. On the **Invoice validation** FastTab, in the **Invoice validation** section, set the **Enable invoice matching validation** option to *Yes*.

1. The **Posting** FastTab includes **Debit** and **Credit** sections. Set the following fields, depending on the ledger that you want to post the charges to:

    - **Type** – Select the type of account that you're posting to (*Ledger*, *Customer*, or *Item*).
    - **Posting** – Select the type of postings to create (such as *Broker fee* or *Customer settlement*).
    - **Account** – Select the account to post the charge for.

1. On the Action Pane, select **Save**.

## Create charge groups

Charge groups automatically apply specific charges to a group of customers or vendors. The following subsections describe how to create and assign these charge groups.

### Charge groups for purchase orders

To create charge groups for purchase orders, follow these steps.

1. Go to **Procurement and sourcing \> setup \> Charges \> Vendor charges group**.
1. On the Action Pane, select **New** to add a row to the grid, and then set the following fields:

    - **Charges group** – Enter the name of the charge group.
    - **Description** – Enter a description of the charge group.

1. On the Action Pane, select **Save**.
1. Go to **Procurement and sourcing \>  Vendors \> All vendors**, and either open an existing vendor or create a new vendor.
1. On the **Purchase order defaults** FastTab, in the **Purchase order** section, set the **Charges group** field to the charge group that you just created.

### Charge groups for sales orders

To create charge groups for sales orders, follow these steps.

1. Go to **Accounts receivable \> setup \> Charges \> Customer charge groups**.
1. On the Action Pane, select **New** to add a row to the grid, and then set the following fields:

    - **Charges group** – Enter the name of the charge group.
    - **Description** – Enter a description of the charge group.

1. On the Action Pane, select **Save**.
1. Go to **Accounts receivable \> Customers \> All customers**, and either open an existing customer or create a new customer.
1. On the **Sales order defaults** FastTab, in the **Sales order** section, set the **Charges group** field to the charge group that you just created.

## Define auto charges

After your charges codes are set up, follow these steps to define the auto charges.

1. Follow one of these steps:

    - For purchase orders: Go to **Procurement and sourcing \> Setup \> Charges \> Automatic charges**.
    - For sales orders: Go to **Accounts receivable \> Setup \> Charges setup \> Auto charges**.

1. In the list pane, in the **Level** field, select the level where your auto charge applies:

    - *Header* – Apply charges to the order header.
    - *Line* – Apply charges to the order lines.

1. Select an existing auto charge to edit it, or select **New** to define a new auto charge.
1. In the **Account code** list, select one of the following values to specify the scope of accounts that will be affected:

    - *Table* – Assign charges to a specific customer or vendor.
    - *Group* – Assign charges to a miscellaneous charges group.
    - *All* – Assign charges to all customers or vendors.

1. In the **Customer relation** or **Vendor relation** field, select a specific customer or vendor if you set the **Account code** field to *Table*. If you set the **Account code** field to *Group*, select a customer or vendor charges group.
1. In the **Item code** field, select one of the following values to specify the scope of items that will be affected. You can select an item code only when you define auto charges at the line level.

    - *Table* – Assign charges to a specific item.
    - *Group* – Assign charges to an item charges group.
    - *All* – Assign charges to all items.

1. In the **Item relation** field, select a specific item if you set the **Item code** field to *Table*. If you set the **Item code** field to *Group*, select an item charges group.
1. **For sales orders only:** In the **Mode of delivery code** field, select one of the following values to specify the scope of delivery modes that will be affected:

    - *Table* – Assign charges to a specific mode of delivery.
    - *Group* – Assign charges to a mode of delivery group.
    - *All* – Assign charges to all modes of delivery.

1. **For sales orders only:** In the **Mode of delivery relation** field, select a specific mode of delivery if you set the **Mode of delivery code** field to *Table*. If you set the **Mode of delivery code** field to *Group*, select a mode of delivery group.
1. On the **Lines** FastTab, define the charges and the charges rates that will be used when the current auto charge is applied. You can use the toolbar on this FastTab to add as many lines as you require. For each line, set the following fields:

    - **Sequence** – Select the currency that should be used to calculate the charge. Only applicable for **Level** *Header* for sales quotation and sales order charges.
    - **Compound** – Select the currency that should be used to calculate the charge. Only applicable for **Level** *Header* for sales quotation and sales order charges.
    - **Currency** – Select the currency that should be used to calculate the charge.
    - **Charges code** – Select the code for the charge.
    - **Category** – Select one of the following values:

        - *Fixed* – The charge is entered as a fixed amount on the line. Fixed charges can be used on charges both in the order header and on the order lines. 
        - *Pcs* – The charge is based on the unit. These charges can be used only on order lines. They will appear when you calculate the order total. This category is only applicable for **Level** *Line*
        - *Percent* – The charge is entered as a percentage on the line. Percentage charges can be used on charges both in the order header and on the order lines.
        - *Intercompany percent* – The charge is entered as a percentage on the line for intercompany orders. Intercompany percentage charges can be used only on order lines.
        - *External* – The charge is calculated by a third-party service that is associated with one or more shipping carriers.
        - *Specific unit* – The charge value is expressed by the unit of measure in the charge line. It is applied proportionally to the sales line unit of measure using unit of measure conversion. This category is only applicable for **Level** *Line*
        - *Specific unit match* – The charge value is expressed by the unit of measure in the charge line. The sales line unit of measure must match the unit on for the charge line to be applied. no unit of measure conversion is applied. This category is only applicable for **Level** *Line*

    - **Unit** – Enter the unit applicable when the category is *Specific unit* or *Specific unit match*.
    - **Charges value** – Enter the charge value, based on the category that you selected.
    - **Charges currency code** – Specify a currency for the charge if you want to use a currency other than the currency that you specified in the **Currency** field. You can use a different currency only if the **Debit type** or **Credit type** field is set to either *Ledger account* or *Item* for the selected charges code.
    - **From amount** – Specify a starting amount to apply the auto charge to. In this context, the amount refers to the order total.
    - **To amount** – Specify the ending amount to apply the auto charge to. In this context, the amount refers to the order total.
    - **Sales tax group** – Specify a sales tax group.
    - **Site** and **Warehouse** – Specify a site and warehouse if charges should be applied only for a specific site and warehouse.
    - **Keep** – Select this check box to keep the charges transactions after invoicing is completed, so that the charge will be applied every time that you create a new invoice for the selected customer account.

1. **For sales orders only:** If you want to calculate tiered charges, see [Tiered charges on sales orders](/dynamicsax-2012/appuser-itpro/about-tiered-charges-on-sales-orders) for information.

> [!NOTE] 
        > To turn on the functionality for working working with sequence and compound on header level charges, you need to enable the feature 'Sequence and compound for customer charges' in feature management. For more information see [link to new page for Sequence and compound]. To turn on the functionality for working with line level charges category types specific unit and specific unit match, you need to enable the feature 'Unit of measure for line level charges' in feature management. For more information see [link to new page for Unit of measure for line level charges].

## Allocate charges from the header to a line

The following procedure shows how to allocate header-level charges to a line. Before you start this procedure, you should already have a header-level charge of the *fixed amount* type and an order where that charge is applied. Additionally, the order should already include at least one line item.

1. Open the purchase order or charge order.
1. On the Action Pane, follow one of these steps:

    - For purchase orders: On the **Purchase** tab, in the **Charges** group, select **Allocate charges**.
    - For sales orders: On the **Sell** tab, in the **Charges** group, select **Allocate charges**.

1. In the **Allocate charges to order lines** dialog box, set the following fields:

    - **Charges allocation** – Select one of the following values to specify how the charges should be allocated:

        - *Net amount* – Allocate charges according to each line amount relative to the total net amount.
        - *Quantity* – Allocate charges according to the number of units for each line relative to the total number of units.
        - *Per line* – Allocate charges equally among the total number of lines.

    - **Allocate charges to lines** – Select a value to specify whether charges should be allocated to all lines, to positive lines only, or to negative lines only.
    - **Allocate all** – Select this check box to allocate charges to order lines even if the charges code has a debit type other than *Item*.
    - **Received** – Select this check box to allocate charges only to received order lines.
    - **Stocked** – Select this check box to allocate charges to only inventoried order lines.
    - **Show selections and clear specific lines** – Select this check box to exclude specific lines from this allocation. When you select this check box, the **Choose lines to exclude from allocation** grid is opened. This grid includes only lines that match the criteria that are defined by the **Allocate charges to lines** and **Stocked** settings. For example, if you set the **Allocate charges to lines** field to *Positive lines* and select the **Stocked** check box, the grid shows only lines that are both positive and inventoried. In addition, the grid automatically filters out any lines that the full quantity has already been received for. While the grid is open, clear the **Include** check box for each line that should be excluded from allocation. 

        > [!IMPORTANT]
        > When you work with the **Choose lines to exclude from allocation** grid, be sure to leave the grid open until you select **Allocate**. If you close the grid before you select **Allocate**, your settings in the grid will be lost. Therefore, charges will be allocated based on the criteria that you previously defined.

1. Select **Allocate** to apply your settings and close the dialog box.


[!INCLUDE[footer-include](../../includes/footer-banner.md)]

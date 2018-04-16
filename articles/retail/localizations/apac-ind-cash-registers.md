---
# required metadata

title: GST integration for cash registers for India
description: This topic provides an overview of the cash register functionality that is available for India. It also provides guidelines for setting up the functionality.
author: EvgenyPopovMBS 
manager: annbe
ms.date: 01/31/2018
ms.topic: article
ms.prod: 
ms.service: dynamics-365-retail
ms.technology: 

# optional metadata

# ms.search.form: 
audience: IT Pro
# ms.devlang: 
ms.reviewer: shylaw
ms.search.scope: Retail, Operations
# ms.tgt_pltfrm: 
# ms.custom: 
ms.search.region: India
ms.search.industry: Retail
ms.author: v-pakris
ms.search.validFrom: 2018-1-31
ms.dyn365.ops.version: 7.3.1
---
# GST integration for cash registers for India

[!INCLUDE [banner](../includes/banner.md)]

This topic provides a walkthrough of the features that are related to Goods and Services Tax (GST) in Microsoft Dynamics 365 for Retail. It also highlights the effect of GST on various types of retail business transactions, and shows the accounting and posting of retail transactions where the receipt is printed at the point of sale (POS).

> [!NOTE]
> This topic applies to both Microsoft Dynamics 365 for Finance and Operations, and Microsoft Dynamics 365 for Retail.

## Prerequisites 

- Set up GST for India in Finance and Operations. For more information, see [India Goods and Services Tax (GST)](../../financials/localizations/apac-ind-gst.md).
- Configure Retail channel components. To enable India-specific functionality, you must configure extensions for Retail channel components. For more information, see the [deployment guidelines](./apac-ind-loc-deployment-guidelines.md).

## India tax entities for Retail

The following table shows the navigation paths for the India tax entities in Retail.

| India tax entities                  | Navigation path in Retail                                                     |
|-------------------------------------|-------------------------------------------------------------------------------|
| Business verticals                  | Retail \> Channel setup \> Sales taxes \> Business verticals                  |
| Enterprise tax registration numbers | Retail \> Channel setup \> Sales taxes \> Enterprise tax registration numbers |
| GST reference number sequence group | Retail \> Channel setup \> Sales taxes \> GST reference number sequence group |
| HSN codes                           | Retail \> Channel setup \> Sales taxes \> HSN codes                           |
| Service accounting codes            | Retail \> Channel setup \> Sales taxes \> Service accounting codes            |
| Maintain setoff hierarchy profiles  | Retail \> Channel setup \> Sales taxes \> Maintain setoff hierarchy profiles  |
| VAT schedules                       | Retail \> Channel setup \> Sales taxes \> VAT schedules                       |
| Tax setup                           | Retail \> Channel setup \> Sales taxes \> Tax configuration \> Tax setup      |

> [!NOTE]
> The navigation paths for the India tax entities in Retail differ from the navigations paths in Finance and Operations. For information about the navigation paths in Finance and Operations, see [India Goods and Services Tax (GST)](../../financials/localizations/apac-ind-gst.md).

## Validate tax information for the retail store

The tax information for the retail store comes from the selected retail warehouse. This warehouse is defined in the warehouse master. The configured tax information from the store is printed on the POS receipt. It's also updated on the retail sales order at the retail headquarters for the financial postings.

Follow these steps to view the tax information for a retail store.

1. Go to **Retail** \> **Channels** \> **Retail stores** \> **All retail stores**.
2. Select a retail store.
3. Select the **Tax information** FastTab.

## Configure language texts and custom fields

You can configure the language text and custom fields that are used in the POS receipt formats. The default company of the user who creates the receipt setup should be the same as the legal entity where the language text setup is created. Alternatively, the same language texts should be created in both the user's default company and the legal entity of the store that the setup is created for.

### Set up the POS language text

1. Go to **Retail** \> **Channel setup** \> **POS setup** \> **POS profile** \> **Language text**.
2. On the **POS** tab, on the **POS language text** FastTab, select the language ID for the text. The language should match the user's preferred language.
3. In the **Text ID** field, enter a unique ID that is equal to or more than **900001**.
4. In the **Text** field, enter the language text.

![Language text](media/apac-ind-gst-language-text.png)

### Create custom fields

When you create custom fields, the value of the **Caption text ID** field must match the value that you entered for the **Text ID** field on the **Language text** page.

1. Go to **Retail** \> **Channel setup** \> **POS setup** \> **POS profile** \> **Custom fields**.
2. Enter a name for the field.
3. Select the field type.
4. In the **Caption text ID** field, enter the **Text ID** value for one of the language texts on the **Language text** page.

![Custom fields](media/apac-ind-gst-custom-fields.png)

## Create the receipt format

You can use Receipt format designer to add custom fields to the appropriate receipt sections. For more information, see [Receipt templates and printing](../receipt-templates-printing.md).

1. Go to **Retail** \> **Channel setup** \> **POS setup** \> **POS profile** \> **Receipt formats**.
2. Select a receipt format for the **Receipt** receipt type, and make the required changes.

![Receipt format design](media/apac-ind-gst-receipt-format.png)

## Update receipt profiles

After you create a receipt format, you can assign that format to a receipt profile.

Follow these steps to update a receipt profile.

1. Go to **Retail** \> **Setup** \> **POS** \> **Receipt profile**.
2. Select the receipt profile to update.
3. Select **Edit**.
4. For each receipt type in the list, select a receipt format.

## Update the POS invoice number

You can reconcile the POS receipt number with the invoice number for customer retail transactions. If you set the **Update POS invoice number** option to **Yes** on the **Posting** tab of the **Retail parameters** page, the POS receipt number is entered in the **Transaction ID** field for corresponding retail sales orders.

You can set the **Update POS invoice number** option to **Yes** only if the existing receipt number format includes both the store number and the terminal number. The following illustration shows a POS functionality profile where the receipt numbering includes the store number and the terminal number.

![POS functionality profile](media/apac-ind-gst-pos-functionality-profile.png)

## Run a distribution schedule

To synchronize Tax Engine (GTE) data from retail headquarter to the POS database, you must add a job to the **Distribution schedule** page.

Follow these steps to verify that the job exists and to run the job.

1. Go to **Retail** \> **Periodic** \> **Data distribution** \> **Distribution schedule**.
2. Verify that a new job, **1180**, has been added for **Generic tax engine**.
3. Run all the jobs (**9999**).

## Example scenarios

The following example scenarios walk you through sample transactions for India:

- Scenario 1: Sell to a registered customer
- Scenario 2: Sell taxable goods to a consumer
- Scenario 3: Sell taxable goods to an anonymous customer where GST is price-inclusive
- Scenario 4: Sell an exempted good
- Scenario 5: Return the transaction that has GST

### Scenario 1: Sell to a registered customer

Sales to a registered customer are known as *business-to-business* (B2B) sales. If the store location and the place of supply (that is, the customer address) are in the same state, the transaction is an *intrastate sale*, and Central GST (CGST) and State GST (SGST) must be paid. If the store location and the place of supply are in different states, the transaction is an *interstate sale*, and Integrated GST (IGST) must be paid.

> [!NOTE]
> For these transactions, the fields for the customer address and the registration number are required.

1. Sign in to the POS.
2. Enter the items, and then select **Enter**.

    ![POS customer order](media/apac-ind-gst-pos-customer-order.png)

3. Validate the GST calculations. Consider the rate that is defined in the tax setup.

    | Item/service | Unit price | Tax rates          | CGST     | SGST      |
    |--------------|------------|--------------------|----------|-----------|
    | M0001        | 10,000.00  | CGST 12%, SGST 11% | 1,200.00 | 1,100.00  |
    | M0002        | 5,000.00   | CGST 10%, SGST 10% | 500.00   | 500.00    |
    | S0001        | 1,200.00   | CGST 11%, SGST 5%  | 132.00   | 60.00     |
    |              | 16,200.00  |                    | 1,832.00 | 1,660.00  |
    | Total amount |            |                    |          | 19,692.00 |

4. Select **Orders** \> **Create customer order**.
5. Select **Add customer**, and select the customer account.
6. Select **Pick up all**.
7. Select the store and the pick-up date.
8. Select **OK**.

    ![POS customer order](./media/apac-ind-gst-pos-customer-order2.png)

    > [!NOTE]
    > In this example, the state of the store location is Delhi, and the state of the customer address is also Delhi. Because the state is the same, intrastate GST is calculated.

9. Select **Exact** to process the deposit payment.
10. Validate the receipt:

    1. Select **Show journal**.
    2. Select the transactions.
    3. Select **Receipt**.

    ![Receipt example](media/apac-ind-gst-receipt.png)

11. Validate the sales order and tax document in Retail headquarters:

    1. Go to **Retail** \> **Customers** \> **All sales orders**.
    2. Select the sales order.
    3. On the Action Pane, on the **Sell** tab, in the **Tax** group, select **Tax document**.

    ![Tax document](media/apac-ind-gst-tax-document.png)

12. Recall and process the customer order:

    1. Sign in to the POS.
    2. Select **Recall order**.
    3. Search for and select the order.
    4. Select **Picking and packing** \> **Pickup**
    5. Select **Select all** and then **Pickup**.

    ![Process a customer order](media/apac-ind-gst-process-customer-order.png)

13. Select **Exact** to process the payment.
14. Validate the receipt.

    ![Receipt example](media/apac-ind-gst-receipt-2.png)

15. Validate the voucher transactions:

    1. Go to **Retail** \> **Customers** \> **All sales orders**.
    2. Select the sales order.
    3. On the Action Pane, on the **Invoice** tab, select **Invoice journals**.
    4. Select **Voucher**.

        | Ledger account name  | Debit amount (Rs.) | Credit amount (Rs.) |
        |----------------------|--------------------|---------------------|
        | Customer account     | 19,692.00          |                     |
        | CGST payable account |                    | 1,832.00            |
        | SGST payable account |                    | 1,660.00            |
        | Sales account        |                    | 16,200.00           |

    5. Select **Tax document**.
    6. Verify that the receipt number is updated as the transaction ID.

    ![Tax document](media/apac-ind-gst-tax-document2.png)

### Scenario 2: Sell taxable goods to a consumer

When you sell to unregistered customers, the sales are referred to as *business-to-consumer* (B2C) sales. Tax is calculated in the same manner for B2B and B2C sales. 

1. Sign in to the POS.
2. Enter an item, and then select **Enter**.
3. Select **Add customer**, and select the customer account.

    ![Example transaction](media/apac-ind-gst-example-trx.png)

    > [!NOTE]
    > In this example, the state of the store location is Delhi, but the state of the customer address is Bengaluru (Bangalore). Because the states differ, interstate GST is computed.

4. Select **Exact** to process the payment.
5. Validate the receipt:

   1. Select **Show journal**.
   2. Select the transactions.
   3. Select **Receipt**.

      ![Receipt validation](media/apac-ind-gst-receipt-validation.png)

6. Validate the retail sales invoice in Retail headquarters:

    1. Go to **Retail** \> **Retail IT** \> **Data distribution**.
    2. Run job **P-0001** (**Channel transactions**).
    3. Close the page.

7. Post the statement:

    1. Go to **Retail** \> **Channels** \> **Retail stores** \> **Open statements**.
    2. Create a statement.
    3. Select **Calculate statement** and then **Post statement**.

8. Validate the voucher transactions:

    1. Go to **Retail** \> **Customers** \> **All sales orders**.
    2. Select the sales invoice.
    3. Select **Sales order lines** \> **Tax information**.
    4. On the appropriate tabs, verify the location (store address) and the customer address.

        ![Tax information](media/apac-ind-gst-tax-info.png)

    5. Select **OK**.
    6. On the Action Pane, on the **Invoice** tab, select **Invoice journals**.
    7. Select **Voucher**.

        | Ledger account name  | Debit amount (Rs.) | Credit amount (Rs.) |
        |----------------------|--------------------|---------------------|
        | Customer account     | 12,500.00          |                     |
        | IGST payable account |                    | 2,500.00            |
        | Sales account        |                    | 10,000.00           |

    8. Select **Tax document**.
    9. Verify that the receipt number is updated as the transaction ID.

### Scenario 3: Sell taxable goods to an anonymous customer where GST is price-inclusive

1. Define price-inclusiveness at the retail store:

    1. Go to **Retail** \> **Channels** \> **Retail stores** \> **All retail stores**.
    2. Select a retail store.
    3. Set the **Prices include sales tax** option to **Yes**.

2. Run the distribution schedule:

    1. Go to **Retail** \> **Retail IT** \> **Data distribution**.
    2. Run the job to update the changes in the POS database.
    3. Close the page.

3. Enter a transaction:

   1. Sign in to the POS.
   2. Enter an item, and then select **Enter**. For this example, use an item that has the following values:

       - **Taxable value:** 10,000.00
       - **CGST:** 12 percent
       - **SGST:** 11 percent

      ![Transaction example](media/apac-ind-gst-trx-example.png)

4. Select **Exact** to process the payment.
5. Validate the receipt.

    ![Receipt example](media/apac-ind-gst-receipt-3.png)

6. Validate the retail sales invoice in Retail headquarters:

    1. Go to **Retail** \> **Retail IT** \> **Data distribution**.
    2. Run job **P-0001** (**Channel transactions**).
    3. Close the page.

7. Post the statement:

    1. Go to **Retail** \> **Channels** \> **Retail stores** \> **Open statements**.
    2. Create a statement.
    3. Select **Calculate statement** and then **Post statement**.

8. Validate the voucher transactions:

   1. Go to **Retail** \> **Customers** \> **All sales orders**.
   2. Select the sales invoice.
   3. On the Action Pane, on the **Invoice** tab, select **Invoice journals**.
   4. Select **Voucher**.


      | Ledger account name  | Debit amount (Rs.) | Credit amount (Rs.) |
      |----------------------|--------------------|---------------------|
      |   Customer account   |     10,000.00      |                     |
      | CGST payable account |                    |       975.61        |
      | SGST payable account |                    |       894.31        |
      |    Sales account     |                    |      8,130.08       |


   5. Select **Tax document**.
   6. Verify that the transaction ID is updated according to the GST number sequence that is defined in the GST reference number sequence group.

      ![Tax document transaction ID](media/apac-ind-gst-tax-doc-trx-id.png)

### Scenario 4: Sell an exempted good

1. Sign in to the POS.
2. Enter an exempted item.

    ![Exempted item transaction](media/apac-ind-gst-exempted-item-trx.png)

3. Select **Exact** to process the payment.
4. Validate the receipt.

    ![Receipt example.png](media/apac-ind-gst-receipt-4.png)

5. Validate the retail sales invoice in Retail headquarters:

    1. Go to **Retail** \> **Retail IT** \> **Data distribution**.
    2. Run job **P-0001** (**Channel transactions**).
    3. Close the page.

6. Post the statement:

    1. Go to **Retail** \> **Channels** \> **Retail stores** \> **Open statements**.
    2. Create a statement.
    3. Select **Calculate statement** and then **Post statement**.

7. Validate the voucher transactions:

    1. Go to **Retail** \> **Customers** \> **All sales orders**.
    2. Select the sales invoice.
    3. On the Action Pane, on the **Invoice** tab, select **Invoice journals**.
    4. Select **Voucher**.

        | Ledger account name    | Debit amount (Rs.) | Credit amount (Rs.) |
        |------------------------|--------------------|---------------------|
        | Customer account       | 12,000.00          |                     |
        | Sales - Finished Goods |                    | 12,000.00           |

    5. Select **Tax document**.
    6. Verify that the **Exempt** option is set to **Yes**.

### Scenario 5: Return the transaction that has GST:

1. Sign in to the POS.
2. Select **Show journal**.
3. Select the transaction, and then select **Return**.
4. Select **Select all** and then **Return**.
5. Verify that the GST calculation is done correctly, based on the selected original transactions that must be returned.

    ![POS return transaction](media/apac-ind-gst-return-trx.png)

6. Select **Exact**.

7. Validate the receipt.

   ![Receipt example](media/apac-ind-gst-receipt-5.png)

8. Validate the retail sales invoice in Retail headquarters:

    1. Go to **Retail** \> **Retail IT** \> **Data distribution**.
    2. Run job **P-0001** (**Channel transactions**).
    3. Close the page.

9. Post the statement:

    1. Go to **Retail** \> **Channels** \> **Retail stores** \> **Open statements**.
    2. Create a statement.
    3. Select **Calculate statement** and then **Post statement**.

10. Validate the voucher transactions:

    1. Go to **Retail** \> **Customers** \> **All sales orders**.
    2. Select the sales invoice.
    3. On the Action Pane, on the **Invoice** tab, select **Invoice journals**.
    4. Select **Voucher**.

        | Ledger account name  | Debit amount (Rs.) | Credit amount (Rs.) |
        |----------------------|--------------------|---------------------|
        | Customer account     |                    | 12,500.00           |
        | IGST payable account | 2,500.00           |                     |
        | Sales account        | 10,000.00          |                     |

    5. Select **Tax document**.
    6. Verify that the return receipt number is updated as the transaction ID.
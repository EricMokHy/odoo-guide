# USC Odoo User Guide - Purchase Order for Sales Order

## Overview
Creating Purchase Orders (POs) for Sales Orders (SOs) in Odoo is essential for managing external purchases related to project execution and subcontractor engagements. This guide explains the concept of POs for SOs, general steps to create a PO linked to a sales order, and specific steps to handle POs for subcontractors, highlighting the key differences.

---

## What is a Purchase Order for Sales Order?

A **Purchase Order for Sales Order** is a procurement document generated to purchase goods or services needed to fulfill a specific sales order. This is particularly important when external suppliers or subcontractors are involved, ensuring costs are directly linked to the sales order for accurate project costing and financial reporting.

- **When to Create a PO for SO:**
    - When purchasing raw materials or products to complete a sales order.
    - When engaging subcontractors to provide services or complete specific project tasks.
    - When outsourcing logistics or other ancillary services related to a sales order.

- **Benefits:**
    - Maintains a clear link between procurement costs and sales revenue.
    - Supports accurate profit and loss analysis for individual sales orders.
    - Simplifies billing and invoicing processes, especially for cost-plus or milestone-based contracts.


---

## General Steps to Create a Purchase Order for a Sales Order

1. **Accessing the Purchase Module**
      - Log into Odoo and navigate to the **Purchases** module.

2. **Initiate a New Purchase Order**
      - Click **Create** to start a new PO.

3. **Select the Vendor**
      - Choose the **Vendor** from whom the goods or services will be purchased.

4. **Add Products or Services**
      - Select the appropriate **Product** from the product list.
      - Enter **Quantity**, **Unit Price**, and **Expected Delivery Date**.

5. **Bind the PO to the Sales Order**
      - Use the **Sales Order (SO)** dropdown list to link the PO to the correct sales order.
      - This ensures that all costs incurred through the PO are directly associated with the sales order.

6. **Confirm the Purchase Order**
      - Review the PO details for accuracy and click **Confirm**.

7. **Manage the Receipt of Products (If Applicable)**
      - For **Stockable Products**, manage the receipt of goods through the **Inventory Module**.

8. **Invoice Management**
      - Create vendor bills and ensure they are linked to the sales order for accurate cost tracking.


---

## Specific Steps to Create a Purchase Order for a Subcontractor

The process of creating a PO for a subcontractor is similar to a general PO but includes specific considerations:

1. **Product Type Selection**
      - Ensure the product is set as a **Service** type. Subcontractor services should not impact inventory management.

2. **Vendor Selection**
      - Choose the **Subcontractor** as the vendor in the PO form.

3. **Define the Scope of Work**
      - Include a clear description of the services to be provided, along with terms and conditions if applicable.

4. **Bind to Sales Order**
      - As with general POs, link the PO to the specific sales order to maintain accurate cost allocation.

5. **Invoice and Payment Handling**
      - Manage the vendor bill according to the invoicing policy of the sales order (e.g., **Milestone-Based**, **Timesheet-Based**, **Delivered Quantities**).

6. **Monitor Subcontractor Performance**
      - Utilize **Project Management** and **Timesheet Tracking** modules to ensure subcontracted tasks are completed as agreed.


---

## Key Differences Between General and Subcontractor POs

| **Aspect**             | **General PO**                                     | **Subcontractor PO**                             |
|-----------------------|--------------------------------------------------|-------------------------------------------------|
| **Product Type**      | Stockable, Consumable, or Service                  | Primarily Service                                |
| **Inventory Impact**  | May involve stock management                       | No impact on inventory                           |
| **Invoicing Policy**  | Can be any (Ordered/Delivered Quantities)          | Usually Milestone or Timesheet-Based             |
| **Vendor Management** | Standard supplier engagement                       | Requires detailed service scope and project link |
| **Cost Tracking**     | Directly through inventory or service expenses     | Through project costs and subcontractor invoices |

---

## Best Practices

- **Select the Correct Product Type:** Avoid errors in inventory and billing by choosing the correct product type.
- **Bind POs to Sales Orders:** Ensure all POs are linked to sales orders to maintain cost transparency.
- **Document Subcontractor Agreements:** Attach quotations, contracts, and service level agreements to POs for reference.

---

## Troubleshooting

### 1. Purchase Order Not Linked to Sales Order

- **Verify Binding:** Ensure the **Sales Order (SO)** field is correctly populated when creating the PO.
- **Check User Permissions:** Make sure you have the required permissions to create and bind POs.

### 2. Incorrect Billing of Subcontractor Services

- **Review Invoicing Policy:** Check if the service product is configured with the correct invoicing policy (e.g., **Milestone-Based**).
- **Update the Purchase Order:** Correct any discrepancies in product type, quantity, or service details.

---

## IT Support Contact

- **Email:** [ericmok@uscpower.net](mailto:ericmok@uscpower.net)
- **Phone:** +852 6622 7663

---

[<- Back to Index](../../../index.md)


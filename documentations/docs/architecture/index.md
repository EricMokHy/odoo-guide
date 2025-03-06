# Work flow for USC Forms

![work-flow](./ms-form-workflow.drawio.svg)

1. Authorized user visit the SharePoint Site.  
2. Find the desired `Forms`.
3. Fill the detail in the `Forms`.
4. Submit the `Forms`.
5. Microsoft Power Automate receive the `Response` from the submitted `Forms`.
6. The Power Automate will:
    1. Insert a record to database.
    2. Generate a PDF
        1. Generate the PDF with Word Template.
        2. Store the PDF to SharePoint document List.
        3. Send email to notify user about the .pdf.

> This operation is asynchronous. As such, users will not receive the document immediately upon submission through Forms. However, they will receive the document via email and will also be able to locate it on the SharePoint site.        

## Estimation
Task | Description | Handler | Est
-- | -- | -- | --
Create a SharePoint Site | To host all related elements, Forms, Document, DB | Eric Mok | 2 days
Create PO Forms | To create a Forms for PO and share it on SharePoint | Eric Mok | 2 days
Create Quotation Forms  | To create a Forms for Quotation and share it on SharePoint | Eric Mok | 2 days 
Create CO, DN, INV Forms | To create a Forms for CO, DN, INV and share it on SharePoint | Eric Mok | 2 days
Create Word Templates | Create Word Template for PO, Quotation, CO, DN and INV, they will be used to generate PDF using Power Automate | Eric Mok | 2 days
Create Power Automate Workflow (PO) | Create a Workflow for PO to generate PDF, send it to user and store it on SharePoint Site | Eric Mok | 3 days
Create Power Automate Workflow (Quotation) | Create a Workflow for Quotation to generate PDF, send it to user and store it on SharePoint Site | Eric Mok | 3 days
Create Power Automate Workflow (CO, DN, INV) | Create a Workflow for CO, DN and INV to generate PDF, send it to user and store it on SharePoint Site | Eric Mok | 3 days

**Total: 19 Man Days**

## Cost
Service | Rate | Sub Total
-- | -- | --
SharePoint Site | inlcuded | 0
Microsoft Forms | included | 0
Word | included | 0
Database | 300/month | 300
Power Automate | USD 15/month | 118

**Total Running Cost per Month: 418**



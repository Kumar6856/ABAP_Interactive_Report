# ZREPORT_INTRECTIVE_1001

## Description

This is an **Interactive ABAP Report** created using **SE38 Transaction** in SAP S/4HANA. The report displays Purchase Order (PO) Header details from table `EKKO` and, upon user interaction, navigates to corresponding PO Item details from table `EKPO`.

## Key Features

- Developed using **Classical Reporting** with **Interactive Functionality**.
- Fetches data from two standard SAP tables:
  - `EKKO` – Purchase Order Header
  - `EKPO` – Purchase Order Item
- Implements the `AT LINE-SELECTION` event to display item-level data on secondary list.
- Uses `SELECT-OPTIONS` for filtering by Purchase Order Number.
- Works **even if the select-option is left blank** (fetches all data).
- Includes `TOP-OF-PAGE` and `TOP-OF-PAGE DURING LINE-SELECTION` for customized headers.
- Proper formatting with colors, line separation, and column headers.

## Selection Screen

The report includes:
---------abap
SELECT-OPTIONS: S_EBELN FOR EKKO-EBELN.

-------------------------------------------------------

Output Screens
Primary List (EKKO Data)
------------------------------------------------

| PO Number                                                   | Company Code | Category | Type | Vendor |
| ----------------------------------------------------------- | ------------ | -------- | ---- | ------ |
|                                                             |              |          |      |        |

------------------------------------------------------------------

Secondary List (EKPO Data)
----------------------------------------------------------------------------
| PO Number | Item | Material | Plant | Storage Location | Material Group | Quantity | Unit |
| --------- | ---- | -------- | ----- | ---------------- | -------------- | -------- | ---- |

---------------------------------------------------------------------------------------------------------------

Events Used
1- START-OF-SELECTION: Fetches PO Header data from EKKO.

2- AT LINE-SELECTION: Triggers on user interaction, fetches and displays PO Item data from EKPO.

3- TOP-OF-PAGE and TOP-OF-PAGE DURING LINE-SELECTION: Print page headers for clarity.

---------------------------------------------------------------------------------------------------------------------
Internal Structures
ty_ekko
Custom structure for EKKO table data.

ty_ekpo
Custom structure for EKPO table data.
---------------------------------------------------------------------
Internal Tables and Work Areas
it_ekko, wa_ekko — For EKKO data.

it_ekpo, wa_ekpo — For EKPO data.
------------------------------------------------------------------------
Example Scenario
Execute the program via T-Code SE38.

Enter PO number(s) or leave the field blank to fetch all.

Output displays Purchase Order headers.

Click on a PO line to drill down to item details.

-----------------------------------------------------------------

Name: VIKAS KUMAR
SAP Version: S/4HANA
Language: ABAP
Email Id : - abhisheksonu8789@gmail.com






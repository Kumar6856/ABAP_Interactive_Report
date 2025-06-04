*&---------------------------------------------------------------------*
*& Report ZREPORT_INTRECTIVE_1001
*&---------------------------------------------------------------------*
*&
*&---------------------------------------------------------------------*
REPORT zreport_intrective_1001.


*-------------------------TABLES-------------------------------

TABLES : ekko,ekpo.

*----------------------------TABLES------------------------------


*-------------------------------------------STRUCTURE_ty_ekko---------------------------------------

TYPES: BEGIN OF ty_ekko,
         ebeln TYPE ekko-ebeln, "Purchase Order
         bukrs TYPE ekko-bukrs, "Company Code
         bstyp TYPE ekko-bstyp, "Category
         bsart TYPE ekko-bsart, "Type
         lifnr TYPE ekko-lifnr, "Vendor
       END OF ty_ekko.

*-------------------------------------------STRUCTURE_ty_ekko---------------------------------------

*-------------------------------------------STRUCTURE_ty_ekpo---------------------------------------

TYPES : BEGIN OF ty_ekpo,
          ebeln TYPE ekpo-ebeln, "Purchase Order
          ebelp TYPE ekpo-ebelp, "PO Item
          matnr TYPE ekpo-matnr, "Material
          werks TYPE ekpo-werks, "Plant
          lgort TYPE ekpo-lgort, "Storage Location
          matkl TYPE ekpo-matkl, "Material Group
          menge TYPE ekpo-menge, "Quantity
          meins TYPE ekpo-meins, "Unit
        END OF ty_ekpo.
*-------------------------------------------STRUCTURE_ty_ekpo---------------------------------------

*----------------------------------INTERNAL_TABLE_AND_WORK_AREA--------------------------------

DATA : it_ekko TYPE TABLE OF ty_ekko,
       wa_ekko TYPE ty_ekko.

DATA : it_ekpo TYPE TABLE OF ty_ekpo,
       wa_ekpo TYPE ty_ekpo,


       v_repid TYPE sy-repid, "Program name
       v_date  TYPE sy-datum, "Current date
       v_user  TYPE sy-uname. "User name
*----------------------------------INTERNAL_TABLE_AND_WORK_AREA--------------------------------

*-------------------------------------------------*
* INITIALIZATION - Set default values
*-------------------------------------------------*
INITIALIZATION.
  v_repid = sy-repid.
  v_date  = sy-datum.
  v_user  = sy-uname.

*PARAMETERS : P_ebeln TYPE ekko-ebeln.


*----------------------------SELECTION_SCREEN---------------------------------------------------

SELECTION-SCREEN : BEGIN OF BLOCK B1 WITH FRAME TITLE Text-001.
SELECT-OPTIONS : S_ebeln FOR ekko-ebeln.
SELECTION-SCREEN END OF BLOCK B1.

*----------------------------SELECTION_SCREEN---------------------------------------------------

*-------------------------------------------------*
* START-OF-SELECTION - Data fetching logic
*-------------------------------------------------*

START-OF-SELECTION.

IF  S_ebeln IS INITIAL.
  SELECT ebeln
           bukrs
           bstyp
           bsart
           lifnr

      FROM ekko
      INTO TABLE it_ekko.


ELSEIF S_ebeln IS NOT INITIAL.
  SELECT ebeln
         bukrs
         bstyp
         bsart
         lifnr

    FROM ekko
    INTO TABLE it_ekko
    WHERE ebeln IN S_ebeln.
ELSE.
  MESSAGE 'Error' TYPE 'E'.
ENDIF.


LOOP AT   it_ekko INTO wa_ekko.
  FORMAT COLOR 1 INTENSIFIED ON.
  WRITE : / sy-vline ,wa_ekko-ebeln,
            18 sy-vline,35 wa_ekko-bukrs,
            43 sy-vline,77 wa_ekko-bstyp,
           84 sy-vline ,132 wa_ekko-bsart,
            135 sy-vline ,180 wa_ekko-lifnr,187 sy-vline.
  FORMAT COLOR OFF.
ENDLOOP.
*-------------------------------------------------*
* END-OF-SELECTION - Footer line
*-------------------------------------------------*
END-OF-SELECTION.
  ULINE.
  WRITE: /70 ' ~~End of Report~~'.


*-------------------------------------------------*
* TOP-OF-PAGE - Header content
*-------------------------------------------------*

TOP-OF-PAGE.
  WRITE: / 'Purchase Order Header',
           / 'Date: ',   12 v_date DD/MM/YYYY,
           / 'User: ',   12 v_user,
           / 'Report: ', 12 v_repid.

  ULINE.
  FORMAT COLOR 3 INTENSIFIED ON.
  WRITE: / sy-vline,'Purchase Order',sy-vline, 35'Company',sy-vline,75 'Category',sy-vline,130 'Type',sy-vline,180 'Vendor',sy-vline.
  FORMAT COLOR OFF.
  ULINE.



*---------------is an event in classical ABAP reports that triggers when the user selects or double-clicks a line in the report output list.

AT LINE-SELECTION.

  DATA : fval(50),
         fnam(20).

  GET CURSOR FIELD fnam VALUE fval.


*------------------------Data_fetching_logic------------------------------------


  SELECT ebeln
         ebelp
         matnr
         werks
         lgort
         matkl
         menge
         meins
    FROM ekpo
    INTO TABLE it_ekpo
    WHERE ebeln = fval.


  LOOP AT   it_ekpo INTO wa_ekpo.
    FORMAT COLOR 1 INTENSIFIED ON.
    WRITE : / sy-vline, wa_ekpo-ebeln,
            14 sy-vline, 25 wa_ekpo-ebelp,
             36 sy-vline,50 wa_ekpo-matnr,
              66 sy-vline,80 wa_ekpo-werks,
             86 sy-vline, 105 wa_ekpo-lgort,
              117 sy-vline, 129 wa_ekpo-matkl,
             140 sy-vline, 145 wa_ekpo-menge,
              164 sy-vline, 175 wa_ekpo-meins,187 sy-vline.
    FORMAT COLOR OFF.
  ENDLOOP.

ULINE.
  WRITE: /70 ' ~~End of PO Item List~~'.


TOP-OF-PAGE DURING LINE-SELECTION.
  WRITE: / 'Purchase Order Item List',
          / 'Date: ',   12 v_date DD/MM/YYYY,
           / 'User: ',   12 v_user,
           / 'Report: ', 12 v_repid.
  ULINE.
  FORMAT COLOR 3 INTENSIFIED ON.
  WRITE: / sy-vline ,'Pur DocNum', sy-vline,20 'Pur Doc ItemNum', sy-vline,50 'MATERIAL NUMBER',sy-vline,80 'Plant',sy-vline,100 'Storage location',sy-vline,125 'MATERIAL GROUP',sy-vline,150 'Pur Order Qty',sy-vline,172 'Pur Order Unit',sy-vline.
  FORMAT COLOR OFF.
  ULINE.

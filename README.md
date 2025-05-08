# BAPI_RESERVATION_CREATE
Here's a very detailed explanation of the input structures for BAPI_RESERVATION_CREATE, broken down into.


1: RESERVATION_HEADER – General info about the reservation.

2: RESERVATION_ITEMS – Each material you want to reserve.

🧠 1. RESERVATION_HEADER (Structure: BAPI_RES_HEADER)
This contains header-level info for the whole reservation. It applies to all reservation items.

| Field        | Type     | Meaning                     | Notes                                                                    |
| ------------ | -------- | --------------------------- | ------------------------------------------------------------------------ |
| `PLANT`      | CHAR(4)  | Plant                       | The plant where the reservation is created (e.g., '1020').               |
| `RES_DATE`   | DATS     | Reservation Date            | Usually `SY-DATUM`, today’s date.                                        |
| `COST_CTR`   | CHAR(10) | Cost Center                 | Mandatory if you use movement type 201 (reservation for internal use).   |
| `MOVE_TYPE`  | CHAR(3)  | Movement Type               | Examples: `'201'` (to cost center), `'261'` (for production order).      |
| `PART_ACCT`  | CHAR(2)  | Account Assignment Category | `'OR'` for cost center.                                                  |
| `GR_RCPT`    | CHAR(12) | Goods Recipient             | Optional. Could be a name, serial number, etc.                           |
| `CREATED_BY` | CHAR(12) | Created By                  | Usually `SY-UNAME`.                                                      |
| `ORDER_NO`   | CHAR(12) | Order Number                | Used if reservation is for a production or maintenance order (optional). |
| `WBS_ELEM`   | CHAR(24) | Project element (WBS)       | Only for project-based reservations.                                     |
| `COSTOBJECT` | CHAR(12) | Cost Object                 | Optional (if used instead of cost center).                               |
| `NETWORK`    | CHAR(12) | Network Number              | Optional. Used in project systems.                                       |


📦 2. RESERVATION_ITEMS (Table: BAPI_RES_ITEM)
Each entry in this table represents one material you want to reserve.

| Field        | Type     | Meaning                    | Notes                                                          |
| ------------ | -------- | -------------------------- | -------------------------------------------------------------- |
| `MATERIAL`   | CHAR(18) | Material Number            | Use function module `CONVERSION_EXIT_MATN1_INPUT` to clean it. |
| `PLANT`      | CHAR(4)  | Plant                      | Must match header plant.                                       |
| `STORE_LOC`  | CHAR(4)  | Storage Location           | Where the material is physically stored.                       |
| `QUANTITY`   | DEC      | Quantity                   | The amount to reserve.                                         |
| `UNIT`       | UNIT(3)  | Unit of Measure            | Use `CONVERSION_EXIT_CUNIT_INPUT` to format it.                |
| `SHORT_TEXT` | CHAR(40) | Short Description          | Optional. Free text (e.g., "Brake pads for truck").            |
| `MOVE_TYPE`  | CHAR(3)  | Movement Type (Item-level) | Optional. Usually set in the header.                           |
| `REQ_DATE`   | DATS     | Required Date              | When the reservation is needed. Optional.                      |
| `MOVEMENT`   | CHAR(1)  | Movement Execution Flag    | Set this to `'X'` to allow processing.                         |


    wait = 'X'.

Without this step, the reservation won’t be saved in the system!

    



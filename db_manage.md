#### **Chapter 1 & 2: The Why and What of Databases**

**1. The Core Problem: Traditional File Processing**
The old way of storing data in separate files for each application was fundamentally broken.
*   **Data Redundancy:** The same data (e.g., a customer's address) is duplicated in multiple files.
*   **Data Inconsistency:** The most dangerous flaw. If you update an address in one file but not another, the data is now unreliable.
*   **Program-Data Dependence:** Programs were tied to the physical file structure. Change the file, and you must rewrite the program.

These issues lead directly to **Data Anomalies**, the problems we solve with normalization.

**2. The Solution: The Database Approach**
A **Database Management System (DBMS)** is the software that solves these problems by:
1.  Centralizing data to eliminate redundancy.
2.  Enforcing rules to maintain consistency.
3.  Separating the logical data from the physical storage.

**3. Key DBMS Functions & Integrity Constraints**
A DBMS provides critical services. You must know these.

*   **Concurrency Control:** Manages simultaneous user access, preventing errors like two people booking the same seat.
*   **Backup and Recovery:** Protects against data loss.
*   **Security:** Handles authentication (who you are) and authorization (what you can do).
*   **Integrity Services:** Enforces data accuracy using rules called **integrity constraints**.

**Oracle Integrity Constraints (Directly from your Midterm):**
*   **PRIMARY KEY:** A combination of `NOT NULL` and `UNIQUE`. It uniquely identifies every row in a table. A table can only have one.
*   **FOREIGN KEY:** The primary key of one table placed in another to create a link. It enforces **referential integrity**, ensuring you cannot link to a non-existent record (e.g., assign an order to a customer who doesn't exist).
*   **NOT NULL:** Ensures a column cannot contain a NULL value.
*   **UNIQUE:** Ensures all values in a column (or set of columns) are unique. It allows NULLs.
*   **CHECK:** A powerful constraint that enforces a custom rule on a column's values.
    *   *Example:* `CHECK (Salary > 0)` or `CHECK (Gender IN ('M', 'F'))`.

---

### **Chapter 3: Database Architecture (The Blueprint)**

**The ANSI-SPARC Three-Level Architecture**
This is the standard model for how databases are structured to provide data independence.

1.  **Internal Level (Physical):**
    *   **Concern:** *How* the data is physically stored on disk.
    *   **Details:** File structures, indexes, pointers.
    *   **Analogy:** The actual electrical wiring and plumbing inside the walls of a house.

2.  **Conceptual Level (Logical):**
    *   **Concern:** *What* data is stored and the relationships between data for the entire organization.
    *   **Details:** The complete ERD, all entities, all attributes, all relationships.
    *   **Analogy:** The architect's complete blueprint of the house, showing every room and how they connect.

3.  **External Level (View):**
    *   **Concern:** A specific user's or application's view of the data.
    *   **Details:** Only the parts of the conceptual model that a user needs to see.
    *   **Analogy:** The view of the house a plumber sees (only pipes and fixtures) or an electrician sees (only wiring and outlets).

**Data Independence: The Main Goal**
*   **Physical Data Independence:** You can change the **Internal Level** (e.g., move to a faster SSD) without affecting the **Conceptual Level**. The blueprint of the house doesn't change just because you use copper pipes instead of PVC.
*   **Logical Data Independence:** You can change the **Conceptual Level** (e.g., add a new room to the blueprint) without affecting existing **External Views**. The plumber's view of the existing bathroom doesn't change just because you added a new bedroom.

---

### **Chapter 4: Relational Algebra (The Language of Data Manipulation)**

This is the formal theory behind SQL. It describes operations on tables.

**Symbols you MUST know:**
*   `σ` (Sigma): SELECT (filters rows)
*   `π` (Pi): PROJECT (selects columns)
*   `⨝`: Natural Join
*   `∪`: Union
*   `∩`: Intersection
*   `-`: Difference

| Operation | Symbol | What it Does | SQL Equivalent |
| :--- | :--- | :--- | :--- |
| **SELECT** | `σ` | Filters **rows** based on a condition. | `WHERE` |
| **PROJECT** | `π` | Selects **columns**. **Crucially, it also removes duplicate rows.** | `SELECT DISTINCT` |
| **UNION** | `∪` | Combines rows from two compatible tables. Duplicates removed. | `UNION` |
| **INTERSECTION** | `∩` | Returns only rows that exist in **both** tables. | `INTERSECT` |
| **DIFFERENCE** | `-` | Returns rows in the first table but **not** in the second. | `EXCEPT` / `MINUS` |
| **NATURAL JOIN**| `⨝` | Combines tables based on matching values in common columns. | `NATURAL JOIN` or `JOIN ... ON` |
| **LEFT JOIN** | `⟕` | Includes all rows from the **left** table, plus matching rows from the right. | `LEFT OUTER JOIN` |

---

### **Chapter 5: Normalization (The Process of Good Design)**

**GOAL:** Eliminate redundancy to prevent data anomalies.
**TOOL:** Functional Dependencies (FDs).

**1. Data Anomalies (The Problems to Solve)**
*   **Insertion Anomaly:** Cannot add a fact about one entity until you have a fact about another.
    *   *Example:* Cannot add a new `Item` with its `UnitPrice` until a `Customer` places an `Order` for it.
*   **Modification (Update) Anomaly:** Changing one fact requires updating multiple rows.
    *   *Example:* To change the `UnitPrice` of a 'Pencil', you must find and update every single order row where a pencil was sold.
*   **Deletion Anomaly:** Deleting one fact causes the loss of another unrelated fact.
    *   *Example:* If customer John's only order (`DO0002`) is deleted, the fact that item `I0003` is "Gum" with a price of `1.00` is completely lost from the database.

**2. Functional Dependency (The Core Concept)**
*   **Definition:** `A -> B` means "A determines B". For any value of A, there is only ONE corresponding value of B.
*   **Determinant:** The attribute on the left side of the arrow (A is the determinant).

**3. The Process: Step-by-Step Normal Forms**

Let's use the **DeliveryOrder** example from your slides.

**Unnormalized Form (UNF):** A source document or a table with repeating groups.
*   *Example:* One row for `DO0001` has multiple items listed within it.

#### **First Normal Form (1NF)**
*   **Rule:** Every cell must have a single, atomic value. No repeating groups.
*   **Action:** "Flatten the table." Create a new row for each item in the repeating group, duplicating the non-repeating data.

**The `DeliveryOrder` table is now in 1NF:**

| DO_No | DelDate | CustNo | CustName | CustPhone | ItemNo | Desc | UnitPrice | Qty |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| DO0001 | 23/8/05 | C0002 | Eric | 03-61381111 | I0007 | Pencil | 3.00 | 200 |
| DO0001 | 23/8/05 | C0002 | Eric | 03-61381111 | I0010 | Paper | 8.00 | 100 |
| DO0002 | 23/8/05 | C0003 | John | 03-41490888 | I0003 | Gum | 1.00 | 350 |
| DO0002 | 23/8/05 | C0003 | John | 03-41490888 | I0007 | Pencil | 3.00 | 50 |
| DO0002 | 23/8/05 | C0003 | John | 03-41490888 | I0010 | Paper | 8.00 | 500 |

**Problem:** Massive redundancy. We now have insertion, deletion, and modification anomalies.

#### **Second Normal Form (2NF)**
*   **Rule:** Must be in 1NF, and **every non-key attribute must be fully dependent on the entire primary key.** (This only applies if the PK is composite).
*   **Action:** Remove **partial dependencies**.

1.  **Identify PK:** The primary key that uniquely identifies each row is `(DO_No, ItemNo)`.
2.  **Identify FDs:**
    *   `(DO_No, ItemNo) -> Qty` (**Full Dependency** - good)
    *   `DO_No -> DelDate, CustNo, CustName, CustPhone` (**Partial Dependency** - bad! These depend only on `DO_No`)
    *   `ItemNo -> Desc, UnitPrice` (**Partial Dependency** - bad! These depend only on `ItemNo`)
3.  **Decompose:** Create new tables to remove the partial dependencies.

**The tables are now in 2NF:**
*   **DeliveryItem**(`<u>DO_No*</u>`, `<u>ItemNo*</u>`, Qty)
*   **DeliveryOrder**(`<u>DO_No</u>`, DelDate, CustNo, CustName, CustPhone)
*   **Item**(`<u>ItemNo</u>`, Desc, UnitPrice)

**Problem:** We still have redundancy in the `DeliveryOrder` table.

#### **Third Normal Form (3NF)**
*   **Rule:** Must be in 2NF, and there must be **no transitive dependencies**. A transitive dependency is when a non-key attribute determines another non-key attribute (`A -> B -> C`).
*   **Action:** Remove **transitive dependencies**.

1.  **Identify Transitive FDs:** In the `DeliveryOrder` table, `DO_No` is the key. We see that `DO_No -> CustNo`, and `CustNo -> CustName, CustPhone`. This is a classic transitive dependency. `CustName` and `CustPhone` depend on `CustNo`, which is not the primary key.
2.  **Decompose:** Create a new table to remove the transitive dependency.

**Final 3NF Relations (The "Correct" Design):**
*   **DeliveryItem**(`<u>DO_No*</u>`, `<u>ItemNo*</u>`, Qty)
*   **Item**(`<u>ItemNo</u>`, Desc, UnitPrice)
*   **DeliveryOrder**(`<u>DO_No</u>`, DelDate, *CustNo*)
*   **Customer**(`<u>CustNo</u>`, CustName, CustPhone)

This structure is now free of redundancy and the associated anomalies. Study this process until it becomes second nature.
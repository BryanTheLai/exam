### **The Complete BACS3183 Study Guide (Chapters 1-5)**

#### **Chapter 1: Information Models & Core Concepts**

**1. The DIKW Pyramid: The Goal of a Database**
Transforms raw facts into decisions.

*   **Data:** Raw, unorganized facts. No context.
    *   *Example:* `S1111`, `Alice Ng`, `25`
*   **Information:** Data processed with context. Answers "who, what, where, when."
    *   *Example:* The student `Alice Ng (S1111)` scored `25` on the exam.
*   **Knowledge:** Information understood and applied. Answers "how."
    *   *Example:* Knowing Alice Ng failed, we know *how* to take the next step: schedule her for a re-sit exam.

**2. Categorization of Information**
*   **By Nature:**
    *   **Quantitative:** Numerical, measurable. **QUANTITY**. (e.g., Height, Price, Age).
    *   **Qualitative:** Descriptive, characteristic. **QUALITY**. (e.g., Color, Material, Shape).
*   **By Level (Decision Making):**
    *   **Strategic:** High-level summaries for long-term goals.
    *   **Tactical:** Mid-level info for departmental planning.
    *   **Operational:** Detailed info for daily tasks.

**3. Metadata: Data About Data**
*   **Definition:** Information that describes the structure, rules, and context of the actual data. It's the "blueprint" of the database.
*   **Data Dictionary:** The central repository where all metadata is stored. The DBMS consults this for every operation.

---

#### **Chapter 2: Database Systems**

**1. The Problem: Traditional File Processing**
The old way of storing data in separate files for each application.

*   **Data Redundancy:** Same data duplicated across many files.
*   **Program-Data Dependence:** Program logic is tied to the physical file structure.
*   **Data Inconsistency:** The critical failure. Update data in one file but not another, leading to unreliable information.

**2. The Solution: The Database Approach**
A **Database Management System (DBMS)** is the software that solves these problems by:
1.  Centralizing data to eliminate redundancy.
2.  Enforcing rules to maintain consistency.
3.  Separating the logical data from the physical storage.

**3. Integrity Constraints (Directly from midterm)**
These are the rules enforced by the DBMS to ensure data quality.

*   **NOT NULL:** Ensures a column cannot contain a NULL value.
*   **UNIQUE:** Ensures all values in a column are unique. Allows NULLs.
*   **PRIMARY KEY:** A combination of `NOT NULL` and `UNIQUE`. Uniquely identifies every row in a table. A table can only have one.
*   **FOREIGN KEY:** A primary key from one table placed in another to create a link. Enforces **referential integrity** (prevents "orphan" records).
*   **CHECK:** Enforces a custom business rule on a column's values.
    *   *Example:* `CHECK (Salary > 0)` or `CHECK (Gender IN ('M', 'F'))`.

**4. ANSI-SPARC Three-Level Architecture**
The standard model for separating different views of data.

*   **Internal Level (Physical):** **How** the data is physically stored (files, indexes).
*   **Conceptual Level (Logical):** **What** data is stored (the entire database schema, ERD).
*   **External Level (View):** The **part** of the database a specific user or application sees.

**5. Data Independence**
The primary benefit of the Three-Level Architecture.

*   **Physical Data Independence:** Change the **Internal Level** without affecting the **Conceptual Level**.
*   **Logical Data Independence:** Change the **Conceptual Level** without affecting existing **External Views**.

---

#### **Chapter 3: Data Modelling**

**1. Core Concepts**
*   **Entity-Relationship (ER) Model:** The blueprint for conceptual design.
    *   **Entity:** An object or concept (`Student`).
    *   **Attribute:** A property of an entity (`StudentName`).
    *   **Relationship:** An association between entities (`Student` *enrolls in* `Course`).
*   **Relational Model:** Data is stored in **tables** (relations).

---

#### **Chapter 4: Relational Algebra**

The formal, mathematical language behind SQL.

**Symbols you MUST know:**
*   `σ` (Sigma): **SELECT** (filters rows)
*   `π` (Pi): **PROJECT** (selects columns)
*   `⨝`: **Natural Join**
*   `∪`: **Union**
*   `∩`: **Intersection**
*   `-`: **Difference**

**Operations Summary:**
| Operation | Symbol | Action | SQL Equivalent |
| :--- | :--- | :--- | :--- |
| **SELECT** | `σ` | Filters **rows**. | `WHERE` |
| **PROJECT** | `π` | Selects **columns**. | `SELECT DISTINCT` |
| **UNION** | `∪` | Combines rows from two tables. | `UNION` |
| **INTERSECTION**| `∩` | Returns rows in **both** tables. | `INTERSECT` |
| **DIFFERENCE** | `-` | Returns rows in table A but **not** B. | `EXCEPT` / `MINUS` |
| **NATURAL JOIN**| `⨝` | Combines tables on common columns. | `NATURAL JOIN` |

---

#### **Chapter 5: Normalization**

**The core goal: Organize tables to minimize redundancy and eliminate anomalies.**

**1. Data Anomalies (The Problems to Solve) (Directly from midterm)**
*   **Insertion Anomaly:** Cannot add new data because other unrelated data is required.
    *   *Example:* Cannot add a new `Item` (e.g., 'Eraser', 'I0011') to the `DeliveryOrder` table because it requires a `DO_No`, but the item hasn't been ordered yet.
*   **Modification Anomaly:** A single logical update requires changing multiple physical rows.
    *   *Example:* If customer 'Eric' changes his phone number, you must find and update it on **every single line item** he has ever ordered. Missing one creates inconsistency.
*   **Deletion Anomaly:** Deleting one piece of information unintentionally deletes another unrelated fact.
    *   *Example:* In the `DeliveryOrder` table, if you delete the only order line for 'Gum' (`I0003`), you lose the fact that `I0003` is 'Gum' and costs `1.00`.

**2. Functional Dependencies (FDs): The Tool for Normalization**
*   **Definition:** `A -> B` ("A determines B"). If you know A's value, you know B's single, unique value.
*   **Partial Dependency:** A non-key attribute depends on only a **part** of a composite primary key.
*   **Transitive Dependency:** A non-key attribute depends on another non-key attribute. (`PK -> Non-key A -> Non-key B`).

**3. The Normal Forms: A Step-by-Step Guide with Examples**

#### **Example 1: Normalizing `DeliveryOrder` (From your slides)**

**1NF: Eliminate Repeating Groups**
*   **Rule:** Each cell must hold a single value. The table must have a primary key.
*   **Action:** Flatten the table by creating a unique row for each line item.

**Before (Unnormalized Form - UNF):** One row for `DO0001` with multiple items.
**After (First Normal Form - 1NF):**
`DeliveryOrder1NF(<u>DO_No</u>, <u>ItemNo</u>, DelDate, CustNo, CustName, CustPhone, Desc, UnitPrice, Qty)`

| DO_No | ItemNo | DelDate | CustNo | CustName | CustPhone | Desc | UnitPrice | Qty |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| DO0001| I0007 | 23/8/05 | C0002 | Eric | 03-61381111 | Pencil | 3.00 | 200 |
| DO0001| I0010 | 23/8/05 | C0002 | Eric | 03-61381111 | Paper | 8.00 | 100 |
| DO0002| I0003 | 23/8/05 | C0003 | John | 03-41490888 | Gum | 1.00 | 350 |

**2NF: Eliminate Partial Dependencies**
*   **Rule:** Every non-key attribute must depend on the **entire** primary key.
*   **Action:** Find attributes that depend on only part of the PK (`DO_No` or `ItemNo`) and move them to new tables.

**FDs in 1NF table:**
*   `DO_No` -> `DelDate`, `CustNo`, `CustName`, `CustPhone` (**Partial!** Depends only on `DO_No`)
*   `ItemNo` -> `Desc`, `UnitPrice` (**Partial!** Depends only on `ItemNo`)
*   (`DO_No`, `ItemNo`) -> `Qty` (Full Dependency - OK)

**After (Second Normal Form - 2NF):**
*   **DeliveryItem**(`<u>*DO_No*</u>`, `<u>*ItemNo*</u>`, Qty)
*   **Item**(`<u>ItemNo</u>`, Desc, UnitPrice)
*   **DeliveryOrder**(`<u>DO_No</u>`, DelDate, CustNo, CustName, CustPhone)

**3NF: Eliminate Transitive Dependencies**
*   **Rule:** No non-key attribute can depend on another non-key attribute.
*   **Action:** Find transitive FDs in the 2NF tables and move them to a new table.

**FDs in 2NF tables:**
*   In `DeliveryOrder`: `DO_No -> CustNo -> CustName, CustPhone` (**Transitive!**)

**After (Third Normal Form - 3NF):**
*   **DeliveryItem**(`<u>*DO_No*</u>`, `<u>*ItemNo*</u>`, Qty)
*   **Item**(`<u>ItemNo</u>`, Desc, UnitPrice)
*   **DeliveryOrder**(`<u>DO_No</u>`, DelDate, *CustNo*)
*   **Customer**(`<u>CustNo</u>`, CustName, CustPhone)

---

#### **Example 2: Normalizing `StudentResult` (Directly from midterm)**

**1NF Table:**
`StudentResult(<u>StudentID</u>, <u>CourseID</u>, <u>ExamDate</u>, StudentName, ProgID, ProgName, CourseName, LevelID, LevelDesc, Mark, Status)`

**2NF: Eliminate Partial Dependencies**
*   **FDs to find:** Attributes depending on just `StudentID` or just `CourseID`.
    *   `StudentID -> StudentName, ProgID` (**Partial**)
    *   `CourseID -> CourseName, LevelID` (**Partial**)
*   **Action:** Create `Student` and `Course` tables.

**Resulting 2NF Tables:**
1.  **Result**(`<u>*StudentID*</u>`, `<u>*CourseID*</u>`, `<u>ExamDate</u>`, Mark, Status)
2.  **Student**(`<u>StudentID</u>`, StudentName, ProgID)
3.  **Course**(`<u>CourseID</u>`, CourseName, LevelID)

**3NF: Eliminate Transitive Dependencies**
*   **FDs to find:** Non-key attributes depending on other non-key attributes in the 2NF tables.
    *   In `Student`: `StudentID -> ProgID -> ProgName` (**Transitive**)
    *   In `Course`: `CourseID -> LevelID -> LevelDesc` (**Transitive**)
*   **Action:** Create `Program` and `Level` tables.

**Final 3NF Relations:**
1.  **Result**(`<u>*StudentID*</u>`, `<u>*CourseID*</u>`, `<u>ExamDate</u>`, Mark, Status)
2.  **Student**(`<u>StudentID</u>`, StudentName, *ProgID*)
3.  **Course**(`<u>CourseID</u>`, CourseName, *LevelID*)
4.  **Program**(`<u>ProgID</u>`, ProgName)
5.  **Level**(`<u>LevelID</u>`, LevelDesc)
**1. The DIKW Pyramid: The Goal of a Database** (IMPORTANT)
Transforms raw facts into decisions.

*   **Data:** Raw, unorganized facts. No context.
    *   *Example:* `S1111`, `Alice Ng`, `25`
*   **Information:** Data with context and meaning.
    *   *Example:* The student `Alice Ng (S1111)` scored `25` on the exam.
*   **Knowledge:** Understanding of information enabling decision making.
    *   *Example:* Knowing Alice Ng failed, we know *how* to take the next step: schedule her for a re-sit exam.

**2. Categorization of Information** (IMPORTANT)
*   **By Nature:**
    *   **Quantitative:** Numerical, measurable. **QUANTITY**. (e.g., Height, Price, Age).
    *   **Qualitative:** Descriptive, characteristic. **QUALITY**. (e.g., Color, Material, Shape).
*   **By Level (Decision Making):**
    *   **Strategic:** **High-level summaries** for long-term goals.
    *   **Tactical:** **Mid-level** info for departmental planning.
    *   **Operational:** Detailed info for **daily tasks.**


**5. Data Independence**
The primary benefit of the Three-Level Architecture.

*   **Physical Data Independence:** Change the **Internal Level** without affecting the **Conceptual Level**.
*   **Logical Data Independence:** Change the **Conceptual Level** without affecting existing **External Views**.

**3. Integrity Constraints (Directly from midterm)** (IMPORTANT)
These are the rules enforced by the DBMS to ensure data quality.

*   **NOT NULL:** Ensures a column **cannot contain a NULL value**.
*   **UNIQUE:** Ensures **all values** in a column are **unique**. Allows NULLs.
*   **PRIMARY KEY:** A combination of **`NOT NULL`** and **`UNIQUE`**. Uniquely identifies every row in a table. A table can only have one.
*   **FOREIGN KEY:** A primary key for 1 table to **link** to another. Enforces **referential integrity** (prevents "orphan" records).
*   **CHECK:** **Enforces** a custom business **rule on a column's** values.
    *   *Example:* `CHECK (Salary > 0)` or `CHECK (Gender IN ('M', 'F'))`.


100% come out:
linking/bridge table to break M:N relationship into 1:M relationship.
Relational algebra: 
- π, σ, ∧, LIKE '%Melati Jaya%', (Staff)
- >=
- Jcount,JSUM, ⨝
#### **Chapter 4: Relational Algebra** (IMPORTANT)

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


**1. Data Anomalies (The Problems to Solve) (Directly from midterm)** (IMPORTANT)
*   **Insertion Anomaly:** **Cannot add new data** because other unrelated data is required.
    *   *Example:* Cannot add a new `Item` (e.g., 'Eraser', 'I0011') to the `DeliveryOrder` table because it requires a `DO_No`, but the item hasn't been ordered yet.
*   **Modification Anomaly:** A single logical update **requires changing multiple physical rows**.
    *   *Example:* If customer 'Eric' changes his phone number, you must find and update it on **every single line item** he has ever ordered. Missing one creates inconsistency.
*   **Deletion Anomaly:** Deleting one piece of information **unintentionally deletes another unrelated fact**.
    *   *Example:* In the `DeliveryOrder` table, if you delete the only order line for 'Gum' (`I0003`), you lose the fact that `I0003` is 'Gum' and costs `1.00`.


# 1NF, 2NF, 3NF

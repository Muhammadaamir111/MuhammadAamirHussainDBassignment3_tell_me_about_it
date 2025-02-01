# Analysis and Reflection on Library Database Design

## 1. Advanced SQL and Normalization

### a) Description of a Complex Database Structure
The **Library Database System** consists of three core tables: `Book`, `Borrower`, and `Loan`. These tables manage books, borrowers, and loan transactions efficiently. To enhance functionality, additional tables such as `Librarian` (for staff management) and `Reservation` (to track book reservations) can be introduced.

- **Book Table**: Stores book details including `title`, `author`, `publication_year`, and `isbn` (unique identifier).
- **Borrower Table**: Maintains borrower information like `name`, `phone`, and `email`.
- **Loan Table**: Links books to borrowers, recording `loan_date`, `due_date`, and `return_date`.
- **Librarian Table** (optional improvement): Stores librarian details managing the system.
- **Reservation Table** (optional improvement): Tracks reservations for books not currently available.

Each table uses **primary keys (PK)** and **foreign keys (FK)** to establish relationships, ensuring data consistency and referential integrity.

### b) Advanced SQL Query with JOINs and a Subquery
The following SQL query retrieves details of overdue books, including borrower information and loan details. It involves **two JOIN operations** and a **subquery** to filter overdue loans.

```sql
SELECT Loan.loan_id, Book.title, Borrower.name, Loan.loan_date, Loan.due_date, Loan.return_date
FROM Loan
JOIN Book ON Loan.book_id = Book.book_id
JOIN Borrower ON Loan.borrower_id = Borrower.borrower_id
WHERE Loan.due_date < DATE('now') 
AND Loan.return_date IS NULL;
```

### c) Normalization Analysis
The database follows **Third Normal Form (3NF)**:
- **1NF**: Each table has atomic values with a unique primary key.
- **2NF**: No partial dependencies; all non-key attributes depend on the full primary key.
- **3NF**: No transitive dependencies; each column is directly dependent on the primary key.

Using **3NF eliminates redundancy**, ensures data integrity, and makes updates efficient.

---

## 2. Database Design and Alternative Solutions

### a) Proposed Improvements
1. **Indexing Frequently Queried Fields**
   - Adding indexes on `title` (Book) and `name` (Borrower) would **speed up search queries**.
2. **Introducing a Reservation System**
   - A `Reservation` table can track **requested books**, preventing unnecessary manual tracking.

### b) Alternative Database Structure
- **Denormalized Approach**: Merge `Book` and `Loan` tables to **store borrower details within the Loan table**.
- **Pros**: Faster retrieval of book loans, fewer joins in queries.
- **Cons**: Data duplication, harder updates (e.g., changing borrower contact info requires multiple updates).
- **Current Approach (Normalized)**: Keeps borrower and book information separate, reducing redundancy.

---

## 3. Comparison: Relational vs. Document Databases

### a) Library System in MongoDB (Document Database)
Instead of separate tables, MongoDB would store **book and loan details as nested documents**:

```json
{
  "_id": "1",
  "title": "The Hobbit",
  "author": "J.R.R. Tolkien",
  "loans": [
    { "borrower": "Anna Svensson", "loan_date": "2024-01-01", "due_date": "2024-01-15" }
  ]
}
```

### b) Relational vs. Document Database
| Feature            | Relational Database (SQLite) | Document Database (MongoDB) |
|-------------------|--------------------------|---------------------------|
| **Schema**       | Fixed structure, strict constraints | Flexible, schema-less |
| **Data Integrity** | High (foreign keys, normalization) | Lower (data duplication risk) |
| **Query Performance** | Optimized for structured queries | Faster reads but may require more updates |
| **Use Case Suitability** | Best for structured data (e.g., library) | Best for dynamic and hierarchical data |

---

## 4. Reflection and Argumentation

### a) Reflection on Design Choices
The design **separates books, borrowers, and loans into distinct tables**, ensuring:
- **Efficient updates**: Borrower details update in one place.
- **Data consistency**: Loans reference book and borrower IDs.

### b) Justification for the Chosen Design
The **relational model** was chosen due to its:
- **Strong data integrity** with foreign keys.
- **Efficient query capabilities** for retrieving loan history.

---

### Conclusion
This analysis showcases a **well-structured relational database**, with possible improvements like **indexing and reservation tracking**. The system meets **3NF normalization**, ensuring **data consistency** and **efficient querying**.

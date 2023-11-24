# Library SQL Test

# Database Schema:

You will be working with a database that models a library. Here are the relevant tables:

## books 
- Contains information about books in the library.
- Columns: 
  - book_id (Primary Key)
  - title
  - author
  - publication_year
  - isbn.

## users 
- Contains information about library users.
- Columns: 
  - user_id (Primary Key)
  - first_name
  - last_name
  - email
  - registration_date

## borrowed_books 
- Records when a user borrows a book.
- Columns: 
  - borrow_id (Primary Key)
  - user_id (Foreign Key)
  - book_id (Foreign Key)
  - borrow_date
  - return_date.

## Test Questions:

1. Write a SQL query to retrieve the top 10 most borrowed books, along with the number of times each book has been borrowed.
~~~~sql
SELECT b.title, COUNT(*) AS borrow_count
FROM borrowed_books bb
JOIN books b ON bb.book_id = b.book_id
GROUP BY bb.book_id
ORDER BY borrow_count DESC
LIMIT 10;
~~~~

2. Create a stored procedure that calculates the average number of days a book is borrowed before being returned. 
The procedure should take a book_id as input and return the average number of days.
~~~~sql
CREATE PROCEDURE CalculateAverageBorrowDays(IN p_book_id INT)
BEGIN
    SELECT AVG(DATEDIFF(return_date, borrow_date)) AS avg_borrow_days
    FROM borrowed_books
    WHERE book_id = p_book_id;
END;
~~~~

3. Write a query to find the user who has borrowed the most books from the library.
~~~~sql
SELECT u.user_id, u.first_name, u.last_name, COUNT(*) AS books_borrowed
FROM users u
JOIN borrowed_books bb ON u.user_id = bb.user_id
GROUP BY u.user_id
ORDER BY books_borrowed DESC
LIMIT 1;
~~~~

4. Create an index on the publication_year column of the books table to improve query performance.
~~~~sql
CREATE INDEX idx_publication_year ON books(publication_year);
~~~~

5. Write a SQL query to find all books published in the year 2020 that have not been borrowed by any user.
~~~~sql
SELECT *
FROM books
WHERE publication_year = 2020
  AND book_id NOT IN (SELECT DISTINCT book_id FROM borrowed_books);
~~~~

7. Design a SQL query that lists users who have borrowed books published by a specific author (e.g., "J.K. Rowling").
~~~~sql
SELECT DISTINCT u.user_id, u.first_name, u.last_name
FROM users u
JOIN borrowed_books bb ON u.user_id = bb.user_id
JOIN books b ON bb.book_id = b.book_id
WHERE b.author = 'J.K. Rowling';
~~~~

7. Create a trigger that automatically updates the return_date in the borrowed_books table to the current date when a book is returned.
~~~~sql
CREATE TRIGGER update_return_date
BEFORE UPDATE ON borrowed_books
FOR EACH ROW
SET NEW.return_date = IF(NEW.return_date IS NULL, NOW(), NEW.return_date);
~~~~

Please remember to create and use any necessary tables, procedures, or triggers in the database to answer these questions

## Appendix
### Schema DDL
~~~~sql
CREATE TABLE books (
    book_id INT PRIMARY KEY,
    title VARCHAR(255),
    author VARCHAR(255),
    publication_year INT,
    isbn VARCHAR(20)
);

CREATE TABLE users (
    user_id INT PRIMARY KEY,
    first_name VARCHAR(255),
    last_name VARCHAR(255),
    email VARCHAR(255),
    registration_date DATE
);

CREATE TABLE borrowed_books (
    borrow_id INT PRIMARY KEY,
    user_id INT,
    book_id INT,
    borrow_date DATE,
    return_date DATE,
    FOREIGN KEY (user_id) REFERENCES users(user_id),
    FOREIGN KEY (book_id) REFERENCES books(book_id)
);
~~~~

### Sample Data
~~~~sql
-- Inserting data into the books table
INSERT INTO books (book_id, title, author, publication_year, isbn)
VALUES
    (1, 'The Great Gatsby', 'F. Scott Fitzgerald', 1925, '978-0743273565'),
    (2, 'To Kill a Mockingbird', 'Harper Lee', 1960, '978-0061120084'),
    (3, '1984', 'George Orwell', 1949, '978-0451524935'),
    (4, 'The Catcher in the Rye', 'J.D. Salinger', 1951, '978-0241950425'),
    (5, 'Harry Potter and the Sorcerer''s Stone', 'J.K. Rowling', 1997, '978-0590353427');
    (6, 'The Lord of the Rings', 'J.R.R. Tolkien', 1954, '978-0545010221'),
    (7, 'Pride and Prejudice', 'Jane Austen', 1813, '978-0141439518'),
    (8, 'The Hobbit', 'J.R.R. Tolkien', 1937, '978-0345339683'),
    (9, 'One Hundred Years of Solitude', 'Gabriel Garcia Marquez', 1967, '978-0061120084'),
    (10, 'The Da Vinci Code', 'Dan Brown', 2003, '978-0307474278'),
    (11, 'The Hitchhiker''s Guide to the Galaxy', 'Douglas Adams', 1979, '978-0345391803'),
    (12, 'The Shining', 'Stephen King', 1977, '978-0307743657'),
    (13, 'Brave New World', 'Aldous Huxley', 1932, '978-0061120084'),
    (14, 'The Grapes of Wrath', 'John Steinbeck', 1939, '978-0143039433'),
    (15, 'The Martian', 'Andy Weir', 2011, '978-0553418026'),
    (16, 'The Alchemist', 'Paulo Coelho', 1988, '978-0061120084'),
    (17, 'The Hunger Games', 'Suzanne Collins', 2008, '978-0439023481'),
    (18, 'The Chronicles of Narnia', 'C.S. Lewis', 1950, '978-0061120084'),
    (19, 'The Girl with the Dragon Tattoo', 'Stieg Larsson', 2005, '978-0307454546'),
    (20, 'Moby-Dick', 'Herman Melville', 1851, '978-0142437247');
    (21, 'Harry Potter and the Chamber of Secrets', 'J.K. Rowling', 1998, '978-0439064866'),
    (22, 'Harry Potter and the Prisoner of Azkaban', 'J.K. Rowling', 1999, '978-0439136358'),
    (23, 'Harry Potter and the Goblet of Fire', 'J.K. Rowling', 2000, '978-0439139595'),
    (24, 'Harry Potter and the Order of the Phoenix', 'J.K. Rowling', 2003, '978-0439358071'),
    (25, 'Harry Potter and the Half-Blood Prince', 'J.K. Rowling', 2005, '978-0439785969'),
    (26, 'Harry Potter and the Deathly Hallows', 'J.K. Rowling', 2007, '978-0545010221');
    (27, 'The Vanishing Half', 'Brit Bennett', 2020, '978-0525536291'),
    (28, 'Such a Fun Age', 'Kiley Reid', 2020, '978-0525541905'),
    (29, 'A Promised Land', 'Barack Obama', 2020, '978-1524763169'),
    (30, 'The Invisible Life of Addie LaRue', 'V.E. Schwab', 2020, '978-0765387561');


-- Inserting data into the users table
INSERT INTO users (user_id, first_name, last_name, email, registration_date)
VALUES
    (1, 'John', 'Doe', 'john.doe@example.com', '2023-01-01'),
    (2, 'Jane', 'Smith', 'jane.smith@example.com', '2023-02-15'),
    (3, 'Robert', 'Johnson', 'robert.johnson@example.com', '2023-03-20'),
    (4, 'Alice', 'Williams', 'alice.williams@example.com', '2023-04-10');

-- Inserting data into the borrowed_books table
INSERT INTO borrowed_books (borrow_id, user_id, book_id, borrow_date, return_date)
VALUES
    (1, 1, 1, '2023-01-10', '2023-02-05'),
    (2, 2, 3, '2023-02-20', NULL),
    (3, 3, 5, '2023-03-05', '2023-04-01'),
    (4, 1, 2, '2023-04-15', NULL),
    (5, 4, 4, '2023-05-01', NULL);
    (6, 3, 29, '2023-06-01', NULL);

~~~~

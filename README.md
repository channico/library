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

2. Create a stored procedure that calculates the average number of days a book is borrowed before being returned. The procedure should take a book_id as input and return the average number of days.

3. Write a query to find the user who has borrowed the most books from the library.

4. Create an index on the publication_year column of the books table to improve query performance.

5. Write a SQL query to find all books published in the year 2020 that have not been borrowed by any user.

6. Design a SQL query that lists users who have borrowed books published by a specific author (e.g., "J.K. Rowling").

7. Create a trigger that automatically updates the return_date in the borrowed_books table to the current date when a book is returned.

8. Please remember to create and use any necessary tables, procedures, or triggers in the database to answer these questions
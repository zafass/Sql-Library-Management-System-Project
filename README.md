Library Management System

Objective:

This Library Management System is designed to manage and track information about books, employees, branches, customers, and the status of book issues and returns in a library. 
The system keeps records of book availability, cost, and the total number of books available.

Database Name: library

Tables
1. Branch
 
* Branch_no: INTEGER, PRIMARY KEY

* Manager_Id: INTEGER

* Branch_address: VARCHAR(255)

* Contact_no: VARCHAR(50)

2. Employee
 
* Emp_Id: INTEGER, PRIMARY KEY

* Emp_name: VARCHAR(50)

* Position: VARCHAR(50)

* Salary: DECIMAL(10, 2)

* Branch_no: INTEGER, FOREIGN KEY (REFERENCES Branch(Branch_no))

3. Books
 
* ISBN: VARCHAR(13), PRIMARY KEY

* Book_title: VARCHAR(50)

* Category: VARCHAR(20)

* Rental_Price: DECIMAL(10, 2)

* Status: VARCHAR(3) (VALUES: 'yes', 'no')

* Author: VARCHAR(20)

* Publisher: VARCHAR(50)

4. Customer
 
* Customer_Id: INTEGER, PRIMARY KEY

* Customer_name: VARCHAR(50)

* Customer_address: VARCHAR(50)

* Reg_date: DATE

5. IssueStatus
   
* Issue_Id: INTEGER, PRIMARY KEY

* Issued_cust: INTEGER, FOREIGN KEY (REFERENCES Customer(Customer_Id))

* Issued_book_name: VARCHAR(50)

* Issue_date: DATE

* Isbn_book: VARCHAR(50), FOREIGN KEY (REFERENCES Books(ISBN))

6. ReturnStatus

* Return_Id: INTEGER, PRIMARY KEY

* Return_cust: INTEGER

* Return_book_name: VARCHAR(50)

* Return_date: DATE

* Isbn_book2: VARCHAR(25), FOREIGN KEY (REFERENCES Books(ISBN))

Queries:


-- 1. Retrieve the book title, category, and rental price of all available books. 

      Select Book_Title, Category, Rental_Price from Books;

-- 2. List the employee names and their respective salaries in descending order of salary.

      Select Emp_Name, Salary       
      From Employee      
      Order By salary desc;

-- 3. Retrieve the book titles and the corresponding customers who have issued those books. 
      
      Select IssueStatus.Issued_Book_Name, Customer.Customer_Name
      from IssueStatus 
      JOIN Customer on Issuestatus.Issued_cust = customer.customer_id;

-- 4. Display the total count of books in each category. 

      Select Category, count(*) AS Total_Counts
      from books
     group by category;

-- 5. Retrieve the employee names and their positions for the employees whose salaries are above Rs.50,000. 

      Select Emp_Name, Position
      from Employee
      Where Salary > 50000;

-- 6. List the customer names who registered before 2023-04-01 and have not issued any books yet. 

      Select c.customer_name
      from customer c 
      LEFT JOIN issuestatus i
      on c.customer_id = i.issued_cust
      where c.reg_date < '2023-04-01';

-- 7. Display the branch numbers and the total count of employees in each branch. 

      Select  Branch_No, count(*) As Total_Employee
      from employee
      group by branch_no;

-- 8. Display the names of customers who have issued books in the month of June 2023.

      Select c.customer_name
      from customer c  
      inner join issuestatus i on c.customer_id = i.issued_cust
      where issue_date between '2023-06-01' and '2023-06-30';

-- 9. Retrieve book_title from book table containing Romance. 
      
      Select Book_title 
      from Books
      where Category Like '%Romance%';

-- 10.Retrieve the branch numbers along with the count of employees for branches having more than 5 employees
      
      Select Branch_No, count(*) As Total_Employee
      from employee
      group by branch_no
      Having (Total_Employee) > 5;

-- 11. Retrieve the names of employees who manage branches and their respective branch addresses.

      Select e.Emp_name As Manager_Name, b.Branch_Address
      from Branch b 
      join employee e
      on b.manager_id = e.emp_id;

-- 12.  Display the names of customers who have issued books with a rental price higher than dollar 5.

      Select c.customer_name
      from customer c 
      join IssueStatus i on c.customer_id = i.issued_cust
      join Books b on i.isbn_book = b.isbn
      where b.rental_price > 5;

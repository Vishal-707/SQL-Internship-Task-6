# SQL-Internship-Task-6
𝒮𝓊𝒷𝓆𝓊𝑒𝓇𝒾𝑒𝓈 𝒶𝓃𝒹 𝒩𝑒𝓈𝓉𝑒𝒹 𝒬𝓊𝑒𝓇𝒾𝑒𝓈 <br>
The  demonstrate advanced query logic skills by implementing various types of subqueries in 
SELECT, WHERE, and FROM clauses. The main focus of the task is to the ability to integrate subqueries within different parts of a SQL queries.<br>
This task demonstrates the use of SQL subqueries and nested queries to retrieve advanced insights from the E-Commerces database. Subqueries allow us to use the result of one query inside another, enabling more complex data analysis. <br>

# 🛠️ Tools Used
  * Database :  E-Commerce Database.
  * Environment: [ MySQL Workbench ].
  * The database structure includes four tables.
    1. Customers (Customer_Id, Full_Name, Contact, Email, Address)
    2. Products (Product_Id, Product_Name, Category, Price, Stock_quantity)
    3. Orders (Order_Id, Customer_Id, Product_Id, Order_Date, Amount, Payment_Type)
    4. Payments (Payment_Id, Order_Id, Payment_Status, Payment_Date)
---
<br>

# Subqueries 
  - A subquery, also known as a nested query, is a query embedded within 
another SQL query. 
  - It is typically enclosed in parentheses and is executed first to provide 
results for the outer query. 
  - Subqueries are used to perform intermediate calculations or filtering in a 
step-by-step manner. <br>
  𝙱𝚊𝚜𝚒𝚌 𝚜𝚢𝚗𝚝𝚊𝚡 <br>
  
  ```sql
  SELECT column1, column2, ...
  FROM table_name
  WHERE column_name operator (SELECT column_name
                           FROM another_table
                           WHERE condition);

  ```
---

  * 𝒯𝒴𝒫𝐸 𝒪𝐹 𝒮𝒰𝐵𝒬𝒰𝐸𝑅𝐼𝐸𝒮.
    - 𝐒𝐢𝐧𝐠𝐥𝐞 𝐑𝐨𝐰 𝐒𝐮𝐛𝐪𝐮𝐞𝐫𝐢𝐞 
      1. Returns one row of data.
      2. Used with operators like =, <, > etc
         
      -- Ex. Find Maximum Price of Product.
      ```sql
      SELECT Product_Name, price 
      FROM 
      Products 
    	WHERE price = (SELECT Max(Price) FROM Products ) ;
      ```
      ---
    - 𝐌𝐮𝐥𝐭𝐢-𝐑𝐨𝐰 𝐒𝐮𝐛𝐪𝐮𝐞𝐫𝐲 
      1. Returns multiple rows.
      2. Used with operators like IN, ANY, or ALL.

      -- Ex. Find Orders Price less than 700
      ```sql
      SELECT Order_Id , Product_Id , Amount 
      FROM 
      Orders 
      WHERE Amount IN ( SELECT Amount FROM Orders WHERE Amount<=700) ;
      ```
      ---
    - 𝐒𝐜𝐚𝐥𝐚𝐫 𝐒𝐮𝐛𝐪𝐮𝐞𝐫𝐲
      1. Returns a single value, often used in SELECT, WHERE, or HAVING clauses.
      
      -- Ex. Retrieve the Product_Name, Price, and Stock_quantity for products that have a stock quantity greater than the average stock quantity of all products.
      ```sql
      SELECT Product_Name, Price, Stock_quantity FROM Products WHERE
      Stock_quantity > ( SELECT AVG(Stock_quantity) FROM Products )
      ORDER BY Stock_quantity DESC;
      ```
      ---

    - 𝐂𝐨𝐫𝐫𝐞𝐥𝐚𝐭𝐞𝐝 𝐒𝐮𝐛𝐪𝐮𝐞𝐫𝐲
      1. References a column from the outer query and executes once for each row in the outer query.
      -- Ex. Identify products whose Stock_quantity is lower than the minimum stock quantity of any other product within the same Category that is also priced above ₹500.
      ```sql
      SELECT p1.Product_Name, p1.Category, p1.Stock_quantity FROM Products p1
  	  WHERE
      p1.Stock_quantity < ( SELECT MIN(p2.Stock_quantity) FROM Products p2 
      WHERE
      p2.Category = p1.Category AND p2.Product_Id != p1.Product_Id  AND p2.Price > 500 )
  	  ORDER BY 
      p1.Category, p1.Stock_quantity;
      ```
      ---
      
    - 𝐒𝐮𝐛𝐪𝐮𝐞𝐫𝐲 𝐢𝐧 `𝐄𝐗𝐈𝐒𝐓𝐒`
      1. EXISTS is a boolean expression that evaluates. `TRUE` `FALSE`.
      2. TRUE if the subquery returns at least one row.
      3. FALSE if the subquery returns no rows.
      4. It is commonly used in the WHERE or HAVING clauses of a main query to filter results based on the existence of related data in another table.

      -- List the Product_Name and Price for products that have been included in at least one order where the order amount was over ₹50,000.
      ```sql
      SELECT p.Product_Name, p.Price FROM Products p WHERE
      EXISTS ( 
      SELECT 1 FROM Orders o
                WHERE
              o.Product_Id = p.Product_Id  AND o.Amount > 50000 )
  		ORDER BY p.Price DESC;
      ```
      ---
       <br>
# 🧠 Key Learnings.
  $ 𝚄𝚗𝚍𝚎𝚛𝚜𝚝𝚊𝚗𝚍𝚒𝚗𝚐 𝚀𝚞𝚎𝚛𝚢 𝙴𝚏𝚏𝚒𝚌𝚒𝚎𝚗𝚌𝚢. <br>
  * ꜱᴜʙqᴜᴇʀɪᴇꜱ ᴠꜱ. ᴊᴏɪɴꜱ : While many problems solved by multi-row subqueries (IN) can also be solved with LEFT JOIN + IS NULL, I learned to recognize which method is clearer and potentially faster depending on the database and the complexity of the filtering logic. <br>
  * ᴅᴇʀɪᴠᴇᴅ ᴛᴀʙʟᴇꜱ :  Using a derived table allows for efficient pre-aggregation (like calculating total customer spending) which simplifies the subsequent outer query's logic and often leads to better execution plans.
  <br>
  $  𝙿𝚛𝚘𝚋𝚕𝚎𝚖-𝚂𝚘𝚕𝚟𝚒𝚗𝚐 𝚂𝚔𝚒𝚕𝚕𝚜. <br>
     Successfully implementing logic like "Find customers where none of their orders are 'Unpaid'" requires combining EXISTS and NOT EXISTS checks, which is a crucial advanced SQL technique. <br>
     I practiced comparing data points against an aggregated group (e.g., comparing an order's amount against the average amount for that specific day or month).
---
    




  

# Advanced-Coding-Functions-
Learn how to write functions in MySQL from Scratch
# Basic syntax to write a function
![image](https://github.com/user-attachments/assets/1103d8fd-7462-4c29-b6e1-b3e1f0ef55bd)

# Let`s start with a simple function: Add 2 numbers (integers)
```diff
DELIMITER $$

CREATE FUNCTION add_numbers(x INT, y INT)
RETURNS INT
DETERMINISTIC
BEGIN
    RETURN x + y;
END$$

DELIMITER ;

```
# Step 2: Use the Function
```diff
SELECT add_numbers(10, 5) AS result;
```
# Why do we need to use DELIMITER $$
```diff
- MySQL now treats $$ as the statement terminator instead of ;.
```
- By default, MySQL uses a semicolon (;) to indicate the end of a statement.
- When creating functions, procedures, or triggers, the code inside them might also use semicolons.
- If MySQL doesn't know when the function ends and when the internal statements end, it gets confused.

# You need to specify one of the following options in your function declaration:

DETERMINISTIC
Indicates that the function always produces the same result for the same input (no randomness or reliance on external factors like database tables).

NOT DETERMINISTIC
Indicates that the function may produce different results for the same input (e.g., it depends on external data like the current date or database contents).

NO SQL
Indicates that the function does not read or write any SQL data.

READS SQL DATA
Indicates that the function reads data from the database but does not modify it.

MODIFIES SQL DATA
Indicates that the function modifies the database.

# Example 2: Converting Temperature
Now let’s create a function to convert temperatures from Celsius to Fahrenheit using the formula:
Where C is decimal(5,2)

F = (C × 9/5) + 32.

```diff
DELIMITER $$
CREATE FUNCTION convert_temp(Cel decimal(5,2))
RETURNS decimal(5,2)
DETERMINISTIC
BEGIN
    RETURN (Cel * 9/5) + 32;
END$$

DELIMITER ;
```

```diff
select convert_temp(2.3);
```
# Example 3: Calculate the square of a number

```diff
DELIMITER $$
CREATE FUNCTION calculate_square(num INT) RETURNS INT
DETERMINISTIC
BEGIN
    RETURN num * num;
END$$
DELIMITER ;
```

```diff
select calculate_square(3)
```
# Example 4: Calculate the cube of a number

```diff
DELIMITER $$
CREATE FUNCTION calculate_cube(num INT) RETURNS INT
DETERMINISTIC
BEGIN
    RETURN num * num * num;
END$$
DELIMITER ;

SELECT calculate_cube(3);
```
# Example 5: Calculate the perimeter of a rectangle
Hint: The perimeter of a rectangle is 2 * (length + width).
```diff
DELIMITER $$
CREATE FUNCTION rectangle_perimeter(length DECIMAL(5, 2), width DECIMAL(5, 2)) RETURNS DECIMAL(5, 2)
DETERMINISTIC
BEGIN
    RETURN 2 * (length + width);
END$$
DELIMITER ;

SELECT rectangle_perimeter(5, 3);
```
# Example 6: Determine if a number is even or odd
Hint:

Input: An integer (num INT).
Output: A string (VARCHAR(5)) indicating "Even" or "Odd".
A number is even if it is divisible by 2 (i.e., num % 2 = 0); otherwise, it is odd.
Use a conditional statement (IF) to check the result of the modulo operation.
For example, if the input is 4, the result is "Even", and if the input is 5, the result is "Odd
```diff
DELIMITER $$
CREATE FUNCTION is_even(num INT) RETURNS VARCHAR(5)
DETERMINISTIC
BEGIN
    RETURN IF(num % 2 = 0, 'Even', 'Odd');
END$$
DELIMITER ;

SELECT is_even(4);
```
-
# @ in MySQL
- In MySQL, the @ symbol defines or references user-defined session variables. These variables exist for the duration of the session and can be used to store values temporarily.
```diff
SET @name = 'Alice';
SELECT @name; -- Output: 'Alice'
```
```diff
SET @x = 10, @y = 20;
SELECT @x + @y; -- Output: 30
```
# CAUTION ( If you know loops feel free to scroll down and attempt the remaining questions)

```diff
 - If it cannot be used alone it has to be used inside a procedure, function or a trigger
 - If you want to check the condition without creating a stored procedure, use CASE in a SELECT statement:
```
```diff
SET @number = 5;

SELECT 
    CASE 
        WHEN @number > 0 THEN 'The number is positive.'
        ELSE 'The number is not positive.'
    END AS result;
```
- IF you don`t use as result you get an unclean o/p
- ![image](https://github.com/user-attachments/assets/de6ddcc8-a130-4111-bc4f-af4f22ece681)

- CASE is used in SQL queries as an expression for conditional logic and doesn't require IF, THEN, or ELSEIF. Instead, the logic is directly written using WHEN, THEN, and ELSE

# Example
```diff
SET @day = 'Saturday';

SELECT 
    CASE @day
        WHEN 'Saturday' THEN 'Weekend'
        WHEN 'Friday' THEN 'Weekend'
        ELSE 'Weekday'
    END AS day_category;
```

# How can you categorize a person's age into groups such as "Child," "Teenager," or "Adult"?
- If the age is less than 13, return 'Child'.
- If the age is between 13 and 19, return 'Teenager'.
- If the age is greater than 19, return 'Adult'.
- Use a CASE Statement:

- Use WHEN to check conditions for different ranges.
- Use THEN to specify the result for each condition.
- Don't forget an ELSE clause for the default result when no conditions match.
```diff
  SET @age = 17;

SELECT 
    CASE 
        WHEN @age < 13 THEN 'Child'
        WHEN @age BETWEEN 13 AND 19 THEN 'Teenager'
        ELSE 'Adult'
    END AS age_group;
```
# How can you categorize product prices into "Budget," "Mid-Range," and "Premium"?
- Define the Price Ranges:
```diff
"Budget" products have prices less than $50.
"Mid-Range" products are priced between $50 and $150 (inclusive).
"Premium" products are priced higher than $150.
Use a CASE Statement:

Use WHEN for each price range and assign the appropriate category.
Use ELSE for any price that doesn’t fit into the above ranges.
Test with an Example:

For @price = 30, the result should be 'Budget'.
For @price = 100, the result should be 'Mid-Range'.
For @price = 200, the result should be 'Premium'.
```

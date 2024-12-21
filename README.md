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

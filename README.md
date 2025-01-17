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

- Input: An integer (num INT).
- Output: A string (VARCHAR(5)) indicating "Even" or "Odd".
- A number is even if it is divisible by 2 (i.e., num % 2 = 0); otherwise, it is odd.
- Use a conditional statement (IF) to check the result of the modulo operation.
- For example, if the input is 4, the result is "Even", and if the input is 5, the result is "Odd
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
    CASE
    WHEN @day = 'Saturday' THEN 'Weekend'
    WHEN @day = 'Friday' THEN 'Weekend'
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
```diff
SET @price = 100;

SELECT 
    CASE 
        WHEN @price < 50 THEN 'Budget'
        WHEN @price BETWEEN 50 AND 150 THEN 'Mid-Range'
        ELSE 'Premium'
    END AS price_category;
```
# Determining Employee Experience Levels
```diff
Think About Experience Levels:

"Junior" employees have less than 3 years of experience.
"Mid-Level" employees have between 3 and 7 years (inclusive).
"Senior" employees have more than 7 years of experience.
Use a CASE Statement:

Each range corresponds to a specific level.
The ELSE clause captures any experience greater than 7 years.
Test with an Example:

For @experience = 2, the result should be 'Junior'.
For @experience = 5, the result should be 'Mid-Level'.
For @experience = 10, the result should be 'Senior'
```
```diff
SET @experience = 5;

SELECT 
    CASE 
        WHEN @experience < 3 THEN 'Junior'
        WHEN @experience BETWEEN 3 AND 7 THEN 'Mid-Level'
        ELSE 'Senior'
    END AS experience_level;
```

# Some keywords you must remember
```diff
1.How can you calculate 2^3
Hint: Use the POW() or POWER() function to raise a number to a power.
SELECT POW(2, 3) AS result;  -- Returns 8


2. Square Root of a Number
Question: How can you calculate the square root of 16?
Hint: Use the SQRT() function to get the square root of a number.
SELECT SQRT(16) AS result;  -- Returns 4

3. Absolute Value
Question: How can you get the absolute value of -15?
Hint: Use the ABS() function to return the absolute value.
SELECT ABS(-15) AS result;  -- Returns 15

4. Rounding Numbers
Question: How can you round 3.6 to the nearest integer?
Hint: Use the ROUND() function to round a number.
SELECT ROUND(3.6) AS result;  -- Returns 4

5. Rounding Down
Question: How can you round 4.9 down to the nearest integer?
Hint: Use the FLOOR() function to round down.
SELECT FLOOR(4.9) AS result;  -- Returns 4

6. Rounding Up
Question: How can you round 2.3 up to the nearest integer?
Hint: Use the CEIL() or CEILING() function to round up.
SELECT CEIL(2.3) AS result;  -- Returns 3

7. Modulus (Remainder)
Question: How can you find the remainder when dividing 10 by 3?
Hint: Use the MOD() function to get the remainder.
SELECT MOD(10, 3) AS result;  -- Returns 1

8. Natural Logarithm
Question: How can you calculate the natural logarithm of 10?
Hint: Use the LOG() function for the natural logarithm.
SELECT LOG(10) AS result;  -- Returns 2.302


9. Exponential of a Number
Question: How can you calculate 
e^2
Hint: Use the EXP() function to calculate 
 SELECT EXP(2) AS result;  -- Returns 7.389


10. String Length
Question: How can you find the length of the string "Hello"?
Hint: Use the LENGTH() function to calculate the length of a string.
SELECT LENGTH('Hello') AS result;  -- Returns 5




11. Find the Maximum Value
Question: How can you find the maximum value between 5 and 8?
Hint: Use the GREATEST() function to return the larger of the two numbers.
SELECT GREATEST(5, 8) AS result;  -- Returns 8

12. Find the Minimum Value
Question: How can you find the minimum value between 3 and 10?
Hint: Use the LEAST() function to return the smaller of the two numbers.
SELECT LEAST(3, 10) AS result;  -- Returns 3

13. Convert String to Uppercase
Question: How can you convert the string "hello" to uppercase?
Hint: Use the UPPER() function to convert a string to uppercase.
SELECT UPPER('hello') AS result;  -- Returns 'HELLO'

14. Convert String to Lowercase
Question: How can you convert the string "WORLD" to lowercase?
Hint: Use the LOWER() function to convert a string to lowercase.
SELECT LOWER('WORLD') AS result;  -- Returns 'world'

15. Get the Current Date
Question: How can you get the current date?
Hint: Use the CURDATE() function to retrieve the current date.
SELECT CURDATE() AS result;  -- Returns current date (e.g., '2025-01-13')
return this and remove the 2 words   and
```

# Calculate the Area of a Circle
- Input: The radius of the circle (radius DECIMAL).
- Output: The area of the circle (DECIMAL).
- Use the formula: Area = 𝜋 * r^2
```diff
DELIMITER $$
CREATE FUNCTION circle_area(radius DECIMAL(5, 2)) RETURNS DECIMAL(5, 2)
DETERMINISTIC
BEGIN
    RETURN PI() *POW (radius,2);
END$$
DELIMITER ;

SELECT circle_area(3);  -- Returns 28.27
```
# Check if a Number is Divisible by 3
- Input: A number (num INT).
- Output: A string ("Yes" or "No") indicating whether the number is divisible by 3.
- A number is divisible by 3 if the result of num % 3 = 0.
```diff
DELIMITER $$
CREATE FUNCTION divisible_by_3(num INT) RETURNS VARCHAR(3)
DETERMINISTIC
BEGIN
    RETURN IF(num % 3 = 0, 'Yes', 'No');
END$$
DELIMITER ;

SELECT divisible_by_3(9);  -- Returns 'Yes'
```

# Find the Maximum of Two Numbers
- Input: Two numbers (num1 INT, num2 INT).
- Output: The maximum of the two numbers.
- Use the GREATEST() function to find the maximum value
```diff
DELIMITER $$
CREATE FUNCTION max_of_two(num1 INT, num2 INT) RETURNS INT
DETERMINISTIC
BEGIN
    RETURN GREATEST(num1, num2);
END$$
DELIMITER ;

SELECT max_of_two(8, 5);  -- Returns 8
```
#  Check Leap Year
- Input: A year (year INT).
- Output: A string ("Leap Year" or "Not Leap Year").
- A year is a leap year if it is divisible by 4, but not divisible by 100, unless it is divisible by 400.

```diff
DELIMITER $$
CREATE FUNCTION leap_year(year INT) RETURNS VARCHAR(15)
DETERMINISTIC
BEGIN
    IF (year % 4 = 0 AND year % 100 != 0) OR (year % 400 = 0) THEN
        RETURN 'Leap Year';
    ELSE
        RETURN 'Not Leap Year';
    END IF;
END$$
DELIMITER ;

SELECT leap_year(2020);  -- Returns 'Leap Year'
```diff
DELIMITER $$
CREATE FUNCTION leap_year(year INT) RETURNS VARCHAR(15)
DETERMINISTIC
BEGIN
    IF (year % 4 = 0 AND year % 100 != 0) OR (year % 400 = 0) THEN
        RETURN 'Leap Year';
    ELSE
        RETURN 'Not Leap Year';
    END IF;
END$$
DELIMITER ;

SELECT leap_year(2020);  -- Returns 'Leap Year'
```
- MySQL server's settings to allow the creation of non-deterministic functions
  ```diff
  SET GLOBAL log_bin_trust_function_creators = 1;
  ```
```diff
  DELIMITER $$
CREATE FUNCTION current_datetime() RETURNS DATETIME
READS SQL DATA
BEGIN
    RETURN NOW();  -- Returns the current date and time
END$$
DELIMITER ;

SELECT current_datetime();  -- Returns the current date and time
```
 

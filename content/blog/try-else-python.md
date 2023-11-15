---
title: "Python3: Try-Else Statement"
description: "Python3 Flow Control using Try-Else Statements."
dateString: April 2023
draft: false
tags: ["Python", "Developer"]
weight: 107
cover:
    image: "/blog/try-else-python/cover.png"
---
### Introduction

Errors are common in programming, regardless of the level of experience and competence. These errors are often referred to as exceptions and occur due to many reasons such as unexpected user inputs, network failures, system crashes, etc. In Python, exceptions are handled using the try-except statement. However, there is another useful variation of the try statement called the try-else statement, which we will be discussing in this article.

### What is Try-Else Statement?

The try-else statement in Python is used to execute a block of code if no exception is thrown in the try block. It is an optional clause that can be added to the end of a try-except statement. The syntax of the try-else statement is as follows:

```
try:
    # a block of code that may cause an exception
except ExceptionType:
    # a block of code to handle the exception
else:
    # a block of code that executes if theres no exception raised
```

In the try block, we write the code that may raise an exception. If an exception is raised, the code in the except block is executed to handle the exception. However, if there is no exception raised in the try block, the code in the else block is executed.

Let's consider an example to understand the try-else statement better. Assume we want to take input from the user and calculate its square. We can write the code using the try-else statement as follows:

```
try:
    x = int(input("Enter a digit: "))
except ValueError:
    print("Invalid input. Please enter another digit.")
else:
    print("The square of the digit is: ", x*x)
```

In this code, we use the input() function to take input from the user. However, since the input can be any string, we use the int() function to convert the input into an integer. If the user enters a non-numeric value, a ValueError exception is raised, and the code in the except block is executed, which prints an error message. If the input is a valid integer, the code in the else block is executed, which calculates and prints the square of the number.

### Benefits of try-else 
Now, let's take a look at some benefits of using the try-else statement in Python.

<ol class="tabs">
  <li class="active" rel="tab1">Improved code readability</li>
  <li class="active" rel="tab2">Better error handling</li>
  <li class="active" rel="tab3">Improved code readability</li>
</ol>

<div class="tab_container">
   <h3 class="d_active tab_drawer_heading" rel="tab1">1. Improved code readability</h3>

The try-else statement can help make your code more readable and easier to understand. By separating the code that may raise an exception from the code that executes when there is no exception, you can make it clear what your code is doing and how it handles errors.

   <div id="tab1" class="tab_content">
   <pre><code class="language-python">

         try:
            f = open('file.txt', 'r')
            contents = f.read()
            print(contects)
         except IOError:
            print("File not found or could not be read.")

   </code></pre>
   </div>

This code opens a file, reads its contents, and prints them. However, if the file does not exist or could not be read, the code in the except block is executed, which prints an error message.

Now, let's rewrite the code using the try-else statement:
   <div id="tab1" class="tab_content">
   <pre><code class="language-python">

         try:
            f = open('file.txt', 'r')
         except IOError:
            print("File not found or could not be read.")
         else:
            contents = f.read()
            print(contects)

   </code></pre>
   </div>

In this code, we use the try-else statement to separate the code that may raise an exception from the code that executes when there is no exception. The code in the try block attempts to open the file, and if there is an IOError, the code in the except block is executed. If the file is successfully opened, the code in the else block is executed, which reads the file's contents and prints them. This code is more readable and easier to understand than the previous code.


<div class="tab_container">
   <h3 class="d_active tab_drawer_heading" rel="tab2">2. Better error handling</h3>

The try-else statement can also help you handle errors more effectively. By separating the code that may raise an exception from the code that executes when there is no exception, you can handle errors more precisely and avoid catching and handling exceptions that are not relevant to the current code.

For example, consider the code below:
   <div id="tab1" class="tab_content">
   <pre><code class="language-python">

         try:
            result = calculate_result()
         except Exception:
            print("An error occurred. Please try again later.")

   </code></pre>
   </div>

In this code, we are catching all exceptions that may occur while calling the `calculate_result()` function. This may include exceptions that are not related to the function's logic, such as network errors or system crashes. By catching all exceptions, we are not handling errors precisely and may hide important error messages.

Now, let's rewrite the code using the try-else statement:

   <div id="tab1" class="tab_content">
   <pre><code class="language-python">

        try:
           result = calculate_result()
        except ValueError:
           print("Invalid input. Please enter a valid number.")
        except ZeroDivisionError:
           print("Cannot divide by zero.")
        else:
           print("The result is:", result)
   </code></pre>
   </div>

In this code, we use the try-else statement to handle specific exceptions that may occur while calling the calculate_result() function. If the function raises a ValueError, we print an error message indicating that the input was invalid. If the function raises a ZeroDivisionError, we print an error message indicating that the input was zero, and division by zero is not allowed. If there is no exception raised, we print the result. This code handles errors more precisely and provides better feedback to the user.

<div class="tab_container">
   <h3 class="d_active tab_drawer_heading" rel="tab3">3. Cleaner code</h3>

The try-else statement can help you write cleaner code by eliminating unnecessary code blocks and reducing the nesting level of your code. By separating the code that may raise an exception from the code that executes when there is no exception, you can write cleaner and more concise code.

For example, consider the following code:

   <div id="tab1" class="tab_content">
   <pre><code class="language-python">

        try:
           x = int(input("Enter a number: "))
        except ValueError:
           print("Invalid input. Please enter a valid number.")
        else:
           try:
              result = 100/x
           except ZeroDivisionError:
              print("Cannot divide by zero.")
           else:
              print("The result is:", result)

   </code></pre>
   </div>

In this code, we use two nested try-except statements to handle different types of exceptions. The outer try-except statement handles the `ValueError` exception that may occur while converting the input to an integer. The inner try-except statement handles the ZeroDivisionError exception that may occur while dividing 100 by the input. This code is more complex and harder to read than the previous code.

Now, let's rewrite the code using the try-else statement:

   <div id="tab3" class="tab_content">
   <pre><code class="language-python">

        try:
           x = int(input("Enter a number: "))
        except ValueError:
           print("Invalid input. Please enter a valid number.")
        else:
           if x == 0:
              print("Cannot divide by zero.")
           else:
              result = 100/x
              print("The result is:", result)

   </code></pre>
   </div>

In this code, we use the try-else statement to handle the input conversion and the division in a single block of code. If the input is zero, we print an error message. If the input is valid, we calculate the result and print it. This code is cleaner and easier to read than the previous code.

### Conclusion

In conclusion, the try-else statement in Python is a useful variation of the try-except statement that can help you write cleaner, more readable, and more effective code. By separating the code that may raise an exception from the code that executes when there is no exception, you can handle errors more precisely, provide better feedback to the user, and write cleaner and more concise.

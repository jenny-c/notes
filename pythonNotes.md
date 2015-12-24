#PYTHON
-> Case sensitive


##Comments: start with #
-> add for explanations


##Common Errors:
-> Roundoff Errors:
	-> display a fixed number of digits or it won't display an exact number
-> Unbalanced parantheses
	-> make the brackets match


##ALGORITHIMS: Plans that describe the steps for a problem (like a formula)
-> Should be unambiguous
-> Should be executable
-> Should be terminating


--
*Pseudocode:*
A language that is half coding and half natural language--
--


##Variables: (NOTE: use ALL CAPS for constants)
-> choose more descriptive names
-> Booleans
  -> either true or false


##ARITHMETIC:
-> Mixing numeric types:
	-> mixing integer and float in an equation will give a float
-> Floor division:
	-> uses the "//" operator -> takes the quotient and discards the fractional part, no rounding, just the whole number (ex. 7//4 = 1)
-> Modulo:
	-> uses the "%" operator -> computes the remainder of a division equation (ex. calculating change)


##FUNCTIONS: sequence of instructions with a name
-> Built-in functions:
	-> abs(x) === absolute value of x
	-> round(x) or (x, n) === rounded to a whole number or n number of decimal places
	-> max(x1, x2 ... xn) === the largest value in the args
	-> min(x1, x2 ... xn) === the smallest value
-> can import modules for different functions (ex. from the math module import the sqrt() function
	-> ex. sin(), pi = constant, log(x, base)
-> should comment the function's behaviour for others to understand
-> *NOTE: don't modify parameter variables*
-> *NOTE: only have one return statement per each branch*
	-> make sure the return takes all cases
-> use to eliminate repetitive code
-> variables declared inside a function are LOCAL and cannot be used outside the function
	-> the same variable names can be used in different functions
	-> global variables can be used everywhere and are defined outside the functions



##PROBLEM SOLVING:
-> first solve by hand
-> test example values
-> keep applying the solution
-> devise an algorithim from your solution


##STRINGS: Sequences of characters (unicode characters)
-> the characters have numeric values
-> each character in a string has an index number (like an array)
	-> first on is 0
	-> spaces count
-> String Escape Sequences:
	-> type \ before things like another \ or " or ' so that it doesn't close the quote
	-> \n will create a newline
-> get user input with input() (ex. name = input("Enter name"))
	-> change to int === age = int(input("Enter age"))
-> formatting strings
	-> print("%-10s" %("Total:")) === left justifys the string Total: in a width of 10
	-> print("10.2f" %(price)) === right justifys the number price with two decimal places
	-> print("%-10s%10.sf" %("Total: ", price)) === prints both; one left justified, one right justified with two decimal places


##METHODS: Govern the behaviour of objects
-> unlike functions, can only be applied to the object where it was defined
-> Useful string methods:
	-> s.lower() === lowercase version of string s
	-> s.upper() === uppercase version
	-> s.replace(old, new) === a new version where the substring old is replaced by the string new


##GRAPHICS: import GraphicsWindow
-> win = GraphicsWindow(640, 480) === to create a window thats 640 * 480 pixels
-> canvas = win.canvas === access the canvas in the graphics window
-> canvas.drawRect(15, 10, 20, 30) === draws a rectangle
	-> *NOTE: win.wait() === makes the program wait for the user to close the window, otherwise the window would close and leave no time to see*
-> canvas.drawRect(15, 10, 20, 30) === top left corner is at (15, 10) and has a height of 20 and a width of 30
	-> works for common shapes such as rects, squares, circles, ovals
-> c.drawLine(x1, y1, x2, y2) === (x1, y1) and (x2, y2) are the endpoints
-> Setting Colours and Outlines:
	-> c.setOutline("black")
	-> c.setFill(0, 255, 0)
	-> c.setColour() === sets the outline and the fill to the same


##IF STATEMENTS: Executes code is a certain condition is met
-> Shortcut: in one line
	-> actualFloor = floor - 1 if floor > 13 else floor
  -> provide an epsilon so you can see if the floats are close enough
-> else if is elif
-> make sure to cover every possible case
  -> make error cases for illegal inputs
-> Combined Conditions:
  -> and === execute code only if both conditions are met
  -> or === execute code if at least one condition is met
-> not === inverts a boolean variable or comparison
  -> != === used to check for inequality rather than negation


##WHILE AND FOR LOOPS:
-> the while loop executes instructions repeatedly while a condition is true:
	```
	i = 1
  while i < 10:
    print(i))
	```
-> *NOTE: WATCH FOR INFINITE LOOPS*
-> can use boolean values to control the loop
-> the for loop is really similar
  -> used to iterate over the contents of a container (count controlled)
		```
		for i in range of (1, 10)
      print(i))
		```

-> For Loop Examples:
  -> for i in range(6) === 0, 1, 2, 3, 4, 5
  -> for i in range(10, 15) === 10, 11, 12, 13, 14
  -> for i in range (0, 9, 2) === 0, 2, 4, 6, 8

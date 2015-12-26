# PYTHON
* Case sensitive language


## Comments: start with "#"
* add for explanations


## Common Errors:
* Roundoff Errors:
	* display a fixed number of digits or it won't display an exact number
* Unbalanced parantheses
	* make the brackets match


## ALGORITHIMS: Plans that describe the steps for a problem (like a formula)
* Should be unambiguous
* Should be executable
* Should be terminating


--<br>
## Pseudocode:
* A language that is half coding and half natural language
--


## Variables: (NOTE: use ALL CAPS for constants)
* choose more descriptive names
* Booleans
  * either true or false


## ARITHMETIC:
* Mixing numeric types:
	* mixing integer and float in an equation will give a float
* Floor division:
	* uses the "//" operator -> takes the quotient and discards the fractional part, no rounding, just the whole number (ex. 7//4 = 1)
* Modulo:
	* uses the "%" operator -> computes the remainder of a division equation (ex. calculating change)


## FUNCTIONS: sequence of instructions with a name
* Built-in functions:
	* abs(x) === absolute value of x
	* round(x) or (x, n) === rounded to a whole number or n number of decimal places
	* max(x1, x2 ... xn) === the largest value in the args
	* min(x1, x2 ... xn) === the smallest value
* can import modules for different functions (ex. from the math module import the sqrt() function
	* ex. sin(), pi = constant, log(x, base)
* should comment the function's behaviour for others to understand
* *NOTE: don't modify parameter variables*
* *NOTE: only have one return statement per each branch*
	* make sure the return takes all cases
* use to eliminate repetitive code
* variables declared inside a function are LOCAL and cannot be used outside the function
	* the same variable names can be used in different functions
	* global variables can be used everywhere and are defined outside the functions



## PROBLEM SOLVING:
* first solve by hand
* test example values
* keep applying the solution
* devise an algorithim from your solution


## STRINGS: Sequences of characters (unicode characters)
* the characters have numeric values
* each character in a string has an index number (like an array)
	* first on is 0
	* spaces count
* String Escape Sequences:
	* type \ before things like another \ or " or ' so that it doesn't close the quote
	* \n will create a newline
* get user input with input() (ex. name = input("Enter name"))
	* change to int === age = int(input("Enter age"))
* formatting strings
	* print("%-10s" %("Total:")) === left justifys the string Total: in a width of 10
	* print("10.2f" %(price)) === right justifys the number price with two decimal places
	* print("%-10s%10.sf" %("Total: ", price)) === prints both; one left justified, one right justified with two decimal places


## METHODS: Govern the behaviour of objects
* unlike functions, can only be applied to the object where it was defined
* Useful string methods:
	* s.lower() === lowercase version of string s
	* s.upper() === uppercase version
	* s.replace(old, new) === a new version where the substring old is replaced by the string new


## GRAPHICS: import GraphicsWindow
* win = GraphicsWindow(640, 480) === to create a window thats 640 * 480 pixels
* canvas = win.canvas === access the canvas in the graphics window
* canvas.drawRect(15, 10, 20, 30) === draws a rectangle
	* *NOTE: win.wait() === makes the program wait for the user to close the window, otherwise the window would close and leave no time to see*
* canvas.drawRect(15, 10, 20, 30) === top left corner is at (15, 10) and has a height of 20 and a width of 30
	* works for common shapes such as rects, squares, circles, ovals
* c.drawLine(x1, y1, x2, y2) === (x1, y1) and (x2, y2) are the endpoints
* Setting Colours and Outlines:
	* c.setOutline("black")
	* c.setFill(0, 255, 0)
	* c.setColour() === sets the outline and the fill to the same


## IF STATEMENTS: Executes code is a certain condition is met
* Shortcut: in one line
	* actualFloor = floor - 1 if floor > 13 else floor
  * provide an epsilon so you can see if the floats are close enough
* else if is elif
* make sure to cover every possible case
  * make error cases for illegal inputs
* Combined Conditions:
  * and === execute code only if both conditions are met
  * or === execute code if at least one condition is met
* not === inverts a boolean variable or comparison
  * != === used to check for inequality rather than negation


## WHILE AND FOR LOOPS:
* the while loop executes instructions repeatedly while a condition is true:
	```python
	i = 1
  while i < 10:
    print(i))
	```
* *NOTE: WATCH FOR INFINITE LOOPS*
* can use boolean values to control the loop
* the for loop is really similar
  * used to iterate over the contents of a container (count controlled)
		```python
		for i in range of (1, 10)
      print(i))
		```
* For Loop Examples:
  * for i in range(6) === 0, 1, 2, 3, 4, 5
  * for i in range(10, 15) === 10, 11, 12, 13, 14
  * for i in range (0, 9, 2) === 0, 2, 4, 6, 8


## LISTS: arrays
* specify a list variable with []
	* values[1] = 0 === use [] to access an element
* differences between strings and lists:
	* lists can hold any type of value
	* lists are mutable -> you can change the values
* use the len() function to determine the number of elements
* Lists Aliases:
	* when you copy a list, both of the variables refer to the same list
	```python
	scores = [10, 9, 7, 4, 5]
	values = scores #refers to the same list
	```
	* you can modify the list through either variable
	* *NOTE: to make a copy, use values = list([scores])*
* negative subscripts provide access to the elements in reverse order
	* ex. values[-1] give you the last element
* List Operations:
	* l.append("element") === adds an element to the end
	* l.insert(1, "element") === inserts an element at the specific position
	* if "element" in list -> print "element"
	* l.index("element") === returns the position of the element
	* l.pop(1) === removes the element at a given position
	* two lists can be concatenated through the +
	* sum() === gives the sum of the values
	* max() and min() === gives the largest and smallest values
	* sort() sorts a list of numbers or strings
	* To Slice Lists:
	```python
	scores = [10, 9, 7, 4, 5]
	values = scores[1 : 4] # first index to include and first index to ignore
	```
* a tuple is similar to a list, but is immutable
```python
triple = (5, 10, 15)
#or without parentheses
triple = 5, 10, 15
```
* Matrix:
	* a matrix is made when each element in a list is another list
	* use two index values -> element[3][1]
		* the first is the outer row, the second for the inner column


## READING AND WRITING TEXT FILES:
* text files are very commonly used to store information
* reading a file:
```python
infile = open("input.txt", "r")
#the "r" makes it read-only
line = infile.readline()
#reads one line
char = infile.read(2)
#reads two characters
```
* writing to a file:
```python
outfile = open("output.txt", "w")
#the "w" makes it write-only
outfile.write("Hello, World!/n")
```
* *NOTE: make sure to close the file by using the close() method (ex. infile.close())*
* to print each word on a seperate line:
```python
inputFile = open("lyrics.txt", "r")
for line in inputFile :
	line = line.rsplit()
	wordList = line.split()
	for word in wordList :
		word = word.rstrip(".,?!")
		print(word)

inputFile.close()
```


## SETS:
* a set is a container that stores values
	* different from lists -> no order, no position
	* much faster than equivalent list operations because they don't need to maintain an order
* Making Sets:
```python
#enclose within curly braces
cast = {"Luigi", "Gumbys", "Spiny"}
#convert any list into a set
names = ["Luigi", "Gumbys", "Spiny"]
cast = set(names)
#to make an empty set, do not use {}
cast = set()
numberOfCharacters = len(cast) #here would be 0
#to access elements, can't use a set position
print("The cast of characters include:")
for character in cast:
	print(character)
#sets are mutable, so you can use the .add() and .discard() methods
cast.add("Arthur")
cast.discard("Arthur")
cast.clear() #removes everything
```
* a set is a subset of another set if every element occurs in the second set
	* first set.issubset("the second set") -> checks whether the first set is a subset of the second set
* set1.union(set2) === contains elements of both sets with no duplicates
	* set1.intersection(set2) === contains the elements that are in both sets
	* set1.difference(set2) === new set that has the elements that aren't in set2
* *NOTE: use sets when managing a collection of unique items -> over 10 times faster than using lists*



## Dictionaries:
* a container that keeps associations between keys and values
	* keys are unique, but a value can be associated with several keys
	```python
	favColours = {
		"Romeo": "Green",
		"Adam": "Red",
	}
	oldFavColours = dict(favColours)
	#creates a duplicate of favColours
	```python
* not sequence-type, no indexes or position
	* can only access a value by its associated key
* defaults keys:
```python
number = contacts.get("Fred", 411)
print(number)
#if Fred doesn't exist, 411 is default
```
* Editing Dictionaries:
	* contacts["John"] = 4578102 === adds a key "John" and value of 4578102
	* contacts["John"] = 2228102 === changes the value of "John"
	* contacts.pop("John") === removes the entire item, key and value -> returns the value so you can store it
* Iterating over Dictionaries:
```python
for key in contacts:
	print(key, contacts[key])
	#shows the keys
for item in contacts.items():
	print(item[0], item[1])
	#the .items() method returns a sequence of tuples
```

#### MODULAR PROGRAMMING:
* Separates files making it easier to modify a certain aspect
* can import modules  

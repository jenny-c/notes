# Swift
* for iOS, OS, and watch OS

## Comments
* start with //

## Simple values
* constants
  * use the "let" keyword
  * doesn't have to be known to compile
  * must be assigned a value once
* variables
  * use the "var" keyword
* arrays + dictionaries
  * typically use the "var" keyword
  * both use the []
  * for dictionaries, use ":" to separate keys and values
  * can make empty arrays (setting value types or not)

## Type Conversions
* ex. String(value)
* including values in strings
* "I have \(numFruits) fruits

## Control Flow
* if statement
  * use {} and "else if"
* switch statement  
```
switch fruit {
case banana:
  print ("Banana")
case apple:
  print ("Apple")
default:
  print ("Some fruit")
}
```
* for-in loop
```
for (key, value) in myDictionary {
  print (key)
}
```
  * use ..> to make a range that omits the top value
  * use ... to make a range that includes both values
* for loop
```
for var i = 0; i < 4; ++i {
  print (i)
}
```
* while loop
  * while condition = true { do something }
  * condition can be placed at the end, ex.
  ```
  repeat {
    print ("Hello")
  } while number < 5
  ```

## Functions
```
func greet(name: String, day: String) -> String {
  // stuff
  return something
}
func greet(names: [String], day: String) -> (SomeInt: Int, SomeString: String) {
  //stuff
  return the stuff
}
```
* use "->" to separate the arguments from the function's return type
* nested functions have access to variables in the outer functions
* can take another function as an argument
* can return another function as its value
* closures are created with no name and with {} - can be used as arguments to functions

## Objects and Classes
```
class NamedShape {
  var numberOfSides: Int = 0
  var name: String

  init(name: String) {
    self.name = name
  }

  func simpleDescription() -> String {
    return "A shape with \(numberOfSides) sides."
  }
}
```
* use keyword "class" followed by the class name and {}
* variables, constants, functions and methods are written normally
* create an instance of the class (ex. var circle = Shape())
* every property needs an assigned value
  * either like numberOfSides
  * or through the initializer
* subclasses have the superclass after it, separated by a ":"
* overriding methods in subclasses
  * use keyword "override" before
* properties can have getters and setters
```
var perimeter: Double {
  get {
    return 3.0 * sideLength
  }

  set {
    sideLength = newValue / 3.0
  }
}
```
  * can use "willSet" and "didSet" for code that is run before and after setting a new value
* optional values
  * if the value before the ? is nil, everything after that is ignored

## Enumerations
* use the keyword "enum"
* can have methods associated
* often use switch statements to match individual enumeration cases
* cases are not automatically given integer values - they are full values
* raw-value is identified by ": Int/String" after the name
  * only need to specify the first raw-value for Int, everything else is in order

## Structures
* use the keyword "struct"
* support many of the same behaviours as classes
  * difference: structures are copied every time they're passed around, classes are by reference
  

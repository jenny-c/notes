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

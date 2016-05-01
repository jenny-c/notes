# Java
* an object orientated language
* uses brace brackets

## Comments
* single-line start with double slash "//"
* multi-line start with slash asterisk "/\*"

## Variables
* declare type then variable name = value
``` Java
int myNumber = 525;
```

## Operators
* relational operators don't need the same two operands

## Control Flow
* if statement
  * syntax
  ``` Java
  if (condition) {
    // code goes here
  } else if {
    // more code goes here
  } else {
    // even more code
  }
  ```
  * shortened for binary cases (ternary conditional statement)
  ``` Java
  (condition) ? executedIfTrue : executedIfFalse;
  ```
* switch statement
  * syntax

  ``` Java
   /*
   break statement exits the case statement
   otherwise it will continue to check whether it matches anything else
   */
  int myNumber = 5;

  switch (myNumber) {

    case 1: System.out.println("Case 1");
    break;

    case 2: System.out.println("Case 2");
    break;

    default: System.out.println("Default case");
    break;  
  }
  ```

## Loops
* for loops
  * 

# Classes
* keyword class
* each class needs a class constructor
* instance variables
  * details associated with the objects (from the classes)
* methods
  * built-in main method
* void means that it will not return a value
* if we want to return a value, specify the value type (int, char etc.)
* inheritance
  * use the "extends" keyword

``` Java
class Dog extends Animal{

  // an instance variable
  int age;

  // class constructor
  public Dog(int dogsAge) {

    age = dogsAge;
  }

  // a method
  public void bark() {

    System.out.println("Woof");
  }

  // built-in main method
  public static void main(String[] args) {

    // creating an instance of the class
    Dog myDog = new Dog(3);
    myDog.bark();
  }
}
```

## Data structures
*

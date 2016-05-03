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

* for loops
  * syntax

  ``` Java
  for (int counter = 0; counter < 5; counter++) {

    System.out.println("Counter: " + counter);

  }

  // or (for each loop)

  for (int number : arrayOfNumbers) {

    System.out.println("The number is " + number);

  }
  ```

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
* .size() to find size of structure
* arrays
  * keyword "ArrayList"
  ``` Java
  ArrayList<Integer> quizGrades = new ArrayList<Integer>();
  ```
  * use .add(value) to add values to the array
    * to insert use .add(index, value)
  * use .get(index) to access the values
* dictionaries
  * keyword "HashMap"
  * consists of keys and values
  ``` Java
  HashMap<String, Integer> myDict = new HashMap<String, Integer>();
  ```
  * use .put(key, value) to add key value pairs to the dictionary
  * use .get(key) to access the value associate with the key
  * use .keySet() to access the keyset of a dictionary

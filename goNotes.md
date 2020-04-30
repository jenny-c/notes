# Go Basics 
- open source language developed by Google employees
- go was designed to "eliminate slowness and clumsiness of software development at Google, and thereby to make the process more productive and scalable"
  - meant to make it faster and easier for developers to develop

- **compiling** 
  - `go build myfile.go` \rarr& will create an executable file of the same name 
  - `go run myfile.go` \rarr& combines the compiling and the running of the file 
- `go doc` will give you detailed information on packages and syntax and everything (can be more or less specific)

# Structure 
- go programs are read top to bottom, left to right 

## Packages 
- every program starts with a package declaration which tells the compiler whether to make an **executable** or a **library**
  - `package main` makes an executable 
  - `` makes a library (idk yet tho)
- `import "fmt"` 
  - import statements let us import packages (must be enclosed in double quotes (compiler error))
  - import statements can be grouped together 
```go
import (
  p1 "package1"
  "package2"
)
```
  - `p1` is an alias for `package1` and functions from `package1` can not be called like `p1.function()` instead of `package1.function()` 

## Main
```go
func main() {
  fmt.Println("Hello World")
}
```
- file with `package main` line will automatically run the main function 

# Syntax 

## Comments 
```go
// this is a line comment 
/*
this is
a multiline
comment
*/
```

## Values (named: var and const)
**Variables**
```go
func main() {
  var stationName string
  var nextTrainTime int8
  var isUptownTrain bool

  stationName = "Union Square"
  nextTrainTime = 12
  isUptownTrain = false
  
  /*
  could also have done: 
  var stationName string = "Union Square"
  var nextTrainTime int8 = 12
  var isUptownTrain bool = false
  OR
  stationName := "Union Square"
  nextTrainTime := 12
  isUptownTrain := false
  these ways will infer the type from the values provided
  */

  fmt.Println("Current station:", stationName)
  fmt.Println("Next train:", nextTrainTime, "minutes")
  fmt.Println("Is Uptown:", isUptownTrain)
}
```
- since variables have the ability to be updated, they don't need to be initially assigned a value (unlike constants)
```go
// can declare multiple variables on one line 
var part1, part2 string 
part1, part2 = "hello", "world""

quote, fact = := "this is a quote", true
```

**Constants**
- cannot be updated while the program is running 
```go
const funFact = "Hummingbirds' wings can beat up to 200 times a second."
fmt.Println("Did you know?")
fmt.Println(funFact)
```

### Datatypes
**Numbers**
- default value of 0
1. int (11 types)
  - integers
  - if you just use `int` or `uint`, Go will check the architecture of the computer and use either the 32-bit one or the 64-bit one
2. float (2 types)
  - can include fractional data 
3. complex (2 types)
  - complex numbers (imaginary parts)

**Booleans**
- 1 bit (0 = false, 1 = true)
- default value is false 

**Strings**
- for storing and processing text 
- surrounded by double quotes 
- can use `+` to concatenate (does not include spaces)
- default value of ""

## fmt Package 

### Print Method 
- `Println`
  - prints with a newline at the end 
  - includes a space between the arguments (comma separated)
- `Print`
  - prints without new line at the end (will have to include \n yourself) 
- `Printf`
  - allows you to do more formatting (ex. `printf("hello there %v", name)`)
  - the % stuff are called verbs
    - `%v`: string
    - `%T`: type of the variable
    - `%d`: int
    - `%f` or `%.nf`: float (.n number of decimal places included
- `Sprint`
  - returns the string instead of printing it 
- `Sprintln`
  - returns the string instead of printing it (includes spaces in between and the newline)
- `Sprintf`
  - returns the string instead of printing it (interpolating)
- `Scan(&inputVar)`
  - gets user input and puts it in `inputVar` (addresses) \rarr& gets tokens (not the whole line)

## Conditionals

### If Statement 
```go
if conditionIsTrue {
  // do stuff
  // note: condition can be wrapped in parentheses but doesn't have to be 
} else if otherConditionIsTrue {
  // do other stuff
} else {
  // do default stuff
}
```
- can include declaration in the statement itself 
```go
if condition := true; condition {
  // do the thing 
  // note that condition is a scoped variable; can't be accessed outside the if statement 
}
```

### Comparators 
- work as you think they would 
  - works with strings too 

### Switch Statement
```go
switch thing {
  case thing1:
    // do stuff if thing == thing1
  case thing2:
    // do stuff if thing == thing2
  default:
    // do stuff is thing != thing1 and != thing2
    // note we don't have to break
}
```
- can also do the same thing as the if statement with the declaration in the statement itself i

- note: rand package for random int 
```go
rand.Seed(time.Now().UnixNano())
rand.Intn(maxInt)
// need the seed number to be different everytime 
  // the default is always 1 which leads to the same result being returned everytime
// the range is from 0 - maxInt
```

## Functions
```go
func funcName(x paramType) returnType {
  // function body goes in here
}
// you can have multiple parameters and multiple returns
func funcName2(x, y paramType) (returnType1, returnType2) {
  // function body goes in here 
}
```
- variables declared in a function will be scoped to that function 
- use `defer` to delay the call until the function ends or returns 
  - useful to purposes like cleanup

## Pointers and Addresses
- Go is a **pass-by-value** language, so changes that happen in a function will stay in the function, not affect the variable outside 

### Addresses 
- address is the location where the data is stored
- `&myVar`: returns the address of `myVar`

### Pointers
- pointers store addresses
- `var pointerForInt *int`: `*int` signifies that this variable will store an address to an int
  - can also be declared implicitly like other vars
- **dereferencing**
  - use `*` to access the value stored at the address held by the pointer 

# Goroutines 
- allow you to execute tasks in parallel 
  - "Goroutines are multiplexed onto multiple OS threads so if one should block, such as while waiting for I/O, others continue to run."

- similar to normal functions but attach `go` in front of the actual function call 
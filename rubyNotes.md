# Ruby
* an object orientated language

## Comments
* start with octothorps

## Output
* keywords
  * puts will move cursor to the next line
  * print will keep the cursor on the same line
* in output statements, you can use #{variable_name} to input variables
* \ (backslash) is used as an escape character
  * \n new line character
  * \t new tab character
  * \r carriage return
  * \v new vertical tab
* using the triple double quotes, the string can span several lines

## Input
* use "print" to keep it on the same line
* "gets.chomp" for strings
  * use ".to_i" to convert to int
  * use ".to_f" to convert to float
* ARGV (argument variable)
  * used when you want the user to input information before the script runs

## Operators
* math operators
  * +
  * -
  * /
  * *
  * %
  * <
  * >
  * <=
  * >=
* boolean operators
  * && (and)
  * || (or)
  * ! (not)
  * != (not equal)
  * == (equal)
  * >= (greater-than-equal)
  * <= (lesser-than-equal)
  * true
  * false

## Variables
* assign them using the = sign
* ||= for default assigning
* argument variable is assigned when you run it
* arrays
  * are set in square brackets ( [] )
  * .push to push to the end
  * use .split(' ') to split strings
* hashes
  * are set in curly brackets ( { } )
  * assigned values using "=>"
  * anything can be used as a key
  * .delete to delete keys and values associated with them

# Reading and writing files
* txt = open(filename)
  * close() to close the file
* readline() reads one line
* truncate() empties the file
* write('stuff') writes "stuff" to the file

## Functions
* use def and end to signify start and end
* \*args is like ARGV but for functions
* can return a value using "return"
``` Ruby
  def print_two(arg1, arg2)
    puts "arg1: #{arg1}, arg2: #{arg2}"
    end

  print_two("Jenny", "Chen")
```
## Modules
* a collection of methods and constants
* can import to use methods and stuff
* can access variables using ::
* "require"

## Classes
* keyword class
* instantiate new objects using .new()
* use @ to distinguish variables inside the class
* use $ to distinguish variables from outside the class
* inheritance
* mixins
  * can use modules in classes as well
  * "include"

  ``` Ruby
  class mammal
    def breathe
      puts "Inhale and exhale."
    end
  end

  # Use the less than sign to signal inheritance
  class cat < mammal
    def speak
      puts "Meow"
    end
  end

  dave = cat.new()
  dave.speak()
  dave.breathe()
  ```

## Loops
* if statements

``` Ruby
if people < cats
  puts "Too many cats! The world is doomed!"
elsif people > cats
  puts "Too many people! The world is doomed!"
else
  puts "Purrfect!"
end
```

* for loops
  * use (firstNumber .. secondNumber) to show ranges

``` Ruby
fruits = ["apples", "oranges", "pears", "apricots"]

for fruit in fruits
  puts "I love #{fruit}"
end

fruits.each do |fruit|
  puts "I love #{fruit}"
end

fruits.each {|fruit| puts "I love #{fruit}"}
```
* while loops

``` Ruby
i = 0
numbers = []

while i < 6
  puts "i = #{i}"

  i += 1

  puts "i = #{i}"
end
```

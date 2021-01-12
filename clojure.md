# Basics
https://clojure.org/api/cheatsheet
- clojure is functional language 

# Syntax

## Literals

### Numeric Types
- are fixed precision 64-bit when in range, arbitrary otherwise
- trailing N can be used to force precision
- octal, hex, arbitray radix etc
- ratios are their own type

### Character Types
- normal string character stuff
- special named characters; `\newline \spec \tab` etc
- literal regular expressions are strings with leading #

### Symbols and Indents
- symbols are as you expect
  - can have namespace separated with a forward slash from the name
- `nil true false` are boolean values and are special symbols
- keywords start with leading colon and evaluate to themselves

### Literal Collections
- four collection types
  1. list '(1 2 3)
  2. vector [1 2 3]
  3. set #{1 2 3}
  4. map {:a 1, :b 2}

## Evaluation
- source code is read as characters by the Reader
  - Reader produces Clojure data
  - Clojure compiler then produces the bytecode for the JVM
notes:
- unit of source code is a **Clojure expression**, not a Clojure source file

### Structure vs Semantics
- most literal Clojure forms evaluate to themselves, except symbols and lists
  - symbols are used to refer to something else
  - lists are evaluated as an invocation
- ex. (+ 3 4)
  - read as a list containing the symbol (+) and two numbers (3 4)
  - first element is called the "function position", where the thing to invoke is
- note: in clojure, everything is an expression that evaluates to some value 
  - some expressions also have side effects
  - contrast to most languages which have statements (affect state but don't return anything) and expressions

### Delaying Evaluation with Quoting
- sometimes useful to suspend evaluation
- so we do `'x` to get `x`
- and we do `'(1 2 3)` to get a list instead of code to evaluate 
- ex. `(1 2 3)` would return an error since we can't evaluate a list of data

## REPL
- read-eval-print-looop
- note: clojure is always compilted to JVM bytecode (no clojure interpreter)
- namespace `clojure.repl` that is included in the standard Clojure library that provides a number of functions
    - `(require '[clojure.repl :refer :all])`
    - `doc` is a command that will show the documentation for any function

## Clojure Basics
- `def`: save piece of data for later (like a variable)
- printing:
  - `println` and `print`: human-readable forms, will translate special print characters to their printed form (will omit quotes in strings)
  - `prn` and `pr`: readable as data, will print surrounding quotes and ignore special characters like \n

# Functions

## Creating Functions
- functions are first class, can be passed-to and returned-from other functions
- more clojure code consists of primarily pure functions
- you can invoke a function by placing the name of the function in the function position (the first element in a list)

### Multi-arity Functions
- functions can be defined to take different numbers of parameters
  - must all be defined in the same `defn` (else they will just replace the previous function instead of adding to it)
- each arity is a list `([param*] body*)`
  - one arity can invoke another
  - ex.
```
(defn messenger
  ([]    (messenger "Hello world!"))
  ([msg] (println msg))
)
```

### Variadic Functions
- can also define a variable number of parameters 
  - the variable parameters must occur at the end of the paramter list (collected in a sequence for use by the function)
- marked with `&`
```
(defn hello [greeting & who]
  (println greeting who))
```

# Clojure for the Brave and True
**PARTS:**
ENVIRONMENT SETUP
1. Building, Running and the REPL
2. How to Use Emacs, an Excellent Clojure Editor
LANGUAGE FUNDAMENTALS
3. Do Things: A Clojure Crash Course
4. Core Functions in Depth
5. Functional Programming
6. Organizing Your Project: A Librarian's Tale
7. Clojure Alchemy: Reading, Evaluation, and Macros
8. Writing Macros
ADVANCED TOPICS
9. The Sacred Art of Concurrent and Parallel Programming
10. Clojure Metaphysics: Atoms, Refs, Vars, and Cuddle Zombies
11. Mastering Concurrent Processes with core.async
12. Working with the JVM
13. Creating and Extending Abstractions with Multimethods, Protocols, and Records
extras: Leiningen, Boot

# 1. Building, Running, and the REPL
## What Is Clojure?
- Clojure language is a Lisp dialect with functional emphasis 
- Clojure compiler is executable JAR file, takes written clojure and compiles it to JVM bytecode
- distinction since Clojure is a hosted language
  - Clojure programs are executed within a JVM and rely on JVM for core features like threading and garbage collection
- some setup stuff, I'll just use repl.it for testing for now

# 2. Emacs
- skip skip skip

# 3. Do Things: A Clojure Crash Course
## Syntax
### Forms
- all Clojure code is written in a uniform structure (the prefix thing with operators and stuff)
- two kinds of structures: literal representations of data structures and operations
- *form* (expression) refers to valid code

### Control Flow
**if**
```Clojure
(if boolean-form
  then-form
  optional-else-form)
```
- boolean-form is a form that evaluates to a truthy/falsey value

**do**
- note how with if you can only have one form associated with each branch
- `do` helps to get around this by wrapping up multiple forms in parantheses and running each of them in the branches

**when**
- combination of `if` and `do` but with no else branch
- returns nil when condition is false

#### truthiness, equality, nil, boolean expr
- has `true` and `false` values
- `nil` indicates no value in clojure
  - also represents logical falsiness
- all other values are logically truthy
- equality operator is `=`

**or**
- returns the first truthy value or the last value 

**and**
- returns the first falsey value or the last value

### Naming Values with def
- use def to bind a name to a value in Clojure
  - note difference between binding a name to a value and assigning a value to a variable
  - we don't really want to change the value associated with a name
ex. ruby ex vs clojure ex
```Ruby
severity = :mild
error_message = "OH GOD! IT'S A DISASTER! WE'RE "
if severity == :mild
  error_message = error_message + "MILDLY INCONVENIENCED!"
else
  error_message = error_message + "DOOOOOOMED!"
end
```
```Clojure
(defn error-message
  [severity]
  (str "OH GOD! IT'S A DISASTER! WE'RE "
    (if (= severity :mild)
      "MILDLY INCONVENIENCED!"
      "DOOOOOOMED!")))

; to call
(error-message :mild)
```

## Data Structures
- all the data structures are immutable, so can't change them in place

### Numbers
- integers, floats, and ratios (which are represented directly)

### Strings
- only double quotes
- concat through `str` function

### Maps
- like dictionaries or hashes
- we have **hash maps** or **sorted maps**
```Clojure
; examples

;; :first-name and :last-name are keywords
{:first-name "Charlie"
 :last-name "McFishwich"}

;; associating with functions
{"string-key" +}

;; nested example
{:name {:first "John" :middle "Jacob" :last "Jingle"}}
```
- or can use hash-map function to create a map
- use `get` function to look up values
```Clojure
(get {:a 0 :b 1} :b)
; => 1

(get {:a 0 :b 1} :c)
; => nil

(get {:a 0 :b 1} :c "default")
; => "default"

; get-in allows look up values in nested maps
(get-in {:a 0 :b {:c "nested"}} [:b :c])
; => "nested"

; or treat map like a function with key as argument
({:name "John Smith"} :name)
; => "John Smith"
```

### Keywords
- primarily used as keys in maps
- can be used as functions to look up the corresponding value in a data structure
```Clojure
(:a {:a 1 :b 2})
; which is equivalent to 
(get {:a 1 :b 2} :a)
; can also provide default values
(:c {:a 1 :b 2} "Some default value")
```

### Vectors
- vector is similar to array (0 indexed collection)
- use same `get` function as before, use the index as the key
- vector elements can be any type and types can be mixed
```Clojure
; ex vector literal 
[1 2 3]

; to add elements to a vector
(conj [1 2 3] 4)
; => [1 2 3 4]
; elements are added to END of a vector
```

### Lists
- lists are quite similar to vectors
- some differences: don't use `get`, instead, use `nth`
- can be any type as well
```Clojure
; ex list literal
'(1 2 3)
; need the single quote or else the compiler will try to evaluate it as an expression

; to add elements to a list
(conj '(1 2 3) 4)
; => (4 1 2 3)
; elements are added to the BEGINNING
```
- from which way you want to add is usually how to decide which one to use, lists vs vectors

### Sets
- collections of unique values
```Clojure
; hash set literal
#{"kurt" 20 :thing}

(hash-set 1 1 2 2)
; => #{1 2}

(conj #{:a :b} :b)
; => #{:a :b}

; from existing vectors/lists
(set [3 3 3 4 4])
; => #{3 4}

; set membership
(contains? #{:a :b} :a)
; => true
(contains? #{:a :b} 3)
; => false
; get and keyword will return the value if it exists or nil if it doesn't
(:a #{:a :b})
; => :a
(nil #{:a nil})
; => nil
; note: searching for nil will always return nil
```

## Functions
### Calling Functions
- all Clojure operations have the same syntax
  - function calls is really just when the operator is a function or function expression (smth that returns a function)
```Clojure
((or + -) 1 2 3)
```
- functions that take functions as arguments or that return functions are called *higher-order functions*

```Clojure
(map inc [0 1 2 3])
; => (1 2 3 4)
; note: it doesn't return a vector even though we supplied a vector
; map function allows the generalization of transforming a collection by applying any function over any collection
```
- note: Clojure evaluates all function arguments recursively before passing them to the function 

### Function Calls, Macro Calls, and Special Forms
- `def` and `if` are examples of special forms
  - main feature that makes them special is that, unlike function calls, they don't always evaluate all of their operands
  - you also can't use special forms as arguments to functions
- macros also evaluate operands differently and can't be passed as arguments but there's probably more stuff that's different

### Defining Functions
- parts of a function definition
  1. `defn`
  2. function name
  3. docstring to describe the function (optional)
  4. parameters listed in brackets
  5. function body
```Clojure
(defn too-enthusiastic
  "Return a cheer that might be a bit too enthusiastic"
  [name]
  (str "OH MY GOD! " name " YOU ARE LIKE THE BEST PERSON EVER"))

(too-enthusiastic "John Smith")
```

#### Arity Overloading
```Clojure
(defn multi-arity
  ([first-arg second-arg third-arg]
    (do-things first-arg second-arg third-arg))
  ([first-arg second-arg]
    (do-things first-arg second-arg))
  ([first-arg]
    (do-things first-arg)))

; can do variable-arity functions using a rest parameter
(defn codger-communication
  [whippersnapper]
  (str "Get off my lawn, " whippersnapper "!!!"))
(defn codger
  [& whippersnappers]
  (map codger-communication whippersnappers))
; will execute the function for each whippersnapper provided

; mixing rest parameters with normal parameters (rest has to come last)
(defn favourite-things
  [name & things]
  (str "Hi, " name ", here are my favourite things: "
    (clojure.string/join ", " things)))
(favourite-things "Doreen" "gum" "shoes" "karate")
; => "Hi, Doreen, here are my favourite things: gum, shoes, karate"
```
- can use this to provide default values for arguments
  - defining a function in terms of itself
```Clojure
(defn x-chop
  "Describe the kind of chop you're inflicting on someone"
  ([name chop-type]
    (str "I " chop-type " chop " name "! Take that!"))
  ([name]
    (x-chop name "karate")))
```

#### Destructuring
- allows binding names to values within a collection
```Clojure
(defn my-first
  [[first-thing]]
  first-thing)

(my-first ["oven" "bike" "war-axe"])
; => "oven"
```
- essentially telling Clojure that the function will receive a list/vector as an argument and that meaningful names should be given to different parts of the argument
  - can also use rest arguments here as well
- same idea with destructuring maps
```Clojure
(defn announce-treasure-location
  [{lat :lat lng :lng}]
  (println (str "Treasure lat: " lat))
  (println (str "Treasure lng: " lng)))
; does same thing as
(defn announce-treasure-location
  [{:keys [lat lng]}]
  (println (str "Treasure lat: " lat))
  (println (str "Treasure lng: " lng)))

; calling
(announce-treasure-location {:lat 28.22 :lng 81.33})

; use this to retain access to the original map
  [{:keys [lat lng] :as treasure-location}]
```

#### Function Body
- function body can contain forms of any kind
- clojure will automatically return the last form evaluated 
- note: clojure is simple and treats all functions the exact same way

### Anonymous Functions
- two ways to create anonymous functions: one with `fn`, the other is more compact
```Clojure
; syntax
(fn [param-list]
  function body)

; example
((fn [x] (* x 3)) 8)
; fn works identically to the normal defn
; you can even associate it with a name using def

; compact of the same example
(#(* % 3) 8)
```
- `%` indicates the argument passed to the function
  - multiple arguments would be denoted by `%1`, `%2`, `%3` and so on
  - rest parameter would be passed with `%&`
      - would return a list of all the arguments

## Pulling It All Together

### let
- binds names to values
- `let` also introduces a new scope 
- can reference existing bindings in the `let` binding
- can also use rest parameters, just like in functions
- note: the value of the let form is the last form in its body that is evaluated
- basically a nice way to introduce local names for values to help simplify the code

### loop
- another way to do recursion (vs just using functions)
  - preferred since it's less verbose and has better performance
```Clojure
; function method
(defn recursive-printer
  ([]
    (recursive-printer 0))
  ([iteration]
    (println (str "Iteration "iteration))
    (if (> iteration 3)
      (println "Goodbye!")
      (recursive-printer (inc iteration)))))
(recursive-printer)

; loop method
(loop [iteration 0]
  (println (str "Iteration " iteration))
  (if (> iteration 3)
    (println "Goodbye!")
    (recur (inc iteration))))
```

### Regular Expressions
- tools for performing pattern matching on text
```Clojure
; syntax
#"regular-expression"
```

### Example Code
```Clojure
(def asym-hobbit-body-parts [
  {:name "head" :size 3}
  {:name "left-eye" :size 1}
  {:name "left-ear" :size 1}
  {:name "mouth" :size 1}
  {:name "nose" :size 1}
  {:name "neck" :size 2}
  {:name "left-shoulder" :size 3}
  {:name "left-upper-arm" :size 3}
  {:name "chest" :size 10}
  {:name "back" :size 10}
  {:name "left-forearm" :size 3}
  {:name "abdomen" :size 6}
  {:name "left-kidney" :size 1}
  {:name "left-hand" :size 2}
  {:name "left-knee" :size 2}
  {:name "left-thigh" :size 4}
  {:name "left-lower-leg" :size 3}
  {:name "left-achilles" :size 1}
  {:name "left-foot" :size 2}])

(defn matching-part
  [part]
  {:name (clojure.string/replace (:name part) #"^left-" "right-") :size (:size part)})

(defn symmetrize-body-parts
  "Expects a seq of maps that have a :name and :size"
  [asym-body-parts]
  (loop [remaining-asym-parts asym-body-parts final-body-parts []]
    (if (empty? remaining-asym-parts)
      final-body-parts
      (let [[part & remaining] remaining-asym-parts]
        (recur remaining
          (into final-body-parts
            (set [part (matching-part part)])))))))
```

### reduce
- pattern of "process each element in a sequence and build a result"
```Clojure
(reduce + [1 2 3 4])
; is the same as
(+ (+ (+ 1 2) 3) 4)
```
- it can also take an optional initial value but it's pretty much the same thing, it'll just use the initial value with the first value in the sequence

### updated example code
```Clojure
(defn better-symmetrize-body-parts
  "Expects a seq of maps that have a :name and :size"
  [asym-body-parts]
  (reduce (fn [final-body-parts part]
            (into final-body-parts (set [part (matching-part part)])))
          []
          asym-body-parts))
```
### Hobbit violence
- yeah i agree it's a little strange
```Clojure
(defn hit
  [asym-body-parts]
  (let [sym-parts (better-symmetrize-body-parts asym-body-parts) body-part-size-sum (reduce + (map :size sym-parts)) target (rand body-part-size-sum)]
    (loop [[part & remaining] sym-parts accumulated-size (:size part)]
      (if (> accumulated-size target)
        part
        (recur remaining (+ accumulated-size (:size (first remaining))))))))

; calling the function
(hit asym-hobbit-body-parts)
; it'll return a random part of the body to hit
```

# 4. Core Functions in Depth

## Programming to Abstractions
- functions like map and reduce in terms of *sequence abstraction*, not specific data structures
- thinkg of abstractions as named collections of operations
  - if you can perform all of an abstraction's operations on an object, then that object is an instance of the abstraction
  - ex. `map` doesn't care about the specifics of the implementations of lists, vectors, sets, and maps
    - only whether it can perform sequence operations on them

### Treating Lists, Vectors, Sets, and Maps as Sequences
- core sequence functions: `first`, `rest`, `cons`
    - if these functions work on a data structure, the data structure implements the sequence abstraction
    - if yes, then one can use the seq library with that data structure (ex. `map` function etc)

### first, rest, and cons
- if you can just implement `first`, `rest`, and `cons`, you get all the other seq library functions for free
- ex. linked list
  - `first`: returns the value for requested node
  - `rest`: returns the remaining values after the requested node
  - `cons`: adds a new node with the given value to the beginning of the list

### Abstraction Through Indirection
- **indirection**: a generic term for the mechanisms a language employs so that one name can be multiple, related meanings
- **polymorphism** is one way that Clojure provides indirection
- also does a kind of type conversion, calling the `seq` function on a data structure whenever it expects a sequence
  - `seq` always returns a value that looks and behaves like a list
    - so a seq of a map consists of two-element key-value vectors
  - you can then convert back by using `into`

## Seq Function Examples

### map
- applying a function to every element to a sequence
- can also give map multiple collections
  - in this case, the elements of the first collection will be passed as the first argument of the mapping function, and the elements of the second collection will be passed as the second arg and so on
```Clojure
; multiple collection example
(map str ["a" "b" "c"] ["A" "B" "C"])
; => ("aA" "bB" "cC")
```
- can use `map` to retrieve the value associated with a keywored from a collection of map data structures
  - since keywords can be used as functions
```Clojure
(def identities
  [{:alias "Batman" :real "Bruce Wayne"}
   {:alias "Spider-Man" :real "Peter Parker"}
   {:alias "Santa" :real "Your mom"}
   {:alias "Easter Bunny" :real "Your dad"}])

(map :real identities)
; => ("Bruce Wayne" "Peter Parker" "Your mom" "Your dad")
```

### reduce
- processes each element in a sequence to build a result
- other uses:
  - transform a map's values: producing a new map with the same keys but updated values
```Clojure
(reduce (fn [new-map [key val]]
          (assoc new-map key (inc val)))
        {}
        {:max 30 :min 10})
; => {:max 31 :min 10}
```
  - filter out keys from a map based on their value
```Clojure
(reduce (fn [new-map [key val]]
          (if (> val 4)
            (assoc new-map key val)
            new-map))
        {}
        {:human 4.1 :critter 3.9})
; => {:human 4.1}
```

### take, drop, take-while, and drop-while
- `take` and `drop` take two arguments: a number and a sequence
  - `take` returns the first n elements of the sequence
  - `drop` returns the sequence with the first n elements dropped
- `take-while` and `drop-while` each take a *predicate function* (return value is evaluated true/false) to determine when it shouls stop taking or dropping
  - so `take-while` will continue taking until the function returns false
  - `drop-while` will continue dropping until the function returns false

### filter and some
- `filter` returns all elements of a sequence that test true for a predicate function
  - `filter` will end up processing all the data so it isn't always the most efficient to use (can use stuff like `take-while` or `drop-while` in those cases)
- `some` tells you whether the collection contains any values that tests true for a predicate function
  - returns the first truthy value (so anything that's not false or nil)

### sort and sort-by
- `sort` sorts elements in ascending order
- `sort-by` allows you to apply a function (key function) to the elements and use the values returned to determine the sort order

### concat
- appends the members of one sequence to the end of another

## Lazy Seqs
- many functions return a *lazy seq*, a seq whose members aren't computed until you try to access them (which is then called *realizing* the seq)
  - makes it more efficient and allows the construction of infinite sequences

### Demonstrating Lazy Seq Efficiency
- think of a lazy seq as two parts:
  1. a recipe for how to realize the elements of a sequence
  2. the elements that have been realized so far
- so when using map, the lazy seq doesn't include any realized elements yet but it has the recipe for generating its elements
- note: clojure chunks its lazy sequences, so it also premptively realizes some of the next elements as well (even if you only want one)
  - lazy seq elements need to be realized only once 

### Infinite Sequences
- `repeat` and `repeatedly` allow for the creation of sequences (pretty intuitive i think)
- lazy-seqs don't have to specify an endpoint, functions that realize the lazy-seq have no way of what will come next so as long as the seq continue providing elements, they'll keep taking them
```Clojure
(defn even-numbers
  ([] (even-numbers 0))
  ([n] (cons n (lazy-seq (even-numbers (+ n 2))))))

(take 10 (even-numbers))
; => (0 2 4 6 8 10 12 14 16 18)
```

## The Collection Abstraction
- closely related to the sequence abstraction
- clojure's core data structures (vectors, maps, lists, and sets) take part in both abstractions
- sequence abstration is about operating on members individually
- collection abstraction is about the data structure as a whole

### into
- can convert from one structure to another
  - useful since things are often converted to `lists` when using the seq library stuff
- also helps with taking two collections and adding all the elements from the second to the first

### conj
- also adds elements to a collection
```Clojure
(conj [0] [1])
; => [0 [1]]

(into [0] [1])
; => [0 1]

(conj [0] 1)
; => [0 1]

(conj [0] 1 2 3 4)
; => [0 1 2 3 4]

(conj {:time "midnight"} [:place "ye olde cemetarium"])
; => {:time "midnight" :place "ye olde cemetarium"}
```

## Function Functions
- these both accept and return functions

### apply
- explodes a seqable data tructure so it can be passed to a function that expects a rest parameter
```Clojure
(max 0 1 2)
; => 2

(max [0 1 2])
; => [0 1 2]

(apply max [0 1 2])
; => 2
```
- explodes the elements of the collection so they get passed to the function as separate arguments

### partial
- takes a function and any number of arguments and then returns a new function
  - calling the returned function calls the original function with the original arguments along with new arguments
```Clojure
(def add10 (partial + 10))
(add10 3)
; => 13
(add10 5)
; => 15
```

### complement
- returns the negation of a boolean function
```Clojure
(def not-vampire? (complement vampire?))
```

# 5. Functional Programming

## Pure Functions: What and Why
- pure function qualifications:
  1. always returns the same result if given the same argument - *referential transparency*
    - they rely only on their arguments and immutable values to determine return value
  2. it can't cause any side effects (function can't make changes that are observable outside the function itself)
    - so there is no uncertainty as to what functions are doing in a program

## Living with Immutable Data Structures

### Recursion Instead of for/while
- clojure doesn't allow for mutation of values 
  - can't associate a new value with a name without creating a new scope
- so we use recursion instead so we don't have to keep some value and continuously mutate it
- note: should generally use `recur` when doing recursion for performance reasons (clojure doesn't provide tail call optimization?)

### Function Composition Instead of Attribute Mutation
- another way to use mutation is to build up the final state of an object
  - in clojure, you'll use function composition, essentially passing return values of one function into another one
    - combining them, or nesting them 

## Cool Thing to Do with Pure Functions

### comp
  - for creating a new function from the composition of any number of functions
- if one of the functions takes more than one argument, you can wrap it in an anonymous function

### memoize
- can memoize pure functions so that clojure remembers the result of a particular function call (since they're referentially tyransparent)
```Clojure
(defn sleepy-identity
  "Returns the given value after 1 second"
  [x]
  (Thread/sleep 1000)
  x)
(sleepy-identity "Mr.Fantastic")
; => "Mr.Fantastic" after 1 second
(defn memo-sleepy-identity (memoize sleepy-identity))
(memo-sleepy-identity "Mr.Fantastic")
; => "Mr.Fantastic" after 1 second
(memoy-sleepy-identity "Mr.Fantastic")
; => "Mr.Fantastic" immediately
```

# 7. Clojure Alchemy: Reading, Evaluation, and Macros
- can use macros to essentially define your own syntax 

## Overview of Clojure's Evaluation Model
- clojure's evaluation model: reader, evaluator, macro expander
- clojure creates trees structured using clojure lists \rarr easy since it's in prefix notation already

```Clojure
(def addition-list (list + 1 2))
(eval addition-list)
; => 3

(eval (concat addition-list [10]))
; => 13

(eval (list 'def 'lucky-number (concat addition-list [10])))
; => #'user/lucky-number

lucky-number
; => 13
```
- clojure is *homoiconic*: represents abstract syntax trees using lists, we write textual reprsentations of lists when we write clojure code
  - so essentially we can manipulate the data structures to do stuff

## The Reader
- converts textual source code you save in a file or enter in the REPL into clojure data structures
  - translate between unicode characters to lists, maps, etc (other data structures)
```Clojure
(read-string "(+ 1 2)")
; => (+ 1 2)
(list? (read-string "(+ 1 2)"))
; => true
```
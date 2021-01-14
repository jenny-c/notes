# Basics
https://clojure.org/api/cheatsheet
- clojure is functional language 

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
- clojure creates trees structured using clojure lists &rarr; easy since it's in prefix notation already

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

;; reading anonymous functions
(#(+ 1 %) 3)
; => 4
(read-string "#(+ 1 %)")
; => (fn* [p1__423#] (+ 1 p1__423#))
```

### Reader Macros
- the above example of reading an anonymous function with `read-string` works through using a *reader macro* (sets of rules for transforming text into data structures)
- essentially they take a abbreviated reader form and then expand it to their full form
  - *macro characters*: `'`, `#`, `@` etc
```Clojure
(read-string "'(a b c)")
; => (quote (a b c))
(read-string "@var")
; => (clojure.core/deref var)
(read-string "; ignore!\n(+ 1 2)")
; => (+ 1 2)
```

## The Evaluator
- think of the evaluator as a function that takes a DS as an argument, processes the DS using rules corresponding to its type and returns a result
- thing that aren't lists of symbols evaluate to themselves (as well as empty lists)

### Symbols
- symbols are used to name functions, macros, data etc, and then evaluates by *resolving* them
**resolves by:**
1. looking up whether the symbol names s special form
2. looking up whether the symbol corresponds to a local binding
3. trying to find a namespace mapping introduced by `def`
4. throwing an exception

### Lists
- empty list evaluates to an empty list
- otherwise, evaluated as a call to the first element in the list

#### Function Calls
- each operand is fully evaluated (in the recursive manner mentioned above)
- often time the operands will resolve to themselves but when we nest we see how it works
```Clojure
;; to get an actual list we use the ' reader macro
'(a b c)
;; is the same as
(quote (a b c))
;; quote special form tells the evaluator to return the DS itself without evaluating like normal
```

### Macros
- can manipulate the DSs that clojure evaluates
```Clojure
(eval (read-string "(1 + 1)")) ; => will throw an exception

(let [infix (read-string "(1 + 1)")]
  (list (second infix) (first infix) (last infix)))
; => (+ 1 1)

(eval
  (let [infix (read-string "(1 + 1)")]
    (list (second infix) (first infix) (last infix))))
; => 2
```
- macros streamline this process
- are executed in between the reader and the evaluator
```Clojure
(defmacro ignore-last-operand
  [function-call]
  (butlast function-call))

(ignore-last-operand (+ 1 2 10))
; => 3

(ignore-last-operand (+ 1 2 (println "this won't get printed")))
; => 3
```
- note: the DS returned by the function is not evaluated, but the DS returned by the macro is
  - can use `macroexpand` to see what data structure a macro returns before the DS is evaluated
```Clojure
(macroexpand '(ignore-last-operand (+ 1 2 10)))
; => (+ 1 2)
(macroexpand '(ignore-last-operand (+ 1 2 (println "this won't get printed"))))
; => (+ 1 2)
```

### Syntactic Abstraction and the -> Macro
- clojure is essentially just a bunch of nested function calls
- `->` (threading or stabby macro) allows you to write stuff to understand from top-down/left-right
```Clojure
(defn read-resource
  "Read a resource into a string"
  [path]
  (read-string (slurp (clojure.java.io/resource path))))
;; can be rewritten as 
(defn read-resource
  [path]
  (-> path
      clojure.java.io/resource
      slurp
      read-string))
;; a pipeline that goes from to pto bottom instead of from inner parentheses to outer parantheses
```
- this is *syntactic abstraction*, lets us write code in a syntax that's different from Clojure's built-in syntax

# Writing Macros


---

# Reframe: The Basics
- functional framework for building web apps in clojurescript using React through reagent

## A Data Loop
- clojurescript is a modern LISP ( **homoiconic** )
- progrmaming through assembling data structures

- to build an app, hang pure functions on certain parts of the loop, re-frame looks after the **conveyance of data** around the loop (tag line for re-frame is "derived values, flowing")

- each iteration of the re-frame loop has 6 stages &rarr; like a domino cascade
1. event dispatch
  - event is sent when something happens (ex. button click, websocket receives a message etc)
  - without a triggering event, no cascade happens 
    - events propel the app from one state to another &rarr; **re-frame is event driven**
2. event handling
  - in response to an event, the app performs as action
  - functions which compute data essentially 
    - figure out what the required `side effects` are
3. effect handling
  - the `side effects` previously calculated are actioned
  - often just change the application state 
  - `v = f(s)` where `v` is a view, `f` is a function, and `s` is the app state
    - every time `s` changes, `f` will be rerun to compute a new `v`
4. query
  - about extracting data from "app state" and providing it in the right format for the fifth stage
5. view
  - many `ViewFunctions` (Reagent components) which render the UI of the application
  - compute and return data in **hiccup** which represents DOM
  - renders the DOM depending on the state/data from the last stage
6. DOM
  - handled by Reagent/React
    - the step in which the hiccup-formatted "descriptions of required DOM" are actioned

## State
- puts all state in one place (a readent atom)
- **benefits:**
  - a single source of truth, no code needed to synchronize state between many different stateful components
  - can be updated with a single `reset!` 
    - the transition from one state to another is instant, not a series of incremental steps
  - the data can be given a strong schema so that, at any moment, we can validate all the data in the app
    - do this check after every "event handler"
  - undo/redo becomes easy to implement since it's easier to make snapshots of the state and restore them
- will want to learn [spec](https://clojure.org/about/spec)
**Stages:**
1. `re-frame.core/dispatch`
  - call re-frame's dispatch to emit an `event`
2. `re-frame.core/reg-event-fx`
  - register handlers for events using `reg-event-fx`
3. `re-frame.core/reg-fx`
  - re-frame app can register `effects handlers` using `reg-fx`
4. `re-frame.core/reg-sub`
  - used to associate a query function with a query id
```Clojure
(re-frame.core/reg-sub ;; part of the re-frame API
  :query-items         ;; query id
  query-fn)            ;; function to perform the query
;; if, in stage 5, you see a (subscribe [:query-items]), then call query-fn to compute it
```
5. `re-frame.core/subscribe`
  - works with the previous and next stages to see which DOMs need to be recomputed
6. hiccup returned by the view function is made into real browser DOM by reagent/React

## Subscriptions
- data flows through **the signal graph**
  - transformed by the interior nodes (which are pure functions)
  - eventually arrives at the leaf `view functions`

### Four Layers of The Signal Graph
1. ground truth (root node, `app-db`)
2. extractors
  - subscriptions that extract data directly from `app-db`
3. materialised view
  - subscriptions which obtain data from other subscriptions (never directly from `app-db`)
4. view functions
  - leaf nodes which compute hiccup (DOM)
- simplest version of the graph doesn't even have a layer 3

## Effectful Handlers
- events happen when they are dispatched (typically triggered by an external agent)
  - then they must be "handled"
  - note: events are *mutative* by nature (if app in one state before an event is processed, it will be in a diff state after)
```Clojure
(re-frame.core/reg-event-db      ;; <-- call this to register a handler
  :set-flag                      ;; thsi is an event id
  (fn [db [_ new-value]]         ;; this function does the handling
      (assoc db :flag new-value)))
```
- the handler `fn` is expected to be pure
- so this works most of the time, but sometimes there has to be side effects which complicate everything
```Clojure
(reg-event-db
  :my-event
  (fn [db [_ bool]]
      (dispatch [:do-something-else 3])  ;; oops, side-effect
      (assoc db :send-spam new-val)))
```
- bad for a number of reasons: function's readers need to try harder to understand it, testing becomes harder, even replay-ability is lost
- note: a function is also not pure if it uses data from somewhere other than its arguments
- **effects:** what the event handler does to the world 
- **coeffects:** the data the event handler requires from the world in order to its computation
- some event handlers will have effects and coeffects (living in a mutative world)
  - instead of doing side-effects, we'll *cause* side-effects (re-frame looks after them basically)
```Clojure
;; this event handler is pure
(reg-event-db
  :my-event
  (fn [db _]
      (assoc db :flag true)))
```

### Effects: The Two Step Plan
1. work out how event handlers can declaratively describe side-effects, in data
2. work out how re-frame can do the "necessary side-effecting" efficiently and discreetly

#### Step 1
```Clojure
;; impure, side effecting handler
(reg-event-db
  :my-event
  (fn [db [_a]]
      (dispatch [:do-something-else 3]) ;; side-effect
      (assoc db :flag true)))

;; rewritten to be pure
(reg-event-fx                               ;; using reg-event-fx instead
  :my-event
  (fn [{:keys [db]} [_ a]]                  ;; destructuring db; first param is a map containing a :db key
      {:db (assoc db :flag true)            ;; handler is returning a map which describes a 1. change to application state, via :db key and 2. a further event, via the :dispatch key
       :dispatch [:do-something-else 3]}))


;; another example
;; impure way
(reg-event-db
  :my-event
  (fn [db [_ a]]
      (GET "http://json.my-endpoint.com/blah" ;; side-effect
          {:handler       #(dispatch [:process-response %1])
           :error-handler #(dispatch [:bad-response %1])})
      (assoc db :flag true)))

;; pure alternative
(reg-event-fx
  :my-event
  (fn [{:keys [db]} [_ a]]
      {:http {:method :get
              :url    "http://json.my-endpoint.com/blah"  ;; describes the side-effects required instead of doing them
              :on-success  [:process-blah-response]
              :on-fail     [:failed-blah]}
       :db   (assoc db :flag true)}))
```

#### The Coeffects
```Clojure
;; -fx handlers have been written this way
(reg-event-fx
  :my-event
  (fn [{:keys [db]} event]  ;; <- destructuring to get db
      {...}))

;; naming the first argument
(reg-event-fx
  :my-event
  (fn [cofx event]  ;; named cofx
      {...}))
```
- when using `-fx` form of registration, the first argument of the handler will be a map of coeffects we name `cofx`
- so `-db` and `-fx` handlers are conceptually the same, differing only numerically 
  - `-db` handlers take one coeffect called `db`, and return one effect `db`
  - `-fx` handlers take potentially many coeffects (map), and return potentially many effects (map)
```Clojure
;; these handlers achieve the same thing
(reg-event-db
  :set-flag
  (fn [db [_ new-value]]
      (assoc db :flag new-value)))

(reg-event-fx
  :set-flag
  (fn [cofx [_ new-value]]
      {:db (assoc (:db cofx) :flag new-value)}))
```
- `-db` is simpler and should be used whenever possible 

## Interceptors
- wrap event handlers
  - implement middleware (but different)
  - assemble functions, as data, in a collection &rarr; chain of interceptors
    - data is iteratively pipelined, first forwards through the chain of functions, then backwards along the same chain
- in re-frame:
  - the forward sweep progressively creates coeffects 
  - the backwards sweep processes the effects
```Clojure
;; interceptor form
{:id     :something           ;; decorative only
 :before (fn [context] ...)   ;; returns a possibly modified `context`
 :after  (fn [context] ...)}  ;; returns a possibly modified `context`
```
- to execute an inteceptor chain:
  1. create a context (a map with a certain structure)
  2. iterate forwards in the chain, challing the :before function and threading context through
  3. iterate over the chain backwards, calling the :after function and threading context through all the calls
- **context**
```Clojure
;; context is a map with this structure
{:coeffects {:event [:some-id :some-param]
             :db    <original contents of app-db>}

 :effects   {:db    <new value for app-db>
             :dispatch [:an-event-id :param1]}

 :queue     <a collection of further interceptors>
 :stack     <a collection of interceptors already walked>}
```
- note: can be self-modifying (interceptors can be dynamically added and removed from `:queue` and `:stack` by other interceptors in the chain)

- general:
  1. when an event handler is registered, a collection of interceptors is supplied
  2. when registering an event handler, associating an event id with a chain of interceptors
  3. interceptor chain is executed in two stages (forward sweep and backwards sweep)
  4. interceptors: add to `:coeffects`, process side `:effects`, produce logs, and further process

## Effects
```Clojure
;; when handler registered with -fx, must return effects
(reg-event-fx
  :my-event
  (fn [cofx [_ a]]    ;; first arg is coeffects, not db
    {:db      (assoc (:db cofx) :flag a) ;; side effect on app-db
     :fx      [[:dispatch [:do-something-else 3]]]})) ;; return effects; dispatch this event
```
- can register your own side effects with `reg-fx`
```Clojure
(reg-fx
  :butterfly   ;; effect key
  (fn [value]  ;; effect handler
    ...
    ))
```
- note: when handler is registered using either `-db` or `-fx`, an interceptor, `do-fx` is inserted at the beginning of the chain
  - so its `:after` function is the last thing run in the chain 
    - extracts `:effects` from `context` and iterates across the key/value pairs it contains, calling the registered effect handlers for each

## Coeffects
- current state of the world, in data, as presented to an event handler
- `coeffects` is a superset of `db`: it contains `db`
- need a way to put the data we want into `cofx` so we can access it the way we want

- each time an event is handled, a new `context` map is created, which contains a `:coeffects` key
  - so the handler's interceptor chain which can add to the data eventually seen by the event handler
  - do this with `inject-cofx`
```Clojure
;; event handler with access to 3 coeffects
(reg-event-fx
  :some-id
  [(inject-cofx :random-int 10) (inject-cofx :now) (inject-cofx :local-store "blah")]
  (fn [cofx _]
    ... )) ;; have access to cofx's keys :random-int :now and :local-store
```
- now it needs to know what to when given these pieces of data; each cofx-id requires an action
  - do this with `reg-cofx`
```Clojure
(reg-cofx             ;; registration function
  :now                ;; what cofx-id we are registering
  (fn [coeffects _]   ;; second parameter not used in this case
    (assoc coeffects :now (js.Date.))))  ;; add :now key, with value

;; another example
(reg-cofx
  :local-store
  (fn [coeffects local-store-key]
    (assoc coeffects
           :local-store
           (js->clj (.getItem js/localStorage local-store-key)))))
```
note: `inject-cofx :db` is also added at the front of each chain

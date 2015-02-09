Ruby
====

## Universal methods

* `.to_s` converts value to a string
* `.to_i` converts value to an integer
* `.to_a` converts value to an array

* `method!` impacts current data instead of a copy

## Symbols

`:symbol` defines a new Symbol

## Strings

`"myString"` defines a new String

### Methods

* `.length` returns the string length
* `.reverse` returns the string reversed
* `.upcase` / `.downcase` changes the string's case 

* `.lines` splits a string by lines into an array
* `.chars` splits a string by chars into an array
* `.bytes` splits a string by bytes (?) into an array

http://ruby-doc.org/core-2.2.0/String.html#method-i-delete

## Arrays

`[1, 2, 3]` defines a new Array

### Methods

* `.length` returns the number of elements in an array
* `.max` returns the highest value in an array
* `.sort` returns a sorted array
* `.join` returns a string with joined elements

* `array['foo'] = 'bar'` called on a string, will replace the first occurence of `foo` by `bar` in the string

## Hashes

* `{}` defines an empty Hash
* `Hash.new(0)` defines an empty Hash

### Methods

* `.length` returns the number of keys in a hash
* `.keys` returns all keys in a hash


## Blocks

    method { |value| ... }
    
<=>
    
    method do |value|
        ...
    end
    
    
## Custom methods

`def method_name(args)` defines a new method


## Import libraries

`require 'library'` 

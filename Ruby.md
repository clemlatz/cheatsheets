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

* `def method(args)` defines a new method
* `def method(arg1, arg2 = nil, arg3 = nil)` defines a method with 2 optional arguments
* `def method(arg1, *args)` defines a method with a splat arg, will create a args array will all other args

Defining a method with multiple optional unordered arguments:

    def book(title, options = {})
        book = Book.new
        book.title = title
        book.author = options[:author]
        book.publisher = options[:publisher]
    
    book("Le Petit Prince",
        author: "Antoine de Saint-Exupéry",
        publisher: "Gallimard"
    )

## Exceptions

    def method()
        unless isAuthorized(@user)
            raise AuthorizationException.new
        end
        # return stuff
    end
    
    begin
        method()
    rescue AuthorizationException
        warn "You are not authorized to access this"
    end

## Import libraries

`require 'library'` 


## Expressions

* `if`/`elsif`/`else`
* `unless` means "if not"

### Truthy and falsy value :

* `nil` == `false`
* `"" == 0 == [] == true`

### Inline condition

* `puts "Password too short" if password.length < 8`
* `puts "No username set" unless username`

### Short-circuit assignment

* `result = nil || 1` returns 1
* `result = 1 || nil` returns 1
* `result = 1 || 2` returns 1

* `books = library.books || []` returns an empty array if `library.books` is nil

### Conditional assigment

* `count ||= 2` will se set to 2 if `count` is nil

    path = if author
        "/books/" + author +"/"
    else
        "/books/"
        
    author_link = case author_url
        when "web"
            "http://www.author.com"
        when "Facebook"
            "https://facebook.com/author"
        else
            nil
    end

# Methods and classes

## Methods

### Optional arguments

    def tweet(message, lat = nil, long = nil)
      ...
    end
    
### Option hash

    def tweet(message, options = {})
        lat = options[:lat]
        long = options[:long]  
    end

    tweet("Hello world", lat: XXX, long: YYY)

### Splat arguments

    def mention(status, *names)
    end

    mention("Hello world", "Riri", "Fifi", "Loulou")

    names = ["Riri", "Fifi", "Loulou"]

## Classes

    class Tweet
        attr_accessor :status # Create getter et setter methods
        attr_reader :created_at # Create only getter
    
        def initialize(status) # constructor
           self.status = status
           self.created_at = Time.new
        end

        def to_s # override existing method
            "#{@status}\n#{@created_at}"
        end

    end

## Exceptions

    def get_tweets(list)
        unless list.authorized?(@user)
            raise AuthorizationException.new
        end
        list.tweets
    end

    begin
        tweets = get_tweets(my_list)
    rescue AuthorizationException
        warn "You are not authorized to see that list"
    end

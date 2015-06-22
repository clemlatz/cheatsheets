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

http://ruby-doc.org/core-2.2.0/String.html

ActiveSupport methods:
* `.pluralize`
* `.singularize`
* `.camelize`
* `.underscore`
* `.titleize`
* `.dasherize`
* `.parameterize`

## Arrays

`[1, 2, 3]` defines a new Array

### Methods

* `.length` returns the number of elements in an array
* `.max` returns the highest value in an array
* `.max_by` returns the highest element according to one of its property
* `.sort` returns a sorted array
* `.sort_by` returns a array sorted according to one of it's element property
* `.join` returns a string with joined elements

* `array['foo'] = 'bar'` called on a string, will replace the first occurence of `foo` by `bar` in the string

* `array << item` or `array.push(item)` add item to the array

ActiveSupport methods:
* `.from(x)` returns all elements after the xth
* `.to(x)` returns all element before the xth


## Hashes

* `{}` defines an empty Hash
* `Hash.new(0)` defines an empty Hash

### Methods

* `.length` returns the number of keys in a hash
* `.keys` returns all keys in a hash

ActiveSupport methods:
* `.diff(hash2)` returns the diff between hash and hash2
* `.stringify_keys` returns the hash with the keys stringified
* `.except(:key)` returns all hash elements except `:key`
* `.assert_valid_keys(:key1, :key2)` throws an exception if hash contains other keys

## Dates

ActiveSupport methods:
* `.advance(years: x, months: x, days: x)` returns a date in the future
* `.tomorrow` returns the day after
* `.yesterday` returns the day before


## Blocks

    method { |value| ... }

<=>

    method do |value|
        ...
    end

### Custom blocks

  def call_this_block_twice
    yield "world" # execute whatever is in the block
    yield "world"
  end

  call_this_block_twice { |name| puts "hello {#name}" } # puts will be executed twice

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

    `def mention(status, *names)
    end`

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

### Inheritence

  class Player
    def initialize(name)
      @name = name
    end

    def die(reason)
      puts "#{@name} died because of #{reason}"
    end
  end

  class Magician < Player
    def initialize(name, magic_points)
      @magic_points = magic_points
      super(name) # Calls the method in an ancester class with the same name
    end
  end

  # If super is called without arg, it passes to parent method current methods args

  class Warrior < Player
    def die(reason) # overring ancester class method
      puts "#{@name} has gone to Walhalla because of #{reason}"
    end
  end


## Modules

* `include` in a class, exposes module's methods as instance methods
* `extend` in a class, exposes module's methods as class methods
* `object.extend(module)` to expose module's to a single instance

    # image_utils.rb
    module ImageUtils
      def self.included(base) # hook called when module is include
        base.extends(ClassMethods) # extends calling class with ClassMethods submodule
      end

      def self.preview(image)
      end

      def self.transfer(image, destination)
      end

      module ClassMethods # will be included as class methods in self.included
        def fetch_from_twitter(user)
        end
      end
    end

    # avatar.rb
    require 'image_utils'
    class Avatar
      include ImageUtils
    end  

    # run.rb
    image = user.image
    image.preview # preview method with image object from ImageUtils module

* `extend ActiveSupport::Concern` can help with complex module dependencies


## Test::Unit

    require test/unit

    class ClassTesting < Test::Unit::TestCase
    
      def test_if_true
        assert true
      end
    
    end
    
## ActiveSupport

    class ZombieTest < ActiveSupport::TestCase
      test "invalid without a name" test_invalid_without_a_name
        z = Zombie.new
        assert !z.valid?, "Name is not being validated"
      end
    end

## Fixtures

Examples loaded in the database on start, rolled back before each tests

    # test/fixtures/books.yml
    book1:
      id: 1
      title: "Foo"
      author: "Bar"
      
    # test.rb
    book = books(:book1)

* ``

### Assert methods

* `assert`
* `assert_not_nil`
* `assert_equal`
* `assert_match`

### Rake

* `rake db:test:prepare` checks for migrations & load schema
* `rake test` or `rake` runs `db:test:prepare` & run all tests
* `ruby -Itest test/unit/my_test.rb` runs an individual test
* `ruby -Itest test/unit/my_test.rb` -n test_if_true runs an single test case
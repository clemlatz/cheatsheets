Objective C
===========

# Basics

### NSLog(@"Hello world %@", string)
Log Hello World to the console with a string object

### [myObject myMessage:argument]
Send myMessage to myObject (or calling myMessage method of myObject)



# Data types


## NSUInteger 

### NSUInteger myInt = 5
Define a Integer object (placeholder: %lu)


## NSNumber 

### NSNumber *myNumber = @21
Define a Number object

### [myNumber unsignedIntegerValue]
Return a NSUInteger from a NSNumber


## NSString 

### NSString *emptyString = [NSString string];
Create a empty NSString

### NSString *myString = @"Hello world"
Create a NSString object containing *Hello World*

### NSString *aCopy = [NSString stringWithString:myString]
Create *aCopy* as a copy of *myString*

### NSString *newString = [NSString stringWithFormat:@"%@ %@", string1, string2]
Create a *newString* by concatenating *string1* and *string2*

### [myString length]
Return the number of character in *myString* as a NSUInteger

### [myString stringByAppendingString:myOtherString]
Concatenate *myString* with *myOtherString*


## NSArray 

### NSArray *emptyArray = [NSArray array]
Create an empty array

### NSArray *myArray = @[@"element1", @"element2", @"element3"]
Create an array with values

### myArray[0]
Access the first item in array


## NSDictionary 

### NSDictionary *emptyDict = [NSDictionary dictionary];
Create an empty dictionnary

### NSDictionary *myDict = @{@"item1": @"value1", @"item2": @"value2"}
Create a dictionnary with values

### * myDict[@"item1"]
Access first item in array




# Universal messages (methods)

### [object description];
Return the content of the object as a NSString

### class *emptyObject = [[class alloc] init];
Create an empty object from any type
* Example: NSString *emptyString = [[NSString alloc] init];

### class *aCopy = [[class alloc] initWithclass:myObect];
Create a copy from an object from any type
* Example: NSSTing *aCopy = [[NSString alloc] initWithString:myString];

























End of file

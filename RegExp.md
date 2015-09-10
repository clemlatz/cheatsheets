Regular Expressions
===================

# Match a word

    /word/
  
# Match a word or another

    /word|another/
  
# Plus operator
Matches 1 or more occurence of the previous character

    /ar+/ # will match ar, arr, arrrrrrrr
    
# Character set & range
Square brackets create a character set aka character class

    /[a-z]/ # will match any letter from a to z of only one characters
    
* A range only works in a character set
* This character set represents 1 character of the subject

    /[a-z]+/ # will match any letter from a to z of at least one char long string
    
* This won't match uppercase characters

# Modifier

    /[a-z]+/i # will match any chars insensitively (upper- or lowercase)
    
# Whitespace

    \s/ # will match any whitespace characters (space, tabs, newlines)
    
    /[a-z\s]+/i # will match "Captain Hook"
    
# Numbers

    /[a-z0-0\s]+/i # will match "Long John the 3rd"
    
# Word Metacharacter
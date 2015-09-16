Regular Expressions
===================

## Match a word

    /word/

## Match a word or another

    /word|another/ # will match "word" or "another"
    /(f|m)ood/ # will match "food" or "mood"

## Plus operator
Matches 1 or more occurence of the previous character

    /ar+/ # will match ar, arr, arrrrrrrr

## Character set & range
Square brackets create a character set aka character class

    /[a-z]/ # will match any letter from a to z of only one characters

* A range only works in a character set
* This character set represents 1 character of the subject

    /[a-z]+/ # will match any letter from a to z of at least one char long string
    /\+/ # will match a litteral plus: +

* This won't match uppercase characters

    /[^\d]/ # will match anything that is NOT a number

* `^` means "not" if in a character set

## Modifiers

* `i` : case insensitive (upper- or lowercase)
* `g` : match all occurences (instead of first one only)
* `m` : multiline (anchors will match end and beginning of line instead of full string)

## Quantifiers

* `?` : zero or one
* `+` : one or more
* `*` : zero or more
* `{4}` : exactly four
* `{4,8}` : four to eight
* `{23,}` : at least 23
* `{,42}` : at most 42

## Whitespace

    \s/ # will match any whitespace characters (space, tabs, newlines)

    /[a-z\s]+/i # will match "Captain Hook"

## Numbers

    /[a-z0-9Ó\s]+/i # will match "Long John the 3rd"

## Metacharacters

* `\s` : whitespace characters
* `\S` : every character except whitespace
* `\d` : numbers
* `\D` : every characters except numbers
* `\w` : every words (= a-zA-Z0-9)
* `\W` : every characters except word

## Dot Metacharacter

    . # wildcard, will match any character except the new line
    \. # will match a literal dot: .

## Anchors

    /^foot$/ # will match "foot" but not "footer" nor "bigfoot"

* ^ start looking at the beginning of the subject
* $ stop looking at the end of the subject
* helps ensure that nothing is after and/of before the string

## Boundary Metacharacter

  /\bok\b/g # will match "ok" but not "okay"

* Matches words surrounded by boundary

## Question mark operator

  /foot?/ # will match ship foot and foo and (partially) food
  /pirate\s(ship)?/ # will match "pirate ship" and (partially) "pirate boat"

* Makes a character or character group optional
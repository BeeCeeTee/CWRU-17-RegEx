# RegEx: Matching an Email

The purpose of this guide is to help explain and illustrate how regular expressions (regex) work and what each of the components contributes to the expression. We will take a look at an example expression, break it down and look at each piece, and then examine how it all fits back together to produce the end result.

## Summary

According to MDN, a regular expression is defined as **_"patterns used to match character combinations in strings."_** Generally, regular expressions are used to search for a specific pattern of characters within a document, such as a phone number or a zip code, without providing a specific set of characters to search for. The result can then be used in a variety of ways, like redacting personal phone numbers or addresses or to find all instances of a specific word in a file. For this example, we will use a regex which has the purpose of validating an email input. The expression checks for a given number of characters of a specified type (letters, numbers, special characters, etc.) in a given format (ex. name@email.com), and validates whether or not the input was a valid email. 

The expression we will be looking at for this tutorial is this:<br/>
`/^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$/`

## Table of Contents

- [Anchors](#anchors)
- [Grouping Constructs](#grouping-constructs)
- [Bracket Expressions](#bracket-expressions)
- [Character Escapes](#character-escapes)
- [Quantifiers](#quantifiers)
- [Character Classes](#character-classes)
- [The OR Operator and Flags](#the-or-operator-and-flags)
- [Review](#review)

## Regex Components

### Anchors
Regular expressions begin and end with a `/` (forward slash) which act as opening and closing tags for the expression. The next component is called an anchor and includes a `^` (caret) to indicate the beginning of the string pattern to be searched, and a `$` (dollar sign) to indicate the end of the string. The search pattern itself goes between the anchors.

The anchors in our example are:<br>
`^` and `$`
<hr>

### Grouping Constructs
Sections of a string to be checked together are grouped using grouping constructs, which group a section of a regex using `()` (parentheses). Whatever is inside of the parentheses is called a subexpression. Subexpressions are matched exactly unless otherwise specified.

Our example expression has three grouping constructs:<br>
`([a-z0-9_\.-]+)` `([\da-z\.-]+)` and `([a-z\.]{2,6})`
<hr>

### Bracket Expressions
If we want to search for a range of characters instead of an exact set of characters within a grouping construct, we can use `[]` (square brackets). Anything inside of square brackets is called a bracket expression or a character set, and can be used to provide more flexibility in our search. So, we can specify which characters we want to search for, such as **[abcdef]**, or we can specify a range of characters using a `-` (hyphen), such as **[a-f]**. Both types of bracket expression will search for the same thing. It is important to note that ranges within character expressions are case sensitive, so **[a-f]** will search for lowercase a-f, and **[A-F]** will search for uppercase A-F.

In our example, we have three bracket expressions:<br>
`[a-z0-9_\.-]` `[\da-z\.-]` and `[a-z\.]`
<hr>

### Character Escapes
Sometimes certain characters can serve other functions within an expression, such as the parentheses serving as grouping constructs or square brackets serving as bracket expressions. However, if we want to literally search for such a character instead of allowing it to serve its default purpose, we need character escapes. Character escapes include a character preceeded by a `\` (backslash), and signify that we want to search for a character literally.

In our example, we escape the `.` (dot) symbol three times as such:<br>
``\.``
<hr>

### Quantifiers
Quantifiers indicate how many characters of a given type we want to search for in part of a string. We have several methods available to do so. We can use an `*` (asterisk) to indicate that we expect a given pattern to match 0 or more times, a `?` (question mark) to match a pattern 0 or 1 times, or a `+` (plus) to match a pattern 1 or more times. We can also use `{}` (curly brackets) to specify how many times we want to match or provide a range of number of times to match. For example, if we provided a phone number and expected three digits in the area code, we could specify {3}. If we had an email address, such as in our example, we could specify to match 1 or more characters using {1,}.

In our example, we specify that the first two bracket expressions should match 1 or more times with `+` and that the third should match between 2 and 6 times with `{2,6}`
<hr>

### Character Classes
Next in our example regex we have character classes. Character classes are essentially shorthand to define a given set of characters we want to search for within an expression. For example, `\w` searches for all alphanumeric characters of any case including an underscore as well. So \w serves the same purpose as [A-Za-z0-9_], it's just a shorter way to write it.

In our regular expression, we include the character class `\d` to indicate that we want to match any arabic digit from 0-9.
<hr>

### The OR Operator and Flags
Sometimes we want to be able to match given characters within a grouping construct in any particular order, as long as the grouping construct order remains the same. In these cases, we would use the OR operator, `|` (pipe), to accomplish this. For example, if we had two grouping constructs (abc) and (123) and we wanted the characters within each to be matchable in any order, we could write (a|b|c) and (1|2|3). This would allow (cba) and (321) to match while maintaining the order of the grouping constructs as a whole (letters first followed by numbers).

Another component which could exist within a regex is the flag. A flag is an optional character placed between the closing slash and an additional slash which defines additional functionality for an expression. Flags can indicate a number of functions, such as making a regex a multi-line search with `m` or bypassing case sensitivity with `i`.

In our example regex, we don't have any OR operators or Flags.
<hr>

### Review
Let's review each component of our regular expression in order:<br>
`/^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$/`

`/^` This is the opening tag and the starting anchor<br>
`([a-z0-9_\.-]+)` This is the first of three grouping constructs<br>
`[a-z0-9_\.-]` This is the first of three bracket expressions<br>
`a-z0-9_\.-` The ranges of characters to match, including an escaped dot, underscore and hyphen<br>
`+` Our 1 or more quantifier<br>
`@` The @ symbol, which is required after the first grouping construct<br>
`([\da-z\.-]+)` Our second of three grouping constructs<br>
`[\da-z\.-]` Our second of three bracket expressions<br>
`\d` A character class to match all arabic digits<br>
`a-z\.-` The range of characters to match, including an escaped dot and hyphen<br>
`+` Another 1 or more quantifier<br>
`\.` An escaped dot, required after the second grouping construct<br>
`([a-z\.]{2,6})` Our third and final grouping construct<br>
`[a-z\.]` Our third and final bracket expression<br>
`a-z\.` The range of characters to match, including an escaped dot<br>
`{2,6}` A quantifer to match between 2 and 6 characters<br>
`$/` Our ending anchor and closing tag<br>

Together, these components will match the following:
 - a range of 1 or more lowercase letters a-z, numbers 0-9, underscore, dot or hyphen, followed by;
 - an @ symbol, followed by;
 - 1 or more of any number digit, lowercase letters a-z, dot or hyphen, followed by;
 - a . symbol, followed by;
 - between 2 and 6 lowercase letters a-z or dots

 And the result will include a valid email address! Hopefully this guide has helped you to understand the components of regular expressions and how they work together.

 Thanks for reading!

## Author

Brendan Trepal is a Web Developer based in Cleveland, Ohio with an interest in both front-end and back-end technologies and development. To check out more of his work, visit his GitHub profile here:

https://github.com/BeeCeeTee

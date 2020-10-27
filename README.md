# An Introduction to Regex
*A compact introduction to using regex.*

Regular expressions (regex) are a sequence of characters which define a pattern of text to be searched for. Being able to use regex, where available, will provide the ability to create significantly more specific search patterns than would otherwise be available with the typical "find" or "find and replace" functions.

It should be noted that there are [several flavours of regex](http://web.archive.org/web/20130830063653/http://www.regular-expressions.info:80/refflavors.html) available - the aim of this guide is to compactly present the core basics of regex that can be used in most places. The content will be presented with a short explanation of the definition or concept, followed by some examples - for a significantly more comprehensive guide see the appropriate language's regex documentation, such as the one for [Python](https://docs.python.org/3/library/re.html) or the one for [Java](https://docs.oracle.com/javase/7/docs/api/java/util/regex/Pattern.html).

## Table of Contents

- [Literals](#literals)
- [The Escape Character](#the-escape-character)
- [Alternation](#alternation)
- [Grouping](#grouping)
- [Wildcards](#wildcards)
- [Character Sets](#character-sets)
- [Ranges](#ranges)
- [Shorthand Character Classes](#shorthand-character-classes)
- [Quantifiers](#quantifiers)
  * [Fixed Quantifiers](#fixed-quantifiers)
  * [Optional Quantifiers](#optional-quantifiers)
  * [The Kleene Star and Kleene Plus](#the-kleene-star-and-kleene-plus)
- [Anchors](#anchors)

## Literals

A regex which contains the exact text that we wish to match is called a *literal*.

**Example:** The regex `a` will match the text `a`. It will also match the `a` in the text `car`.

**Example:** The regex `42` will match the text `42`. It will also match the `42` in the text `I wish I had 42 apples.`.

**Example:** The regex `banana` will match the text `banana`. It will also match the `banana` in the text `I have too many bananas.`.

## The Escape Character

Throughout this guide we'll see many examples of symbols which have regex functionality, also called regex [*metacharacters*](https://en.wikipedia.org/wiki/Metacharacter), but may also appear in text which we want to match. For example, in the [Wildcards] section, we'll see that the period, `.`, symbol has regex functionality. If we wish to match a symbol in text without invoking its regex functionality, then we need to precede it with the *escape character*, `\`.

## Alternation

We can use the pipe symbol, `|`, to match all of the characters before the `|`, or all of the characters after the `|`. This is called *alternation*.

**Example:** The regex `apple|banana` will match the `apple` in the text `I love apples.` and the `banana` in the text `I love bananas.`.

**Example:** The regex `I love apples|bananas` will match the text `I love apples` or the text `bananas`.

## Grouping

If we want to restrict the alternation to only a part of the regex, then we can using *grouping* to define where the alternation should start, via `(`, and where the alternation should end, via `)`. The part of the regex which is grouped is called a *capture group*.

**Example:** The regex `I love (apples|bananas)` will match the text `I love apples` or the text `I love bananas`.

## Wildcards

If we want to specify the existence of a character in our regex without being exact, then we can use the *wildcard* character, `.`, which represents any single character (including whitespace).

**Example:** The regex `....` will match the text `boat`, the text `bike`, the text `car!`, and any other text consisting of four characters.

**Example:** The regex `I ate . bananas` will match the text `I ate 3 bananas` and the text `I ate 5 bananas`.

## Character Sets

We can choose to match one character from a series of characters by including those characters inside square brackets, `[]`. These series of characters, along with their accompanying `[]`, are called *character sets*.

**Example:** The regex `se[ea]` will match the text `see` and the text `sea`.

**Example:** The regex `[car]` will match the `c`, the `a`, or the `r` in the text `car`, but will not match with `car`.

Character sets can be negated by using the caret symbol, `^`, before the series of characters, to match any characters which are not in the character set.

**Example:** The regex `[^car]` will match any character which is not `c`, `a`, or `r`, such as `b`, `i`, `k,` or `e`.

## Ranges

We can use the `-` character to specify a *range* of characters in a character set, rather than manually enumerating all of the characters in the range.

**Example:** The regex `[a-d]` will match `a`, `b`, `c`, `d`. Note that this is equivalent to the regex `[abcd]`.

**Example:** The regex `I own [2-6] [b-h]ats` will match the text `I own 2 bats`, the text `I own 3 cats`, and the text `I own 6 hats`.

It is possible to specify multiple ranges in the same character set by concatenating them.

**Example:** The regex `[A-Za-z]` will match any single uppercase or lowercase letter in the alphabet.

## Shorthand Character Classes

Using ranges with character classes is useful, but we can simplify the regex for some common ranges by using *shorthand character classes* such as:

- The word character class, `\w`, which is equivalent to `[A-Za-z0-9_]`.
- The digit character class, `\d`, which is equivalent to `[0-9]`.
- The whitespace character class, `\s`, which is equivalent to `[ \t\r\n\f\v]` (i.e. space, tab, carriage return, line break, form feed, vertical tab).

**Example:** The regex `\d\s\w\w\w\w\w\w\w` will match a digit character, followed by a whitespace character, followed by seven word characters, such as the text `4 bananas`.

Shorthand character classes can be negated by using an uppercase letter in the shorthand instead. For example, the *negated shorthand character classes* of the above would be:

- The non-word character class, `\W`, which is equivalent to `[^A-Za-z0-9_]`.
- The non-digit character class, `\D`, which is equivalent to `[^0-9]`.
- The non-whitespace character class, `\S`, which is equivalent to `[^ \t\r\n\f\v]`.

## Quantifiers

A *quantifier* is a set of regex metacharacters which modifies the matched quantity of the preceding regex character.

Note that quantifiers are greedy, i.e. they will match the maximum number of characters that they can. For example, if a quantifier had the choice between returning a match of `hello`, `helloo`, or `hellooo` in a particular text, then it would return a match of `hellooo`.

Note also that all quantifiers can take advantage of [grouping](#grouping).

### Fixed Quantifiers

Instead of repeatedly using regex characters to match a certain quantity, we can instead specify the quantity that should be matched by using curly braces, `{}`. This quantity, along with their accompanying `{}`, are called *fixed quantifiers*.

- `{n}`will match the preceding regex character exactly `n` times.
- `{n,m}` will match the preceding regex character at least `n` times, and at most `m` times.

**Example:** The regex `wo{4}f` will match the text `woooof`.

**Example:** The regex `wo{2,5}f` will match the text `woof`, the text `wooof`, the text `woooof`, and the text `wooooof`.

### Optional Quantifiers

We can indicate that the existence of a regex character is optional, i.e. it appears `0` or `1` times, by using the question mark, `?`, after it. The `?` is called an *optional quantifier*.

**Example:** The regex `colou?rful` will match the text `colorful` and the text `colourful`.

**Example:** The regex `The (really fast)? car was red.` will match the text `The car was red.` and the text `The really fast car was red.`.

### The Kleene Star and Kleene Plus

The following Kleene quantifiers, named after the mathematician [Stephen Cole Kleene](https://en.wikipedia.org/wiki/Stephen_Cole_Kleene), are useful when we don't want to specify a maximum number of character matches in a particular text:

- The *Kleene star*, `*`, specifies that the preceding regex character should be matched `0` or more times.
- The *Kleene plus*, `+`, specifies that the preceding regex character should be matched `1` or more times.

**Example:** The regex `vro*m` will match the text `vrm`, the text `vrom`, the text `vroom`, and the text `vroooooooooom`.

**Example:** The regex `vro+m` will match the text `vrom`, the text `vroom`, and the text `vroooooooooom`, but will not match the text `vrm`.

**Example:** The regex `.*` and `.+` will match all texts.

## Anchors

When using regex we may make matches with unintended texts. To alleviate this, we can choose to be more specific with our regex by using *anchors* to specify exactly what character the matched text should start or end with.

- The claret symbol, `^`, is placed before a regex character to anchor the starting position of the matched text.
- The dollar symbol, `$`, is placed after a regex character to anchor the ending position of the matched text.

**Example:** The regex `^b` will match the `b` in the text `bc`, but not in the text `abc`.

**Example:** The regex `c$` will match the `c` in the text `bc`, but not in the text `bcd`.

**Example:** The regex `^abc$` will match the text `abc`, but not anything in the text `abcd` or in the text `zabc`.

**Example:** The regex `^I love dogs$` will match the text `I love dogs`, but not anything in the text `Sometimes I love dogs` or in the text `I love dogs and donkeys`.

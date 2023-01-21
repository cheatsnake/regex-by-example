# Regex by example

[Regular Expressions](https://en.wikipedia.org/wiki/Regular_expression) is a powerful tool for finding and replacing symbols in text. A regular expression itself is just a string composed according to certain rules. This line has two slashes `/ /`, where after the first line there is a special pattern for searching, and after the second – a set of flags that affect the result.

The power of regular expressions will be very useful in many programming tasks. Almost every language now has built-in tools for working with regular expressions. For example [Python](https://docs.python.org/3/library/re.html#regular-expression-examples), [JavaScript](https://javascript.info/regular-expressions), [Go](https://gobyexample.com/regular-expressions), [Kotlin](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.text/-regex/), [C#](https://learn.microsoft.com/en-us/dotnet/api/system.text.regularexpressions.regex?view=net-7.0), and so on.

To practice using regular expressions, use [Regex101.com](https://regex101.com/).

## 📄 Content

  - [🔍 Basic ussage](#-basic-ussage)
  - [🚩 Flags](#-flags)
  - [🗂️ Main syntax](#️-main-syntax)
    - [Any symbol `.`](#any-symbol-)
    - [List `[]`](#list-)
    - [Excluding list `[^]`](#excluding-list-)
    - [Range `[-]`](#range--)
    - [Repeats `*`](#repeats-)
    - [At least repeat `+`](#at-least-repeat-)
    - [Optional symbol `?`](#optional-symbol-)
    - [Number of repetitions `{}`](#number-of-repetitions-)
    - [Repetition range `{,}`](#repetition-range-)
    - [Grouping `()`](#grouping-)
    - [Logical OR `|`](#logical-or-)
    - [Shielding `\`](#shielding-)
    - [Search at the beginning `^`](#search-at-the-beginning-)
    - [Search at the end `$`](#search-at-the-end-)
    - [Classes of symbols](#classes-of-symbols)
      - [Any verbal symbol `\w`](#any-verbal-symbol-w)
      - [Any non verbal symbol `\W`](#any-non-verbal-symbol-w)
      - [Any number `\d`](#any-number-d)
      - [Any character except numbers `\D`](#any-character-except-numbers-d)
      - [Space symbol `\s`](#space-symbol-s)
      - [Any character except a space `\S`](#any-character-except-a-space-s)
    - [Lookarounds](#lookarounds)
      - [Preemptive inspections `(?=)` `(?!)`](#preemptive-inspections--)
      - [Retrospective inspections `(?<=)` `(?<!)`](#retrospective-inspections--)
  - [✍️ Practice](#️-practice)
  - [📚 Additional materials](#-additional-materials)

---

## 🔍 Basic ussage

Let's take a random text as an example. Imagine that we need to find all the words `Park` in this text. This is the easiest way to use regular expressions, we just need to write the word we want between the slashes:

<p align="center"><img src="https://i.imgur.com/hq3ZOCe.png" alt="Regex example"/></p>

## 🚩 Flags

Flags affect the search result. There are only five of them:

-   `i` – allows you to ignore letter cases (there is no difference between _A_ and _a_).
-   `g` – allows you to search for all matches in the text, without it - only the first one.
-   `m` – enable multiline mode (only affects the behavior of `^` and `$`).
-   `s` – text is treated as a single line, in which case the metasymbol `.` (dot) corresponds to any single character, including the newline character.
-   `u` – unicode interpretation. Indicates that the expression may contain special patterns specific to Unicode.

## 🗂️ Main syntax

### Any symbol `.`

Any character can take the place of a dot. The number of dots can determine the length of words.

```js
/t..k/g;
```

> take look team `took` hike track `teak` time

### List `[]`

Allow you to specify a specific list of characters.

```js
/t[aoi]k/g;
```

> tek `tok` tdk `tak` `tik` tuk tyk took taoik

### Excluding list `[^]`

Allows you to exclude a certain set of characters from the search, used in conjunction with square brackets.

```js
/ba[^td]/g;
```

> `ban` `bag` bat `bas` bad

### Range `[-]`

Specifies the range from the first to the last character (inclusive) in alphabetical order.

```js
/[a-d]../g;
```

> ost hst `ast` fst `cst` `bst`

It works the same way with numbers:

```js
/201[5-9]/g;
```

> 2010 2012 `2015` `2017` `2019` 2022

### Repeats `*`

An asterisk after a character indicates that the character may be missing or match one or more times.

```js
/wo*w/g;
```

> `wow` waw wiw `woooow` wawe `ww` `woow`

### At least repeat `+`

A plus after a character indicates that the character must be present one or more times.

```js
/go+gle/g;
```

> `google` ggle `gogle` gugle g00gle `goooogle`

### Optional symbol `?`

A question mark after a character indicates that the character is optional (may be absent or occur only once).

```js
/bou?nd/g;
```

> `bond` `bound` bouuund boynd

### Number of repetitions `{}`

To specify the exact number of repetitions, you must write curly brackets with the desired number after the symbol.

```js
/bo{3}m/g;
```

> boom bom `booom` bm boooom

### Repetition range `{,}`

To specify a range of repetitions, you must write a curly bracket after the symbol with the desired range, separated by a comma.

```js
/lo{2,4}k/g;
```

> lok `look` lk `loook` `looook` loooooook

The upper bound can be omitted. For example, the entry `a{3,}` says that the character _a_ must occur at least three times.

### Grouping `()`

Brackets allow you to group any sequence of characters so that you can refer to them later using the expression `\number`, where the number is the sequence number of the grouped sequence.

```js
/(la)-\1{2}-\1{3}/g; // Group the expression "la" and then, refer to it with "\1"
```

> la-laaa-`la-lala-lalala`-lalala-la-la-la

```js
/(la)-\1-(laa)-\2/g;
```

> laa-la-laa-`la-la-laa-laa`-lalal

The `(?:)` construction is used to ignore the saving of the group.

```js
/(?:abc)-(test),\1,\1/g; // In this case the group "abc" will not be saved, so the first index points to "test".
```

> abc,test-`abc-test,test,test`-abc-test

You can give any name to groups. For this purpose the construction - `(?P<Name>...)` is used, where `Name` is the name of the group, `...` - any sequence of characters. To refer to named groups use the construction - `(?P=Name)`.

```js
/(?P<seven>7{3})-(?P=seven){2}-(?P=seven)/g;
```

> 7777-77-7777777-`777-777777-777`-777-7-7-7-7777-7

If you have trouble understanding the grouping, I suggest [watch this video](https://youtu.be/NrzFle7RD0g).

### Logical OR `|`

The vertical slash allows you to specify alternatives to search for. This is somewhat similar to using square brackets `[abc]`, but only vertical slash can handle whole words and expressions, not just individual characters.

```js
/yes|no/g;
```

> `yes`,maybe,`no`,idk,ok

### Shielding `\`

In order to use the special characters `{} [] / \ + *. $ ^ |?`, you must put a slash `\` in front of it.

```js
/\.|\?/g; // Search for dots "." or question marks "?"
```

> What now`?` What next`?` Times up`.` Wake up`.`

### Search at the beginning `^`

The carriage symbol in the regular expression indicates that the search is only performed at the beginning of lines.

```js
/^[0-9]*/gm; // Search for numbers that are at the beginning of a string
```

> `1`. Apples x10 <br> 
> `2`. Cookies x5 <br> 
> `3`. Eggs x7

### Search at the end `$`

The dollar symbol in the regular expression indicates that the search is done only by the end of the string.

```js
/com$|net$/gm;
```

> google.`com` <br>
> command <br>
> sourceforge.`net` <br>
> netflix

### Classes of symbols

There are built-in notations to make it easier to find an entire class of symbols.

#### Any verbal symbol `\w`

The two entries below are equivalent.

```js
/[a-zA-Z0-9_]/g;
```

```js
/\w/g;
```

> `some` `random` `words` `for` `example` #@$% \*%(^)`_`+# `1234`

#### Any non verbal symbol `\W`

```js
/[^a-zA-Z0-9_]/g;
```

```js
/\W/g;
```

> developer_2022`@`gmail`.`com

#### Any number `\d`

```js
/[0-9]/g;
```

```js
/\d/g;
```

> developer\_`2022`@gmail.com

#### Any character except numbers `\D`

```js
/[^0-9]/g;
```

```js
/\D/g;
```

> `developer\_`2022`@gmail.com`

#### Space symbol `\s`

Spaces also include various line break characters.

```js
/[\r\n\t\f\v ]/g;
```

```js
/\s/g;
```

#### Any character except a space `\S`

```js
/[^\r\n\t\f\v ]/g;
```

```js
/\S/g;
```

### Lookarounds

In order to find a phrase that should be before or after another phrase, position checks (lookarounds) are used.

#### Preemptive inspections `(?=)` `(?!)`

To find an expression X followed by an expression Y, use the construction `X(?=Y)`.

```js
/\d+(?=€)/g;
```

> 200$ `750`€ 100$ `330`€ 550$

To find an expression X after which there is NOT an expression Y, use the construction `X(?!Y)`.

```js
/\d{4,}(?!€)/g;
```

> This car was costed about 7000€ in `2015`

#### Retrospective inspections `(?<=)` `(?<!)`

To find an expression X preceded by an expression Y, use the construction `(?<=Y)X`.

```js
/(?<=:)\d+/g;
```

> { "id":`4`, "value":`123`, name:"test" }

To find an expression X that is NOT preceded by an expression Y, use the construction `(?<!Y)X`.

```js
/(?<!\$)\d+/g;
```

> $5 $6 $7 `2019` `2009` `1999`

## ✍️ Practice

Take some time to reinforce what you've learned. Write a simple library in your favorite programming language that will validate given strings. For example, to verify if a phone number or email is valid. Write a validator for passwords to meet the specified requirements for length, special characters, uppercase letters or digits. This will be doubly useful because you can use this library in your applications in the future.

Additionally, by searching on Google - `regex practice`, you can find many interesting assignments on the subject of regular expressions.

## 📚 Additional materials

1. 📄 [**Awesome Regex** – GitHub](https://github.com/aloisdg/awesome-regex)
1. 📺 [**Practice Regular Expressions with Regex Golf!** – YouTube](https://youtu.be/Qv_RYpREz5k)
1. 📘 [**Regular Expressions Cookbook** – J. Goyvaerts and S. Levithan, 2012](https://doc.lagout.org/programmation/Regular%20Expressions/Regular%20Expressions%20Cookbook_%20Detailed%20Solutions%20in%20Eight%20Programming%20Languages%20%282nd%20ed.%29%20%5BGoyvaerts%20%26%20Levithan%202012-09-06%5D.pdf)

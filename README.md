# ⚡Javascript Regex Quick Reference

## Up and running with regular expressions in Javascript

### ✌️Two common ways to use regular expressions

1. Constructor form -> `let regex = new RegExp(<pattern>, <flags>)`
2. Literal form -> `let regex = /<pattern>/<flags>`

### Example:

```javascript
const str = `What is what?`;

// Constructor form
let regex = new RegExp("wh");

// Literal form
let regex = /wh/;

// Execution options:
// test() method - returns true/false about a potential match

// exec() method - returns an array containing the matches alongwith the match index and original input. Running `exec` multiple times will keep finding subsequent occurrences.

regex.test(str);
// Output: true

regex.exec(str);
// Output: [ 'wh', index: 8, input: 'What is what?', groups: undefined ]
```

### 💡Additional search options

- \``g`\` flag to search globally
- \``i`\` for case insensitive search
- \``m`\` to include multi-lines in search
- for e.g. `let regex = /wh/gim;`

### Usage with `String.prototype`

- `str.match(regex);` -> returns an array of all the matches
- `str.replace(regex, str => "zzz");` -> replace matches with preferred characters
- `str.search(regex);` -> returns the index of the first match

---

### 📌Plain-text patterns

```javascript
const str = `Blake ate the cake.`;

let regex = /.ak/g;

str.match(regex); // Output: [ 'lak', 'cak' ]

str.replace(regex, str => "XX");
// Output: BXXe ate the XXe.
```

- In the above example the dot/period \``.`\` meta character preceeding the pattern \``ak`\` is trying to find a match where characters \``ak`\` is preceded by exactly one character.
- Use more dots to find \``n`\` occurrences of characters preceeding \``ak`\`
- \``.`\` meta character finds characters, digits, non-breaking spaces, dashes but not line-breaks

### 💡A note on escape sequences

- To escape a character/sequence anytime, use the special backslash \``\`\` character
- In the above example, to find the \``.`\` character, use the expression `let regex = /\.ak/g;`

---

### Finding repeating patterns using quantifiers

```javascript
const str = `ddddddd`;

// Find 4 occurrences of char `d` in a row
let regex = /dddd/g;

// Same as above, but using quantifier {4}
let regex = /d{4}/g;

// Find at least 3 matches of `d` on to infinity
let regex = /d{3,}/g;

// Find at least 3 matches of `d` up to 4
let regex = /d{3,4}/g;
```

### ✨Special patterns: ✨

- \``/<char>{0,}/g (or) /<char>*/g`\` -> zero matches to infinity
- \``/<char>{1,}/g (or) /<char>+/g`\` -> one or more matches to infinity
- \``/<char>{0,1}/g (or) /<char>?/g`\` -> zero or one match

### Example:

```javascript
// Find 0 to infinity - matches empty spaces as well
let regex = /d{0,}/g;

let regex = /d*/g; // short-hand version

// Since there's no `c` char, matches all empty spaces
let regex = /c*/g;

// Find 1 or more occurrences to infinity incl. spaces
let regex = /d+/g;

// Find 0 or 1 occurrence incl. spaces
let regex = /d?/g;

// A more real-world scenario - finding valid web addresses
const str = `http://github.com
some text
http://
https://www.github.com`;

let regex = /https{0,1}:\/\/.{1,}/g;

let regex = /https?:\/\/.+/g; // short-hand version

// Output: [ 'http://github.com', 'https://www.github.com' ]
```

---

### 📌Character classes

```javascript
const str = `hot lot Dot !ot 0ot`;

// Find occurrences of char `ot` preceeded by either `h` or `l`
let regex = /[hl]ot/g;
// Output: XX XX Dot !ot 0ot

// Find occurrences of char `ot` not preceeded by `h` or `l`
let regex = /[^hl]ot/g;
// Output: hot lot XX XX XX

// Character ranges
let regex = /[a-z]ot/g;
// Output: XX XX Dot !ot 0ot

let regex = /[a-zA-Z]/g;
// Output: XXXXXX XXXXXX XXXXXX !XXXX 0XXXX

let regex = /[a-kA-Z]/g;
// Output: XXot lot XXot !ot 0ot

let regex = /[^a-kA-Z]ot/g;
// Output: hot XX Dot XX XX

let regex = /[a-zA-Z0-9]ot/g;
// Output: XX XX XX !ot XX

let regex = /[a-zA-Z0-9!]ot/g;
// Output: XX XX XX XX XX
```

### ✨Short-hand versions ✨

```javascript
const str = `Blue #220 10.5%`;

// Match all characters except symbols, spaces, digits & unicode characters
let regex = /[a-zA-Z0-9]/g; // (or) use short-hand /\w/g;
// Output: XXXXXXXX #XXXXXX XXXX.XX%

// Match all digits
let regex = /\d/g;
// Output: Blue #XXXXXX XXXX.XX%

// Match all white-space
let regex = /\s/g;
// Output: BlueXX#220XX10.5%

// --- Negation ---
// Match only white-space and symbols
let regex = /[^\w]/g; // (or) use short-hand /\W/g;
// Output: BlueXXXX220XX10XX5XX

// Match everything that's not a digit
let regex = /[^\d]/g; // (or) use short-hand /\D/g;
// Output: XXXXXXXXXXXX220XX10XX5XX

// Match everything that's not white-spaces
let regex = /[^\s]/g; // (or) use short-hand /\S/g;
// Output: XXXXXXXX XXXXXXXX XXXXXXXXXX
```

---

### 📌Capture groups

Search for matches by specifying terms within parens for e.g. \``/<primary text>(secondary|tertiary)/g;`\`

### Example:

```javascript
const str = `hello
helloworld
hellothere
hellohi`;

// Search for matches where `hello` is followed by either `world` or `hi`
let regex = /hello(world|hi)/g;

/* Output:
    hello 
    XX 
    hellothere 
    XX
*/

// Same as above, but search for 0 or 1 occurrence using quantifier
let regex = /hello(world|hi)?/g;

/* Output:
    XX 
    XX 
    XXthere 
    XX 
*/

// A more real-world example - capturing area codes from phone numbers:
const str = `800-234-4321
(444) 342-7861
7778976754`;

// $1 in the following `console.log` line is getting a reference to the capture group
let regex = /\(?(\d{3})\)?[\s-]?\d{3}[\s-]?\d{4}/g;

// If you don't need to store that reference, use `?:` inside the capture group. for e.g.
let regex = /\(?(?:\d{3})\)?[\s-]?\d{3}[\s-]?\d{4}/g;

console.log(str.replace(regex, "area code: $1"));

/* Output:
    area code: 800
    area code: 444
    area code: 777
*/
```

---

### 📌Lookaheads

Similar to capture groups, but if you want to exclude the capture group result.

### Example:

```javascript
const str = `hello
helloworld
hellothere
hellohi`;

// Positive lookahead - i.e. find matches of `hello` followed by either `world` or `hi`
let regex = /hello(?=world|hi)/g;

/* Output:
    hello 
    XXworld 
    hellothere 
    XXhi 
*/

// Negative lookahead - i.e. find matches of `hello` not followed by either `world` or `hi`
let regex = /hello(?!world|hi)/g;

/* Output:
    XX 
    helloworld 
    XXthere 
    hellohi
*/
```

---

### 📌Word boundaries

Search for occurrences by specifying starting and ending boundaries using \``\b`\`

### Example:

```javascript
const str = `This bus is isomorphic. It truly is.`;

// Find occurrences that begins and ends with `is`
let regex = /\bis\b/g;

// Output: This bus XX isomorphic. It truly XX.

// Negate the search using `\B` - i.e. find occurrences of `is` which is not the beginning
let regex = /\Bis/g;

// Output: ThXX bus is isomorphic. It truly is.

// You can use the `\B` at the end as well - i.e. find occurrences of `is` which is not the end
let regex = /is\B/g;

// Output: This bus is XXomorphic. It truly is.

// You can also combine the negation both the start and end of the search. For e.g. find occurrences of `is` which is not at the beginning or at the end (more like middle)
const str = `My wallet is missing.`;

let regex = /\Bis\B/g;

// Output: My wallet is mXXsing.
```

---

### 📌Backreferences

Match the same string twice using backreferences.

```javascript
const str = `He went to to the mall.`;

// Match the first `to` which is followed by a space and a second `to`

// `\1` is the backreference of the first capture group `(to)`
// `(?=)` -> is the positive lookahead to filter to just the first `to` occurrence
let regex = /(to)\s?(?=\1)/g;

// Output: He went XXto the mall.

// Applying to real-world scenarios:

// 1. Clean-up duplicate characters/words
str.replace(regex, "");

// Output: He went to the mall.

// 2. Grab inner content from html tags
const str = `<div>Hello</div><p>World</p>`;

// `(\w+)` -> capture any numbers of characters (cap. grp #1)
// `(.*)` -> capture any content
// `\1` -> backreference for cap. grp #1
let regex = /<(\w+)>(.*)<\/\1>/g;

/* Output:
    Hello
    World
*/
```

---

### 📌Line Anchors

Capture beginning and end of lines using line anchors.

### Example:

```javascript
const str = `11/2/18
11/18/17
09/12/18
11-10-2018`;

// Find occurrences of `11` at the beginning of line that also ends with `18` - in other words, pick all the dates in Nov 2018

// `^` -> beginning indicator vs negation in capture groups
// `m` -> mult-line flag. Looks for beginning of line in multi-lines as well
let regex = /^11.+18$/gm;

/* Output:
    XX 
    11/18/17 
    09/12/18 
    XX 
*/
```

---

### 🔥Simple regex highlighter

For the purpose of evaluating various regex patterns, use the following simple regex highlighter function in a sample .html file.

```javascript
// Regex highlighter function
const output = (str, regex, el) => (
    el.innerHTML = str.replace(regex, str =>
        `<span>{str}</span>`
    );
);

// Regex test condition
const str = `This is pretty cool!`;

let regex = /is/g;

// Call the output fn to highlight pattern matches
output(str, regex, document.querySelector('pre'));
```

And in the body of the file use a \``<pre>`\` tag to display the output with some styling to highlight the matches. For e.g.

```css
pre {
  font-size: 20px;
}

span {
  background-color: dodgerblue;
  border: 1px solid black;
  padding: 2px;
  color: white;
}
```

---

🙏Last but not least, this quick reference is an inspiration from [Joe Maddalone's](https://egghead.io/instructors/joe-maddalone) [egghead Regex series](https://egghead.io/courses/regex-in-javascript). His video series is a great resource to get up to speed on regex in Javascript.

# Regular Expression to "validate" a Bitcoin address

The [specs](https://en.bitcoin.it/wiki/Address) of a valid Bitcoin address say:

> A Bitcoin address, or simply address, is an identifier of 26-35 alphanumeric characters, beginning with the number 1 or 3, that represents a possible destination for a Bitcoin payment. 

> Most Bitcoin addresses are 34 characters. They consist of random digits and uppercase and lowercase letters, with the exception that the uppercase letter "O", uppercase letter "I", lowercase letter "l", and the number "0" are never used to prevent visual ambiguity. Some Bitcoin addresses can be shorter than 34 characters (as few as 26).

The first requisite of the RegEx will be making sure the length is between 26 and 35 characters.

`.{26,35}`

Then lets check for digits, and lowercase and uppercase letters.

`[a-zA-Z0-9]{26,35}`

But to prevent visual ambiguity we need to remove O, I, l and 0.

`[a-km-zA-HJ-NP-Z1-9]{26,35}`

We then need to make sure it start with either 1 or 3.

`[13][a-km-zA-HJ-NP-Z1-9]{25,34}`

Note how the range had to be shifted to take the first character into consideration.

Lastly we need to make sure that the text to pass contains **only** the Bitcoin address, so not stuff like: something<bitcoin address>something else.

We do that with the start `^` and end `$` anchors.

`^[13][a-km-zA-HJ-NP-Z1-9]{25,34}$`

That's it! 

Looking at [SO](http://stackoverflow.com/questions/21683680/regex-to-match-bitcoin-addresses) I've seen several people suggest using the `[^OIl0]`, but that's incorrect, because it says _anything that's not O, I, l or 0_, and therefore would match stuff like * or &.

All that being said in the spec itself they suggest to **avoid** using a RegEx or a length and content check to validate a Bitcoin address string, and points to [this thread](https://bitcointalk.org/index.php?topic=1026.0), where they show a validator that _"does a "deep" validation, checking that the checksum built into every bitcoin address matches the address."_

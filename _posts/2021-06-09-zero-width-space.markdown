---
layout: page
title:  "Zero Width Space"
date:   2021-06-08 12:00:00 -0500
categories: jekyll update
---
I was working on an open source project recently and came across a couple issues that contained `[ZWSP]` 
displayed in the test files. The `[ZWSP]` was only displayed in the IntelliJ IDE. Eclipse, Atom and 
vim showed no issues with the code. The `[ZWSP]` stands for zero-width space. It was breaking the test file
and there were multiple issues in the github repository documenting it.

I found nine zero-width space characters in the [unicode characters table](https://unicode-table.com/en/)
and there may be more.

Character | Unicode | Hex
--- | --- | ---
Zero-width space | U+200B | 0xe2 0x80 0x8b
Zero-width non-joiner | U+200C | 0xe2 0x80 0x8c
Zero-width joiner | U+200D | 0xe2 0x80 0x8d
Left-To-Right Mark | U+200E | 0xe2 0x80 0x8e
Right-To-Left Mark | U+200F | 0xe2 0x80 0x8f
Left-To-Right Embedding	| U+202A | 0xe2 0x80 0xaa
Right-To-Left Embedding | U+202B | 0xe2 0x80 0xab
Word joiner | U+2060 | 0xe2 0x81 0xa0
Zero-width no-break space | U+FEFF | 0xef 0xbb 0xbf

Zero-width characters are non-printing characters that are not displayed by most applications. They are Unicode 
characters, typically used to mark possible line breaks or join/separate characters in writing systems that use ligatures.
They are not removed if the text is formatted, copied or pasted. It’s really hard to detect them without special tools.
It most likely happened from copy and pasting code from the [AssertJ API](https://joel-costigliola.github.io/assertj/core/api/org/assertj/core/api/AbstractIterableAssert.html#containsExactlyInAnyOrder(ELEMENT...)). 

I used a tool called [HexView](https://plugins.jetbrains.com/plugin/10-hexview) to view the hexidecimal
format of the file and inspect for it.

![hexview](/assets/hexview/contains-zero-width-space.png)

HexView allowed me to inspect the code using the column on the right. As you can see, it is not the IDE or editor,
the value is in the actual code. I removed the hex value<br> `e2 80 8b`<br>
<br>
![hexview](/assets/hexview/removed-zero-width-space.png)

It was interesting to note that IntelliJ was the only IDE or editor to pick up the zwsp. Users that did
not use IntelliJ experienced no issues.

The ability to use invisible characters with no width has serious security implications. Users could create 
usernames, email addresses and websites that look identical to a human, but different to computers. 
Luckily, zero-width spaces are prohibited in email addresses and domain names. See 
[homograph attack](https://en.wikipedia.org/wiki/IDN_homograph_attack#Non-displayable_characters) for
an example. Zero-width spaces can hide an encoded binary message in a piece of text. 
`String -> byte -> bit -> 1 -> ZWSP and 0 -> ZWNJ`. They can be added to sensitive documents, hidden in 
different places for each of the recipients, creating a unique finger print to identify the source. 

This reminded me of hidden dots and values produced by printers. Surely, there is a tool to inspect for these. 
Insert [DEDA](https://github.com/dfd-tud/deda), created from the researchers of 
[Forensic Analysis and Anonymisation of Printed Documents](/assets/hexview/Forensic-Analysis-and-Anonymisation-of-Printed-Documents.pdf).
I'm not sure how valuable or effective it is but I will dig into it.

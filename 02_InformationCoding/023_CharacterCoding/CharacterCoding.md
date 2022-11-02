---
title: "Character Coding"
subtitle: "Introduction to Computation for the Social Sciences"
author: "David Garcia"
output: html_document
---

### ASCII code

 American Standard Code for information interchange
• Standard since 1963, still in use today
• initially 7 bit code for tele printers, all codes of US keyboard in one byte
• 128 characters, 33 control characters, 95 printable characters
• Missing characters for other keyboards

first codepage

Extension of ASCII-Codes to 8 bit (1 byte)
• CP850, CP437, IS0-8859-1 to ISO-8859-15 with characters for other languages

### Unicode

Addresses problems with ASCII and code pages
- not all possible characters can be represented, e.g., kanji, chinese
- meaning of a code is dependent on code page, therefore frequent
confusion
Development of standardized Unicode
- simultaneous representation of all possible characters
- fixed translation table
- version 1.0 in 1991 with 65 536 different characters
- 2022 version (15.0) has 149,186 characters

 Division of full code space into 17 planes each with 65 536 characters
• specification using plane U+ and at least 4 hexadecimal numbers
• one code correspond to exactly one character
• e.g.: U+00DF = ß
• 3 Bytes per code
• Plane 0, Basic Multilingual Plane (BMP)
- currently used character sets, punctuation marks, control characters etc.
- highly fragmented and mostly occupied
• Plane 2, Supplementary Ideographic Plane (SIP)
-
 Chinese, Japanese and Korean characters
 
 
 3 bytes are necessary for all possible characters
- wasting memory in many geographical regions, e.g. Europe
• Definition of Unicode Transformation Formats (UTF) as solution
• UTF-16
- 2 byte per characters but still wastes memory
- other areas are covered by combination of UTF-16 characters
•
 UTF-8
- 1 byte per character, covers most important Western characters
- Compatible with ASCII Codes
- additional characters can be covered by a combination of up to three
UTF-8 characters


Character encoding is a usual source of problems with social science data
Try to control coding in documentation and keep it general (e.g. UTF-8)
>>> u = chr(40960) + u'abcd' + chr(1972)
>>> u.encode('utf-8')
'\xea\x80\x80abcd\xde\xb4'
>>> u.encode('ascii')
Traceback (most recent call last):
File "<stdin>", line 1, in ?
UnicodeEncodeError: 'ascii' codec can't encode character '\ua000' in position 0: ordinal not in range(128)
>>> u.encode('ascii', 'ignore')
'abcd'
>>> u.encode('ascii', 'replace')
'?abcd?'
>>> u.encode('ascii', 'xmlcharrefreplace')
'&#40960;abcd&#1972;'

 Started as emoticons (e.g. :)) in the 1980s
• Included in the early 2000s in chat software, standard unicode since v6.0 (2010)
•2015: Oxford Dictionary word of the
year is the Face with Tears of Joy
emoji (😂)
•Visualization depends on software
similarly to how fonts change
alphabetic characters
•Currently allow skin color or gender
modifiers by concatenating emoji
characters

### Emoji Research

A global analysis of emoji usage. Ljubešić & Fišer.
Proceedings of the 10th web as corpus workshop (2016)

Using millions of emoji occurrences to learn any-domain representations
for detecting sentiment, emotion and sarcasm. Felbo et al. Proceedings
of the 2017 Conference on Empirical Methods in Natural Language
Processing (2017)

🤌 and its meanings: An emojical study. Anna Di Natale, Emma Fraxanet, Max Pellert, Jana Lasser, Hannah
Metzler, Alina Herderich, Segun Aroyehun, Apeksha Shetty, David Garcia. Holiday Paper Series 2021
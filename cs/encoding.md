# Encoding

## Introduction

*Encoding* is the process of converting data from one form to another. There are many types of encoding, such as character encoding, binary-to-text encoding, and others. This note will go through some of the most common types of encoding and how they are used. *Decoding* is the reverse process of encoding, which converts encoded data back to its original form.

## Character encoding

Character encoding is the process of converting characters into a binary format that can be stored or transmitted over a network. The characters are usually represented as numbers, which are then converted into binary data because numbers are easily converted into binary.

There are many different character encoding schemes, such as ASCII, Unicode, and UTF-8. Each encoding scheme has its own set of rules for converting characters into binary data.

A *character set* is a collection of characters that are used to represent text in a computer system. A character set can include letters, numbers, punctuation marks, and other symbols. Each character in a character set is assigned a unique code point, which is a numerical value that represents the character.

An *encoding scheme* is a set of rules that defines *how* characters are converted into binary data. An encoding scheme maps each character in a character set to a binary value, which can be stored or transmitted over a network.

### ASCII

ASCII (American Standard Code for Information Interchange) is one of the oldest character encoding schemes which was developed in the 1960s for standardizing the representation of text in computers, namely teleprinters and teletypes at the time. ASCII is also a character set.

ASCII uses 7 bits to represent characters, which allows for 128 (0 to 127) different characters to be encoded. The first 32 characters (0 to 31) are control characters, such as newline, tab, and carriage return. The remaining 96 characters (32 to 127) are printable characters, such as letters, numbers, and symbols.

ASCII became the de facto standard of character encoding in the English-speaking world in the early days of computing, but it had some limitations. For example, it only supports the English alphabet and does not include characters from other languages or special symbols.

<details>
  <summary><i>Click to show an ASCII table</i></summary>

  ```markdown
  | Dec | Char | Dec | Char | Dec | Char | Dec | Char |
  |-----|------|-----|------|-----|------|-----|------|
  | 000 | NUL  | 032 | Space| 064 | @    | 096 | `    |
  | 001 | SOH  | 033 | !    | 065 | A    | 097 | a    |
  | 002 | STX  | 034 | "    | 066 | B    | 098 | b    |
  | 003 | ETX  | 035 | #    | 067 | C    | 099 | c    |
  | 004 | EOT  | 036 | $    | 068 | D    | 100 | d    |
  | 005 | ENQ  | 037 | %    | 069 | E    | 101 | e    |
  | 006 | ACK  | 038 | &    | 070 | F    | 102 | f    |
  | 007 | BEL  | 039 | '    | 071 | G    | 103 | g    |
  | 008 | BS   | 040 | (    | 072 | H    | 104 | h    |
  | 009 | TAB  | 041 | )    | 073 | I    | 105 | i    |
  | 010 | LF   | 042 | *    | 074 | J    | 106 | j    |
  | 011 | VT   | 043 | +    | 075 | K    | 107 | k    |
  | 012 | FF   | 044 | ,    | 076 | L    | 108 | l    |
  | 013 | CR   | 045 | -    | 077 | M    | 109 | m    |
  | 014 | SO   | 046 | .    | 078 | N    | 110 | n    |
  | 015 | SI   | 047 | /    | 079 | O    | 111 | o    |
  | 016 | DLE  | 048 | 0    | 080 | P    | 112 | p    |
  | 017 | DC1  | 049 | 1    | 081 | Q    | 113 | q    |
  | 018 | DC2  | 050 | 2    | 082 | R    | 114 | r    |
  | 019 | DC3  | 051 | 3    | 083 | S    | 115 | s    |
  | 020 | DC4  | 052 | 4    | 084 | T    | 116 | t    |
  | 021 | NAK  | 053 | 5    | 085 | U    | 117 | u    |
  | 022 | SYN  | 054 | 6    | 086 | V    | 118 | v    |
  | 023 | ETB  | 055 | 7    | 087 | W    | 119 | w    |
  | 024 | CAN  | 056 | 8    | 088 | X    | 120 | x    |
  | 025 | EM   | 057 | 9    | 089 | Y    | 121 | y    |
  | 026 | SUB  | 058 | :    | 090 | Z    | 122 | z    |
  | 027 | ESC  | 059 | ;    | 091 | [    | 123 | {    |
  | 028 | FS   | 060 | <    | 092 | \    | 124 | |    |
  | 029 | GS   | 061 | =    | 093 | ]    | 125 | }    |
  | 030 | RS   | 062 | >    | 094 | ^    | 126 | ~    |
  | 031 | US   | 063 | ?    | 095 | _    | 127 | DEL  |
  ```
</details>

### Unicode

Unicode is a character encoding standard that was developed to address the limitations of ASCII by providing a universal character set that can represent characters from all languages and scripts in the world. Unicode uses hexadecimal system (16 bits) to represent characters. Unicode is designed to be extensible, meaning that new characters can be added to the standard as needed.

Unicode is a superset of ASCII, which means that the first 128 characters of Unicode are the same as ASCII. This makes it backward compatible with ASCII, so existing ASCII text can be converted to Unicode without any loss of information.

Unicode standard is maintained by the Unicode Consortium, a non-profit organization that develops and promotes the Unicode standard. The Unicode Consortium also maintains the Unicode Character Database, which contains information about the properties of each character in the Unicode standard.

#### UTF-8

There are several encoding schemes for Unicode, such as UTF-8, UTF-16, and UTF-32. UTF-8 is the most widely used encoding scheme for Unicode because it is backward compatible with ASCII and can represent all Unicode characters.

UTF-8 (Unicode Transformation Format 8-bit) is a variable-length encoding scheme that can represent any Unicode character using 1 to 4 bytes. Variable-length encoding means that different characters can be encoded using a different number of bytes, depending on the character's code point. This allows UTF-8 to be more efficient in terms of storage and transmission, as it uses fewer bytes to encode characters that are commonly used. Using 4 bytes to encode all characters would be inefficient because most characters are in the ASCII range and can be encoded using 1 byte.

UTF-8 also includes a mechanism for being backward compatible with systems that interpret a sequence of eight zero bits as a null character. This is done by using a header bits pattern that is not used for encoding characters.

The first byte of a UTF-8 sequence has a header which is used to determine the number of bytes used to encode a character. For example, if the first byte starts with `0`, it means the character is using only one byte, the same byte that inclues the header and leaving seven bits to be used for the encoding. This allows the encoder to easily include the 7-bit ASCII character set in the first byte. If the first byte starts with `110`, it means the character is using two bytes, and so on. 

The following bytes in a UTF-8 sequence start with `10`, which indicates that they are continuation bytes. This allows the decoder to easily identify the start of a new character in a sequence of bytes.

 From 2 to 4 bytes, there's 8 - (b +1) + 6 * (b - 1) bits available for encoding the character, where `b` is the number of bytes used to encode the character. At most, 21 bits are used to encode a character which equals to 2,097,152 different characters. This is enough to encode all Unicode characters which are 1,114,112 characters.

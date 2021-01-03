# Tecendil Mode File Format

The Tengwar script is used to transcribe several languages by following
transcription rules called ''mode''.

A Tecendil mode file is a "JSON with comments" file.

This format is a standard JSON file, with the addition that `//` and `/* */`
style comments are allowed in the file. It has a file extension of`.jsonc` .

The mode file contains the following properties:

- `name`: the name of the mode, typically including the language for which it is
  a mode of, e.g., "English Phonemic"
- `languageCode`: the ISO 639-2/3 code of the language. Note that the language
  code for sindarin is `sjn` and quenya is `qya1
- `normalizeVowels`: if `true`, vowels with a macron or grave accent and
  double-vowels are replaced with a circumflexed vowel. Vowels with a diaeresis
  and converted to a vowel with no diacritic. While this might be useful for
  some languages, if diacritics are meaningful in your language (french,
  spanish, etc...) you'll want this option set to `false`
- `tehtarFollow`: if `true`, tehtar are expected to be on the following tengwa:
  "cat" would be "c" + "t with a above".
- `preprocess`: a list of conversions to do before processing the text. This can
  be useful to normalize some notations. Each key is a string that will be
  interpreted as a regular expression, and the value is the value it will be
  replaced with. For example:

```
		"ĉ":    "ch"
```

would replace all circumflex-C with "ch". The entry:

```
		"^r":       "rr",
```

would replace all "r" at the begining of a word with 'rr'

- `words` is an optional list of words that will be replaced directly. The key
  is the word itself, and the value is the replacement in Normalized Encoding
  (see below)
- `map`, finally, maps the input characters to tengwars. For each entry, the key
  is a string of up to 7 characters, with `^` representing the beginning of a
  word and `$` the end. The value is a Normalized Encoding, as explained below.

Note that punctuations and numbers are handled by default and don't need to be
specified in the mode file.

# Normalized Encoding of Tengwar

## Purpose

Fonts to display Tengwar use different encodings.

The most common is the so-called "Dan Smith" [1] encoding, which arbitrarily
maps latin letters to Tengwar. There are several variants of this encoding in
use.

A Unicode encoding [2] has also been proposed, but it has not been formalized
yet and there several variants of it in use.

## Tengwar Modes

There are three family of Tengwar modes. Regardless of the mode used, the
encoding remains the same.

- **Beleriand-type Modes**: vowels and consonants are represented by separate
  signs (tengwar).

- **Quenya-type Modes**: vowels are represented as diacritic signs (tehtar) on
  the consonants that **precedes** them.

- **Sindarin-type Modes**: (also used for English) vowels are represented as
  diacritic signs (tehtar) of the consonants that **follows** them. Example: nen
  (water) is written {nuumen}[e]{nuumen}

The representation is therefore consistent, regardless of the mode used: a
tengwar letter, followed by zero or more diacritics modifying it. This also
means that the order of the signs may not match the phonetic order since the
sign for a vowel may come after a consonant, although it would be pronounced
before it, for example in Sindarin mode.

## Examples

- nelde (Quenya: three) `{nuumen}[e]{alda}[e]`
- neled (Sindarin: three) `{nuumen}{lambe}[e]{ando}[e]`
- river (english) `{roomen}{ampa}[i]{oore}[e]`

## Syntax

```
WORD = (TENGWA, TEHTA*)+ ;

TEHTA = "{", TEHTA-NAME, "}" ;

TENGWA = "[", TENGWA-NAME ,"]" ;
```

### Tengwa Name

The symbol TENGWA-NAME can be one of the following, taken from the proposed
Unicode encoding. Note that they are not case sensitive, so "TINCO", "tinco" and
"Tinco" are equivalent.

- TINCO
- PARMA
- CALMA
- QUESSE
- ANDO
- UMBAR
- ANGWA
- UNGWE
- THUULE
- FORMEN
- AHA aka HARMA
- HWESTA
- ANTO
- AMPA
- ANCA
- UNQUE
- NUUMEN
- MALTA
- NOLDO
- NWALME
- OORE
- VALA
- ANNA
- VILYA
- EXTENDED-TINCO
- EXTENDED-PARMA
- EXTENDED-CALMA
- EXTENDED-QUESSE
- EXTENDED-ANDO
- EXTENDED-UMBAR
- EXTENDED-ANGA
- EXTENDED-UNGUE
- ROOMEN
- ARDA
- LAMBE
- ALDA
- SILME
- SILME-NUQUERNA
- AARE
- AARE-NUQUERNA
- HYARMEN
- HWESTA-SINDARINWA - wh
- YANTA
- UURE
- LONG-CARRIER
- HALLA
- SHORT-CARRIER
- STEMLESS-VALA used as [w] in some english modes
- A-TENGWA = STEMLESS-ANNA used as "a" vowel in the mode of Beleriand
- OPEN-ANNA
- REVERSED-PARMA
- REVERSED-FORMEN
- TALL-STEMLESS-VALA
- MH
- REVERSED-OSSE

- TELCO
- AARA

### Tehta Name

A tengwa letter may be accompanied of zero or more tehta. The order of the tehta

The symbol TEHTA-NAME is one of the following.

- A = THREE-DOTS-ABOVE
- A-BELOW = THREE-DOTS-BELOW
- E = ACUTE = andaith, long mark
- NUNTICSE = dot below, silent-e
- EE = DOUBLE-ACUTE
- EE-BELOW = DOUBLE-ACUTE-BELOW
- I = AMATICSE = dot above
- II = TWO-DOTS-ABOVE
- II-BELOW = TWO-DOTS-BELOW
- O = RIGHT-CURL
- OO = DOUBLE-RIGHT-CURL
- O-BELOW = RIGHT-CURL-BELOW
- U = LEFT-CURL
- UU = DOUBLE-LEFT-CURL
- U-BELOW = LEFT-CURL-BELOW
- E-BELOW = ACUTE-BELOW

- BAR-ABOVE = NASALIZER
- BAR-BELOW = DOUBLER
- TWIST = TILDE
- BREVE
- GRAVE
- YANTA-ABOVE = CIRCUMFLEX?
- THREE-INVERTED-DOTS-ABOVE
- LONG-CARRIER-BELOW

- LEFT-HOOK for use with QUESSE and similar shapes
- RIGHT-HOOK
- CURLY-HOOK
- UP-HOOK

?? CURL-BELOW = RIGHT-CURL-BELOW ?? ACCENT-BELOW = LEFT-CURL-BELOW

### Numbers

- ZERO
- ONE
- TWO
- THREE
- FOUR
- FIVE
- SIX
- SEVEN
- EIGHT
- NINE
- TEN
- ELEVEN
- TWELVE
- RING-BELOW = OPEN-DOT-BELOW? = LEAST-SIGNIFICANT-MARK

### Punctuation

- PUSTA
- DOUBLE-PUSTA
- TRIPLE-PUSTA
- QUADRUPLE-PUSTA
- QUINTUPLE-PUSTA
- EXCLAMATION-MARK
- QUESTION-MARK
- PARENTHESIS-MARK
- SECTION-MARK
- DOUBLE-SECTION-MARK

# References

[1] Dan Smith Encoding [2]
http://www.evertype.com/standards/iso10646/pdf/tengwar.pdf [3] Tengwar and
LaTex:
http://get-software.net/macros/latex/contrib/tengwarscript/tengwarscript.pdf [4]
Telcontar Unicode encoding:
http://freetengwar.sourceforge.net/html-files/tengtelc-discussion-008.pdf

# Tecendil Mode Files

The Tengwar script is used to transcribe several languages by following
transcription rules called ''mode''.

A Tecendil mode file is a "JSON with comments" file.

This format is a standard JSON file, with the addition that `//` and `/* */`
style comments are allowed in the file. It has a file extension of`.jsonc` .

## Creating a New Mode File

To get you started, it's a good idea to have a look at some of the existing mode
files. The simplest are `beleriand.jsonc` and `sindarin.json`.

The mode file will be describing how groupings of letters should be transcribed.
These are described as "rules" in the `map` table of the mode file. The left
hand-side of the rule indicate when the rule should apply, and the right hand
side what the applicable transcription is.

There are 'built-in' rules for consonants and vowels that map single consonants
and vowels to some common mapping, i.e. "p" to "{parma}", "a" to "\[a\]".

You may want to start with the default rules, and see what you need to change.
You want to have as few rules as possible, both for efficiency, and because it's
much easier to change/fix a problem in a mode file that has fewer rules. The
automated mode file validation step in Tecendil will try to detect and flag
unnecessary rules. However, this validation step can give both false positives
and false negatives, so they need to be reviewed carefully.

The left-hand side of a rule indicate a target sequence of characters that will
be matched. Targets that are more specific (longer) takes precedence over
shorter targets, so you can have a rule for both "ai" and "aia" for example.

The targets are not case sensitive, that is a rule with a target of "q" will
apply to "Q" as well.

You can indicate that a rule should apply only if the pattern is at the begining
of a word by preceding the target with a "^" or at the end of a word by ending
the target with "$".

So the target "^t" would apply if a word begins with "t" and a target "s$" would
apply if a word ends with "s".

The right hand side of a rule indicate which tengwa (in `{...}`) and which
diacritics (in `[...]`) to replace the target with.

The Tecendil engine will apply these rules to the letters in a word to derive
the transcription. When necessary, short-carriers are inserted automatically and
when applicable, vowels/diacritics are combined. In some cases, the combinations
may be undesirable. You can prevent vowels to be combined by using an "empty"
tengwar: `{}`.

If your target language has digraphs or diacritics, you can use the `preprocess`
table to specify how to handle them. For example, in German you could indicate
that the `ß` letter maps to `ss` or that `ö` maps to `oe`.

If some words are exceptions to the rules, you can provide a custom
transcription for them in the `words` table. Some languages may not need any.
English has many exceptions due to the nature of english orthography.

## Mode File Structure

A mode file contains the following properties:

- `name`: the name of the mode, typically including the language for which it is
  a mode of, e.g., "English Phonemic"
- `languageCode`: the ISO 639-2/3 code of the language. Note that the language
  code for sindarin is `sjn` and quenya is `qya1`
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
  diacritic signs (tehtar) of the consonants that **follows** them. Example:
  _nen_ (water) is written `{nuumen}[e]{nuumen}`

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

- `{tinco}` = TINCO
- `{tinco-extended}
- `{parma}` = PARMA
- `{parma-extended}`
- `{calma}` = CALMA
- `{calma-extended}`
- `{quesse}` = QUESSE
- `{quesse-extended}`
- `{ando}` = ANDO
- `{ando-extended}`
- `{umbar}` = UMBAR
- `{umbar-extended}`
- `{angwa}` = ANGWA
- `{anga-extended}`
- `{ungwe}` = UNGWE
- `{ungwe-extended}`
- `{suule}` = THUULE
- `{formen}` = FORMEN
- `{formen-extended}`
- `{aha}` AHA aka HARMA
- `{hwesta}` HWESTA
- `{hwesta-sindarinwa}` HWESTA-SINDARINWA - wh
- `{anto}` ANTO
- `{ampa}` AMPA
- `{anca}` ANCA
- `{unque}` UNQUE
- `{nuumen}` NUUMEN
- `{malta}` MALTA
- `{noldo}` NOLDO
- `{nwalme}` NWALME
- `{oore}` OORE
- `{vala}` VALA
- `{anna}` ANNA
- `{vilya}` VILYA
- `{roomen}` ROOMEN
- `{arda}` ARDA
- `{lambe}` LAMBE
- `{alda}` ALDA
- `{silme}` SILME
- `{silme-nuquerna}` SILME-NUQUERNA
- AARE
- AARE-NUQUERNA
- `{hyarmen}` HYARMEN
- `{yanta}` YANTA
- `{uure}` UURE
- `{long-carrier}` LONG-CARRIER
- `{halla}` HALLA
- `{osse}`
- `{short-carrier}` SHORT-CARRIER
- STEMLESS-VALA used as \[w\] in some english modes
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

A tengwa letter may be accompanied of zero or more tehta.

The symbol TEHTA-NAME is one of the following.

- `[a]` = THREE-DOTS-ABOVE
- `[a-below]` = THREE-DOTS-BELOW
- `[reverse-triple-dot]`
- `[reverse-triple-dot-below]`
- `[e]` = ACUTE = andaith, long mark
- `[dot-below]` = NUNTICSE = dot below, silent-e
- `[double-e]` = EE = DOUBLE-ACUTE
- `[double-acute-below]` = EE-BELOW = DOUBLE-ACUTE-BELOW
- `[i]` = I = AMATICSE = dot above
- `[double-dot]` = II = TWO-DOTS-ABOVE
- `[double-dot-below]` = II-BELOW = TWO-DOTS-BELOW
- `[o]` = O = RIGHT-CURL
- `[double-o]` = OO = DOUBLE-RIGHT-CURL
- `[curl-below]` = O-BELOW = RIGHT-CURL-BELOW
- `[u]` = U = LEFT-CURL
- `[double-u]` = UU = DOUBLE-LEFT-CURL
- `[accent-below]` = U-BELOW = LEFT-CURL-BELOW
- E-BELOW = ACUTE-BELOW

- `[bar-above]` = BAR-ABOVE = NASALIZER
- `[bar-below]` = BAR-BELOW = DOUBLER
- `[tilde-above]` = TWIST = TILDE
- `[tilde-high]`
- `[tilde-below]`
- `[chevron]` = BREVE
- `[grave]` = GRAVE
- YANTA-ABOVE = CIRCUMFLEX?
- THREE-INVERTED-DOTS-ABOVE
- LONG-CARRIER-BELOW

- `[x-curl]` = LEFT-HOOK for use with QUESSE and similar shapes
- `[left-follow-silme]` = RIGHT-HOOK
- `[swarsh-curl]` = CURLY-HOOK
- `[upward-curl]` = UP-HOOK

?? CURL-BELOW = RIGHT-CURL-BELOW ?? ACCENT-BELOW = LEFT-CURL-BELOW

### Numbers

- `{0}` ZERO
- `{1} ONE
- `{2}` TWO
- `{3}` THREE
- `{4}` FOUR
- `{5}` FIVE
- `{6}` SIX
- `{7}` SEVEN
- `{8}` EIGHT
- `{9}` NINE
- `{10}` TEN
- `{11}` ELEVEN
- `{12}` TWELVE
- `[ring-below]` = RING-BELOW = OPEN-DOT-BELOW? = LEAST-SIGNIFICANT-MARK

### Punctuation

- `{comma}` PUSTA
- `{full-stop}` DOUBLE-PUSTA
- `{colon}` TRIPLE-PUSTA
- QUADRUPLE-PUSTA
- QUINTUPLE-PUSTA
- `{exclamation-mark}` EXCLAMATION-MARK
- `{question-mark}` QUESTION-MARK
- `{left-parenthesis}` and `{right-parenthesis}` PARENTHESIS-MARK
- SECTION-MARK
- DOUBLE-SECTION-MARK

# References

[1] Dan Smith Encoding

[2] http://www.evertype.com/standards/iso10646/pdf/tengwar.pdf

[3] Tengwar and LaTex:
http://get-software.net/macros/latex/contrib/tengwarscript/tengwarscript.pdf

[4] Telcontar Unicode encoding:
http://freetengwar.sourceforge.net/html-files/tengtelc-discussion-008.pdf

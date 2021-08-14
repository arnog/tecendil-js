# Normalized Encoding of Tengwar

## Purpose

Fonts to display Tengwar use different encodings.

The most common is the so-called "Dan Smith" encoding [1], which arbitrarily
maps latin letters to Tengwar. There are several variants of this encoding in
use.

A Unicode encoding [2] has also been proposed, but it has not been formalized
yet and there are also several variants of it in use.

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

The representation is consistent, regardless of the mode used: a tengwar letter,
followed by zero or more diacritics modifying it. This also means that the order
of the signs may not match the phonetic order since the sign for a vowel may
come after a consonant, although it would be pronounced before it, for example
in Sindarin mode.

### Examples

- nelde (Quenya: three) `{nuumen}[e]{alda}[e]`
- neled (Sindarin: three) `{nuumen}{lambe}[e]{ando}[e]`
- river (english) `{roomen}{ampa}[i]{oore}[e]`

## Syntax

A word is a sequence of tengwar (letters) enclosed by square brackets `[]`
followed by zero or more tehta (diacritics) enclosed by curly brackets `{}`.

```
WORD = (TENGWA, TEHTA*)+ ;

TEHTA = "{", TEHTA-NAME, "}" ;

TENGWA = "[", TENGWA-NAME ,"]" ;

```

### Tengwa Name

The symbol TENGWA-NAME can be one of the following, taken from the proposed
Unicode encoding.

An all caps version of the symbol (e.g. `{TINCO}`) can be used to represented an
uppercase version of the corresponding tengwa which can be denoted in some fonts
with double-bars or other graphical variants. Some fonts display uppercase and
lowercase identically.

- `{tinco}` = TINCO
- `{tinco-extended}`
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

## References

[1] Dan Smith Encoding

[2] http://www.evertype.com/standards/iso10646/pdf/tengwar.pdf

[3] Tengwar and LaTex:
http://get-software.net/macros/latex/contrib/tengwarscript/tengwarscript.pdf

[4] Telcontar Unicode encoding:
http://freetengwar.sourceforge.net/html-files/tengtelc-discussion-008.pdf

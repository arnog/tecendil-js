# Tecendil Mode File Format

The Tengwar script is used to transcribe several languages by following
transcription rules called **modes**. Some languages have several modes.

A _Tecendil mode file_ is a _JSON with comments_ file that describes the rules
for a specific mode.

This format is a standard JSON file, with the addition that `//` and `/* */`
style comments are allowed in the file. It uses a `.jsonc` file extension.

## Creating a New Mode File

To get you started, it's a good idea to have a look at some of the existing mode
files. The simplest are `beleriand.jsonc` and `sindarin.json`.

The mode file describes how groupings of letters should be transcribed.

### `map`

These are described as **rules** in the `map` table of the mode file. The left
hand-side of the rule indicates when the rule should apply, and the right hand
side describes the corresponding transcription.

There are 'built-in' rules for vowels, digits and some punctuation that, e.g.
"a" to "\[triple-dot-above\]".

You need to provide rules for consonants, and if applicable, combination of
vowels and consonants.

Try to have as few rules as possible, both for efficiency, and because it's much
easier to change/fix a problem in a mode file with fewer rules.

The automated mode file validation step in Tecendil will try to detect and flag
unnecessary rules. However, this validation step can give both false positives
and false negatives, so they need to be reviewed carefully.

The left-hand side of a rule indicates a pattern of sequence of characters.
Patterns that are more specific (longer) take precedence over shorter patterns,
so you can have a rule for both "ai" and "aia" for example.

The patterns are not case sensitive, that is a rule with a target of "q" will
apply to "Q" as well.

You can indicate that a rule should apply only if the pattern is at the begining
of a word by preceding the pattern with a "^" or at the end of a word by ending
the pattern with "$".

So the pattern "^t" would only apply if a word begins with "t" and a pattern of
"s$" would only apply if a word ends with "s".

The right hand side of a rule indicate which tengwa (in `{...}`) and which
diacritics (in `[...]`) to replace the target with. Read more about
[Tengwar Literals](https://www.tecendil.com/inside-tecendil/)

The Tecendil engine will apply these rules to the letters in a word to derive
the transcription.

When necessary, short-carriers are inserted automatically and when applicable,
vowels/diacritics are combined. In some cases, the combinations may be
undesirable. You can prevent vowels to be combined by using an "empty" tengwa:
`{}`.

### `preprocess`

If your target language has digraphs or diacritics, you can use the `preprocess`
table to specify how to handle them.

For example, in German you could indicate that the `ß` letter maps to `ss` or
that `ö` maps to `oe`.

This can be useful to normalize some notations. Each key is a string that will
be interpreted as a regular expression, and the value is the value it will be
replaced with. For example:

```
"ĉ":      "ch"
```

would replace all circumflex-C with "ch".

The entry:

```
"^r":       "rr"
```

would replace all "r" at the begining of a word with 'rr'

### `words`

If some words are exceptions to the rules, you can provide a custom
transcription for them in the `words` table. Some languages may not need any.
English has many exceptions due to the nature of english orthography.

## Mode File Structure

A mode file contains the following properties:

- `name`: the name of the mode, typically including the language for which it is
  a mode of, e.g. `"English Phonemic"`
- `languageCode`: the ISO 639-2/3 code of the language. The language code for
  sindarin is `sjn` and for quenya it is `qya`
- `info`: a short snippet of HTML markup that provides additional information
  about the mode, such as its source and author.
- `normalizeVowels`: if `true`, vowels with a macron or grave accent and
  double-vowels are replaced with a circumflexed vowel. Vowels with a diaeresis
  and converted to a vowel with no diacritic. While this might be useful for
  some languages, if diacritics are meaningful in your language (french,
  spanish, etc...) you'll want this option set to `false`
- `tehtarFollow`: if `true`, tehtar are expected to be on the following tengwa:
  "cat" would be "c" + "t with a above".
- `preprocess`: a list of conversions to do before processing the text.
- `words` is an optional list of words that will be replaced directly. The key
  is the word itself, and the value is the replacement in Literal Tengwar.
- `map`, finally, maps the input characters to tengwar. For each entry, the key
  is a string of up to 7 characters, with `^` representing the beginning of a
  word and `$` the end. The value is a Literal Tengwar.

Note that punctuations and numbers are handled by default and don't need to be
specified in the mode file.

## Testing a Mode File

Once you have a first draft of a mode file written and saved to a file with a
`.jsonc` extension, you can try it out in Tecendil.

Open [Tecendil](https://tecendil.com) in a desktop browser, then drag the mode
file into the Tecendil window. The mode will be available in the **Mode** menu.

When the mode file is added, Tecendil will validate it to make sure there are no
mistakes in it. If applicable Tecendil may display a message with a warning if
some rules are incorrect, or possibly redundant. Review carefully the warnings.
It may be safe to ignore some, but others may need to be corrected.

## Adding a Mode to Tecendil

Once you are satisfied with your mode file, you can request for the mode file to
be added to Tecendil.

To do so, if you have a GitHub account or are interested in creating one, you
can submit a pull request against `tecendil-js`.

If you don't know GitHub, you can send your mode file to `arno@tecendil.com`

## A Note About Fonts

Fonts to display Tengwar use different encodings (a way to map characters to
glyphs).

An older one is the so-called "Dan Smith" [1] encoding, which arbitrarily maps
latin letters to Tengwar. There are several variants of this encoding in use. In
this encoding, to represent "hello" you would need to type <kbd>9j$¸`N</kbd> on
your keyboard.

A more recent Unicode encoding [2] has also been proposed, but it has not been
formalized yet and there are several variants of it in use. Most of the
characters in this encoding cannot readily be typed on keyboards, i.e.
<kbd></kbd>.

Therefore, it is generally quite tricky to get Tengwar to display correctly in
other software.

However, if you install some Tengwar font on your system, you can use Tecendil
to transcribe what you desire, then copy and paste the result, selecting a font
with an encoding matching the one of your local font.

[1] Dan Smith Encoding

[2] http://www.evertype.com/standards/iso10646/pdf/tengwar.pdf

[3] Tengwar and LaTeX:

- https://ctan.math.utah.edu/ctan/tex-archive/macros/latex/contrib/tengwarscript/tengwarscript.pdf
- https://tex.stackexchange.com/a/169156/170418

[4] Telcontar Unicode encoding:
http://freetengwar.sourceforge.net/html-files/tengtelc-discussion-008.pdf

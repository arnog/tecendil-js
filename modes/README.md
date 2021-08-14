# Tecendil Mode Files

The Tengwar script is used to transcribe several languages by following
transcription rules called **modes**. Some languages have several modes.

A _Tecendil mode file_ is a _JSON with comments_ file that describes the rules
for a specific mode.

This format is a standard JSON file, with the addition that `//` and `/* */`
style comments are allowed in the file. It uses a `.jsonc` file extension.

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
  a mode of, e.g. `"English Phonemic"`
- `languageCode`: the ISO 639-2/3 code of the language. The language code for
  sindarin is `sjn` and for quenya it is `qya1`
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
"ĉ":      "ch"
```

would replace all circumflex-C with "ch".

The entry:

```
"^r":       "rr",
```

would replace all "r" at the begining of a word with 'rr'

- `words` is an optional list of words that will be replaced directly. The key
  is the word itself, and the value is the replacement in
  [Tengwar Normalized Encoding](normalized-tengwar-encoding.md)
- `map`, finally, maps the input characters to tengwars. For each entry, the key
  is a string of up to 7 characters, with `^` representing the beginning of a
  word and `$` the end. The value is a Normalized Encoding, as explained below.

Note that punctuations and numbers are handled by default and don't need to be
specified in the mode file.

# language-tokenizer

## Overview

`language-tokenizer` is a convenience wrapper around various Unicode and Natural Language Processing libraries used for analyzing, segmenting and tokenizing text.

The main purpose of this library is to tokenize text and use it in matching.

For processing Indo-European languages as well as Arabic, Indonesian etc., it uses custom normalization algorithm combined with battle-tested Snowball stemmer.

For processing CJK languages it uses `lindera` crate, which provides multiple dictionaries for Chinese, Japanese and Korean **or** ICU dictionary segmentation.

For processing Southeast Asian languages, it uses ICU LSTM segmentation.

## Example

Tokenizing text is simple as:

```rust
use language_tokenizer::{tokenize, Algorithm};

let text = "that's someone who can rizz just like a skibidi! zoomer slang rocks, 67";
let tokens = tokenize(text, Algorithm::English, false).unwrap();

assert_eq!(tokens, vec!["that", "someon", "who", "can", "rizz", "just", "like", "a", "skibidi", "zoomer", "slang", "rock", "67"])
```

Matching text is also built-in.

```rust
use language_tokenizer::{MatchMode, Algorithm, find_match, tokenize};

let haystack = "that's someone who can rizz just like a skibidi! zoomer slang rocks, 67";
let needle = "like a skibidi";

let haystack = tokenize(haystack, Algorithm::English, false).unwrap();
let needle = tokenize(needle, Algorithm::English, false).unwrap();

assert!(find_match(&haystack, &needle, MatchMode::Exact, false).is_some());
```

## Features

No tokenizer is available by default, you should opt-in everything manually with features.

- `snowball` - Enables tokenization for [all languages supported by Snowball](https://snowballstem.org/algorithms/).

- `japanese-ipadic-neologd-lindera` - Enables tokenization for Japanese with `ipadic-neologd` dictionary. **Slow compilation, if you don't need such quality this dictionary provides - consider using `ipadic`/`unidic` or even ICU**.
- `japanese-ipadic-lindera` - Enables tokenization for Japanese with `ipadic-neologd` dictionary. **Slow compilation, if you don't need such quality this dictionary provides - consider using ICU**.
- `japanese-unidic-lindera` - Enables tokenization for Japanese with `ipadic-neologd` dictionary. **Slow compilation, if you don't need such quality this dictionary provides - consider using ICU**.

- `chinese-lindera` - Enables tokenization for Chinese with `cc-cedict` dictionary. **Slow compilation, if you don't need such quality this dictionary provides - consider using ICU**.
- `korean-lindera` - Enables tokenization for Chinese with `ko-dic` dictionary. **Slow compilation, if you don't need such quality this dictionary provides - consider using ICU**.

- `japanese-icu` - Enables tokenization for Japanese using ICU dictionary.
- `chinese-icu` - Enables tokenization for Chinese using ICU dictionary.

- `southeast-asian` - Enables tokenization for Southeast Asian languages, such as Burmese, Khmer, Lao, and Thai using LSTM.

- `full` - Shorthand for `snowball`, `japanese-ipadic-neologd-lindera`, `chinese-lindera`, `korean-lindera`, `southeast-asian` features.

- `serde` - Some serialization/deserialization for types.

## License

Project is licensed under WTFPL.

---
title: Data Objects and Annotations
keywords: data objects
permalink: '/data_objects.html'
---

This page describes the data objects and annotations used in Stanza, and how they interact with each other.

## Document

A [`Document`](data_objects.md#document) object holds the annotation of an entire document, and is automatically generated when a string is annotated by the `Pipeline`. It holds a collection of sentences ([`Sentence`](data_objects.md#sentence)) and entities ([`Span`](data_objects.md#span)), and can be seamlessly translated into a python native object.

[`Document`](data_objects.md#document) contains these properties:

| Property | Type | Description |
| --- | --- | --- |
| text | `str` | The raw text of the document. |
| sentences | `List[Sentence]` | The list of sentences for this document. |
| entities (ents) | `List[Span]` | The list of entities in this document. |
| num_tokens | `int` | The number of tokens for this document. | 
| num_words | `int` | The number of words for this document. |

[`Document`](data_objects.md#document) contains these methods:

| Method | Return Type | Description |
| --- | --- | --- |
| to_dict | `List[List[Dict]]` | Dumps the whole document into a list of list of dictionary for each token in each sentence in the doc. |

## Sentence

A [`Sentence`](data_objects.md#sentence) object represents a sentence (as is predicted by the [TokenizeProcessor](/tokenize.html)), and holds a list of the tokens ([`Token`](data_objects.md#token)) in the sentence, a list of all its words ([`Word`](data_objects.md#word)), as well as a list of the entities ([`Span`](data_objects.md#span)). 

[`Sentence`](data_objects.md#sentence) contains these properties:

| Property | Type | Description |
| --- | --- | --- |
| doc | `Document` | The parent doc of this span. |
| text | `str` | The raw text for this sentence. |
| dependencies | `List[(Word, str, Word)]` | The list of dependencies for this sentence. |
| tokens | `List[Token]` | The list of tokens for this sentence. |
| words | `List[Word]` | The list of words for this sentence. |
| entities (ents) | `List[Span]` | The list of entities in this sentence. | 

[`Sentence`](data_objects.md#sentence) contains these methods:

| Method | Return Type | Description |
| --- | --- | --- |
| to_dict | `List[Dict]` | Dumps the sentence into a list of dictionary for each token in the sentence. |
| print_dependencies | `None` | Print the dependencies for this sentence. |
| print_tokens | `None` | Print the tokens for this sentence. | 
| print_words | `None` | Print the words for this sentence. |

## Token

A [`Token`](data_objects.md#token) object holds a token, and a list of its underlying words ([`Word`](data_objects.md#word)). In the event that the token is a [multi-word token](https://universaldependencies.org/u/overview/tokenization.html) (e.g., French _au = à le_), the token will have a range `id` as described in the [CoNLL-U format specifications](https://universaldependencies.org/format.html#words-tokens-and-empty-nodes) (e.g., `3-4`), with its `words` property containing the underlying [`Word`](data_objects.md#word)s. In other cases, the [`Token`](data_objects.md#token) object will be a simple wrapper around one [`Word`](data_objects.md#word) object, where its `words` property is a singleton.

[`Token`](data_objects.md#token) contains these properties:

| Property | Type | Description |
| --- | --- | --- |
| id | `int` | The index of this token. |
| text | `str` | The text of this token. Example: 'The'. |
| misc | `str` | The miscellaneousness of this token. |
| words | `List[Word]` | The list of syntactic words underlying this token. |
| start_char | `int` | The start character index for this token in the raw text. |
| end_char | `int` | The end character index for this token in the raw text. |
| ner | `str` | The NER tag of this token. Example: 'B-ORG'. |

[`Token`](data_objects.md#token) contains these methods:

| Method | Return Type | Description |
| --- | --- | --- |
| to_dict | `List[Dict]` | Dumps the token into a list of dictionary for this token with its extended words if the token is a multi-word token. |
| pretty_print | `str` | Print this token with its extended words in one line. |

## Word

A [`Word`](data_objects.md#word) object holds a syntactic word and all of its word-level annotations. In the example of multi-word tokens (MWT), these are generated as a result of [MWTProcessor](/mwt.html), and are used in all downstream syntactic analyses such as tagging, lemmatization, and parsing. If a [`Word`](data_objects.md#word) is the result from an MWT expansion, its `text` will usually not be found in the input raw text. Aside from multi-word tokens, [`Word`](data_objects.md#word)s should be similar to the familiar "tokens" one would see elsewhere.

[`Word`](data_objects.md#word) contains these properties:

| Property | Type | Description |
| --- | --- | --- |
| id | `int` | The index of this word. |
| text | `str` | The text of this word. Example: 'The'. |
| lemma | `str` | The lemma of this word. |
| upos (pos) | `str` | The universal part-of-speech of this word. Example: 'NOUN'. |
| xpos | `str` | The treebank-specific part-of-speech of this word. Example: 'NNP'. | 
| feats | `str` | The morphological features of this word. Example: 'Gender=Fem'. |
| head | `int` | The id of the governer of this word. |
| deprel | `str` | The dependency relation of this word. Example: 'nmod'. |
| deps | `str` | The dependencies of this word. |
| misc | `str` | The miscellaneousness of this word. |
| parent | `Token` | The parent token of this word. In the case of a multi-word token, a token can be the parent of multiple words. |

[`Word`](data_objects.md#word) contains these methods:

| Method | Return Type | Description |
| --- | --- | --- |
| to_dict | `Dict` | Dumps the word into a dictionary. |
| pretty_print | `str` | Print the word in one line. |

## Span

A [`Span`](data_objects.md#span) object stores attributes of a textual span. A range of objects (e.g., named entities) can be represented as a [`Span`](data_objects.md#span).

[`Span`](data_objects.md#span) contains these properties:

| Property | Type | Description |
| --- | --- | --- |
| doc | `Document` | The parent doc of this span. |
| text | `str` | The text of this span. |
| tokens | `List[Token]` | The list of tokens that correspond to this span. |
| words | `List[Word]` | The list of words that correspond to this span. |
| type | `str` | The type of this span. Example: 'PERSON'. |
| start_char | `int` | The start character offset of this span. |
| end_char | `int` | The end character offset of this span. |

[`Span`](data_objects.md#span) contains these methods:

| Method | Return Type | Description |
| --- | --- | --- |
| to_dict | `Dict` | Dumps the span into a dictionary. |
| pretty_print | `str` | Print the span in one line. |
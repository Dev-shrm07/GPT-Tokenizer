# GPT Tokenizer

This document provides an overview of the tokenization process used by GPT (Generative Pre-trained Transformer) models, such as GPT-2 and GPT-4. Tokenization is a crucial step in natural language processing (NLP) that converts text into a sequence of tokens, which are then used to train language models.

## Table of Contents

- [Introduction](#introduction)
- [Unicode and String Representation](#unicode-and-string-representation)
- [Tokenization Process](#tokenization-process)
 - [Training the Tokenizer](#training-the-tokenizer)
 - [Decoding](#decoding)
 - [Encoding](#encoding)
- [Regex](#regex)
- [Special Tokens](#special-tokens)
- [Vocabulary Size](#vocabulary-size)

## Introduction

Tokens are used as keys to look up embeddings in the embedding table, which are then used to train large language models (LLMs). Tokens are chunks of text, not necessarily words. Tokenization can lead to issues such as spelling mistakes, whitespace handling, arithmetic failures, and language-specific challenges. Different languages may have different numbers of tokens for the same text, affecting context length, attention, and translation.

## Unicode and String Representation

Unicode is a universal representation of characters, assigning each character a unique code point. In Python, you can use `ord(char)` to get the Unicode code of a character. Strings are immutable lists of Unicode code points in Python. UTF-8 is the computer or binary representation of strings, encoding all possible characters in a string as Unicode code points using one to four bytes per character.

## Tokenization Process

### Training the Tokenizer

Training the tokenizer involves building two dictionaries: `vocabulary` and `merges`. The `vocabulary` maps tokens to their byte representations, while the `merges` dictionary contains information about how to merge tokens and what new token to assign to these merges.

The training process starts by encoding the text into UTF-8 and listing all tokens. Then, iteratively, the most frequent consecutive pairs of tokens are merged and assigned a new token. This process continues until the desired vocabulary size is reached.

### Decoding

Decoding a sequence of tokens (e.g., predicted by the model) involves iterating over the tokens, combining their byte representations using the `vocabulary` array, and then decoding the resulting sequence using `.decode("utf-8")`.

### Encoding

Encoding text into a sequence of tokens involves first encoding the text into UTF-8 and generating a list of integers or tokens. Then, the tokens are compressed using the `merges` dictionary by iteratively finding and merging the most frequent consecutive pairs of tokens according to the `merges` dictionary.

## Regex

To avoid merging characters or text that may change the meaning, GPT-2 and GPT-4 tokenizers use a special regex pattern to split the text into chunks based on patterns. These chunks are then merged individually, and the results are concatenated. The regex patterns enforce rules to prevent merging numbers with letters, for example.

## Special Tokens

Both GPT-4 and GPT-2 tokenizers use special tokens in the text to represent specific functions, such as `<|endoftext|>` to indicate the end of a conversation or document.

## Vocabulary Size

The vocabulary size is a hyperparameter that needs to be tuned for the model. A larger vocabulary size increases the size of the embedding table and computational requirements, but a smaller vocabulary size may lead to rarer token occurrences and reduced model performance.

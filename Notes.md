# Notes

## Introduction

BPE: Byte Pair Encoding

We will build bpe from scratch

Tokenization is at the heart of much weirdness in LLMs and we can not brush it off!!!
Here's a list of problems caused due to improper/insufficient tokenization:

- Why can't llms spell words?
- Why is llm bad at simple arithmetic?
- Why is llm bad at Non-English languages?
- What is the real root of suffering?

The answer to all the questions phrased aboved is **TOKENIZATION**

# Relation between context length and embeddings vocabulary

Tradeoff between context size and embeddingTable size (number of tokens):

Up until now we have treaded each character of an input string to be treated as a singular token.
This means that in our transformer architecture, each character in the input is treated seperately to check if it is attending to any other characters.
Let's say that our practical limit of calculating attention between different embeddings is n. Then, in our current framework we can only generate a token( which will also be a singular character) based on the previous n tokens (for which we are calculating the attention) and the previous n tokens only amount to n characters.
In some sense, while our embedding table or vocabulary only has 26 entries pertaining to the alphabets in the english language; our context length is severely limited.
Instead we can increase the dimensionality of the embeddings and also add more entries to the embedding table and create singular embedding for a sequence of characters. This will result in a larger embedding table; but the architecture will be able to look at more information and make a better guess to predict the next token. This is the tradeoff between the token lookup table and context length.
Now, we might ask, if this is the case then why don't we increase the embedding lookup table's size by an arbitrary amount. The catch here is, that it leads to sparsity of information. Each embedding in the lookup table is only fired when it is encountered as a part of the input. Hence the embedding, needs to appear a threshold number of times in the training set to sufficiently be modified to have an optimal value. If we, keep making new embedding entries into the lookup table for arbitraary sequences of characters, but we can't ensure there existence in the training set, we won't actually be able to train the network in our case the language model.

# Relation between Context Length and Embeddings Vocabulary

Tradeoff between context size and embedding table size (number of tokens):

Up until now, we have treated each character of an input string as a singular token. This means that in our transformer architecture, each character in the input is treated separately to check if it is attending to any other characters. Let's say that our practical limit of calculating attention between different embeddings is n. Then, in our current framework, we can only generate a token (which will also be a singular character) based on the previous n tokens (for which we are calculating the attention), and the previous n tokens only amount to n characters.

In some sense, while our embedding table or vocabulary only has 26 entries pertaining to the alphabets in the English language, our context length is severely limited. Instead, we can increase the dimensionality of the embeddings and also add more entries to the embedding table and create singular embeddings for sequences of characters. This will result in a larger embedding table, but the architecture will be able to look at more information and make a better guess to predict the next token.

This is the tradeoff between the token lookup table and context length. Increasing the embedding table size can enhance context processing but risks sparsity if embeddings for infrequent character sequences aren't sufficiently trained. Now, we might ask, if this is the case, then why don't we increase the embedding lookup table's size by an arbitrary amount? The catch here is that it leads to sparsity of information. Each embedding in the lookup table is only used when it is encountered as a part of the input. Hence, the embedding needs to appear a threshold number of times in the training set to be sufficiently modified to have an optimal value. If we keep making new embedding entries into the lookup table for arbitrary sequences of characters, but can't ensure their existence in the training set, we won't actually be able to train the network—in our case, the language model—effectively.

In summary, while increasing the embedding table size can improve model performance by capturing more context, it also increases computational cost and memory usage, and requires sufficient training data to avoid sparsity and underrepresentation of infrequent tokens. Hybrid tokenization methods, such as Byte-Pair Encoding (BPE) or SentencePiece, are often used to balance this tradeoff by creating subword units that are more frequent and better trained.

# A little bit of a segue into Unicode

Good Reference: [A Programmer’s Introduction to Unicode](https://www.reedbeta.com/blog/programmers-intro-to-unicode/)
It is important to understand how strings are represented as byte streams and how they are encoded and decoded for users.
This can be thought as the first level of abstraction already present between human readable language and machine interpreted language.
With tokenization, we can say that we are providing an additional layer of abstraction or a different layer of abstraction by converting these byte streams into a formast which is more beffiting for an input to a neural network model.
We have discussed the major ideas that we want to ensure with the design of individual tokens and the token table as a whole.
BPE: Byte Pair Encoding is one of the most popular techniques of tokenization; but before we can fully understand BPE we need to understand what the B in BPE stands for. The B precisely stands for the pairing of byte level units which are achieved through the encoding of unicode (possibly UTF-8, UTF-16 or even UTF-32 (less likely)).

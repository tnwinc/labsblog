---
title: "Text Generation from Sample Data in C#"
layout: post
tags: [text, procedural, generation]
published: true
author: Adam Moss
mail: "adamrmoss@gmail.com"
summary: "Quickly generate large sets words that bear a passing resemblance to provided sample words.  The results
          are usually pronounceable!"
---

We found that we needed to generate some large XML datasets in order to create text fixtures for about bad-ass dimension import.
Rather than spend too much time on this totally useful task, I instead elected to obsess about generating names similar to those
in a sample set, even though "Dimension1", "Dimension2", etc would have been perfectly acceptable.  So then the question was,
"How to represent the characteristics of our sample way in a manner that would allow us to reproduce them?"

So I went with a very simple approach that doesn't bake in any assumptions about English grammar.  Rather, we chunk the word into
subwords, specifying a minimum and a maximum subword size.  Once we have these subwords, we generate new words using the following
principles:

> 1. We construct words by appending the extracted subwords together, from left to right.
> 2. If one subword frequently follows another subword, then the latter subword is a good choice for next subword if the word
>    currently ends with the former subword.

We use some cool, but very simple techniques for discovering discrete probabilistic distributions and then transforming those in a
useable form.  To tally all the values of a type `T` that we encounter, we use a `Dictionary<T, int>`.  The `Tally` function looks like this:

        public static TValue FailproofLookup<TKey, TValue>(this IDictionary<TKey, TValue> dictionary, TKey key)
          where TValue : new()
        {
          if (dictionary.ContainsKey(key)) {
            return dictionary[key];
          } else {
            var value = new TValue();
            dictionary[key] = value;
            return value;
          }
        }
        
        public static void Tally<TKey>(this IDictionary<TKey, int> dictionary, TKey key)
        {
          var currentCount = dictionary.FailproofLookup(key);
          dictionary[key] = currentCount + 1;
        }

Then, when we want to transform a distribution into what I call a "Choice Array":

        public static TKey[] ToChoiceArray<TKey>(this IDictionary<TKey, int> dictionary)
        {
          return dictionary.SelectMany(kvp => Enumerable.Repeat(kvp.Key, kvp.Value)).ToArray();
        }

Now we can make weighted choices from a distribution in constant time.

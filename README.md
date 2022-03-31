<h2 style="text-align: center;">Semantic highlighting</h2>

<h4 style="text-align: center;">Dzhekshembaev Ramazan</h4>

## **Contact info** ##

***
E-mail: rmzs0711@gmail.com

Telegram: [@RMZS0711](https://t.me/RMZS0711)

GitHub: [rmzs0711](https://github.com/rmzs0711)

Phone number: +996-502-236-424

## **Abstract** ##

***
This project intended to implement a semantic highlight feature.

Motivation issue:

* [#1654](https://github.com/haskell/haskell-language-server/issues/1654)

Expected deliverables:

* Perform semantic highlight for tokens that match with Haskell correctly
* Think about other types and maybe add new tokens that are better for Haskell things

## **Tasks** ##

***
This part exists to make me sure, that my project understanding is correct

We already have Semantic Tokens support in [LSP](https://github.com/haskell/lsp/pull/314). So the rest of the work, if I understand correctly, is implementing a creation of Semantic Tokens and sending them to the LSP client.

### **Way of implementation** ###

Apparently, we have 2 ways of implementation:

a) as another plugin

b) as a feature inside `ghcide`

In a) according to the conversation in [#1654](https://github.com/haskell/haskell-language-server/issues/1654), we will have some extra work caused by a feature of a multiple plugins structure. In b) we will work with LSP directly, but it seems is not a canonical way to add new features.

### **Targets to highlight** ###

Based on **michalpj** comments in the same issue, I came to the conclusion, that semantic highlighting is not necessary for places, where basic syntax highlighting works well, e.g. string literals, keywords, numbers, operators. So this is a list of things, where semantic highlighting will be a good option:

* `class`, `type constructor`, `data constructor` and `type families` (they all have the same colour)

* [Other kinds of "type-like things", e.g. constraints](https://github.com/haskell/haskell-language-server/issues/1654#issue-849356185).  Honestly, I did not understand what's the point

* Deprecated functional (can be identified only with google)

* Class methods (have the same colour with regular functions)

* Haskell syntax in doctest comments (have the same colour as regular comments)

### **Token types and modifiers** ###

[Token types and modifiers from VScode docs](https://code.visualstudio.com/api/language-extensions/semantic-highlight-guide#standard-token-types-and-modifiers). It seems we have no problems with, for example, `class` or `method` token types, so we can implement highlighting for them correctly. But with the `data constructor` and `type constructor`, we haven't a suitable variant. We have an option to add a new token type, but this solution is contradicting of main LSP idea because we will add some work to the client to handle new tokens.

## **Solution** ##

***
In this part, I will share my thoughts about a solution.

Apparently, **pepeiborra** and **michaelpj** agreed on option b) of realisation.

In the problem of the token types, it seems that super economy on adding a new type is unnecessary, but the decision to create new types will add some extra routinely in `LSP`. Also I figured out that Kotlin and OCaml already have semantic syntax highlighting support, Scala has the same GSoC idea for the summer of 2022. So I think it will be a good idea to ask some questions to other contributors and investigate other solutions for other functional programming languages

## **About me** ##

***
I am attending the Computer Science Bachelor's course at the Saint-Petersburg campus of the Higher School of Economics. However, I am a citizen of the Kyrgyz Rebuplic and I am located in Bishkek right now.

I have finished a Haskell course at my university, but this is only my experience with Haskell. Last 2 weeks in my free time I was diving into the `HLS` codebase. As a result, I understand how to build the project and have a general idea of how to add new features. Also I figured out that current docs are a bit out of date, so I am going to try to fix currently confusing parts as my first PR in the ramp-up to GSoC

I had finished a Formal Languages course at my university, so I have experience in generating parsers and in building AST. During this course I had a study project, that can analyse code in an abstract language and give information about integers values bounds. This information can be useful for static code analyzers, to determine integers overflow. This project was interesting for me, that's why I decided to choose `HLS` and start with semantic highlighting

<h2 style="text-align: center;">Semantic highlighting and change annotations</h2>

<h4 style="text-align: center;">Dzhekshembaev Ramazan</h4>

## **Contact info** ##

E-mail: rmzs0711@gmail.com

Telegram: [@RMZS0711](https://t.me/RMZS0711)

GitHub: [rmzs0711](https://github.com/rmzs0711)

Phone number: +996-502-236-424

## **Abstract** ##

This project intended to implement a semantic highlight and a change annotation features

Semantic highlight motivation issue:

* [#1654](https://github.com/haskell/haskell-language-server/issues/1654)

Expected deliverables:

* Perform semantic highlight for tokens that match with Haskell correctly
* Think about other types and maybe add new tokens that are better for Haskell things

Change annotation motivation:

* These let you annotate pieces of edits with notes explaining what they do, and some clients may let the user review them before accepting

Expected deliverables:

* Look inside plugins, that provide code edits (e.g. [Hlint](https://haskell-language-server.readthedocs.io/en/latest/features.html#apply-hlint-fixes)), and add the possibility to add `change annotations`

## **Tasks** ##

### Semantic highlight ###

We already have Semantic Tokens support in [LSP](https://github.com/haskell/lsp/pull/314). So the rest of the work is implementing a creation of Semantic Tokens after analysis of document and sending them to the LSP client.

### **Interaction between semantic and basic highlighting** ###

[Basic highlighting](https://marketplace.visualstudio.com/items?itemName=justusadam.language-haskell) now is working inside another extension and LSP spec doesn't tell us about how 2 versions of highlighting will work together.

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

### Change annotation ###

Maybe we will need to think about general annotations format rules to make it more understandable.

Again, it would be good to see what other people are doing - **michaelpj** hasn't been able to find any other server implementation that uses change annotations, so maybe other servers have some unexpected problems with this feature.

## **Solution** ##

In this part, I will share my thoughts about a solution.

### Semantic highlight ###

**michaelpj**  suggested, that we will want to add the new functionality as a plugin, which may require tweaking the plugin infrastructure slightly, but that should be okay.

In the problem of the token types, it seems that super economy on adding a new type is unnecessary, but the decision to create new types will add some extra routinely in `LSP`. Also I figured out that Kotlin and OCaml already have semantic syntax highlighting support, Scala has the same GSoC idea for the summer of 2022. So I think it will be a good idea to ask some questions to other contributors and investigate other solutions for other functional programming languages

### Change annotation ###

We don't need a new plugin or something like that, we need to add a functionality in existing plugins, that will add `change annotations` info in requests

Maybe is a good idea to investigate existing plugins, that provide code edits, in order to generalize `change annotations` logic. As I see, this logics implementation can be not necessarily as a plugin, but as a library with useful functions.

## **Tentative plan** ##

### May ###

Familiarize more with the `HSL` code and `Semantic Highlight` implementations in other projects. Decide which way we want to go in realisation, and which features we want to provide. Prepare a plugins general routine part and process a request from the client to the server

### 30 May - 12 June ###

Implement a comfortable interface with which we can add highlighting of new targets more easily and test it. Start with the simple targets of highlighting: class method, class, type constructor, data constructor.

### 13 June - 1 July ###

At this time I will have my university exams, so it is a high chance that my impact during this time will be at my minimum. However, I think it will be possible to finish with the simple targets of highlighting and testing it.

### 2 July - 17 July ###

Continue with other targets of highlighting, that are not so obvious for me: deprecated functional, Haskell syntax in doctest comments, maybe something else.

### 18 July - 29 July ###

Polish done work, add more tests, prepare to submit Phase 1 evaluations

### 30 July - 4 September ###

Implement `Change annotations`. Since I started to study this topic recently, I am not confident in my vision of it, so that's why this part of the schedule is without details. Also `Change annotations` are for me is plan B, if `Semantic highlighting` will be ready by this time

## **About me** ##

I am attending the Computer Science Bachelor's course at the Saint-Petersburg campus of the Higher School of Economics. However, I am a citizen of the Kyrgyz Repuplic and I am located in Bishkek right now.

I have finished a Haskell course at my university, but this is only my experience with Haskell. Last 2 weeks in my free time I was diving into the `HLS` codebase. As a result, I understand how to build the project and have a general idea of how to add new features. Also, I figured out that current docs are a bit out of date, so I am going to try to fix currently confusing parts as my first PR in the ramp-up to GSoC

I had finished a Formal Languages course at my university, so I have experience in generating parsers and in building AST. During this course, I had a study project, that can analyse code in an abstract language and give information about integer values bounds. This information can be useful for static code analyzers, to determine integers overflow. This project was interesting for me, that's why I decided to choose `HLS` and start with semantic highlighting

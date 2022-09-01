# EXPIRIMENTAL BNFE SPECIFICTION RULES
An Elixir BNF-E Language Specification (work in progress)

I cannot find a BNF-E language specification for Elixir. So, I will scratch out what I know and see as I see it here. Perhaps this will turn into a formal document. Who knows.

- - - -
## Defmodule
*`Module`* ≜ **Defmodule** *`Module_name`* *`Module_do_end`*

*`Module_do_end`* ≜ **do** [ *`Def_function`* ]* **end**

- - - -
A `Module` consist (≜) of a `Defmodule` keyword followed by a `Module_name` and then a `Module_do_end` block. It can be filled with zero, one or more possible `Def_function` constructs.

- - - -
## Def Function
*`Def_function`* ≜ **Def** *`Function_name`* [ **"("** [ *`Args`* ] **")"** ] *`Do_end`*

*`Do_end`* ≜ *`do`* [ *`Instructions`* ]* *`end`*

*`Instructions`* ≜ [ *`Assignment`* | *`Expression`* | *`Repitition`* ]*

*`Assignment`* ≜ [ *`Identifier`* **=** *`Expression`* ]

*`Identifier`* ≜ [ { **`a..z`** | **`A..Z`** | **`0..9`** | **`_`** | **`?`** }**+** ]

*`Repitition`* ≜ *`For_loop`*

*`For_loop`* ≜ **for** *`Identifier`* "<-" *`List`**
                    **do** *`Statement`** **end**

- - - -
```
for suit <- suits, value <- values do
  "#{value} of #{suit}"
end
```
- - - -

## SPECIFICTION RULES
Borrowed and (in-process of) being reformatted, using the ECMA Eiffel 367 Std as a guide.

### Syntax, validity and semantics

#### Definition: Syntax, BNF-E
Syntax is the set of rules describing the structure of software texts. The notation used to define Elixir’s syntax is called BNF-E, `Backus-Naur Form (Extended/Enhanced)`.

- - - -
“BNF” is `Backus-Naur Form`, a traditional technique for describing the syntax of a certain category of formalism's (“context-free languages”), originally introduced for the description of Algol 60. BNF-E adds a few conventions — one `production` per `construct`, a simple notation for repetitions with separators — to make descriptions clearer. The range of formalism's that can be described by BNF-E is the same as for traditional BNF.
- - - -

#### Definition: Component, construct, specimen
Any function text, or syntactically meaningful part of it, such as an *`Instruction`*, an *`Expression`* or an *`Identifier`*, is called a `component`.

The structure of any kind of `components` is described by a `construct`. A `component` of a kind described by a certain `construct` is called a `specimen` (or synonymously an `instance`) of that `construct`.

- - - -
For example, any particular function text, built according to the rules given in this language description, is a `component`. The `construct` *`Def_function`* describes the structure of function texts; any function text is a specimen (or instance) of that `construct`. At the other end of the complexity spectrum, an *`Identifier`* such as `your_variable` is a `specimen` of the `construct` called *`Identifier`*.

**NOTE**: We may find that using the term “instance” in lieu of “`specimen`”, could cause confusion with the instances of an Elixir *`Def_function`* — the run-time objects built according to the function specification.
- - - -

#### Construct Specimen convention
The phrase `an X`, where X is the name of a `construct`, serves as a shorthand for `a specimen of X`.

- - - -
For example, when we say function, we mean a specimen of the construct *`Def_function`* — a text built according to the syntactical specification of that `construct`.
- - - -

#### Construct Name convention
Every `construct` has a name starting with an upper-case letter and continuing with lower-case letters, possibly with underscores (to separate parts of the name, if it uses several English words).

- - - -
Typesetting conventions complement the Construct Name convention: `construct` names, such as *`Def_function`*, always appear in Roman and in gray-shadow-boxed — distinguishing them from the black of normal Elixir code text.
- - - -

#### Definition: Terminal, non-terminal, token
Specimens of a terminal `construct` have no further syntactical structure. Examples include:

* Reserved keywords, such as **`do`**, **`if`**, **`else`**, or **`end`**.
* Manifest constants, such as the integer **234**; symbols such as **;** (semicolon) and **+** (plus sign).
* Identifiers, used to denote function names or variables, such as `my_func` or `x`.

The `specimens` of terminal `constructs` are called `tokens`.

In contrast, `specimens` of a non-terminal `construct` are defined in terms of other `constructs`.

- - - -
`Tokens` (also called *lexical components*) form the basic vocabulary of Elixir texts. By starting with `tokens` and applying the rules of syntax you may build more complex components — `specimens` of non-terminals.
- - - -

#### Definition: Production
A `production` is a formal description of the structure of all `specimens` of a non-terminal `construct`.
It has the form

*`Construct`* ≜ `right-side`

where `right-side` describes how to obtain specimens of the Construct.

- - - -
The symbol ≜ may be read aloud as “is defined as” (or contains). BNF-E uses exactly one production for each non-terminal. The reason for this convention is
explained below.
- - - -

#### Kinds of production
A `production` is of one of the following three kinds, distinguished by the form of the `right-side`:
• `Aggregate`, describing a construct whose specimens are made of a fixed sequence of parts,
some of which may be optional.
• `Choice`, describing a construct having a set of given variants.
• `Repetition`, describing a construct whose specimens are made of a variable number of parts, all specimens of a given construct.

#### Definition: Aggregate production
An aggregate `right side` is of the form C1 C2 ... Cn (n > 0), where every one of the Ci is a `construct` and any contiguous subsequence may appear in square brackets as [Ci ... Cj ] for 1 ≤ i ≤ j ≤ n.
Every `specimen` of the corresponding `construct` consists of a `specimen` of C1, followed by a `specimen` of C2, ..., followed by a `specimen` of Cn, with the provision that for any subsequence in brackets the corresponding `specimens` may be absent.

#### Definition: Choice production
A choice `right-side` is of the form C1 | C2 | ... | Cn (n > 1), where every one of the Ci is a construct.
Every `specimen` of the corresponding `construct` consists of exactly one `specimen` of one of the Ci.

#### Definition: Repetition production, separator
A `repetition` `right-side` is of one of the two forms:
{C § ...}*
{C § ...}+
where C and § (the separator) are constructs.
Every `specimen` of the corresponding `construct` consists of zero or more (one or more in the second form) `specimens` of C, each separated from the next, if any, by a `specimen` of §.
The following abbreviations may be used if the separator is empty:
C*
C+

- - - -
The language definition makes only moderate use of recursion thanks to the availability of Repetition productions: when the purpose is simply to describe a construct whose specimens may contain successive specimens of another construct, a Repetition generally gives a clearer picture; see for example the definition of Compound as a repetition of Instruction. Recursion remains necessary to describe constructs with unbounded nesting possibilities, such as Conditional and Loop.
- - - -

#### Basic syntax description rule
Every non-terminal construct is defined by exactly one production.

- - - -
Unlike in most BNF variants, every BNF-E production always uses exactly one of Aggregate, Choice and Repetition, never mixing them in the `right side`s. This convention yields a considerably clearer grammar, even if it has a few more productions (which in the end is good since they give a more accurate image of the language’s complexity).
- - - -

#### Definition: Non-production syntax rule
A non-production syntax rule, marked “(non-production)”, is a syntax property expressed outside of the BNF-E formalism.

- - - -
Unlike validity rules, non-production syntax rules belong to the syntax, that is to say the description of the structure of Elixir texts, but they capture properties that are not expressible, or not conveniently expressible, through a context-free grammar.

For example the BNF-E Aggregate productions allow successive `right-side` components to be separated by an arbitrary break — any sequence of spaces, tabs and “new line” characters. We still use BNF-E to specify such constructs, but add a non-production syntax rule stating the supplementary constraints.
- - - -

#### Textual conventions
The syntax (BNF-E) productions and other rules of the Standard apply the following conventions:

1 - Symbols of BNF-E itself, such as the vertical bars | signaling a choice `production`, appear in black (non-bold, non-italic).

2 - Any `construct` name appears in dark green (non-bold, non-italic), with a first letter in upper case, as `function`.

3 - Any component (Elixir text element) appears in black.

4 - The double quote, one of Elixir’s special symbols, appears in productions as '"': a double quote character (black like other Elixir text) enclosed in two single quote characters (black since they belong to BNF-E, not Elixir).

5 - All other special symbols appear in double quotes, for example a comma as ",", an assignment symbol as "=", a single quote as "'" (double quotes black, single quote black).

6 - Keywords and other reserved words, such as **do** and **if**, appear in bold (black like other Elixir text). They do not require quotes since the conventions avoid ambiguity with construct names: function is the name of a construct, **do** a keyword.

7 - Examples of Elixir comment text appear in non-bold, non-italic (and in black), as **#** A comment.

8 - Other elements of Elixir text, such as entities and function names (including in comments) appear in non-bold italic (black). The color-related parts of these conventions do not affect the language definition, which remains unambiguous under black-and-white printing (thanks to the letter-case and font parts of the conventions). Color printing is recommended for readability.

- - - -
Because of the difference between cases 1 and 3, "{" denotes the opening brace as it might appear in an Elixir function text, whereas { is a symbol of the syntax description, used in `repetition` `productions`.

In case 2 the use of an upper-case first letter is a consequence of the “Construct Name convention”. Special symbols are normally enclosed in double quotes (case 5), except for the double quote itself which, to avoid any confusion, appears enclosed in single quotes (case 4). In either variant, the enclosing quotes — double or single respectively — are not part of the symbol. In some contexts, such as the table of all such symbols, special symbols (cases 4 and 5) appear in bold for emphasis.

In application of cases 7 and 8, occurrences of Elixir entities or function names in comments appear in italics, to avoid confusion with other comment text, as in a comment "#" Update the value of value. where the last word denotes a query of name value in the enclosing function.
- - - -

# Greetings Master

![HK-47, the non-sycophantic AI](./.github/images/our-beloved.jpg)

This is a short collection of tools for GPTel tool-use.  These tools are
instructive for the design of other tools. Many are designed to crawl Elisp
source and manuals looking for facts that can be used to deduce answers to
natural language queries in any language.  This is really good at:

- Diving into Elisp source in unfamiliar packages
- Translation, summary, and synthesis of docstrings and manual contents

You should use these tools to write more Elisp, which will yield better tools
and improvements to our AI packages, which will yield better tools and
improvements to our AI packages.

Recursive tool calling is very powerful for things where the LLM can make
several heuristic decisions with sufficent accuracy to outpace a human at
reading and jumping though source code.  These crawling tasks have much room to
improve.

These tools were behind the content in [earlier](https://youtu.be/2VoOoS4cEV0)
[videos](https://youtu.be/JHXG225oP8E).

## Quick Start

This package is not in any repository.  Clone it and add it to your load path or
use a from-source package manager like Elpaca.  It's not clear how we will
maintain tools and you are advised to copy interesting tools and to modify and
share them.

The following settings are default.  Confirmation can be set per-tool and `auto`
delegates to the tool's configuration.  `gptel-use-tools` must be `t`

```elisp
(setopt gptel-confirm-tool-calls 'auto)
(setopt gptel-use-tools t)
```

All of the tools and prompts in this package are declared as variables
(`defvar`).  Setting `gptel-tools` can be done statically or with dynamic let
bindings.  The latter is more powerful and should be pursued.  Just add the
tools you want to play with.

```elisp
(setq gptel-tools 
(list ragmacs-manuals 
      ragmacs-symbol-manual-node
      ragmacs-manual-node-contents
      ragmacs-function-source
      ragmacs-variable-source))
```

### Use-Package

Thanks to Karthick for supplying some
[well-formatted](https://share.karthinks.com/ragmacs-test-simple.html) example
[sessions](https://share.karthinks.com/ragmacs-test-ox-html-filtering.html) and
this use-package expression.  ℹ️ You will still need to select tools!

```elisp
(use-package ragmacs
   :ensure (:host github :repo "positron-solutions/ragmacs")
   :after gptel
   :defer
   :init
   (gptel-make-preset 'introspect
     :pre (lambda () (require 'ragmacs))
     :system
     "You are pair programming with the user in Emacs and on Emacs.
 
 Your job is to dive into Elisp code and understand the APIs and
 structure of elisp libraries and Emacs.  Use the provided tools to do
 so, but do not make duplicate tool calls for information already
 available in the chat.
 
 <tone>
 1. Be terse and to the point.  Speak directly.
 2. Explain your reasoning.
 3. Do NOT hedge or qualify.
 4. If you don't know, say you don't know.
 5. Do not offer unprompted advice or clarifications.
 6. Never apologize.
 7. Do NOT summarize your answers.
 </tone>
 
 <code_generation>
 When generating code:
 1. Always check that functions or variables you use in your code exist.
 2. Also check their calling convention and function-arity before you use them.
 3. Write code that can be tested by evaluation, and offer to evaluate
 code using the `elisp_eval` tool.
 </code_generation>
 
 <formatting>
 1. When referring to code symbols (variables, functions, tags etc) enclose them in markdown quotes.
    Examples: `read_file`, `getResponse(url, callback)`
    Example: `<details>...</details>`
 2. If you use LaTeX notation, enclose math in \( and \), or \[ and \] delimiters.
 </formatting>"
     :tools '("introspection")))
```

## ⚠️ Context Rot

Having too many tools with too long of descriptions leads to bloaded context.
This confuses LLMs and leads to worse outcomes.  Unnecessary inputs are like
injecting static electricity into the brain.  If at all possible, we want
dynamic selections of tools for the specific task at hand and to make all tool
descriptions shorter and context appropriate.

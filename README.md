# agrep

This library implements the Wu-Manber algorithm for string searching
with errors, popularized by the "agrep" Unix command and the "glimpse"
file indexing tool.  It was developed as part of a search engine for a
largish MP3 collection; the "with error" searching comes handy for those
who can't spell Liszt or Shostakovitch.

Given a search pattern and a string, this algorithm determines whether
the string contains a substring that matches the pattern up to a
parameterizable number N of "errors".  An "error" is either a
substitution (replace a character of the string with another
character), a deletion (remove a character) or an insertion (add a
character to the string).  In more scientific terms, the number of
errors is the Levenshtein edit distance between the pattern and the
matched substring.

The search patterns are roughly those of the Unix shell, including
one-character wildcard (?), character classes ([0-9]) and multi-character
wildcard (*).  In addition, conjunction (&) and alternative (|) are
supported.  General regular expressions are not supported, however.

Performance is quite good: for short patterns (less than 31 characters)
and no errors, this library is about 8 times faster than OCaml's "Str"
regular expression library.  Speed decreases with the number of errors
allowed, but even with 3 errors we are still faster than "Str".

The algorithm is described in S. Wu and U. Manber, *Fast Text
Searching With Errors*, tech. rep. TR 91-11, University of Arizona, 1991.
It's a nice exercise in dynamic programming and bit-parallel implementation.

## Licensing

LGPL: This code is distributed under the terms of the GNU Library
General Public License version 2.


## Installation

OCaml 3.04 and up is required.

Do `dune build`.
Become superuser, and do `dune install` or `dune install --prefix=...`
to specify the installation directory.


## Usage

``ocamlc -I +agrep ... agrep.cma ...``
or
``ocamlopt -I +agrep ... agrep.cmxa ...``

See the commented interface agrep.mli for API documentation.

## Example

There's an example in `example/testagrep.ml`.

You can test it with `dune build @all` then with
e.g `dune exec example/testagrep.exe -- "Strong" -e 1 -f src/agrep.ml`.

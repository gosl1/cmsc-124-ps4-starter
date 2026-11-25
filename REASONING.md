<!--no-pdf-->
# Why Each Prediction Reads the Way It Does

Write one entry per case, before you run anything. Name the rule behind each
predicted field. One to three sentences is enough. The grader checks the
structure of this file only. It does not judge how good the reasoning is, and
it sets no word minimum.

For every case you predicted wrongly, add a short correction after you run
it: what you predicted, what the language printed, and which rule you
misread. Corrections belong in the correction commits, not the prediction
commit.

Commit this file together with `predictions.tsv`, before any case runs:

```bash
git add predictions.tsv REASONING.md
```

The example below uses a query that is not one of the 16 cases, so it gives
no answer away. `parent(pat, X)` reads `parent(pat, jim)` because the `parent`
facts are tried top to bottom and only that one has `pat` as its first
argument:

```text
## parent(pat, X)

The goal scans the parent facts in source order. Only parent(pat, jim) has
pat first, so X binds to jim. No second fact matches, so the first ; finds
nothing and the search ends after one answer.
```

# Why Each Prediction Reads the Way It Does

## P01

Prolog checks the parent facts from top to bottom. It finds two matches, so X gets liz first and then bob.

## P02

The query matches an existing fact, so it succeeds once. There are no other matching facts to give another answer.

## P03

There is no parent fact where tom is the child, so the query fails and gives no answers.

## P04

First, Prolog makes X equal to f(Y,a). Then Y gets bound to b, which also fills in the Y inside that term. This gives one answer.

## P05

The two f terms match with each other, so Prolog binds X to a and Y to b.

## P06

The two atoms are different, so they cannot match. Because of that, a=b fails.

## P07

grandparent/2 checks tom's children in the order they appear in the facts. liz has no child, but bob has ann and pat, so those are the answers.

## P08

ancestor/2 checks its base clause first before moving to the recursive clause. Because of this, it finds tom's descendants in that order.

## P09

Unlike the previous one, ancestor_r/2 checks the recursive clause first. This makes it find the deepest descendant, sue, before finding jim.

## P10

The base and recursive ancestor clauses check the different possible parent chains. The first matching chain gives pat, while the last one gives tom.

## P11

reach/2 calls itself before checking edge/2. Since it keeps calling itself, Prolog gets stuck recursing and never reaches an answer.

## P12

reach_r/2 checks an edge first before calling itself again. It first finds the path from a to b to c to d, but when it backtracks, it keeps going through the cycle.

## P13

len/2 counts each of the three elements in the list. It adds 1 each time it returns from a recursive call, giving N=3.

## P14

join/3 starts by checking its empty-list clause. It then recursively splits [1,2] into the possible prefixes.

## P15

X is still unbound when Prolog tries to evaluate X>2. Because it needs a value for X first, Prolog raises an instantiation error before elem/2 gets a chance to bind it.

## P16

elem/2 checks the elements in the list and binds X to each one. After that, X>2 only succeeds when X is 3, so there is just one answer.



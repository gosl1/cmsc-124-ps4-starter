# Why Each Prediction Reads the Way It Does

#Erine

## P01

The goal scans the parent facts in source order and takes the first whose first argument is tom, which is parent(tom, liz), so X binds to liz. The first ; backtracks to the next matching fact, parent(tom, bob), so the second answer is X=bob. No fact is left after that, so the count is 2.

## P02

The goal has no variables, so the answer is true or false with nothing to bind. Scanning the facts in source order finds parent(tom, bob), so the first answer is true. That fact appears once, so the first ; finds no other way to prove it, the second is none, and the count is 1.

## P03

The goal looks for a parent that whose child or the second argument is tom. Scanning the facts in source order, tom only appears as the first argument, so no fact coincides with parent(X, tom). The query fails, so the first answer is false, the second is none, and the count is 0.

## P04

The goals run left to right, X = f(Y, a) is unification. X becomes the term f(Y, a) with Y still unbound, and then Y = b binds Y, which fills in X as f(b, a). Unification has only one way to succeed, so the first ; finds nothing, the second is none, and the count is 1.

## P05

The goal is also unification, so it matches f(X, b) and f(a, Y) argument by argument. X meets a and binds to a, and b meets Y so Y binds to b, which makes both sides f(a, b). Unification has one way to succeed, so the first ; finds nothing, the second is none, and the count is 1.

## P06

The goal tries to unify two different atoms, a and b. Two different atoms can never be made the same, because no binding can change an atom. The query fails, so the first answer is false, the second is none, and the count is 0.

## P07

The goals run left to right, so parent(tom, P) sets P to liz first, but parent(liz, X) fails because liz has no children. The search goes back, sets P to bob, and parent(bob, X) gives X=ann first and X=pat on the next ;. Nothing is left after that, so the count is 2.

## P08

The base clause parent(A, D) is written first, so the direct children come out first, giving X=liz and then X=bob. After those, the recursive clause goes deeper following the order of the facts, giving ann, pat, sue, and jim. The order is liz, bob, ann, pat, sue, jim, so the first is X=liz, the second is X=bob, and the count is 6.

## P09

The recursive clause is written first, so ancestor_r keeps going down through the parent facts before the base clause can give an answer. The deepest descendants come out first, so the first answer is X=sue and the second is X=jim. The same six people come out in a different order, sue, jim, ann, pat, liz, bob, so the count is 6.

## P10

The base clause asks parent(X, jim), which matches parent(pat, jim), so the first answer is X=pat. The recursive clause then goes through the parent facts in file order and finds tom through bob and pat, giving X=tom, and then bob through pat, giving X=bob. The answers are pat, tom, bob, so the count is 3.

## P11

The first clause of reach starts with reach(a, Z), so it calls itself before it reads any edge fact. It never reaches a fact and never fails, so it never goes back to try another choice and never gives an answer. This is left recursion, so all three fields are unbounded.

## P12

reach_r reads an edge fact before it calls itself, so it finds the path a to b to c to d and the first answer is true. The graph has a cycle, a to b to c to a, so when the search goes back it loops around the cycle and finds true again, so the second answer is true. It never runs out of answers, so the count is unbounded.

## P13

len has a clause for [] that gives 0, and a clause for [_|T] that calls itself on the rest of the list and then uses is to add 1. With three elements this adds up to N=3. Only one clause fits at each step, so there is no second answer and the count is 1.

## P14

The first join clause matches the empty list, so X=[] and Y=[1,2] come first. Going back into the recursive clause moves one element at a time into X, giving X=[1] and Y=[2], and then X=[1,2] and Y=[]. That is three ways to split the list, so the count is 3.

## P15

The goals run left to right, so X > 2 is asked before elem gives X a value. The > comparison needs a number and X has no value yet, so Prolog throws an error before elem is even called. All three fields are error.

## P16

The goals are in the other order, so elem gives X a value first, trying 1, then 2, then 3. X > 2 fails for 1 and 2 and works for 3, so the first answer is X=3. Nothing comes after that, so the second is none and the count is 1.


#Marc

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



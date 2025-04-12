## 2025 0411
### 2025 0412 0119
[Matios Berhe](https://www.twitter.com/MatiosTV) suggests I read
- Kant's Critique of Pure Reason, chapter 2, section 12
- Kant's Critique of Pure Judgement, Critique of the Teleological Judgement
- Locke's Essay Concerning Human Understanding

I have now added these to my long list of "stuff to read".
Some day, sooner rather than later, I shall provide a complete list of texts in my library and the schedule on which I read them.
There are two main reasons to do so:
1. to point up the origins of what I have to say (so that others do not suspect me of originating, initiating, or creating my verbal behavior)
2. to better scheduling my reading and writing.

I already have a copy of Kant's Critique of Pure Reason that I read first when I was in high school.
There are a few texts from that time which have survived long enough to find themselves on my shelves.
One of the treasures from that time is Russell's "Problems of Philosophy".
In no way did I learn all that Russell and Kant had to teach in their books when I first read them so very long ago.
There is something special though about reading things that you do not yet understand.

The more I read the more I don't at first understand, and yet, at the same time, the more I learn about the world as a whole.
There is something about reading which makes the world bigger and smaller than it once seemed.
When I'm not in such a metaphorical mood, I am quick to say the world is no bigger nor no smaller than it is: to say otherwise is to plunge such a theory into contradiction from which no logic prevents each and every imaginable conclusion.
People are usually scared by contradiction for the wrong reasons: this is somewhat corrected by phrases like "law of explosion".
The problem with contradiction is not really a problem: when you have a contradiction you find yourself without the benefits of a consistent theory.
Some people are ok with that.


### 2025 0412 0102
I finally have a place to put all my daft drafts without chopping them into little tweet size pieces.
Why has it taken me so long to return to this long form method of writing?
It's mostly that strange feeling I get that this is like a message in a bottle sent out into an enormous sea of nothing.

Once I figured out that it wasn't sending the message that mattered most, I warmed up to the practice of writing from my little island on the internet.

### 2025 0411 2248
I'm implementing a little LISP.
Like the LISPs of yore, there are two basic items: pairs and atoms.

Pairs split into left and right parts.
Paul Graham wrote of "halves" when explaining pairs in [Bel](https://www.paulgraham.com/bel.html).

Whether you talk about the anatomy of a pair by "breaking it into pieces", "splitting it into halves", or "decomposing it into components", there is one rule that governs the logic of pairs: 

> pairs are identical where and only where their left components are identical and their right components are identical.

This is otherwise know as "the law of extensionality of ordered pairs".
It is a fancy way of mentioning the precise conditions upon which to count two items in a universe of ordered pairs as identical.
The single premise "each item is (a,b,c,x,y, and z such that a pairs b with c, x pairs y with z, and a is identical to x, if and only if b is identical to y and c is identical to z)" logically implies each conclusion of *any* theory of ordered pairs.

Now is not the time or place to get distracted by problems of logic, theories, and ordered pairs.
As much as I would enjoy a full digression on the origins of logic, the extensionality of effective theories, and ordered pairs as a paradigm of philosophic investigation, those are not the problems that I am presented with here and that I have to solve now.

Back to my little LISP.

Pairs are identical where and only where their parallel components are.
For now, "(x,y)" is short for "the pair whose left part is x and whose right part is y".
Thus, (x,y) is identical to (a,b) where and only where x is identical a and y is identical to b.

Since I'm coding up my little LISP in javascript I'm using the following definitions:

```
let cons=(x,y)=>[x,y]
, car=x=>x[0]
, cdr=x=>x[1]
```

These just say that 'cons' is the name of a function that takes two arguments and returns an array whose zeroth component is the first argument and whose first component is the second argument (note this confusing sentence is why some people prefer different methods of indexing into arrays of data).
The function named 'car' takes one argument and returns the item at index zero of it.

> If you do not already know how javascript arrays work, and how indexing into javascript arrays works, then these explanations are not helpful.
> Thankfully, if you do not already know how javascript arrays work, you get the joy of learning that first before coming back here.
>
> I have not yet found a faster way to introduce programming than to present it after teaching logic up to and including Quine's main method (of proof for predicate logic).
> Since more people are familiar with javascript than they are with Quine's main method-- or with the methods of logic-- I shall continue to avoid what might otherwise appear as an entirely roundabout way of teaching programming from scratch.

The function named 'cdr' takes one argument and returns the item at index one of it.

Both 'car' and 'cdr' are names inherited from working with programmable machines (aka "computers") during the late 1950s and early 1960s.
If you want to learn more about where 'car' and 'cdr' come from, and perahsp even how to pronounce 'cdr' (which I say as "could-er"), then track down McCarthy's history of LISP and give it a compassionate read.

With 'cons', 'car', and 'cdr' we can make pairs, get their left part, and their right part respectively.
There are algebraic or equational ways of introducing these basic functions e.g.

```
car(cons(x,y))==x
cdr(cons(x,y))==y
'car(x)==car(y) and cdr(x)==cdr(y)' is equivalent to 'x==y'
```

but again this is just a distraction from the problem to solve (implementing a little lisp like language).
Note, such purportedly algebraic or equational theories appear to follow entirely from pure logic when Quine's method of schematic theories is extended to schematic theories of functional predicates (see Quine's 'Philosophy of Logic' for a schematic theory of identity).

Following the conventions established in the wonderful book "The Little Schemer" by Friedman and Felleisen, cdr and car are only used in contexts where their arguments are pairs with left and right parts.
There is one pair without any parts.
It is unique (in more ways than one).

```
let nil=[]
, id=(x,y)=>x==y
, empty=x=>id(x,nil)
```

The item named 'nil' is an empty javascript array.
Not all empty arrays are equal in javascript.
Equality, identity, and indiscernibility are hard to deal with when you don't start with logic.
Surprisingly, 'id' works almost like it does in a language like [Bel](https://www.paulgraham.com/bel.html) even though it is not obvious that it should.
Remember the 'law of extensionality of ordered pairs'?
Well, it is violated by almost all programming languages.

There is a silent assumption that is among the many hidden away behind any explanation of a foreign programming language: they actually run on real machines.
On most machines there are places and objects at those places.
Objects in different places can't be identical!
(Think that over: identical things can't differ in place apparently.)
And yet, sometimes they are!

Pairs may have parallel components that are identical without being identical themselves.
This happens when two pairs are silently assumed to be in different places but each of them places their components *in the same places*!
There are lots of solutions to this problem of identity.
Most of them are needlessly complex and overburdened with crusty philosophies.
My solution is as sad as it is simple: just go with what javascript gives you.

My real solution is to go with logic: identity is indiscernibility in that any two predicates which fill out each instance of the premises of identity are coextensive and indiscernibiltiy is one such predicate (in a logical theory with a finite lexicon).
For more on this see Quine's "Philosophy of Logic".

Atoms are not pairs (and pairs are not atoms).

```
let pair=x=>Array.isArray(x)
, atom=x=>!pair(x)
```

Is nil an atom?
No, not here.
Is nil an atom elsewhere?
Sometimes.
Early LISPs only had pairs and symbols.
Symbols are often a large part of LISPs.
They are the workhorses of reference: they end up sometimes playing the part of proper nouns and sometimes playing the part of pronouns.
Here again, there is much to be learned from predicate logic.
Here again, I must avoid dipping into such unfamiliar wells of simplicity.

The general problem of atomic items provides ample opportunity to make use of degenerate cases of common objects e.g. Quine provides for atoms in his "Set Theory and its Logic" by picking out atoms with the reflection of membership i.e. 'x is atomic' is short for 'x belongs to x'.
Most people avoid making convenient use of alarming degeneracies: they're really missing out on some good fun.

Atomic items and empty items have a long and sordid history.
Atoms are not supposed to have parts, and yet we are often forced to say that they do.
Empty items always end up behaving like atoms in many respects-- e.g. they are empty and presumably "don't have parts"-- but they almost always tend to be unique where as there tend to be many many atoms in any given theory.

The only place I have found clarity on these boundary problems is, you guessed it, logic.
In a theory of ordered pairs, it is enough to provide for the existence of an ordered pair that neither has a left part nor has a right part.
It is then unique since any two purportedly distinct empty pairs have identical parallel components (vacuously).
Atoms are then accomodated by one of a few methods, each of which match up with Quine's atomic methods in "Set Theory and its Logic".
An item of a theory of ordered pairs is atomic when it is identical to its components (or, just one of its components, in which case the other components allows us to distinguish between atoms with, e.g., identical left components: are these even atoms?).

Sadly neither of these principles is used in LISPs.
Again, this is because there is that silent assumption working in the background of most explanations: the programming language runs on machines in our shared material world.

This is enough thinking on this for now.


### 2025 0411 2217
I can not guarentee that there will be no spelling errors in what I write.
Rarely do I spell well, and rarely do I take the time to check my spelling.
Rather than check my spelling I would keep on thinking.
Spell checking stops thinking except when looking up the word to be spelled spurs some better bits of writing.
Definitions have the power to prompt better writing when they present short and sticky explanations.
Those sensitive to spelling errors will be greatly disappointed (that is until I go back through and spell check this document, that is, if I ever get around to doing that).

### 2025 0411 2158
I do not have much familiarity with using this software and can not be certain how what I've written here will appear.
I am also not familiar with markdown.
Will there be new lines in the final document if I put them here?
The answer is no.

Does this start a new paragraph? The answer is yes.

Does this [20250411](#2025-0411) automatically.

ok, rather than get lost in the details of editing I will just go on and start writing.

# What I Must Do Before I Die
Discover, predict, and control changes in counts, rates, and accelerations as selections from variations on physical, chemical, biological, behavioral, and cultural scales by making and maintaining strong practices mediated by strong people marked by strong principles from the sciences of logic (denotative, Boolean, and functor), mathematics (calculi, collections, and categories), physics (quantum field theory, statistical thermodynamics, gravity), chemistry (phyiscal, biophysical, and biological), biology (oranelles, organisms, environments), behavior (biological, biosocial, social), and culture (history, technology, survival).

## 2025 0419

### 2025 0419 1349 The Parts of "The Introduction to Philosophy" and My Outlook
The readings in the fourth edition of Perry, Bratman, and Fischer's "Introduction to Philosophy" are divided into six parts:

1. Philosophy and the Meaning of Life
2. God and Evil
3. Knowledge and Reality
4. Minds, Bodies, and Reasons
5. Ethics and Society, and
6. Puzzles and Paradoxes.

Each part is filled with selections from primary sources.
One of my aims is to cover the breadth of these sources and to supplement that breadth with the depth of the texts from which these selections were made.

There must be no doubt as to where I am coming from when looking over the data of philosophy: they are records of responses whose origins are in through reinforcing practices of verbal communities whose cultures evolved by selection from variations on cultures past.
Such reinforcing practices were and are mediated by bits of behaving biology: organizations mediated by organisms mediated by organelles and so on.

There are doubts as to what specific chains of contingencies selected these organisms, these behaviors, and these cultures.
The boundary between what is known and what is unknown is often, and perhaps unavoidably, blurry.
Science and logic have done better than the rest of human's practices to uncover the shape of our strongest doubts.
There appears to be no better reason for them than that.

What ohters have to say about the world, or the worlds, enters into a mix.
The soup of social behaviors is seasoned by each of us.
Our favorite flavors are contingent upon the ingredients with which we are endowed.

Philosophy is a part of science.
The behavior of the philosopher is behavior and is, hence, part of the science of behavior.
Psychology as somethign other than the science of behavior is part of the speculations of philosophers: it too is a part of science as much as any pseudoscience is part of what is to be explained by a complete account of human behavior.

Just as the molecular biologist can spend their life po9uring over the peculiairites of abberant corners of the ocean, so too can teh experimental analyst probe the behaviors and practices of prescientific practitioners.
They are, after all, the progenitors of modern science, which, if human practices are to be taken as a going concern, is the porgenitor of future sciences largely unknown or perhaps even the progenitors of some more effective or, goodness forbid, less effective practices beyond science itself.

To read philosophy is to triangulate: where iswhat you have to say located with respect to what others have to say?
How far can speaking and listening take each of us when dealing with our shared world?
These are among the questions I ask, and which others have asked and answered well before me.
What can be learned from listening to my self as I do those others?
As an other to my self, what can be said of my speech and its relation to the speech of others better spoken than mine?

With that I turn to the texts, and I shall return here to tease out more questions, and, if I am so lucky, to find the threads of wisdom that often pose as our best tentative answers to them.

## 2025 0418

### 2025 0418 1601
A new mistake I make with markdown to add to the first I identified in [202504171525](#2025-0417-1525):
> I put the link inside parenthesis and then put the display text inside square brackets AFTER the parenthesis! 

### 2025 0418 1456
This continues the work on my little lisp from [202504171613](#2025-0417-1613).

I apologize for these fragments of progress on this particular project: I've decided to just get things done as they occur to me rather than go out of my way to first make them more easily explainable up front.
Everything will still be completely explained, but I am certain there are better ways of writing this all out than what I have done thus far.

Runes are special atoms that are designated by javascript strings that start with the "runeMark":

```
let runeMark = '^'
, isRune = x => ('string'== typeof x ) && runeMark == x[0];
```

Examples:
```
isRune('^test') 
 true
isRune('test') 
 false
```

A list is runic if it is proper and each of its left parts is a rune:
```
let isRunic = list => isNil(list) || (isRune(carOf(list)) && isRunic(cdrOf(list)));
```
Runic lists shall paly the part of strings in my little lisp (for now because I dont' know what consequences may come to select a new design).
This follows the convention of [Paul Graham's Bel](https://www.paulgraham.com/bel.html) where he spoke of chars I speak of runes: for this project chars are always native javascript.
The strange distinction must be made to preserve the distinction between symbols in lisp, runic lists, and javascript strings: this is something that has already tricked me and may very likely trick you as well!

I'll write some examples for the function designated by 'isRunic' after I introduce some slightly edited string functions from [Bit Strings and Binary Trees](#2025-0413-1513-bit-strings-and-binary-trees):

```
let emptyString=''
, isIdenticalString = (x,y) => x==y
, isEmptyString = string => isIdenticalString(string,emptyString)
, firstCharOf = string => isEmptyString(string) ? emptyString : string[0]
, restCharsOf = string => isEmptyString(string) ? emptyString : string.slice(1)
, concatenationOf = (...strings) => 
   strings.length ? strings.shift() + concatenationOf(...strings) : emptyString
, isString = x => 'string' == typeof x
, isRuneMark = x => isIdenticalString(x,runeMark);

isRune = x => isString(x) && isRuneMark(firstCharOf(x));
```

This is also the first example of redefining a variable in javascript: 'isRune' comes to denote a function specified in language more like the idioms I've adopted so far with their 'of's and 'is's.
The names are much longer than I'd write if I was writing just for my self, but I'm not doing that am I?
Examples (I do not yet have a comprehensive method of testing, but have tested out a few different methods of testing and so far examples are good enough):

```
isRune(concatenationOf(runeMark,'test')) 
 true
isRune('test') 
 false
```

Next, the two functions that go from strings to runic lists and back again:
```
let runeOf = char => concatenationOf(runeMark,char) 
, runicListOf = string => 
  isEmptyString(string) ? nil 
  : consOf(runeOf(firstCharOf(string)), runify(restCharsOf(string)))
, charOf = rune => restCharsOf(rune)
, stringOf = runicList => 
  isNil(runicList) ? emptyString
  : concatenationOf(charOf(carOf(runicList)), stringify(cdrOf(runicList)));
```

Examples:
```
isRunic(runicListOf('this is a test')) 
 true
stringOf(runicListOf('this is a test')) 
 this is a test
```

Why start with strings and runic lists?
Runic lists are the foreign companion to javascript's native strings and they shall be the porthole through which the foreign items of my little lisp are displayed in our native tongue.

The first main part of my little lisp that must be made is the printer.
It has to take a pair or an atom and transform it into a runic list that can then be read by our human eyes.
Perhaps if we could see into the computer like superman with x-ray vision, then we wouldn't need to worry about making a printer.
But, making a printer also helps when making a reader which transforms key presses into foreign code.

For my little lisp there is only going to be one kind of atom: symbols.
Runes are then special symbols: those that start with the rune mark as above.
What was spoken of as 'strings starting with the rune mark' shall now be spoken of as 'symbols starting with the rune mark'.
While I'd rather not use the word 'symbol' because it leads to questions like "What does this symbol symbolize?" there is a long history of its use in lisp and I am not yet prepared to break that chain of consequences.


### 2025 0418 1453
The notation used to [explain The Popr Programming Language](https://www.hackerfoo.com/posts/popr-tutorial-0-dot-machines.html) is very much like the notation I devised for my own language, and is a clear indication that there is a convergence of notational practices that lend themselves to visualization near these concatenative methods.

It is sad that they are spoken of as "concatenative" when, as far as I can tell, this is just one way of elaborating on any principles of programming stack machines.
Without the concrete contingencies of stack machines there is no concatenative programming and concatenative methods may not even end up being the most appropriate for stack machines.
Again, as far as I can tell, the key to a specific method of programming is how you would write an interpreter for that programming language in that programming language.

This is not just some honorary show to LISP: it is the most accessible way to bring up the logic of the theory upon which a given method of programming is built.
It forces the programmer to confront the difference between use and mention, logic and theory, and often reveals the language's place on among models of computability.


### 2025 0418 1439
A major conclusion from the substance of [accumulating the links](#2025-0417-2020) to papers on what is called "concatenative programming languages" is that modern writers do not know the difference between a theory and its logic.
The inability to notice that the predicate functors of Quine are not combinators as in the more familiar combinatory calculi is a great sadness: both to our sciences and to our teachers.

Finally done accumulating the links that I wrote down.
Now on to my little lisp.

## 2025 0417

### 2025 0417 2026

All the links across the internet are rotting.
Searches are no longer helpful: one system often says that there is nothing relevant while another quickly brings what you're looking for to the top only to bury it later.
My saddness knows no bounds.
There are a few people that could break out of the environments which control them (and the world) in unhelpful ways, but they have no good reasons for doing so.

If your shit isn't in plain text documents then it is more likely to rot than anything else.
There are no easy ways to accept or deal with this: it is a kind of culture war both foreign and yet somehow familiar.

### 2025 0417 2020

Some links to papers and presentations on concatenative programming languages and related things:

* ["Stanford Seminar - Concatenative Programming: From Ivory to Metal" by John Purdy, November 15, 2017](https://www.youtube.com/watch?v=_IgqJr8jG8M)
* ["The Theory of Concatenative Combinators" by Brent Kerby 2002](http://tunes.org/~iepos/joy.html)
    > This is unlikely to be around for long: it will be too hard to find it.
* ["Linear logic and permutation stacks—the Forth shall be first" by Henry Baker 1994](https://dl.acm.org/doi/10.1145/181993.181999)
* ["Iota and Jot" by Chris Barker 2001](https://en.wikipedia.org/wiki/Iota_and_Jot)
   > There is a lot more to this one than can be got at right now.
   > I really do hope that someone is out there (besides just the way back machine) keeping track of things like this before all is lost.
* ['Chris Barker's Iota-Jot-Zot family of esolangs' 2020 by Ilia Chtcherbakov](http://cleare.st/code/iota-jot-zot)
   >Ilia shares my unhappiness with broken links directly to Chris Barker in this one!
* [Chris Barker's current homepage](https://cb125.github.io/) (who knows how long it will be up?)
* [The Joy Programming Language](https://hypercubed.github.io/joy/joy.html)
* ["Concatenative programming and stack-based languages" by Douglas Creager](https://www.youtube.com/watch?v=umSuLpjFUf8)
* [the online space of Douglas Creager.](https://dcreager.net/)
* [Douglas Creager links to concatenative programming stuff](https://dcreager.net/concatenative/)
* [“Factor: A Dynamic Stack-based Programming Language” 2010 by Slava Pestov, Daniel Ehrenberg, Joe Groff](https://dcreager.net/papers/Pestov2010/)
* ["A denotational semantics of a concatenative/compositional programming language" Jurij Mihelič, William Steingartner, Valerie Novitzká. 2021](https://dcreager.net/papers/Mihelic2021/)
* [Robert Kleffner. “A foundation for typed concatenative languages”. Master's thesis, Northeastern University. April 2017](https://dcreager.net/papers/Kleffner2017/)
* ["Foundations of Dawn: The Untyped Concatenative Calculus" by Maddox](https://www.dawn-lang.org/posts/foundations-ucc/)
* ["Continuation-Passing Style, Defunctionalization, Accumulations, and Associativity" by Jeremy Gibbons 2021](https://arxiv.org/abs/2111.10413)
* [The Popr Programming Language by Dusty DeWeese](https://www.hackerfoo.com/posts/popr-tutorial-0-dot-machines.html)
    * [on github](https://github.com/HackerFoo/poprc/?tab=readme-ov-file)
* [Functional Bits: Lambda Calculus based Algorithmic Information Theory John Tromp April 23, 2023](http://tromp.github.io/cl/LC.pdf)
* [Tree Calculus by Barry Jay ](https://treecalcul.us/)
* [John Earnest](http://beyondloom.com/)
* [no stinking loops](http://www.nsl.com/)

It is in large part because of the difficulty in assembling this list of links (and finding the sources mentioned by the documents pointed to from these linked sites) that I have largely committed to monolithic methods of writing.
It is not what I would prefer, but it is also not so inconvenient when what I'm ultimately going for is an accurate record of the evolution of my verbal behavior.

### 2025 0417 1613
This continues work on my little lisp from [2025 0415 1548](#2025-0415-1548).

Nil is a proper list and any pair whose right part is a pair whose right part is itself a proper list is a proper list; and a nonproper list is called a dotted list.

```
let isProperList = x => isNil(x) || (isPair(cdrOf(x)) && isProperList(cdrOf(x)))
, isDottedList = x => !isProperList = x;
```

Historically, a pair whose left part is designated by 'x' and whose right part is designated 'y' was designated by '(x . y)' and '(x y)' was short for '(x . (y . nil))' so that, in general, '(x y ...z)' is schematically short for '(x . (y . (...z . nil)...))'.
Thus, the only abbreviations which contained a dot were those whose right parts were nonnil atoms e.g. '(^a ^b . ^c)' is short for '(^a . (^b . ^c))' and '((^a ^b) . ^c)' is short for '((^a . (^b . nil)) . ^c)' where all atoms beginning with '^' designate their self.



### 2025 0417 1525

Some writing projects that I may never get around to:

* [Metamath](https://us.metamath.org/) isn't like Quine's schematic methods (even though they go into a lot of detail about how they extend Quine's schematic methods)?
    * The theory upon which metamath is based: [Megill, “A Finitely Axiomatized Formalization of Predicate Calculus with Equality,” Notre Dame Journal of Formal Logic, 36:435-453, 1995](https://us.metamath.org/downloads/finiteaxiom.pdf)
    * Some explanation of how that theory works in practice: [Metamath is a metalanguage that describes first-order logic](https://us.metamath.org/mpeuni/mmset.html#mmname)
* FORTH and the final sentence of McCarthy's [history] of LISP.
   > Who am I kidding, I'm definitely going to write about that.
* [The Kitten Programming Language](https://kittenlang.org/) and its relation to FORTH.
* [Big History](https://en.wikipedia.org/wiki/Big_History) and ages old cosmological methods.
* more to come...

> Side note. the markdown convention of using square brakets around the display text for a link which is given in curved brackets does not work: I constantly put square brackets where curved brackets go and vice versa.
> This seems like something that would have been quickly uncovered with a little "beta testing".
> Dare I say that a solution may be found in some method of slashes? 

## 2025 0416

### 2025 0416 2358
Today is the first day that a problem I've had for some time was clearly revealed to me: I'm having trouble keeping up with my self.
Hopefully tomorrow I can get out some more work that I did yesterday and today:

* Paths in binary trees and bit strings (still haven't found a beautiful bijection between bits and trees)
* I never finished the [latest entry on my little lisp](#2025-0415-1548) (even though I worked a lot more on it that day)
* I never got around to writing out what I meant by "we live in a stack based world".
* Read more of Durant's "story of philosophy" and failed to write down any of what I had to say (just because I have something to say does not imply it is even remotely important, but it is easier to make that judgement once I've said it on a page than to my self).
* Haven't even started writing about working out through setbacks: I want to capture the pain of that dip and better understand why it always seems like a bigger deal than it ever really is.
Something about that old saying "You're stronger than you think." e.g. there is greater strength which your culture has yet to release.

Oh! And the Durant reading was on Greece, Socrates, and Plato.
To my great surprise, I just discovered that Whitman's 1865 "O Captain! My Captain!" is specifically described as a "Ship of State" metaphor which is most famously mentioned in Plato's "Republic" Book 6.
This is something like an example of what I mean when I say, metaphorically, reading always makes the world seem larger and smaller than it once was.

### 2025 0416 2313
I left a note for my self on twitter.
This is me making the note I'd made a note for my self to note.

The account @ResonantPyre, a friend, wrote a few paragraphs on Wittgenstein's 1935 'Lectures on Personal Experience'.
Nonsense is under the microscope.
'Sound and Sense' was the title of a book on poetry that I was forced to buy to finish some requirement of my undergraduate education (little did I know what a wonderful friend I would find in that professor even though I was a fish out of water).
I defaced my copy by prepending a 'non' at each place where 'sense' occurred.

Meaning, sense, semantic agreement, and all other spooks of sentences are cast out, reluctantly, with the full thrust of Ramsey's proxy functions encapsulated by Quine's "cosmic complement".
Indeterminancy of reference alone does not preclude propositions (the poltergeists of sentences).
On occasions we fix our meaning of a sentence to outright intersubjective agreement: *unus pro omnibus, omnes pro uno*.

The problem with propositions--- meanings made eternal--- is their purported transcendence: everywhere forever, forever everywhere.
A well paraphrased verbal response stands for now and does well enough without proposing propositions.
We lose nothing by leaving those who are lost without them: they come back around eventually.

We go to dictionaries "to get the meaning of the word" and when we open to the right page we get on where we left off.

There's obviously much more to this all than anything like the flob above.
It's okay to have some fun when you fill it out with fuller explanations sooner rather than later.

### 2025 0416 2234 O Russell! My Russell!

When I was young, and still very much under the control of Russell's *Principles* and *Principia*, I found my way to Egner and Denonn's "The Basic Writings of Bertrand Russell" and clung to it as an other might the primary text of their religion.
Everywhere I found the methods of Principia applied to Russell's world.
That world was one I hoped to glimpse within my own.

It is only now, some fifteen years later, that I see less of Russell and more of logic in his memorabilia.
It was always the logic of his works, and not their well written projections, that caught and kept me reading.
The error of Russell's ways were somewhat corrected by him.
Clarity and exactness were logic disguised and upon confronting this perversion (he excised logic overtly in "An Inquiry into Meaning and Truth") he left the fruits of philosophy for something more attractive to modern minds.

Logic looms throughout his works, but the mind left him open to mystical influence.
He rested in a way somewhat more obscure than Descartes "I think therefore I am."
It is, for me, the difference between singular terms and Quine's elimination of them by Russell's own singular descriptions.

Expedience was never sufficient for Russell, there was always more to it than just that.
Thus, even though he is so often worshipped as a controversial figure, it has gone unnoticed that public comprehension is a halmark of convention (whether such conventions are in vogue is another matter e.g. nothing sells quite like good versus good).

Russell was no real threat to the world.
For all that was hoisted upon him through his personal affairs, he made a cuckold of Frege and logic got lost in the shuffle.
Quine remediated what Russell ruffled.
Carnap did too, but never got far enough from Russell's outlook e.g. his philosophy of physics is limp.

What once of logic had exhausted Russell was energized by Quine: sprawling theories and unspooled speculations were cut off from their metaphysical wellsprings and reality pierced the veils of clarity and exactness.
Rather than complexify the world, Quine made due with the complications of a worldclass watchmaker.
But whither the warmth of love and the sensations of the soul amongst such machinations? 
Those who clutched for them betrayed their own insecurities.

Principia Mathematica was a report from the frontlines of the final war which we wage on to this day.
It tormented him for the rest of his life: had he done more to help the world or to hurt it by having so vividly revealed the brutality of the battlefront?
There seemed to be no peace nor no hope in the trenches.
This would not keep the homefires burning: what else was he to do?

## 2025 0415

### 2025 0415 1915
As is often the case, I have added some other books to read in parallel with Druant's "The Story of Philosophy". They are
* Russell's "The History of Western Philosophy", and
* Grayling's "The History of Philosophy".

I'm pretty sure those are the only books in my library that are directly aimed at giving a sorta story or a sorta history of philosophy as a whole.
There is also the classic

* "Introduction to Philosophy: Classical and Contemporary Readings, 4th edition" edited by Perry, Bratman, Fischer

that may come up in what may be a larger piece of the reading puzzle I have to solve in order to do all that I must before I die.
As much as I recommend people go directly to primary texts, there is no good way to get started on the whole of a thing than by reading how someone else did it: there are some writers who can explain what they did as if it was done by an other and they are to be cherished.
We're all human, in the end, and that alone binds us into the most startling of enterprises. 

### 2025 0415 1557

It seems that people who were a part of history wish they had written down more of the history of which they were a part.

There is less to learn from what was forgotten than from what was remembered.

Time tramples all.

> This occurred to me while reading [John McCarthy's 1979 "History of Lisp"](https://justine.lol/sectorlisp/lisp-history.pdf).

### 2025 0415 1548

First a summary of the work done on my little LISP from [202504112248](#2025-0411-2248)

```
let consOf = (car,cdr) => [car,cdr]
, carOf = cons => cons[0]
, cdrOf = cons => cons[1]
, nil = []
, isIdentical = (x,y) => x==y
, isNil = x => isIdentical(x,nil)
, isPair = x => Array.isArray(x)
, isAtom = x => !isPair(x);
```

In English (that carefully distinguishes between quotations and their purported designations for reasons that shall be explained later),
* the function designated by 'consOf' takes two arguments and gives back a javascript array with the first argument indexed by zero and the second argument indexed by one;
* the function designated by 'carOf' takes one argument, which it expects is the result of an application of 'consOf', and returns the item of the argument indexed by zero;
* the function designated by 'cdrOf' is like the one designated by 'carOf' but it returns the item of the argument indexed by one;
* the item designated by 'nil' is a javascript array of length zero;
* the function designated by 'isIdentical' takes two arguments and returns the javascript item designated by 'true' where the first argument is `==` to the second (in the language of javascript) and returns the javascript item designated by 'false' elsewhere;
* the function designated by 'isNil' takes one argument and returns the js item deisgnated by 'true' where it is `==` to the js item designated by 'nil', and returns the js item designated by 'false' elsewhere;
* the function designated by 'isPair' takes one argument and returns the js item designated by the application of 'Array.isArray' to it; and
* the function designated by 'isAtom' takes one argument and returns the js item designated by 'false' if the js item designated by the application of the function designated by 'isPair' is designated by 'true', and the js item designated by 'true' otherwise.  

From my work on [Bit Strings and Binary Trees](#2025-0413-1513-bit-strings-and-binary-trees), the practice of designating a function by ending it in 'Of' and designating the functional representation of a predicate by beginning it with 'is' helps a lot when working out the logic of the programs under construction.
This is also a partial explanation for all the 'designated by's and quotations that spell out the letters of what most people would call the *names* of the designated functions or objects.

I'm writing here now after having written the following section to say that it goes a little too far from talk about programming a LISP in javascript and if that's all that you care about then you can just skip past this break to the next one.
What you're missing is a more detailed explanation of the general plan for dealing with syntax and semantics.
It doesn't get too deep, but does go even further out of bounds than I can presumably tolerate.

---

As much as there is the temptation to write phrases like 'the function consOf' or, better, 'the function `consOf`' which resorts to a different typeface in order to weakly emphasize that there is something different about the purported designatum of the so rendered phrase, this has only led to decades and centuries of mistakes and wasted efforts promulgated by untaught writers and readers.

This is especially the case when writing and reading programs e.g. the problems that so many people have with so called 'pointer arithmetic' is no such simple thing as a 'bad design decision of a particular programming language'.
The problems of meaning and reference, the life blood of semantics, crop up wherever languages are planted (which includes some of our most powerful social technologies).
They are fundamental problems that can not be swept under the rug before the guests arrive.

When a writer leaves it to the reader to figure out what is supposed to be a quotation and what purports to be designated by such a quotation, there is only hell to pay.
Even if the writer commands the reader to take on the responsibility of "keeping conventions in mind" this only works if there are other reasons for them to do so (I think of Kleene's 1952 "Introduction to Metamathematics").

This problem is not as unfamiliar nor as fussy as it seems e.g. classic problems of scoping in logic and in programming are fundamentally about not having resolved a concrete method of reference or meaning which deals well with syntactic and semantic agreement.

Rather than ignore the fundamental problems I'll pick the strongest, perhaps tentative, methods that are already within my reach and take it from there: nothing is as final as finality claims to be.

I have already mentioned Quine multiple times throughout these notes and to anyone who is so unfortunate to have followed me on twitter.
As much as people detest Quine's obstinate rigor (a term that I only just discovered is already stated poetically as "Ostinato Rigore"), the articulation of his uncertainty is beyond anything else that I've seen.

The plan is simple: semantics breaks into reference and meaning, reference breaks into designation and denotation, and denotation 

> "is where the action is. It takes designation in stride, for a singular term can be recast as a predicate that happens to denote just one thing if any.
> The singular term 'Boston' *designating* Boston, can be reconstrued as a predicate 'is Boston', *denoting* only Boston.
> Anything said about Boston can be paraphrased using 'is Boston'."[pg. 59 of Quine's "From Stimulus to Science"]

Quine shows just how denotation comes to be defined in consistent theories by first generalizing Tarski's method of defining truth to denotation and then admitting a hierarchy of predicates of denotation, finite in number for a given theory of denotation.
This settles, once and for all, the work of denotation in a theory which is rich enough to admit a method of semantic ascent e.g. by way of quotations.

While such a hierarchy of predicates of denotation are less than what most expect, e.g., from a comprehensive theory of truth ("Truth, one might risk being quoted as saying, is just a degenerate case of denotation."[pg. 65 Quine's "From Stimulus to Science"]) it is all that can be got without plunging any such theory into the classic antimonies.

Now, among the practices of programming there are those called "denotational semantics".
This is but one way of explaining what so many people speak of as the meaning of a given program.
It is much easier to see how all such talk paraphrases into Quine's methods by beginning with a programming language which drops variables from the beginning and hence eases the complexity of any subsequent definition of programmatic denotation as a projection from a logically consistent theory of denotation.

Sadly, now is not the time or place for me to complete such a project.
As almost always seems to happen when working on interesting things, other interesting things come up, they are temporarily entertained for the sake of maintaining momentum, and then when they start to take over and drive at a new direction, there's nothing left to do but drop it and return to where the enthusiasm last let off on the path you had original planned.

I won't leave the other part of semantics hanging though: meaning.
The simplest explanation of how I deal with it is to point again to Quine.
This is not because I accept Quine without criticism or correction, but because there is no one who has yet combined the philosophies of Quine with the philosophy of Skinner's radical behaviorism in the way that seems to be characteristic of my outlook and actions in regards to these matters.

In the classical story, sentences have meaning.
It is what a sentence means that matters most when we speak with each other.
If you don't know what a sentence means, then the sentence is effectively worthless to you.
Sentences are merely the aftereffects of meaning.
In the metaphor of traditional signals processing, it is sentences that send the message but it is the meaning that is encoded or decoded from the sentence.

Skinner's analysis of verbal behavior ends all speculation as to what potential philosophic value there can be to disputing the existence or nonexistence of meanings (or information, in the modern parlence, or propositions in more sacred realms).
But, meanings die hard, and it is harder still for most to imagine a world without meanings much less to get along in one.
While Skinner drops meanings outright and simply begins without them until he comes around to the verbal behavior that we otherwise said would not occur without the existence of meanings, Quine gives traditionalists a good shake.

He starts with "sameness of meaning" rather than with meaning.
At least we can say when two sentences "have the same meaning" if we can not say what the meaning is, or if we might somehow get along without purporting anything more than a predicate "means the same as".
Just as in the case of denotation, I can not go through how "sameness of meaning" goes broke.
The concluding outlook is this:

* reification through joint reference in that weaker form of universal closures of conditionals that Quine calls "focal observation categoricals" and which burgeon into micro spatiotemporal theories of the world cover their own ground, i.e. we are justified in speaking of "sameness of reference" between the pronouns of focal observation categoricals,
* these very grounds give us the power of intersubjective sameness of reference with repsect to gross bodies (this being little more than the classic "by ostention"), and
* abstract items, such as orderd pairs, receive only the most conservative of treatments
   > "I submit that intersubjective sameness of reference makes no sense, as applied to abstract objects, beyond what is reflected in successful dialogue." [pg. 70 Quine's "From Stimulus to Science"].

For more detail on any of these things I can point to the text quoted, oh so many times in this entry, Quine's 1995 refinement of his 1990 talk "From Stimulus to Science".
It is a hard read, something which I have said many times before, but the value of each of its words and sentences is second only to the value of his "Methods of Logic 4th edition".

That all being said...

---

Thankfully I wrote up all that thinking because, for some reason which is not clear to me, it let me see that 'nil' is to designate a symbol/atom and that should help me with the design of this little LISP!

Here is the code from above revised 

```
let consOf = (car,cdr) => [car,cdr]
, carOf = cons => cons[0]
, cdrOf = cons => cons[1]
, nil = 'nil'
, isIdentical = (x,y) => x==y
, isNil = x => isIdentical(x,nil)
, isPair = x => Array.isArray(x)
, isAtom = x => !isPair(x);
```

This entirely avoids the problems that come with 'nil' designating a javascript array.
So the key examples that show the critical change are:

```
isPair(nil) 
 false
isAtom(nil) 
 true
```

The 'LISt' in 'LISt Processing' is defined inductively as

1. the item designated by 'nil' plays the part of the empty list
2. a list whose first item is designated by 'x' and the rest of whose items are listed by the item designated by 'y' is designated by 'consOf(x,y)'.

Recursively we define proper lists as 
```
let isProperList = x=> isNil(x) || (isPair(cdrOf(x)) && isProperList(cdr(x)))
, isDottedList = x => !isProperList(x);
```

As the second definition suggests, a list which is not proper is called dotted because of the historic practice of writing a pair whose left part is x and whose right part is y as '(x . y)'.
This matters because when printing atoms and pairs there will be times when a dotted list must be constructed so that the person looking at the printed result can tell the difference between '(^a ^b)' and '(^a . ^b)'.

## 2025 0414

### 2025 0414 2055
This entry continues my read of Will Durant's "The Story of Philosophy" from [202504132323](#2025-0413-2323).

The introduction hovers around a familiar analogy: science is to knowledge as philosophy is to wisdom.
He says that science analyzes and philosophy synthesizes; that science takes apart what philosophy must put back together; that science gives us the power to do only what philosophy can tell us is worth doing; that science without philosophy has no value in that science without philosophy is like a fact without a feeling; that we must not forget that science is a descendant of philosophy; and that philosophy brought science into this world and can just as easily take it out of this world.

Next, he breaks apart philosophy in the traditional way, only to follow his own advice and focus on "the great men of philosophy" rather than the great fields of philosophy:
* Logic "is the study of the ideal method in thought and research: observation and introspection, deduction and induction, hypothesis and experiment, analysis and synthesis" [pg. 3]
* Esthetics (or Aesthetics) "is the study of ideal form, or beauty; it is the philosophy of art" [pg. 3]
* Ethics "is the study of ideal conduct; the highest knowledge, said Socrates, is the knowledge of good and evil, the knowledge of the wisdom of life" [pg. 3]
* Politics "is the study of ideal social organization (it is not,a s one might suppose the art and science of capturing and keeping office); monarchy, aristocracy, democracy, socialism, anarchism, feminism--- these are the *dramatis personae* of political philosophy." [pg. 3]
* Metaphysics (which he disdains as you will see) "And lastly, *metaphysics* (which gets into so much trouble because it is not, like the other forms of philosophy, an attempt to coordinate the real in the light of the ideal) is the study of the "ultimate reality" of all things: of the real and final nature of "matter" (ontology), of "mind" (philosophic psychology), and of the interrelation fo "mind" and "amtter" in the process of perception and knowledge (epistomology)." [pg. 3]

The chapters outline the path Will takes through the story of philosophy:

1. Plato
2. Aristotle and Greek Science
3. Francis Bacon
4. Spinoza
5. Voltaire and the French Enlightenment
6. Immanuel Kant and German Idealism
7. Schopenhauer
8. Herbert Spencer
9. Friedrich Nietzsche
10. Contemporary European Philosophers
11. Contemporary American Philosophers

In the introduction to the second edition Will makes repeated apologies for what he has already apologized for in the main text: it is riddled with sentences that would have experts on the relevant topics fuming and it leaves out Eastern philosophers entirely (which he tried to correct in the first volume of "The Story of Civilization").

There is no book on philosophy as a whole which has not failed to present that whole without holes.


### 2025 0414 2041
Yesterday, I stumbled on <https://www.game-cities.com/> by Konstantinos Dimopoulos who shared a link to [Ultima and Worldbuilding in the Computer Role-Playing Game
Carly A. Kocurek and Matthew Thomas Payne](https://services.publishing.umich.edu/Books/U/Ultima-and-Worldbuilding-in-the-Computer-Role-Playing-Game) that can be read online for free.

It's a short and quick read.
I'm always left wanting more from the books I read on the history or philosohpy of various video games.
In the end, the books that I want on my favorite games are basically literate programs that give an elaborate history of each design decision.
That is not yet the kind of book that is easy to get your hands on.

I hope to come back to Dimopoulos' site and read some of his [articles](https://www.game-cities.com/articles-talks) on urban design in video games.


### A Stack Notation for Predicate (Functor) Logic 2025 0414 1626
A way of eliminating variables from predicate logic which combines the methods of Quine's predicate functors with the practices of programming in J and FORTH permits predicate letters which, initially, generalize the familiar two place predicate notation--- e.g. 'x is identical to y', 'x belongs to y', and 'x is the father of y'--- by schema such as 'xyzFstuvw' so that each predicate has a number of left places--- three in the example schema--- and a number of right places--- five in the example.
Thus, the predicate 'x pairs y with z' is rendered perspicuously as 'xPyz'.
The left places are referred to as the pile of the predicate and the right places are the list of the predicate.

The predicate functors of recombination (which generalize the stack notation of FORTH to predicate logic) are 

1. '...xy(DROP F)...z' for '...xF...z'
2. '...wxy(HEM F)...z' for '...wxyFx...z'
3. '...x(PUSH F)y...z' for '...xyF...z'

from which all others are defined e.g.

4. 'OVER F' for 'HEM PUSH F'
5. 'OVER2 F' for 'OVER OVER F'
6. 'OEM F' for 'OVER HEM F'
7. 'DSH F' for 'DROP PUSH F'
8. 'DUP F' for 'OEM DSH F'
9. 'DROP2 F' for 'DROP DROP F'
10. 'POP F' for 'OEM DROP2 F'
11. 'NIP F' for 'POP DSH F'
12. 'HIP F' for 'HEM NIP F'
13. 'HIP2 F' for 'HIP HIP F'
14. 'SWAP F' for 'HIP PUSH F'
15. 'PUSH2 F' for 'PUSH PUSH F'
16. 'TOR F' for 'HIP2 PUSH2 F'
17. 'ROT F' for 'TOR TOR F'

and so on.
The remaining predicate functors (which generalize the tacit notation of the J programming l to predicate logic) are defined from

18. '...x(NOT F)...y' for 'not ...xF...y'
19. '...x(F AND G)...y' for '...xF...y and ...xG...y'
20. '...x(SOME F)...y' for 'some item is (z such that ...xzF...y)'.

For more on predicate functors and the method of homogenization which permits translation back and forth between predicate functor logic and 'predication' logic see Quine's "Methods of Logic 4th edition".

> Predication logic is the most appropriate name for what is unhappily called 'first-order logic' or otherwise called 'predicate logic'.
> As I see it, with predicate functors elimination of variables, the only items in the lexicon of a logical theory are the predicates and hence 'predicate logic' is what might otherwise be called 'predicate functor logic'.

There are two things forthcoming:
1. an implimentation of the algorithms that transform a pure predicate functor term into a sentence (perhaps open or perhaps closed) of predicate logic, and vice versa; and
2. a further generalization of the above notation which incorporates all four of the following:
    * Quine's predicate functor notation,
    * FORTH's stack notation,
    * J's tacit notation, and
    * LISP's list notation.

There are additional changes which permit a full generalization of APL and J's functional operators that generalize e.g. matrix multiplication.
These correspond to predicate functor functors: you can continue to go up and up the grammatical hierarchy and even contemplate predicate functor functor functors if you desire.

Though, it appears as if there is a limit to how high you can go and still get something helpful out of it: it seems to me that predicate functor functors are as far as you can go since after that everything else looks exactly like the first three levels of operations on the grammatical hierarchy of a logical language.

### 2025 0414 1605
Today is a Nat King Cole day.
His voice, his piano, and his trio have brought me hours of delight.

Some things I'd like to get done today:
1. Finish the very short paper on "A Stack Notation for Predicate (Functor) Logic"
2. Finish my little parenthetical space sperated language
3. Read some more of "The Rise and Fall of the Third Reich" by Shirer and published October 17, 1960.

I've read that Shirer's account is 'outdated' or 'narrow' or 'not historically accurate'.
No book is historically accurate: we wouldn't know how to check for historic accuracy even if our life dependend on it.
That is a bit of an overexaggeration: it is largely the result of the metaphorical distance between our records of day to day life and works like "Schedules of Reinforcement" by Skinner and Ferster where the challenges of keeping an accurate record of behavior are uncovered in all their ugly details.

Whether Shirer's book is adequate for an understanding of the third reich is moot: no book is going to give "the whole story" as if there was some big story in the sky that would truly enlighten us if we could just reach it.
I see what I've read of Shirer so far as a starting point: it is so well written that you would be hard pressed NOT to finish it.
Not all history books have that same luxury.

So far, I've read that Richard J. Evan's "Third Reich Trilogy":

1. "The Coming of the Third Reich" published October 2003
2. "The Third Reich in Power" published October 2005
3. "The Third Reich at War" published October 2008

is a much more accurate report on the contingencies that selected and ultimately extinguished the third reich.

I have so many books to read that it is hard to see how I'll ever get to reading them all. 

## 2025 0413

### 2025 0413 2353
While skimming over [John McCarthy's 1979 "History of Lisp"](https://justine.lol/sectorlisp/lisp-history.pdf) two things caught my attention:
1. he mentioned that Quine had used prefix notation in some thing he or those around him had read and that this had some influence on LISP's prefix notation (pg. 7)
2. He mentioned a paper he wrote with Cartwright (1978) that "show how to represent pure LISP programs by sentences and schemata in first order logic and prove their properties"(pg. 8)

He also mentioned something about LISP having no effect on those working in recursion theory, but that now seems to be a historical hiccup.

My interest in (1) is much less than my interest in (2).
I am reading the mentioned paper now: ["First Order Programming Logic" by Cartwright and McCarthy in 1979](https://dl.acm.org/doi/10.1145/567752.567759).

I just finished reading it (202504140019) and am both happy and sad.
My interest was primarly the result of being consumed by the methods of logic programming.
But, alas, "programming logic" and "logic programming" though just one swap away from each other does not bring them together as one.
That is the sadness: this paper does not not reduce pure LISP to the methods of logic programming.

What it does do though is still very important, and I will have to read this paper again before I can explain it and the many things that it brings up as they relate to logic programming: it calms the anxieties of those who continue to seek something beyond predicate logic when making proofs about programs.

Cartwright shows how to set up a theory (kind of like the traces in what Hoare first introduced as [1978 "Communicating Sequential Processes"](https://dl.acm.org/doi/10.1145/359576.359585)) that transforms partial functions into total functions allowing for the introduction of an equivalence axiom that permits convenient proofs of properties of the original partial function e.g. an interpreter.

McCarthy's contribution is a minimization schema for each partial function (this is very likely akin to the schematic method I devised for setting up a schematic theory of transitive closures of an unspecified predicate of the lexicon of the theory) that ends up being equivalent to Cartwright's method.

Together these methods indicate clearly and exactly that no more than predicate logic is needed to reason about recursive programs, even those once as unfamiliar as interpreters/compilers.

These are happy things to know because it strengthens the general significance of my proposal that the practices of programmign are simply a subcollection of the practices of predicate logic.

The significance to this specific paper, and any others like it, is that such formalisms are no more a surprise than a formalism of predicate logic itself.


### 2025 0413 2323
I got a copy of Will Durant's "The Story of Philosophy" some months ago from a book fair.
Though I read some of it back then, I do not recall any of it now.
One of the joys of reading SO MANY *fundamentally interesting* things (sometimes over and over again) is that you can actually forget some of them!

Most of what I read is nonfiction, but I've started reading fiction recently e.g. Agatha Christe, le Carre, Chaucer, and Dante.
Most of the fiction I've read in my life is from when I was less than fifteen years old e.g. Asimov, Pullman, Tolkein, Salinger, Wells, Orwell, and Shelly.
Since I haven't reread those books I'm uncertain whether I have forgotten them or not.

Anyway, I finished reading the introduction to Will Durant's "The Story of Philosohpy" and had some things to say (apparently).

---

The success of Will's book "The Story of Philosophy" allowed him and his wife to write "The Story of Civilization" which I have yet to finish reading (in fact, I still have yet to finish the first book in the series!).
I am drawn to the Durants because they wished to see the world as a whole: to put Humpty Dumpty back together again.

Mine is the second edition and the eighth printing: copyrighted last in 1933 and published no later than 1953 by Simon and Schuster.

The introduction to this second edition echos my own outlook on our modern world: although we are technically more interconnected now than ever before, there is less wisdom among us than ever there has been.
Much of our world was designed and built by a few of us.
They have drained knowledge of all that makes it rich.

To be wise is to threaten the prevailing world order.
The wise see beyond the shades of political esotericism through to the fundamental conflict between controllers and countercontrollers.
Hope has not died, and it is still expected by a few of us that beyond the bounds of individualism and collectivism there is something like a peaceful culture: one that does the controlling and countercontrolling through us in only the most conspicuous of ways.


### 2025 0413 2249
I finally finished my entry on [Bit Strings and Binary Trees](#2025-0413-1513-bit-strings-and-binary-trees)!
It took longer than I expected, but I also uncovered some unexpected things along the way e.g. a quick and easy way to find shorter bit strings for a given binary tree.

Something I also learned: there is no faster way to catch bad writing than to publish it.

Something else I learned: if you don't end a markdown list with two returns it thinks the trailing line of text is part of the last entry in the list.

### 2025 0413 1754
A friend just introduced me to two great bits of music:
* [Alberto Ginastera](https://en.wikipedia.org/wiki/Alberto_Ginastera) - Harp Concerto (1956)
* [Seru Giran](https://en.wikipedia.org/wiki/Ser%C3%BA_Gir%C3%A1n_(album)).

I was able to guess that Ginastera had learned from Copland.
I described Ginastera as a "faster Copland".

Seru Giran was too short: I wanted more after listening to it.
It is a very comforting album that seemed to combine popular styles from both North America and England.

### 2025 0413 1644
If I could just finish one thing before finishing another thing then I would be able to finish more things that people care about now than things they'll care about later.
This is a roundabout explanation of why I work the way that I do.

### 2025 0413 1634 Buffett's Jet and Apple's Three Jets Full of iPhones

Warren Buffett wrote about selling and buying a jet in his [letter to Berkshire sharehodlers in 1989](https://www.berkshirehathaway.com/letters/1989.html).
On more than one occasion I have had to explain to people that it is more than a luxury, though that is mostly what the shareholder letters show off as the big problem.

There is almost no limit to the potential value of being able to go from point A to point B around the world with tangable objects in tow.
Apple's three jets full of iPhones should help people to more easily grasp why Warren went from naming his jet the 'indefensible' to the 'indispensable'.

### 2025 0413 1513 Bit Strings and Binary Trees
Bytes of memory stored in a computer can be treated as strings of binary digits or as little binary trees.
Let me explain what I mean while I give you a javascript program that decodes a binary tree into a bit string and encodes a bit string into a binary tree.
(Most people would swap 'encode' and 'decode' in that sentence.)

Computers tend to use bits, bytes, and sometimes other more exotic words to do either place value arithmetic (binary in almost all cases, ternary in some rare cases) or to address other words of memory.

> Technically they do place value arithemetic of remainders (sometimes with respect to the word size, and othertimes in less obvious ways that are otherwise more familiar as "logical operations").

Storing addresses is an easy way to avoid having to copy words to new locations: go to where the relevant data is rather than waste the time and space to replicate it somewhere else.

Words also have another vital role to play in computers: they store commands that end up telling the computer what to do with all those addresses and numbers.

There is an alternative way of looking at words as lists of ones and zeros: each string of bits corresponds to a binary tree and each binary tree corresponds to a string of bits.
It is easier to decode a binary tree into its binary string than it is to encode a binary string into its tree.

Since the binary trees contemplated here are all dirty--- they are all built from pairs whose subcomponents are pairs or the nil pair--- there is only one terminal condition which must be checked in any recursive definition: is this the nil tree?
Before that, here is the code for building pairs from their left and right parts and for getting the left and right parts from a pair.

```
let pair=(leftPart,rightPart)=>({leftPart,rightPart})
, leftPartOf=pair=>pair.leftPart
, rightPartOf=pair=>pair.rightPart;
```

The nil pair is not to be confused with the empty pair: the empty pair has no parts, but the nil pair is a special pair (or what some might call an atom) that is identical to its left and right parts (see [2025 0411 2248](#2025-0411-2248) for more on the logic of atomic and empty items).
The way we make an object like that in javascript is a bit esoteric:

```
let nil={get leftPart(){return this},get rightPart(){return this}};
```

Setting up getters (and setters) with javascript objects is just weird and I really only read the documentation for such things when I already really know what I want.
Otherwise, you will get lost trying to make sense of the design choices that brought us the programming language of the internet.
There is an alternate world where LISP was the language of the internet: that world would have been better than this one.
An even better world is one where it was FORTH, but I digress.

The above definition of 'nil' makes it so that 

```
nil == leftPartOf(nil) && nil == rightPartOf(nil)
  true
```

which happens to be the defining feature of atoms in a more general setting:

```
let atom=x=> x==leftPartOf(x) && x==rightPartOf(x);
```

Sadly, we won't be needing to talk any more about atoms.
We just need to know whether a given tree is nil or not:

```
let isNil=x=> x==nil;
```

Oh, I should explain the difference between 'dirty trees' and 'pure trees' because most people are familiar enough with 'pure trees' and few people use the phrase 'dirty trees'.
Dirty trees are ones that include something other than pairs or the empty pair among their subcomponents.
Pure trees are ones that don't include anything other than pairs or the empty pair among their components.
They are pure in more ways than one: they do not admit of talk of atoms, and if you go down one branch or another you eventually hit the empty tree (but it may take infintely long to get there in some cases, those also being cases that I would say are inappropriate).

So now we have all we need in order to talk about binary trees.
Onto what we need to talk about bit strings:

```
let zero='.'
, one=','
, concatenate=(...strings)=> strings.length ? strings[0]+concatenate(...strings.slice(1)) : '';
```

There are two building blocks to bit strings: zero and one.
They are to be distinct:

```
zero != one
  true
```

and they can be combined by concatenation.
The principles governing concatenation, like the principles governing binary trees, are actually a lot more elusive than it first seems: thankfully, if we just go with what javascript gives us there is no reason to get stuck in the theories of strings and trees, even though those can be fun places to be stuck.

The function designated by 'concatenation' takes any number of arguments that are strings and puts them together in the order they are taken (from left to right).

```
concatonate(zero,one,zero,one,one)
  .,.,,
```

Why use '.'s for zeros and ','s for ones?
Because then the binary decoding of a tree is also the program for the stack machine that actually constructs the binary tree it decodes!
But first, lets decode some trees:

```
let decode= tree => isNil(tree) ? zero 
: concatenate(decode(leftPartOf(tree)), decode(rightPartOf(tree)), one);
```

In English, decode takes a tree and first checks if it is identical to nil.
If it is then we already know how to decode that: it's just a zero.
If it isn't nil then we have already made the assumption (quite a strong one really) that it must be a pair with a left part and a right part.
So, all we need to know to write out the decoded tree is to
1. decode the left part of the tree and
2. decode the right part of the tree.

Once we have those two things we just write out the code for the left tree, followed by the code for the right tree, and cap it all off at the end with a one.

It will be shown that the decoded strings are almost always longer than they need to be: any initial zeros can be trimmed from the front of the decoded string without any loss to subsequent encoding.

Some examples

```
decode(nil) 
   .
decode(pair(nil,nil)) 
   ..,
decode(pair(nil,pair(pair(nil,nil),nil))) 
   ...,.,,
```

The decode function is one-to-one (injective i.e. each string that comes out of it is only the result of decoding one and only one tree).
There are some bit strings that will never be the result of such decoding e.g. ',' and '.,'.
Thus the method of decoding is not a one-to-one correspondence (bijection i.e. each tree matches to one and only one string and each string matches to one and only one tree).
In general, there is a philosophic simplicity to establishing one-to-one correspondences from one universe of items to another.
For more on the fruits of such labors see Quine's 1946 "Concatenation as a Basis for Arithmetic".

When treated as postfix notation (as in reverse [Łukasiewicz notation](https://en.wikipedia.org/wiki/Polish_notation)) the ',' comes to work as the function 'pairUp' of a [stack machine](https://en.wikipedia.org/wiki/Stack_machine).
So many of the practices of programming are described by tiny stack machines that there is great value to being familiar with them perhaps without ever programming a real stack machine.

From the method of decoding it can be seen that the last ',' pairs together the left and right parts of the tree.
This is how the encoder works!
It simulates a little stack machine and uses the bit string as the list of commands necessary to construct the decoded tree.

First, we can  use a tree to simulate a stack.
All we need is a way to push, pop, and maybe peek at the top of the stack.
When I implement stacks with trees, I always take the top item to be the right part of the tree simulating the stack.
Some people prefer to take the left part of the simulating tree as the top, and there's nothing mathematically wrong with that (perhaps not even something logically wrong with that), but the programming language LISP has long since forced us to interpret such a setup as a list and not a stack i.e. the left part of a tree is the first item in a tree simulating a list.

```
let push= (stack,item) =>pair(stack,item)
, pop= stack => leftPartOf(stack)
, peek = stack => rightPartOf(stack);
```

If instead of allowing nil into our universe of pairs we had stuck ourselves with a pure theory of pairs, then 'peek' and 'pop' would have to be written differently e.g.

```
let error = message => console.log(message)
, emptyPair=[]
, isEmpty = pair => pair.length == 0
, purePop = stack => isEmpty(stack) ? emptyPair : leftPartOf(stack)
, purePeek= stack => isEmpty(stack) ? error('empty stack') : rightPartOf(stack);
```

Most people teach and prefer the implementation that includes error messages.
They want the machine to tell them when something is wrong by anticipating how it could go wrong.
My preference is for a crash based method of design: rather than go looking for errors leave as much of the design as possible out on the rim of "don't cares".
Not only does this make your code a lot shorter (which makes it so there are fewer places where things could ever possibly go wrong), it also releases you from the anxiety of trying to catch your mistakes before you make them.

There are some standard techniques that we pick up over time for avoiding problems that have already happened again and again in the past.
Some people are taught by others to avoid them and some are taught by the immediate consequences of having made such mistakes.
The latter almost always teaches better than the former, but that is not the end of this general problem as to how to practice programming.

There is no universal way to detect logical errors in programs.
No one is there to look over your shoulder and tell you "Hey, this isn't actually what you wanted to make in the first place."
Though we have new machines that speak to us in such conversational tones--- after having been forcefed some scrap of code--- there is no part of such machines that prepares them for anything more than a world like the selecting past (but that goes a much longer way than most people once supposed, e.g., half a decade ago).

But, there is a way to entirely avoid logical errors: that is what logic itself is for!
Thus, rather than make trouble for ourselves by starting with the logic of pure binary trees (where, e.g., the definition of 'properPop' deals clumsily with degenerate cases), we begin with a theory that admits no such road bumps even if, in degenerate cases, it gives us unfamiliar results (e.g. the definition of 'pop' I have adopted here gives nil when you pop nil as the simulation of the empty stack).

Back to encoding bit strings to trees!
I already gave away the key to solving this problem: the bit string is a program that tells a tiny stack machine how to build the encoded tree.
Thus, we go along the bits, from left to right, and when we hit a zero we push a nil onto the stack, and when we hit a one we pop the top two trees on the stack and pair them into a new one that we push back on the stack.

Unlike pairs, there are no standard ways of getting at the anatomy of a string (there is much more to say about that and about Quine's protosyntax in general).
Thus, we go with some traditional methods:

```
let emptyString=''
, isEmptyString = string => string==emptyString
, isZero = string => string == zero
, isOne = string => string == one
, firstOf = string => isEmptyString(string) ? emptyString: string[0]
, restOf = string => isEmptyString(string) ? emptyString : string.slice(1);
```
Some examples:
```
isEmptyString(emptyString) 
  true

isEmpty(concatenate(zero,emptyString,one,one)) 
 false

isZero(firstOf(concatenate(zero,one,one,emptyString)))
  true

isOne(firstOf(restOf(concatenate(zero,one,one,emptyString))))
  true

concatenate(zero,one,zero)==restOf(concatenate(one,zero,one,zero))
  true
```

Not all of those string functions are really needed in the encoder, but there's nothing wrong with covering familiar ground when it doesn't take you far from your path.
There are two operations that our little simulation of a stack machine has to accommodate: push a nil and push a pair made from popping the top two items from the stack.
The first is simple enough:
```
let pushNil=stack=>push(stack,nil);
```
It is the second that poses a few problems (but not anything too troublesome).
Rather than explain it, I'll just show the code and go from there:
```
let emptyStack = nil
, isEmptyStack = stack => stack==emptyStack
, topOf = stack => peek(stack)
, secondOf= stack => peek(pop(stack))
, pop2 = stack => pop(pop(stack))
, pairUp=stack=>push(pop2(stack),pair(secondOf(stack),topOf(stack)));
```
> For those familiar with stack operations in other circumstances, it may be strange to take a functional view of stacks: popping a stack returns a stack without its top, it doesn't change the state of some stack accessible outside of the scope of the current executing function.

Now all that is left is to break the encoder into a function that gets the ball rolling with an empty stack and one that takes a string and a stack and gets to work:

```
let encodeHelperBeta = (string, stack) =>
  isEmptyString(string) ? topOf(stack)
  : isZero(firstOf(string)) ? encodeHelperBeta(restOf(string), pushNil(stack))
  : isOne(firstOf(string)) ? encodeHelperBeta(restOf(string), pairUp(stack))
  : encodeHelperBeta(restOf(string),stack)
, encodeBeta = string => encodeHelperBeta(string,emptyStack)
```
Lo, this is not the last version of 'encode' and that is why it is called 'encodeBeta'.
There are three things of note in the beta definition given:

1. it asks if the string is empty and when it is it returns the top of the stack,
2. it checks if the first item of the string is zero or one and executes the corresponding operation on the stack (pushing a nil on top of the stack or pairing up the top two items on the stack), and
3. if it runs into an item of the string that we haven't talked about yet it does nothing and goes on its merry way.

Here are some examples, but because we have no way of seeing the encoded tree I have to put it through the decoder--- this ends up being helpful because it should decode into a string that we could put back into the encoder to make the tree all over again.
```
decode(encodeBeta(concatenate(zero,zero,one)))
  '..,'
decode(encodeBeta(decode(encodeBeta(concatenate(zero,zero,one)))))
  '..,'
decode(encodeBeta(concatenate(zero,zero,one,zero,one)))
  '..,.,'
decode(encodeBeta(concatenate(one,one)))
  '...,,'
```
> Alternatively, we could have built up the parenthetical notation familiar to most people for ordered pairs e.g. where '(x,y)' is short for "the pair whose left part is x and whose right part is y".
> It would also be helpful to know when two trees are equal so that we might be able to write out the construction of a tree and compare it to the encoding of its abbreviation as a bit string.
> While javascript comes with its own built in string functions, it doesn't give us built in pairs or functions for working with pairs.
> Given what is known from Solomon Fefferman's work on [Finitary Inductively Presented Logics](http://virtualmath1.stanford.edu/~feferman/papers/presentedlogics.pdf), it is probably a bad idea to design a language that doesn't work with ordered pairs (and for those less theoretically minded, there's always the conveniences of LISP to look at).

That last example is well worth looking at more closely: it took the concatenation of one with one (which is not a string that will ever come out of the decoder) and it encoded just fine and when we sent that tree through the decoder it returned the concatenation of zero, zero, zero, one, and one!
So we have a much shorter way of decoding this particular tree: we can write it as ',,' instead of '...,,' when we have said how 'encoderBeta' works!

The following example shows why it is still called 'encoderBeta':
```
decode(encodeBeta(concatenate(zero,zero,zero,zero)))
  '.'
```
Where did all those other zeros go that we concatenated together?
How is it that we only got a single zero when we decoded the encoded tree?

After the encoder checks that the string is empty it peeks at the top of the stack and returns it as the result.
Since there are no ones in that concatenation, there are no pairs made out of the nil trees on the stack (of which there are four before it shows us the top nil).

There's nothing wrong with the beta encoder except that we can make a new one that doesn't leave anything on the stack.
Presumably, doing so will give us some new ways of abbreviating binary trees as bit strings!
The simplest change is to pair up everything that's left on the stack and then return that.
```
let pairUpEverything = stack =>
  isEmptyStack(pop(stack)) ? topOf(stack)
  : pairUpEverything(pairUp(stack))

, encoderHelper = (string, stack) =>
  isEmptyString(string) ? pairUpEverything(stack)
  : isZero(firstOf(string)) ? encodeHelper(restOf(string), pushNil(stack))
  : isOne(firstOf(string)) ? encodeHelper(restOf(string), pairUp(stack))
  : encodeHelper(restOf(string),stack)
, encode = string => encodeHelper(string,emptyStack);
```

Everything on the stack gets paired up by repeatedly applying 'pairUp' to the stack until there is only one item left on it (this is checked by seeing if popping the stack leaves only the empty stack behind).
Some examples:
```
decode(encode(concatenate(zero,zero,one)))
  '..,'
decode(encode(concatenate(zero,zero,zero,zero)))
  '....,,,'
decode(encode(concatenate(one,zero,one,zero,zero,zero)))
  '..,.,...,,,'
decode(encode(concatenate(zero,zero,one,zero,one,zero,zero,zero,one,one,one)))
  '..,.,...,,,'
```
The last two examples suggest a simple way of getting the shorter decoded string from the longer decoded string of an encoded tree without having to guess and check encoding and decoding:

1. if the long code is a concatenation of all zeros or all ones then you already have the shorter bit string you're looking for, but
2. if at least one zero and one one both occur in the long code then trim off any zeros on the left and any ones on the right.

There are some more complicated javascript functions being used to define the operations of trimming and checking for occurrences of ones and zeros, but no more than what you can figure out for yourself from context clues or a quick search:

```
let occursIn = (string, item) => string.includes(item)
, trimLeftZeros = string => string.slice(string.indexOf(one))
, trimRightOnes = string => string.slice(0,1+string.lastIndexOf(zero))
, trim = string => trimLeftZeros(trimRightOnes(string))
, shorten = string =>
  isEmptyString(string) ? '.'
  : occursIn(string,zero) && occursIn(string,one) ? trim(string)
  : string;
```

And, here are some examples:

```
shorten(concatenate(zero,zero,one,zero,one,zero,zero,zero,one,one,one))
  ',.,...'
decode(encode(shorten(concatenate(zero,zero,one,zero,one,zero,zero,zero,one,one,one))))
  '..,.,...,,,'
```

There are a few ways that these shorter decoded bit strings help out e.g. they let us fit more trees within a single word of memory and, if we are silly enough to do so, fit multiple tiny trees within a single word of memory.

Next I'll come up with a way to showcase how each bit string or binary tree locates chuncks, down to words, of memory.
After that it will probably be time for some bit string and binary tree arithmetic.
No promises!

Here's all the code I used to write this whole note:
```
let pair=(leftPart,rightPart)=>({leftPart,rightPart})
, leftPartOf=pair=>pair.leftPart
, rightPartOf=pair=>pair.rightPart;
let nil={get leftPart(){return this},get rightPart(){return this}};
let run=code=>{console.log(code,'\n  ',eval(code));}
run('nil == leftPartOf(nil) && nil == rightPartOf(nil)');

let atom=x=> x==leftPartOf(x) && x==rightPartOf(x);
run('atom(nil)');

let isNil=x=> x==nil;
let zero='.'
, one=','
, concatenate=(...strings)=> strings.length ? strings[0]+concatenate(...strings.slice(1)) : '';
run('zero != one');
run('concatenate(zero,one,zero,one,one)');

let decode= tree => isNil(tree) ? zero 
: concatenate(decode(leftPartOf(tree)), decode(rightPartOf(tree)), one);
run('decode(nil)');
run('decode(pair(nil,nil))');
run('decode(pair(nil,pair(pair(nil,nil),nil)))');

let push= (stack,item) =>pair(stack,item)
, pop= stack => leftPartOf(stack)
, peek = stack => rightPartOf(stack);

let error = message => console.log(message)
, emptyPair=[]
, isEmpty = pair => pair.length == 0
, purePop = stack => isEmpty(stack) ? emptyPair : leftPartOf(stack)
, purePeek= stack => isEmpty(stack) ? error('empty stack') : rightPartOf(stack);

let emptyString=''
, isEmptyString = string => string==emptyString
, isZero = string => string == zero
, isOne = string => string == one
, firstOf = string => isEmptyString(string) ? emptyString: string[0]
, restOf = string => isEmptyString(string) ? emptyString : string.slice(1);
run("isEmptyString(emptyString)");
run("isEmpty(concatenate(zero,emptyString,one,one))");
run("isZero(firstOf(concatenate(zero,one,one,emptyString)))");
run("isOne(firstOf(restOf(concatenate(zero,one,one,emptyString))))")
run("concatenate(zero,one,zero)==restOf(concatenate(one,zero,one,zero))");

let pushNil=stack=>push(stack,nil);
let emptyStack = nil
, isEmptyStack = stack => stack==emptyStack
, topOf = stack => peek(stack)
, secondOf= stack => peek(pop(stack))
, pop2 = stack => pop(pop(stack))
, pairUp=stack=>push(pop2(stack),pair(secondOf(stack),topOf(stack)));
let encodeHelperBeta = (string, stack) =>
  isEmptyString(string) ? topOf(stack)
  : isZero(firstOf(string)) ? encodeHelperBeta(restOf(string), pushNil(stack))
  : isOne(firstOf(string)) ? encodeHelperBeta(restOf(string), pairUp(stack))
  : encodeHelperBeta(restOf(string),stack)
, encodeBeta = string => encodeHelperBeta(string,emptyStack);
run("decode(encodeBeta(concatenate(zero,zero,one)))");
run("decode(encodeBeta(decode(encodeBeta(concatenate(zero,zero,one)))))")
run("decode(encodeBeta(concatenate(one,one)))")
run("decode(encodeBeta(concatenate(zero,zero,zero,zero)))");

let pairUpEverything = stack =>
  isEmptyStack(pop(stack)) ? topOf(stack)
  : pairUpEverything(pairUp(stack))
, encodeHelper = (string, stack) =>
  isEmptyString(string) ? pairUpEverything(stack)
  : isZero(firstOf(string)) ? encodeHelper(restOf(string), pushNil(stack))
  : isOne(firstOf(string)) ? encodeHelper(restOf(string), pairUp(stack))
  : encodeHelper(restOf(string),stack)
, encode = string => encodeHelper(string,emptyStack);
run("decode(encode(concatenate(zero,zero,one)))")
run("decode(encode(concatenate(zero,zero,zero,zero)))")
run("decode(encode(concatenate(one,zero,one,zero,zero,zero)))");
run("decode(encode(concatenate(zero,zero,one,zero,one,zero,zero,zero,one,one,one)))");

let occursIn = (string, item) => string.includes(item)
, trimLeftZeros = string => string.slice(string.indexOf(one))
, trimRightOnes = string => string.slice(0,1+string.lastIndexOf(zero))
, trim = string => trimLeftZeros(trimRightOnes(string))
, shorten = string =>
  isEmptyString(string) ? '.'
  : occursIn(string,zero) && occursIn(string,one) ? trim(string)
  : string;
run("shorten(concatenate(zero,zero,one,zero,one,zero,zero,zero,one,one,one))");
run("decode(encode(shorten(concatenate(zero,zero,one,zero,one,zero,zero,zero,one,one,one))))");
```

## 2025 0412

### 2025 0412 2335

It is late and it feels like there is a fat tire tightening around skull.
My temples throb and my eyes hurt.
Nothing comes from closing my eyes but the feeling of pains marching across my face.
If I don't tell myself to relax then I'll soon be clenching my teeth until my ears start to ring.
A few deep breaths may loosen my neck and shoulders, but my forehead remains scrunched.

Despite these unfavorable conditions, I have added some more to the [2025 0412 1422 Paper on Logic](#2025-0412-1422-paper-on-logic).

### 2025 0412 1619
I'm making a note here because I am so happy to have a single text document that I can easily copy and paste onto a single monolithic website.
This is the closest I have come to explaining myself to others in a way that is mostly accurate and mostly accessible.

For now there are only a list of timestamped entries to show you.
Soon these shall evolve into more substantial blocks of templates, contemplations, and conclusions.
As what I have to say reaches a critical mass, it bubbles up into a more solid and long standing brick in the foundation of my principles and practices.
I am glad to share these steps in the evolution fo my behavior.

### 2025 0412 1422
It has been a while since I've worked directly on my paper on logic.
In the past I have had to shove what little I can into a thin thread of tweets.
Now I am not stuck in a tight corner and can let my explanations relax into the more casual style that my friends and family are subjected to when I trap them in a "Johnversation".

My outlook on logic is now almost entirely that of Quine's (amended by Skinner's outlook on verbal behavior).
His "Methods of Logic 4th Edition" (and you need the 4th edition!) has almost split in half from how many times I've read it.
It doesn't help that Quine's main method of proof for predicate logic is about half way through the book and that it is the one section I've probably read and reread the most.

Whereas Quine's "Methods of Logic 4th Edition" narrowly focuses on the mechanics of logical practices, his "From Stimulus to Science" broadly applies those mechanics to, what Quine called, a "Breast Pocket Theory of the World".
As much as I disagree with the details of Quine's theory, e.g. he mistakes something like Skinner's experimental analysis of behavior for a dispositional stimulus-response theory, I agree with his methods.

Where Quine goes wrong, Skinner often goes right, and where Skinner goes wrong, Quine is there to clearly identify the problems if not also to provide compact solutions.

That is enough on where I'm coming from when I talk about logic.
Now for parts of my paper on logic.

***

There are a few unique things about my methods of logic.
The most important is that I aim to present predicate functor logic not as the elimination of variables from predicate logic, but as an autonomous system independent of predicate logic.

Whereas predicate logic is a consequence of grammar and truth, predicate functor logic is a consequence of grammar and denotation.
Closed sentences are true, predicates denote, and closed sentences are a degenerate form of predicate: they are zero place predicates.
Correspondingly, truth is a degenerate form of denotation.
In predicate functor logic we can define truth of a zero place predicate as the universal (or existential closure) of the quotation of its padding denoting.

> The phrases "existential closure", "quotation", "padding", and "denoting" are all technical terms and I have used them as such with full knowledge that they are foreign to most people e.g. "padding" is most likely unknown to anyone who is not yet familiar with Quine's predicate functors.

The first sentence of the paper, so far, is little more than the opening of a mystery:

> Logic is the science of validity as a consequence of grammar and denotation.

It is a sad sentence, and I am unhappy with it.
Simpler alternatives occur to me e.g.

> Logic is a consequence of grammar and denotation.

or perhaps

> Logic is that science which is the consequence of grammar and denotation.

but neither of those, nor the first, really clear the way for what is to come.
Such introductory sentences tend to come much later in my writing of a paper.
They are a summary of what is to come, and when what is to come has not yet come, there is nothing to concretely summarize.

The key components to the introductory sentence shall remain 'grammar' and 'denotation'.
They are an echo of Quine's "Philosophy of Logic 2nd Edition" which clearly shows the ways in which logic is the sum of grammar and truth.
He has a beautiful metaphor:

> Logic chases truth up the tree of grammar. [pg. 35]

and

> The logician talks of sentences only as a means of achieving generality along a dimension that he cannot sweep out by quantifying over objects.
> The truth predicate then preserves his contact with the world, where his heart is. [pg. 35]

Together these give a clear hint as to where Quine is coming from when he talks about the methods of logic.
The problem of "achieving generality" by talking of sentences is a critical part of Quine's method of semantic ascent:

> The move is what I call *semantic ascent*: mentioning an expression by name instead of using it as a component clause. [pg. 92 of From Stimulus to Science]

In "From Stimulus to Science" Quine does a better job of showing how quotation is just one potential method of semantic ascent by showing how it can be extracted from 'that' clauses e.g. 

- perceives that
- thinks that
- it occurred to him that
- believes that
- doubts that
- expects that
- hopes that
- fears that
- regrets that
- says that
- denies that
- predicts that
- strives that

[pg. 90 of From Stimulus to Science].
For most people familiar with Quine, semantic ascent is intertwined with his theory of propositional attitudes (as the examples amply indicate).
But, there is nothing so grand in the method: it is a technical solution to a technical complication:

> We can generalize on 'Tom is mortal', 'Dick is mortal', and so on, without talking of truth or of sentences; we can say 'All men are mortal'.
> We can generalize similarly on 'Tom is tom.', 'Dick is Dick', '0 is 0', and so on, saying 'Everything is itself'.
> When on the other hand we want to generalize on 'Tom is mortal or Tom is not mortal', 'Snow is white or snow is not white', and so on, we ascend to talk fo truth and of sentences, saying "Every sentence of the form 'p or not p' is true', or 'Every alternation of a sentence with its negation is true'.
> What prompts semantic ascent is not that 'Tom is mortal or Tom is not mortal' is somehow about sentences while "Tom is mortal' and 'Tom is Tom' are about Tom.
> All three are about Tom.
> We ascend only because of the oblique way in which the instances over which we are generalizing are related to one another. [pg. 11 Philosophy of Logic]

Quotations as a method of semantic ascent are haunted by philosophies of the past which are unable to distinguish between schematics and the items they purport to schematize.
For example, "of the form 'p or not p'" is usually written without the interior quotes, that is as "of the form p or not p", and this is otherwise excused as an expedient to effective writing where the reader is expected to know where the invisible quotations are to be put.
This has continued to confuse people who then take the occurrence of the letter pee in "of the form p or not p" as a pronoun referring to some otherwordly item called a proposition that somehow is *the meaning* of what a sentence like "Tom is Tom" says.

To be trapped down such dark allys of past mistakes is as silly as being stuck doing arithmetic with roman numerals.
We can look back and marvel at our mistakes and laugh at the labors we now save by teaching better methods.

All of this is to say that the single sentence

> Logic is the sciene of validity and validity is a consequence of grammar and denotation.

which is the first in my paper on logic is woefully inadequate as a summary of even what little I have said above and what little I have yet to say on the rest of logic.

***

Perhaps the proper "first sentence" is the second one

> Compounding is (denotative) functional when, exclusively, each like compound denotes or each like compound does not denote, where and only where (waow), exclusively, each like component denotes or each like component does not denote.
> Chains are compounds compounded functionally.

The phrase "denotative functional" generalizes to predicates what is said as "truth functional" of sentences.
The definition of denotational compound is clumsy with its "exclusively"s and "like"s, but the alternatives are woefully inaccurate and undermine the task at hand.
Logic does not have the benefit of its fruits before they have ripened.
As much as I would love to say that whether a compound denotes or not is a function of whether its components denote or not, there is no thing that can yet be spoken of as "a function".

It is only from the methods of logic that we come to pin down the items of the world.
While there are many who say that they are in touch with the world in some direct way, e.g. Russell repeatedly leaves room for singular terms so that they can be used to designate particulars of immediate experience, there is not yet a way that I have come to be in touch with the world as such others describe.
It is only through their reports that I am even confronted with the problems of such seemingly personalized revelations.

There is another problem that sometimes occurs when making such definitions: do I not appear to be speaking of "exclusive alternations" when I have not said what it means to say an instance of the sentence schema "exclusively p or not p"?
Is there not a circularity to my methods?

Quine was confronted with the same problem so often in the first publication of his early text "Mathematical Logic" that he had to add an appendix to help the anxious reader.
There he explains the difference between "theorem" and "metatheorem" as well as "deduction" and "formal deduction".
There (and in his Methods of Logic) he is confined to philosophic methods and only indirectly mentions in Methods of Logic the scientific origins of logical practices.

I have gone out of my way to use the phrase 'where and only where' or 'when and only when' to distinguish the preformal/informal or preschematic methods from the schematic methods of logic.
To explain the origin of verbal behavior with the forms "Exclusively Tom is wet or Tom is dry" is to leave the science of logic for the science of verbal behavior more generally.
Just as humans spoke grammatically before speaking of grammar, they spoke logically before speaking of logic.

At this time I do not have a better explanation as to why I can use phrases that appear to have the same form as those said to be defined e.g. 

> Joint denials denote waow each of their components do not.
> Negations are self joint denials: they denote waow their component does not.
> Alternations are negations of joint denials: they denote waow some of their components do.
> Conjunctions are joint denials of negations: they denote waow each of their components do.

***

With all that said, I have only introduced the following sentences from my paper on logic

> Logic is the science of validity and validity is a consequence of grammar and denotation.
>
> Compounding is (denotative) functional when, exclusively, each like compound denotes or each like compound does not denote, where and only where (waow), exclusively, each like component denotes or each like component does not denote.
> Chains are compounds compounded functionally.
>
> Joint denials denote waow each of their components do not.
> Negations are self joint denials: they denote waow their component does not.
> Alternations are negations of joint denials: they denote waow some of their components do.
> Conjunctions are joint denials of negations: they denote waow each of their components do.

Here is all that I have so far:

#### 2025 0412 1422 Paper on Logic

Logic is the science of validity and validity is a consequence of grammar and denotation.

##### Functional Compounding and Chains

Compounding is (denotative) functional when, exclusively, each like compound denotes or each like compound does not denote, where and only where (waow), exclusively, each like component denotes or each like component does not denote.
Chains are compounds compounded functionally.

##### Example Chains: Joint Denials, Negations, Alternations, and Conjunctions

Joint denials denote waow each of their components do not.
Negations are self joint denials: they denote waow their component does not.
Alternations are negations of joint denials: they denote waow some of their components do.
Conjunctions are joint denials of negations: they denote waow each of their components do.

##### Subcompounds and Functional Substitutions

Subcompounds of compounds are their self or those of their components.
Substitutions of like compounds for like nonchain subcompounds are (denotative) functional.
Functional substitutions of functional substitutions of compounds are functional substitutions of their self.

##### Functional Validity, Consistency, Implication, and Equivalence

Compounds are (functionally)
* valid waow each of their functional substitutions denote,
* consistent waow their negation is nonvalid (i.e. soem of their functional substitutions denote),
* implied by others waow the conjunction of their self (the conclusion) with the negation of the other (the premise) is nonconsistent (i.e. each of their functional substitutions denotes where the same of the other does), and
* equivalent to others waow they are mutually implicative (i.e. each of their functional substitutions denotes waow the same of the other does).
[See pg. 36 of POL]

##### Example Validities and (Non)consistencies: Laws of Excluded Middle, Contradiction, Self Implication, and Self Equivalence

Alternations of compounds with their negations are valid (they denote waow the compound does or its negation does, i.e. waow it does or does not, so, each functional substitution denotes).
Conjunctions of compounds with their negations are nonconsistent (they denote waow their compound does and its negation does i.e. waow it does and does not, so, each functional substitution does not denote).
Compounds are implied by and equivalent to their self.

##### Functional Substitutions Keep Validity, Nonconsistency, Implication and Equivalence

Functional substitutions in
* validities are validities (each functional substitution of the functional substitution of the validity is a functional substitution of the validity and hence denotes),
* nonconsistencies are nonconsistencies (each function substitution of the negation of the functional substitution of the nonconsistency is a functional substitution of the negation of the nonconsistency i .e. is a functional substitution of a validity and hence the negation of the functional substitution of the nonconsistency is valid so that the function substitution of the nonconsistency is nonconsistent),
* implications are implications (the conjunction of the conclusion with the negation of the premise is nonconsistant and hence its functional substitution is nonconsistent and identical to the conjunction of the function substitution of teh conclusion with the negation of the functional substutituion of the premise), and
* equivalences are equivalences (functional substitutions of mutual implications are mutual implications).

##### Interchanges of Equivalents are Equivalent
Interchanges of equivalents in a compound are equivalent to that compound (each functional substitution of a compound matches the same of its interchange, except perhaps for the same of the equivalents which otherwise denote in tandem, so each denotes waow the other does i.e. they are equivalent).

##### Interchnage of Equivalents Keeps Validity, Nonconsistency, Implication, Equivalence, Nonvalidity, Consistency, Nonimplication, and Nonequivalence

Interchanges of equivalents in
* validities are validities (each functional substitution of the interchange denotes waow the same of the validity does),
* nonconsistencies are nonconsistent (their negation is a validity and so the interchange in the negation is a validity),
* implications are implications (interchange into the nonconsistency is a nonconsistency),
* equivalents are equivalents (interchange of mutual implications are mutual implications),
* nonvalidities are nonvalidities (a compound is nonvalid waow some functional substitution does not denote, i.e. some functional substitution of its negation denotes, i.e. its negation is consistent, and since the negation of the interchange is identical tot he interchange of the negation which is consistent and consistency is kept by interchange then the negation is consistent i.e. it is nonvalid)
* consistencies are consistencies (the negation of the interchange is identical to the interchange of the negation which is nonvalid hence it is nonvalid),
* nonimplications are nonimplications (nonimplication is consistency of the conjunction ...)
* nonequivalences are nonequivalences (one is a nonimplication ...).

##### Equivalents of Identity
Compounds are equivalent to
* their double negation (which denotes waow the negation of the compound does not, i.e. waow it does, so, each functional substituion of it denotes waow the same of its double negation does),
* their self alternation/conjunction (which denotes waow some/each of its components does i.e. waow the compound does), and 
* their alternation/conjunction with nonconsistencies/validities.

##### Equivalents of Distributivity of Conjunctions and Alternations



### 2025 0412 1349
I have found that it is easier to be wrong than it is to be right and that it is easier to be right after having been wrong.
If there was a way to avoid being wrong then I would only act in that way.
Logic helps but is slow.
Sorting through the wrong bits takes more than what logic has to offer.

I see logic as part of science and see science as picking up what logic fumbles and drops.
Logic works on the theoretical side of science and laboratories work on the experimental side.
As I have said many times before "Libraries select theoretical practices and laboratories select experimental practices".
Science is compounded of theoretical and experimental practices.

One of the reasons I've allowed myself to write in this longer form is to make it easier for other people to see that I am often wrong, and that it is only from first being wrong that I get anywhere near being right.
The definitions, the natures, or the essences of what is right and what is wrong are not easy to suss out.

Many people have spent their whole lives trying and failing to establish what is right and what is wrong.
As much as I have a sort of optimism that if I work hard enough then I may one day uncover some piece of a complete theory of right and wrong, I am not so foolish to think that I shall do anything like what the best already have.

With that in mind, I offer these writings that are most certainly mistaken for no other reason than that they occurred to me.

***

I talk about behavior a lot.
Does that imply that my behavior is under my careful control?
No.
It is one thing to know a science and another thing to have a technology in hand.
The things of technology are the consequences of science, more or less.
But, the knowledge of science lacks the concrete dimensions of levers, pullies, medicines, and computers.
All that goes into scinece is often ephemeral: here for but a moment and gone forever, lost to our remote contact with the past through fragmentary records that soon fall apart.

Those who manufacture products, be they assemblies of machines, or chemicals, or organisms, or programs, or etc., work in ways often mistaken as scientific.
They manufacture with the controls offered by technologies i.e. with the descendants of scientific practices.

The difference between science and such manufactureres is the difference between sensitivity and insensitivity.
The manufacturer is deliberately insensitive to the strange variations responsible for science.
This is not what most people think or say: they say that business is as much a science and an art as physics or biology.
They insist that it is so in large part because the best businesses are almost always the result of some new technological consequence from science.

This does not always seem to be the case.
Some businesses appear to survive despite science.
Much that is "made for TV" objects to being called pseudoscience.
It sells, and there is a science to sales!

These are all problems of control: can you make a person buy your product?
Some say you can't, others say you can.
Some say that people only buy what is an intrinsically good product: you can't trick your way into becoming a good business when your product is technologically sophistocated.

***

What's with people who make a living as oracles?
They make predictions, often asserted with the authority of a proclamation, and profit, one way or another, from betting that they're right and ultimately being right.
But, are they betting on what they think they are?
Are they right when their bet pays off?
There seems to be some difference between constructing bets that are verly likely to pay off and making predictions like those people assume come from our best sciences.

How do we know the future?
When we speak of the future, as in predictions, what is it that we are speaking of?
Can speach actually be said to be about anything?
Certainly the speaking occurrs, but what good reasons do we have for saying that such speach is about this, that, or the future?

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

## 2025 0411

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
> Since more people are familiar with javascript than they are with Quine's main method--- or with the methods of logic--- I shall continue to avoid what might otherwise appear as an entirely roundabout way of teaching programming from scratch.

The function named 'cdr' takes one argument and returns the item at index one of it.

Both 'car' and 'cdr' are names inherited from working with programmable machines (aka "computers") during the late 1950s and early 1960s.
If you want to learn more about where 'car' and 'cdr' come from, and perahsp even how to pronounce 'cdr' (which I say as "could-er"), then track down [John McCarthy's 1979 "History of Lisp"](https://justine.lol/sectorlisp/lisp-history.pdf) and give it a compassionate read.

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
Empty items always end up behaving like atoms in many respects--- e.g. they are empty and presumably "don't have parts"--- but they almost always tend to be unique where as there tend to be many many atoms in any given theory.

The only place I have found clarity on these boundary problems is, you guessed it, logic.
In a theory of ordered pairs, it is enough to provide for the existence of an ordered pair that neither has a left part nor has a right part.
It is then unique since any two purportedly distinct empty pairs have identical parallel components (vacuously).
Atoms are then accomodated by one of a few methods, each of which match up with Quine's atomic methods in "Set Theory and its Logic".
An item of a theory of ordered pairs is atomic when it is identical to its components (or, just one of its components, in which case the other components allows us to distinguish between atoms with, e.g., identical left components: are these even atoms?).

Sadly neither of these principles are used in LISPs.
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

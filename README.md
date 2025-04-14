Discover, predict, and control changes in counts, rates, and accelerations as selections from variations on physical, chemical, biological, behavioral, and cultural scales by making and maintaining strong practices mediated by strong people marked by strong principles from the sciences of logic (denotative, Boolean, and functor), mathematics (calculi, collections, and categories), physics (quantum field theory, statistical thermodynamics, gravity), chemistry (phyiscal, biophysical, and biological), biology (oranelles, organisms, environments), behavior (biological, biosocial, social), and culture (history, technology, survival).

## 2025 0413

### 2025 0413 2249
I finally finished my entry on [Bit Strings and Binary Trees](#2025-0413-1513-Bit-Strings-and-Binary-Trees)!
It took longer than I expected, but I also uncovered some unexpected things along the way e.g. a quick and easy way to find shorter bit strings for a given binary tree.

Something I also learned: there is no faster way to catch bad writing than to publish it.

### 2025 0413 1754
A friend just introduced me to two great bits of music:
* [Alberto Ginastera](https://en.wikipedia.org/wiki/Alberto_Ginastera) - Harp Concerto (1956)
* [Seru Giran](https://en.wikipedia.org/wiki/Ser%C3%BA_Gir%C3%A1n_(album)).

I was able to guess that Ginastera had learned from Copland.
I described Ginastera as a "faster Copland".

Seru Giran was too short: I wanted more after listening to it.
It is a very comforting album that seemed to combine popular styles from both America and Britian.

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
Otherwise, you will get lost trying to make sense of the design choices that brought us the programming langauge of the internet.
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
My preference is for a crash based method of design: rather than go looking or errors leave as much of the design as possible out on the rim of "don't cares".
Not only does this make your code a lot shorter (which makes it so there are fewer places where things could ever possibly go wrong), it also releases you from the anxiety of trying to catch your mistakes before you make them.

There are some standard techniques that we pick up over time for avoiding problems that have already happened again and again in the past.
Some people are taught by others to avoid them and some are taught by the immediate consequences of having made such mistakes.
The latter almost always teaches better than the former, but that is not the end of this general problem as to how to practice programming.

There is no universal way to detect logical errors in programs.
No one is there to look over your shoulder and tell you "Hey, this isn't actually what you wanted to make in the first place."
Though we have new machines that speak to us in such conversational tones--- after having been forcefed some scrap of code--- there is no part of such machines that prepare them for anything more than a world like the selecting past (but that goes a much longer way than most people once supposed, e.g., half a decade ago).

But, there is a way to entirely avoid logical errors: that is what logic itself is for!
Thus, rather than make trouble for ourselves by starting with the logic of pure binary trees (where, e.g., the definition of 'properPop' deals clumsily with degenerate cases), we begin with a theory that admits no such road bumps even if in degenerate cases it gives us unfamiliar results (e.g. the definition of 'pop' I have adopted here gives nil when you pop nil as the simulation of the empty stack).

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
> For those familiar with stack operations in other circumstances, it may be strange to take a functional view of stacks: popping a stack returns a stack without its top, it doesn't change the state of some stack accessible outside of a the scope of the current executing function.

Now all that is left is to break the encoder into a function that gets the ball rolling with an empty stack and one that takes a string and a stack and gets to work:

```
let encodeHelperBeta = (string, stack) =>
  isEmptyString(string) ? topOf(stack)
  : isZero(firstOf(string)) ? encodeHelperBeta(restOf(string), pushNil(stack))
  : isOne(firstOf(string)) ? encodeHelperBeta(restOf(string), pairUp(stack))
  : encodeHelperBeta(restOf(string),stack)
, encodeBeta = string => encodeHelperBeta(string,emptyStack)
```
Lo, this is not the last version of encode and that is why it is called 'encodeBeta'.
There are three things of note in the beta definition given:
1) it asks if the string is empty and when it is it returns the top of the stack,
2) it checks if the first item of the string is zero or one and executes the corresponding operation on the stack (pushing a nil on top of the stack or pairing up the top two items on the stack), and
3) if it runs into an item of the string that we haven't talked about yet it does nothing and goes on its merry way.

Here are some examples, but because we have no way of seeing the encoded tree I have to put it through the decoder: this ends up being helpful because it should decode into a string that we could put back into the encoder to make the tree all over again.
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
> Given what is know from Solomon Fefferman's work on Finitary Inductively Presented Logics, it is probably a bad idea to design a language that doesn't work with ordered pairs (and for those less theoretically minded, there's always the conveniences of LISP to look at).

That last example is well worth looking more closely at: it took the concatenation of one with one (which is not a string that will ever come out of the decoder) and it encoded just fine and when we sent that tree through the decoder it returned the concatenation of zero, zero, zero, one, and one!
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
The simplest change is pair up everything that's left on the stack and then return that.
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
The last two examples suggest a simple way of getting the shorter decoded string from teh longer decoded string of an encoded tree without having to guess and check encoding and decoding:
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
Much taht is "made for TV" objects to being called pseudoscience.
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

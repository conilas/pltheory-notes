### The call

Recently, as I currently am in my pseudo-pretending intelectual phase (as one would expect from a millenial in their early 20s), I've been reading some content on post-structural philosophy, more specifically Deleuze and Guattari. Yes, I was driven to it by the very fact that it's hard to grasp even a single paragraph of their writtings in an attempt to fullfill an ego I did not realize I had before - and I don't mean a James Strachey translation of Freudian Ego as a piece for constituion of the mind, but a modern western-world-appropriated concept of ego as a reflection of our self worth - and I regret that already, because my brain hurts every day. 

I also came to the conclusion that I need to have some presence online, and that I have superficial knowledge on the two areas now - both compsci and whatever constitutes post-structural philosophy's basis  - so I can pretend to be smart and fullfill this giant hole I have (which I assume to be the aforementioned ego) by getting them together in a post. And this here will be my first attempt.

### Some notes

I'm not the first and I will not be the last to try this exercise of interdisciplinarity. However, I trully believe that compsci lacks this a lot due to the fact that most people in the industry are extremely pragmatic and find in the field the perfect escapism from theoretical nonsense. It goes without saying that there are other people who've accomplished more than I have (both in this attempt of culture-merging and in the industry itself) and have way more to say on it [1], but I'll take it as inspiration and try it on my own.

### Layman's definition of post-structural philosophy

Let's breath a little. Yeah, yeah, I'm a programmer; why would I waste my time with such useless concept? Re-read the title of this section and I'll bet that you have already formulated what post-structuralist means here. My gut tells me you are right for two reasons: 1. there's not really a specific meaning for it apart from "an opposition to structuralism" and 2. `post` something normally means defyiance or change (just think about it; you are moving towards something new that sublates the previous. It's difference in itself. Can I invoke dialetics in a post where I pretend to know something about Deleuze?).

But there's an important concept here that does not come up with the name: binary constructions. Think of them as simple dichotomies, an either/or relation. Hell, I'm writing this for programmers - `true` or `false`. That is one of the basis for **structuralism** itself; defining the liminal parts of structure, the `is a or b` predicate that lives on and forever in any relation.

And here's where it gets interesting. I will invoke my little knowledge of Anti-Oedipus for the term `disjunctive synthesis`, or the series of differences [3]. The most important point I want to get at is this: this disjunctive synthesis for those philosophers is not binary; is not restrictive. This disjunction is not only productive, but also broad and inclusive. So yes, it is still and `either` construction; but it is and `either...or...or`, where all the options belong to the very same concept. **This is clearly a break from the structuralist format**. I'll keep it simple, short and bitter as a hook if you want to dig more into this, but I got to the point where I wanted. The subject (here, philosophy) is breaking it's chain with binary-only logic.

### Layman's definition of some math concepts

Okay, I promised programming-related content, but we are still on our way. Let's talk a little bit about math. 

As a programmer, you might not have realized, but you have some decent level of understanding when it comes to set theory and basic classical logic. For you, it is not hard to see and understand that a proposition, a predicate, is either true or false. Heck, you look at the return of a boolean function and *you know* what are the two decisions you may take (unless, of course, that `bool` return is an object in a language that lacks real type safety, and then you have to treat null types. Ignore these failed languages here). That is true for mathematicians, too. Well, partially.

You see, math and logic have been long governed by the three laws of thought. They are the law of identity, the law of non-contradiction and the **law of excluded middle**. I won't focus on the first two, but will give a brief on the third one. You can easily find content that digs deeper in them in the internet; I, obviously, am no expert.

The law of excluded middle is simple and clear. `Every truth value is either true or false`. Clear, simple, transparent; as you've ever treated any computer program in your life. This is logical, you say.

And here's where I am supposed to challenge you to change your thinking, break the chain, break the structure: what if it's not?

That's when we get to intutionistic logic. You see, it's a different, **constructive** form of mathematics and logic, which the principle stated above is not assumed to be true; it is not a law or axiom in itself. The idea is to construct proofs or objects for formulations and given this construction analyse what the "truth" concept means. 

Hey, **I'm obviously and purposefully being shallow here**. The hole goes deep. Go play and read in ncat-lab as much as you want. You can dig into it and fall into topos, higher-cat theory, etc; but I'll leave that decision to you. My job in this section is done: I hope I've shown to you that math/logic can, in a way, escape from the binary constructs.

### Back to programming

Phew. This all goes way beyond my radar and areas of day-to-day contact, so this has been a pain for my brain (rhyme intended). Let's take it down a notch to easy and more close-to-our-reality concepts.

By now, there's a chance you, as a developer, have heard of the idea of Dependent Types, or dependent type theory. If you are not really keen into type theory and/or if you don't really like research that much, that is fine. I want to give you a brief overview on dependent types; you can find some more in [4].

Have you read it? I'll wait. [...]

Welcome back!

And now comes the magic: dependent types are rooted in intuitionistic type theory, or constructive mathematics! And, as you may have guessed, inside this type theoretical framework, we also reject the law of excluded middle and the law of double negative (i.e `not(not(a)) != a`).

You see, even here. Even where you most expect things to be logical, clear, transparent, exact - they escape from you. They escape from your control and the structures in your mind. The very idea of logic - in the way we talk about it - is flawed: there are many formats for logic. If you are interested in this piece here, read more about, say, modal logic (have you heard about S3? did you know it was proven correct using TLA+, which uses some form of temporal logic, a form of modal logic?), linear logic (rust ownership types? wow!), and on goes the list. Wanna dig *even deeper*? Since you are still here, that means that you find at least the idea for the post interesting. I'll bet that the Curry-Howard-Lambek isomorphism will blow your mind.

Wow, what a kick! That means we were able to draw something from philosophy/logic to math and then back to programming?

This is the invitation and the purpose for this blog. Try to merge other areas with compsci, play with the idea. Maybe you are a genius in semiology and want to apply the triad of signs, signifiers and signifieds in programming (wow!). Maybe you are a hardcore cook and composition of food says something about commutativity in computer programs. I don't know! Give it a shot. Compsci has been left alone as an only child for two long.

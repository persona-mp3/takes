# package blog
## Why coding is somewhat still hard

Well, you've always heard a lot of engineers say 'writing code is the easy 
part' and in some way, I think they're right, and also very wrong.


I think they could be right because they moved across a different domain of thinking. 
For example, thinking about what the next line of code to write is different 
from deciding what database to choose for of your application depending on reads 
vs writes. It would make sense to say coding is easy part, from that perspective 
But from a broader perspective, that's simply not true. 
Code looks cheap and easy, until it isn't

### 1. Interfaces
---
We live in an era where code hasn't been more easier to churn. Through the use
of LLMs which are highly non-deterministic and really cool, the code they produce 
can sometimes come out as too clever, no coherent layers of abstraction or 
just strange things. 
We've tried to circumvent this through the use of 'SKILLS' or 'Agentic' stuff, 
but that just feels like a problem being patched instead of solved, where we 
try and bend it towards somthing of 'taste', 'best practices'
inside a non-determinsitic space

This really gets painful, the lower the stack you go.  For this example,
you don't really have to use LLMs, in fact I dare you to do it yourself.
For me, I've being working on building a distributed storage engine. The engine
layer is very simple,with WAL, lock-free readers and single-writer concurrency model
and it's written in Java. At the moment, it's doing right about 300k requests
per/second.  And arguably the harder part is the replication protocol, Raft 
which is written in Go.


For the first whole working version of the distributed storage engine, the Raft
codebase was a mess, and every milestone, I'd set aside time to refactor some 
aspect. I would start with deciding what part of the protocol needed to be done 
next, do a rough scaffold, and the when the bit has finally working, I would
take a step back and refactor it to make it look better, maintainable and more 
'extensible'. But I promise you, this can only work for so long, before you 
debate on becoming a farmer.
While there are 'Design Patterns', those are really general ideas that help i
in the grandscheme of sketching a picture, but you don't get the lines out of 
it. (at least in my experience). For example looking into the storage engine 
concurrency model the single writer could look like an Actor Pattern, but it
really isn't.


In the current rewrite of the raft protocol, I've started implementing Interfaces
mostly with the goal of opting into Deterministic Simulation Testing (DST), which
might look like 'Mocking' or 'Fault Injection', but it really isn't. 
While these patterns are REALLY helpful to know, it's mostly always sometimes 
a better to arrive to these things from first principles, because your project 
needs it.



And when the code eventually gets larger and more complex, you really start to 
feel the TODO comments all over the codebase, technical debt accumulated, 
minefields just waiting to be triggered. You start to see the flaws, and 
start debating whether to rewrite, patch, refactor or become a farmer

Looking into TigerStyle created by founder of TigerBettle, Joran Dirk 
he often mentions the way you design your code/interface is really important. 
The interfaces, error boundaries, abstractions. 
These, sadly don't get taught, and possibly, is also very hard to teach. 
Experience and domain is the best teacher for this kind of thing. 
```
 Do the hard part today, so tommorrow is easy - someone from TigerBettle
```

I've also seen this in actual codebases and from top-tier developers. If you
take a look at the first few commits of hashicorp-raft,  Armon Dadgar owns 
those commits and all he does are interface designs, and 90% of them are 
still in the codebase today as we speak. 



### 2. Documentation
---

This isn't talked about enough, and no one seems to be focused 
on it. How do we write good docs? How do we make sure they don't lie and are 
up to date? How do we maintain them? Because for some reason, some docs really 
do need jira tickets assigned to them, otherwise you have someone changing 
some parts of the code, and can only be caught in some code review, which could
have being prevented in the first place if the dev knew. I don't really have 
a solution to this.
Whether you like it or not, you begin to rely on documentation more than the 
code as it grows heavily. I had written a custom tool for infra and deplopyment 
for my initial raft protocol onto different servers and locally. I did have 
docs on them, but after 3months, it was sufficient to say that those docs did 
lie, they were not good enough. 
When I was fairly new to programming, I always heard people say 
`don't write comments`. `Comments should explain why not what`, but then they
give a useless example or nothing or also mention `code should be self-explanatory`

And by forcing a single constraint, you end up writing something relatively good
but could be better, or something entirely bad. 
This is the exact opposite advice I've seen in large OSS codebases. There are 
comments for every struct field. Imagine the stdlib of your language followed 
those rules, how would you feel? LSP's will become useless, you'd have to 
read the source code yourself and hope you understood it well or how to use it

One thing I would advise anyone to do to learn how to get better at documentation, 
is to look at how the stdlib for your language does it. And then take look at Go's 
stdlib docs.
And then look at the tooling for Go's docs and Rust too. And then look at 
highly specialised OSS projects, even though it's not your expertise, just do 
it like CockroachDB or Ghostty. More often than not, you will find 100 lines 
or more at the top of a file dedicate to explaining alot of the things in the 
file or package. 
Before writing a single doc or comment ask yourself, can a drunk version of me
understand this? Is it worth writing or does this block of code need docs? 
Can I say what this function does in less than 5 points? 
Question it, evaluate it like an idiot, give enough context is what I would say



### 3. Testability
---
I don't really like writing tests, just to preface that.
Well, I think this is also another thing gone wrong in the industry. We always 
scream TDD, BDD, write tests! pipeline must green! test failing! coverage 100% test 
must be! But after adding 50 tests that just tests things in a _when i implemented
it, it was working_ , you'll see that all 100 of you test files all look similar
While one size doesn't fit all, I think there's also some nuances to this 
too. And I'll talk about them later on.
First, tests should be not only be approached by `is this working within my 
assumptions and my rules?` but how `how do i find an edgecase that flips the 
world upside down?`. Property testing or fuzz testing are good examples of this


A good example is a friend of mine who worked for a US company mentioned they 
had an issue where a periodic batch process broke because of hex characters found 
in the data users provided because they copied and pasted text from other sources 
(I don't know how they got hex characters but that is really funny). 
As a dev, your mind wouldn't even think about that possibilty. So all the ascii 
validation and stuff lets it through. 

But I beleive that if there was a way they wrote tests that included 
non-ascii or utf8 characters or even some of the most obsure inputs that still 
fall into the range of a 'text', that shouldn't be allowed, they could have 
anticipated the bug or at least expected it


Secondly. Put more test effort on what is hard to get right, and has alot of 
invariants. For example, how do you test that a node in the network while 
it's in an Election state, it does not give out it's Vote to another node 
in an Election state. But also drops down to a Follower if someone has already 
won the election? How do you test networks? 


With these little nuggest, you begin to see how your code design and dare i 
say infrastructure really matters. Look into Will Wilson, CEO Antithesis one 
of engineers of FoundationDB. Look into TigerBettle and how they test their 
software 


On the little nuances, not every project needs Property based testing, neither 
do you need to mock the TCP transport layer. But start treating your tests like 
a hacker. Tests should not only verify that your implementation _works_, but 
it also handles edgecases you could have never thought of, look into fuzz testing
Prioritize your tests on which is the hardest to get right.


You'd be surpised how much this affects the way you write and design your code
In my own case, specifically the network and database communication layer. In the 
previous implementation, there were 0 abstractions over which made it hard to 
write tests, so I had to come up with  some 'simulation' that needed a cluster to
be running and hammer nodes, or hijack the whole cluster. But now, I've been 
looking to using interfaces for abstracting the network layer. So that I can
easily peice a fake one during tests. Using Go makes this much an easier feat 
in terms of interfaces.
While I've also tried doing the same for time, I haven't commited to doing that
yet, but etcd does implement a time/clock abstraction and you should try taking 
a look at it too.


Closing statement
---
While I do have a couple more to share on this like 
- Error Handling
- Logging
- Extensibility


The cases discussed are the core things to remove this guise that coding has 
been solved, or 'writing the code is the easy part'. You know maybe I haven't 
prompted enough, or haven't spent an hour tweaking a `claude.md` file or I'm 
simply the holding the tool wrong, but there's only so much you can get away 
with 

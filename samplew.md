---
layout: default
title: Why coding is somewhat still hard
---

<style>
  body, html {
    background-color: #1a1d23 !important;
  }

  pre.editor-block {
    background-color: #1a1d23;
    color: #e59a5c;
    font-family: "JetBrains Mono", "Fira Code", "Cascadia Code", monospace;
    font-size: 14px;
    line-height: 1.6;
    border-radius: 6px;
    padding: 16px 20px;
    overflow-x: auto;
    white-space: pre-wrap;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.4);
  }
</style>
<pre class="editor-block"><code>package blog
// Why coding is somewhat still hard
//
//
//
//
// Well, you've always heard alot of engineers say 'writing code is the easy
// part' and in some way, I think they're right, and also very wrong.
//
// Why I think they could be right is simply because they might be speaking from
// within their domain of operation and experience. And then later on moved to
// roles that demanded And I also think that's also
// wrong. Code looks cheap until it isn't
//
// 1.
// We live in an era where code hasn't been more easier to churn. Through the use
// of LLMs which are highly non-deterministic and really cool, the code they produce
// can sometimes come out as too clever, no coherent layers of abstraction or
// just strange things.
// We've tried to circumvent this through the use of 'SKILLS' or 'Agentic' stuff,
// but that just feels like a problem being patched instead of solved, where we
// try and bend it towards somthing of 'taste', 'best practices'
// inside a non-determinsitic space
//
// This really gets painful, the lower the stack you go.  For this example,
// you don't really have to use LLMs, in fact I dare you to do it yourself.
// For me, I've being working on building a distributed storage engine. The engine
// layer is very simple,with WAL, lock-free readers and single-writer concurrency model
// and it's written in Java. At the moment, it's doing right about 300k requests
// per/second.  And arguably the harder part is the replication protocol, Raft
// which is written in Go.
//
// For the first whole working version of the distributed storage engine, the Raft
// codebase was a mess, and every milestone, I'd set aside time to refactor some
// aspect. I would start with deciding what part of the protocol needed to be done
// next, do a rough scaffold, and the when the bit has finally working, I would
// take a step back and refactor it to make it look better, maintainable and more
// 'extensible'. But I promise you, this can only work for so long, before you
// debate on becoming a farmer.
// While there are 'Design Patterns', those are really general ideas that help i
// in the grandscheme of sketching a picture, but you don't get the lines out of
// it. (at least in my experience). For example looking into the storage engine
// concurrency model the single writer could look like an Actor Pattern, but it
// really isn't.
// In the current rewrite of the raft protocol, I've started implementing Interfaces
</code></pre>

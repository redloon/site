+++
title = "Niche Single-Purpose Programming Languages"
date = 2024-10-02T05:00:00.000Z
categories = [ "Misc" ]
tags = [ "rego", "sieve" ]
description = "Programming languages that do one thing, and do it well."
+++

A few languages that I've stumbled on recently that I find very interesting - they aren't general purpose like Python - they do what it says on the tin, and do it well. I think I'm also drawn to these languages because they often have great documentation that makes them seem like an attractive tool to work with. 

## Sieve - server-side mail filtering

A language built for filtering email messages, on either a client or server. I'm sure others use it as well, but Fastmail was what introduced me to [Sieve](https://sieve.info). They've done a bang-up job putting together an interface with a simple user interface for "writing" Sieve rules. They were humble enough to recognize they can't cover everything, though, so they get to join the ranks of software that I respect for adding an escape hatch for advanced use cases.

- [Sieve Tester | Fastmail](https://app.fastmail.com/sievetester/)
- [Using Sieve in Fastmail](https://www.fastmail.help/hc/en-us/articles/1500000280481-Using-Sieve-scripts-in-Fastmail)

## Rego - policy definitions

[Rego](https://www.openpolicyagent.org/docs/latest/policy-language/) is part of the Open Policy Agent project, and lets you write policy definitions that can be evaluated against inputs. They provide the best description of the project "OPA decouples policy decision-making from policy enforcement". In my mind, I think of it like writing a boolean function that decides whether an `if` block should be evaluated - but you were able to hide a ton of complexity in the function, despite the simple yes/no answer.

Despite what I just said, which is my mental model for it, _"Policy decisions are not limited to simple yes/no or allow/deny answers...your policies can generate arbitrary structured data as output."_. How cool is that?

## What else?

What languages am I missing? I'm very curious to see what else is out there that retains a niche scope, and providing a great developer experience.

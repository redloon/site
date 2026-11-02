+++
title = "Dumpster Fires and Humble Pies: A Junior Engineer’s Tale"
date = 2025-02-17T06:00:00.000Z
description = "From ego to empathy: What a messy codebase taught me about engineering, collaboration, and eating elephants one bite at a time."
featured = true
+++


I began my career carrying a quiet conviction: I cared about the craft of coding in a way corporate drones didn’t. While they mindlessly churned out code to meet ticket requirements, I was obsessed with elegance, readability, and the 'why' behind every line. If you’d asked me, I would’ve said I was just passionate, not judgmental. But if I’m being honest, I knew the truth: I thought I was better than them.  When my work team inherited a dumpster-fire of a corporate codebase, I was ready to throw it all away and start fresh. My tech lead had other ideas. Through his methodical approach, he didn't just fix the code — he showed me that the hardest problems in software often aren't technical at all.

## A dumpster fire we didn’t ask for

My journey towards humility began with a project we didn’t want: a system hastily built to meet a deadline, then passed on to us like a hot potato. Its purpose was to simply send a stream of API requests to an external Partner Company, notifying them when events occurred on our side. The rub: it had an overly complex architecture, was getting hundreds of error responses a day[^1], and the only solution they gave us was *“keep retrying”*. Needless to say, I was less than thrilled.

{{< bootstrap-panel title="⚠️" >}}
I’m explicitly not going to talk about the architecture details of the app, because I don’t need a bunch of yak-shavers giving unsolicited technical advice and missing the damn point.
{{< /bootstrap-panel  >}}

![architecture diagram of the error backlog generator application](/uploads/hot-potato-service-diagram.png#center)

As a brash junior engineer who had read too many [Joel Spolsky](https://www.joelonsoftware.com/) articles, I was fuming. I expected to be in awe of the systems that run our world after leaving the safe harbor of school. Instead, I was met with a sharp dose of reality. I was convinced the engineers who built this dumpster-fire must be lazy or incompetent, and the monstrosity of a system beyond saving.  I was confident my tech lead felt the same way, and was waiting for him to tell us to overhaul every process they gave us & rewrite this scourge off the face of the Earth. My tech lead had other plans. Instead of joining me in my outrage, he did… nothing.

## Chesterton’s Codebase

To be clear, we didn’t just sit on our hands. He had us sit down with the outgoing team for knowledge transfer. Then, to my utter confusion, he insisted we follow their processes. No refactoring, no rewrites, just observation and patience. It felt like watching someone stand idly by as a fire raged, extinguisher in hand. I couldn’t understand why he wasn’t rushing to put it out.

![a cartoon of a junior developer running to rip up a fence as a senior engineer tries to understand - the audience sees it protects from a cliff and angry swarm of bees, hidden to the characters.](/uploads/chestertons-fence-illustrated.webp)

After a couple weeks of dutifully watching the error backlog pile up, we learned the ins & outs of the system, and began to understand some of the choices the original developers had made. With a little distance, we could see the previous engineers weren't lazy or stupid - they were dealing with shifting requirements and tight deadlines. Eventually it clicked: my tech lead had multiple reasons for taking this approach. The first one which got through to my technically-minded brain was  [Chesterton’s Codebase](https://fs.blog/chestertons-fence/) — don’t start changing things until you understand why they exist. The other I hadn’t considered was that to a business, engineering time to refactor often costs more than sticking with a sub-optimal architecture you already have. It turns out engineers are expensive, and refactoring isn’t always the best use of their time.

## The Partner Company problem

Since this API was integrated with a Partner Company (PC) that legally required us to run it, we needed a working relationship with their teams catching what we were sending. Unfortunately, the previous developers had a… difficult… dynamic. They described the PC  as “*unhelpful*” and *“slow, not communicative”,* which isn’t a great place to start.

Upon digging deeper, we realized this was only one side of the story. The previous team’s communication strategy was something like:

- Send an email with a giant list of errors saying “*these are your fault, when will they be fixed?*”
- They then would reply *“many of these are 4xx[^2] errors, which means you’re sending something wrong”*.
- Cue the blame game. No one fixed anything, and the relationship soured


![playing the blame game - cartoon stick figures playing tennis but instead of a ball, they're passing errors back and forth](/uploads/error-blame-game-tennis.webp#center)

In a shocking twist, people don’t like being blamed for failures — especially when it might not be their fault. It’s like blaming your spouse for muddy footprints while ignoring the trail behind you. Of course the PC wasn’t going out of their way to help; they were too busy fending off accusations.

So how did my tech lead react to this situation? After taking stock of the landscape, he immediately sought to build up a rapport with the Partner Company and try to repair the relationship. We knew the service wasn't high priority on the Partner Company side, so patience and choosing your battles was required.  

He left his ego at the door and reached out to the PC, showing them we were willing to take responsibility and see where we might be at fault for errors. Once they saw we weren’t just pointing fingers, they immediately became more responsive. Through his efforts, my tech lead got everyone working collaboratively towards the goal of a stable service, rather than an adversarial blame game. Seeing this dramatic shift made it clear to me that it’s much easier to solve problems by making allies who want to help than enemies you have to force to your will.

## Eating the elephant

We now had a better understanding of how to operate the system and a Partner Company willing to work alongside us, but we were still experiencing the same daunting deluge of errors growing our backlog every single day. Something needed to change, and soon.

Without panicking, our tech lead took the pile of error messages we had, sorted them by volume & severity, then went to work investigating one at a time, finding a root cause before moving on to the next. At no point did he suggest refactoring the entire system. It turns out being humble and considering some of the 4xx errors could actually be our fault was a good choice - because there were in fact numerous bugs in our system. He guided our team to detecting and fixing these errors that all the retries in the world couldn’t have fixed.

Where I saw an insurmountable mountain of errors, my tech lead saw a series of solvable problems. His calm, methodical approach revealed a different path—one that didn’t involve trying to fix everything at once.  Instead, he stepped back, took a holistic view of the problem, and broke it down into manageable pieces. Then, with a clear sense of priority, he tackled the most urgent issues first.  It was a lesson I’ve carried with me ever since, perfectly captured by a tweak to an old saying: *“how do you eat an elephant? One (prioritized) bite at a time”*.


![A cartoon of a timid figure intimidated by an elephant named "Error Backlog" while a senior engineer prepares to chow down.](/uploads/error-backlog-elephant.png#center)

## Boring is good

Within a couple months, the service had become relatively stable and boring, in a good way. What had seemed insurmountable at first had quietly melted away. By understanding the system, building relationships, and tackling issues methodically without panic, my tech lead had transformed our experience from stressful firefighting to playing ping-pong during deploys. These days, when I catch myself judging a ‘messy’ codebase, I remember this early lesson: great engineering isn’t just about clean code—it’s about understanding the humans on the other side of the API.

---

[^1]: These errors had a long SLA to get fixed, so it gave us breathing room where we didn’t have to work around the clock to fix them immediately.

[^2]: 4xx means HTTP error codes between 400-499, this is usually reserved for situations where the error is happening on the sender side, and the receiver is saying *“Hey, you didn’t give me the right data/format/etc for me to do my thing.”*

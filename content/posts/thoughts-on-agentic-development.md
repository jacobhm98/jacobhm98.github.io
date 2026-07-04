+++
title = 'My thoughts thus far on agentic development, and its implication on the future of software engineering'

date = 2026-07-04T13:40:54+02:00
draft = false
+++

Sometime in January of 2026, the raw capability of LLMs in relation to software engineering crossed a critical threshold.

Until that point, I had not primarily been using LLMs to generate code. Instead, I would write the code "by hand", but when faced with a specific syntax question or (_gasp_) a need to craft a SQL statement, I would shoot off a question to my favorite LLM.

I had tried using LLMs for end-to-end code generation, but found the tradeoff not worth it. While they generally produced code faster than I did, they produced code of a low quality: poorly organized for sure -- monstrous methods and plenty of "run-on sentences", but also more often than not with obvious and non-obvious bugs. Thus, completing a ticket often took about the same time with and without an agent writing the code.

My reasoning was that if there was no time saved, why would I sacrifice on the readability, understanding, and enjoyment I got from writing code?

## The shot heard round the world

In January, this calculus, in my experience, fundamentally changed.

I was now able to hand Claude most "ticket level" pieces of work, spend some time iterating on a plan, leave him to go off implementing, and more often than not, the code was, although not always tastefully, usually correctly implemented.

This, although heralded by gradual, iterative improvement in model capability, signified a critical threshold being crossed. The implications were, and have continued to be, immense.

### My new workflow

This newfound increase in model autonomy allowed my workflow to change. Whereas before, I needed to be present at pretty much all stages of development in order to course correct inevitable errors, now I needed to be present only at the beginning and at the end of a task.

When kickstarting a task (always in plan mode), I start off by giving the model initial instructions and acceptance requirements. It then begins its exploration phase -- reading through the code base and, by any other means, finding necessary context pertaining to the task. When complete, the model presents a detailed plan for review. This plan contains key architectural decisions, which files will be modified and how, which automated steps the model will take for automated verification at the end, etc...

I usually still have a few back and forths with the model at this stage. I will tell it to use a slightly different pattern for accomplishing a specific task, or ask it to organize the code slightly more consistently with the code base, and sometimes larger course corrections are necessary. Generally, the larger the task, the more work I need to put into getting the plan correct at this stage.

When acceptable, I tell it to auto-implement the change. This will often take upwards of 20-30 minutes.

When finished, I will manually verify the behavior is correct.

I will usually spin up a local development environment mimicking production with the changes added, open a PR and do a relatively quick review. If all LGTM I'll deploy.

Usually, most of my mental energy is spent at the beginning of the task.

This, compared to the previous model of work where I needed to be mentally present during all parts of the development process, unlocks a few things.

#### Parallelization

The obvious change this unlocks is parallelization. A large portion of work will now happen unsupervised. This means that you can spawn multiple agents in parallel, each working on their own thing independently. Through this, you can optimize your workflow to focus on the portion where you _do_ need to be mentally switched on -- the planning and verification phases.

In practice, I have found that 4 parallel agents is a good number. Any more and I find myself not giving each task its proper due, any less and I'm spending time twiddling my thumbs waiting for an agent to complete.

#### Practical challenges

This change presented a few novel challenges.

First, there was now a serious need to allow the agent to independently obtain as much useful context as possible.

Some things we have implemented that work well to this end:

1. Give your agent read-only access to your data
2. Write CLAUDE.md/AGENTS.md files pointing your agent to which systems contain which data, and instructions for how to access that data.
3. Use monorepos!

The third point, combined with parallelization of tasks, presented a new problem.

Normally, when you work in git, you work on a task sequentially, you check out a feature branch, develop, merge, and repeat.

With agents, especially with monorepos, you need to facilitate the parallel, but independent, modification of your codebase. Git worktrees are the common way to accomplish this, they check out your repository into a separate directory, allowing "self-contained" changes.

In my opinion, raw worktrees are lacking.

1. They don't, by default, bring files that are not version controlled along for the ride
2. They don't natively allow for development environment isolation
3. They require manual cleanup and tracking if you care about keeping your dir tree clean

I think the second point here is really key. If we think back to how the development process has changed, writing code is no longer the bottleneck. Instead, the planning and verification of changes are.

Thus, we want to make both of these processes as streamlined as possible.

The second point in the list above facilitates manual verification. We need to be able to rapidly spin up a development environment with the changes self-contained, check that it works well, and tear it down upon task completion.

This need has facilitated an explosion of "worktree managers". A few popular alternatives:

- https://github.com/max-sixty/worktrunk
- https://github.com/kdcokenny/opencode-worktree
- https://www.conductor.build/
- https://github.com/jacobhm98/yati

The last one was built by yours truly, and is a tool I use pretty much every day, check it out! I'm particularly proud of my solution to problem 2.

## The implication

### Death of a software engineer

The first, tragic, implication of this change, is that the trade of software engineering has profoundly changed. A lot of the skills that we have painstakingly built up over years are no longer valuable. Apart from some niche contexts, the skill of writing code by hand no longer carries the same value it once did.

To me, this was a hard pill to swallow. I genuinely enjoyed writing code. I enjoyed the mental challenge of it, the beauty and clarity and satisfaction delivered by a complex problem being broken down into its constituent parts, and the solution frozen in code.

The good news is that if you fully embrace these tools and the new workflows they bring, you'll find that the key part of what made you fall in love with software engineering is still present.

To most of us, I suspect that the allure of software engineering can be ascribed to two reasons:

1. solving interesting, hard problems
2. crafting technology that delivers value to people

If you primarily cared about the second reason, you're golden. Building usable software has never been easier than today.

If you primarily cared about solving interesting problems (through code or otherwise), a worry I had was that as these models get better and better, there won't be any interesting problems left to solve.

### Why that's wrong

Using myself as a case study, I have fully embraced agentic software development over the past half year. I have not meaningfully hand written code during that time. Sure, I still open up a text editor to read code sometimes, and I'll do the odd one line or config change by hand, but the vast majority of code I produce, goes via an LLM.

I am still as busy as ever.

Perhaps busier than I've ever been. Predominantly with technical work.

How come? What's stopping me from outsourcing all of it to the agent? Where is the bottleneck now occurring, if it isn't code production?

An impression I got from my time at Spotify working with extremely strong engineers is that most of the art of engineering lies in making the right trade-offs.

Oftentimes, you'll have two or more solutions that are all entirely technically correct. But the particulars of your situation: how the solution will interact with the rest of your system; _what_ your users truly care about; how you anticipate the needs of your company will develop in the future; what the shape of the team maintaining the system long-term will be; etc..., will nudge the fit of each alternative _for you_ in one direction or another.

Making the choice between one correct solution and another, is where I find more and more of my mental capacity going. This is still technical work -- I need to understand each option completely, I need to have a complete understanding of the system I'm working on, and I need to have a mental model of how I think the system is likely to develop in order to make the correct choice.

#### Stewarding and planning

I am also still critically involved in stewarding the system we have built. This is, and has always been, part of the role of a software engineer. As time passes, your user profile or count changes, or your data consumption and processing patterns change, and as a result your system is placed under novel strain. Operating such a system means continuous introspection in the form of metrics and alerts, and making the correct assessment of which parts need to be investigated, made more efficient, or given more resources.

Debugging outages or errors is also something I am wholly "in-the-loop" for.

Similarly, foreseeing what we need to build and why is something that is critically done by humans. We need to ingest feedback, talk to users, think about world trends, and synthesize all of this into a roadmap.

### What's stopping HAL?

I think there are a few things I have going for me.

I am able to learn on the job.

Over time, this materializes into a deep understanding of the system I am working on. I can tell you why each part is built the way it is. I can give you a history of problems we have had, and the modifications we made in response. I can tell you where we've had most issues recently and why. I can describe our users to you, what they care about, what their usage patterns are. I understand the motivations of my colleagues, what they care about and which directions they want our product to move.

An agent, given a specific context, is more or less a deterministic machine. That is to say, an agent's behavior is entirely dependent on their context. They do not learn on the job.

This means they never build up "understanding" for the code base they are working on, or the business and people they are producing code for. They have gotten extremely good at mimicking this understanding for scoped tasks by intelligently enriching their contexts autonomously, but they still need someone to give them scoped tasks!

I also think there's a level of wisdom/intelligence they lack. This to me, is apparent when debugging. Unless it's a simple problem where the error message contains a smoking gun, and a simple grep points the agent to where the problem is occurring, I have found that the agent often jumps at the first plausible answer it sees. It takes a human in the loop with understanding of the problem domain to see that this can't possibly be the right answer and then course correct.

Don't get me wrong, I am tremendously reliant on agents when debugging. But I instruct it specifically what data to pull, what hypothesis to verify and how, and will ask questions to poke holes in its reasoning until I am content. I also always ask it to present me with means of verifying the conclusion.

To me, this is further evidence of the lack of long-term understanding that is necessary for software engineering.

## Still worth loving

The benefit is, if you embrace this new way of working, use the agent as a tool rather than as an intern, and build up an intuition for where your experience and skill is necessary, you can still spend most of your time solving interesting problems.

In fact, if you embrace the new, higher level of abstraction that an agent affords you to work at, you will be able to solve a much wider array of problems, quicker. Not having to worry about semantics or the syntax of specific CLI commands allows you to work at a rate more closely approximating the speed of thought. It's almost like learning how to touch type.

The other, obvious benefit, the reason a lot of us got into this in the first place, is that you will be able to deliver useful software much more rapidly than in the past.

I want to caveat this by saying that I am working on a startup with 2 other people. This means that there's exceptionally little in the way of bureaucracy or process, and most of my time is spent building, shipping, fixing, and improving software. For various reasons, most of them valid, this would not be the case at an established company.

With that said, if I take a typical "Jira-ticket-sized" task (create a new API handler with wiring for a specific set of resources, for example), previously I'd probably get through one in about 5 hours of heads-down work.

Currently, the end to end agent workflow would probably take 2 hours. This includes all 3 stages from before, planning, independent implementation and verification, and manual review + deploy by me.

If I were just working sequentially, this would be a reduction in time by a factor of 2.5x.

The key insight however, is that I am able to run ~4 of these tasks in parallel. This makes me a ~10x engineer compared to my previous self.

## Where this leaves us

All this to say, I see the current crop of agents as an immensely powerful _tool_ to be wielded in the arsenal of a competent human software engineer.

The bottlenecks for complete autonomy are in my mind fundamentally incompatible with the current form of LLMs. We need on-the-job learning, human-to-human understanding, and an interface into real-world problems in order to be great software engineers. LLMs are constrained by all of these.

However, if you embrace LLMs, there has never been a better time to build software. Your productivity will go through the roof, and if done well, your enjoyment of software development will follow.

I don't see this changing until the technology underlying the LLMs changes in some fundamental way to address the above constraints.

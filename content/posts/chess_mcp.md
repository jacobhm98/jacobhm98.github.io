+++
title = 'Building a chess MCP'
date = 2025-07-08T11:33:12+02:00
draft = false
+++

## Recent life

I recently found myself with more free time and less direction than I've had since joining the prestigious ranks of _real adults_ sometime in my early 20s. It's been fun.

I spent the first week doing absolutely nothing productive. I tented for 4 days somewhere in the forest in southern Sweden while attending a techno festival, spent a combined 20 hours on trains to and from Cologne in order to attend a Denzel Curry concert with an old friend, and started reading a new book, [Growth](https://www.danielsusskind.com/growth-a-reckoning), so far so good!

However, sometime between the last paycheck hitting my bank account and the epiphany that all this fun requires financing, my fingers have started itching more and more for something productive adjacent to do.

## Catching up with recent developments

Of course, I knew before beginning this journey that I was out of the loop w.r.t the fast moving ML landscape. But as the unknown unknowns became known unknowns it was even difficult figuring out _what_ I wanted to learn.

### Vibe coding

I knew that at Spotify I'd had limited opportunity to vibe code properly. When you're writing code for large, enterprise systems model context and complexity quickly become bottlenecks. Additionally, no matter how good the AI gets I suspect there's always going to be an impulse urging restraint from fully embracing the vibes when I know that the code I write will end up serving millions of paying customers. So vibe coding was something I really wanted to test out.

### MCP

[Model context protocol](https://modelcontextprotocol.io/introduction) (MCP) is an open source protocol developed by Anthropic. It specifies an extensible way of exposing tools and context to large language models.

A definition from the official website:
"MCP is an open protocol that standardizes how applications provide context to LLMs. Think of MCP like a USB-C port for AI applications. Just as USB-C provides a standardized way to connect your devices to various peripherals and accessories, MCP provides a standardized way to connect AI models to different data sources and tools."

Ok, but what does that concretely mean? Why does that warrant all the associated hype?

A realization I had was that the MCP is a way of giving agency to LLMs.

The standard LLM is sandboxed to a chat interface. Of course, you can interact with it through an API and construct business logic for when it gets invoked and how it's responses are handled in a traditional SaaS as if it were just another service dependency.

This likely works great for simpler AI based applications, you know deterministically at what point in your UX flow the LLM should be invoked, and what you want to use its data for. However, the LLM is fundamentally still 'just' text in -> text out.

If you've spent any time at all sifting through the AI circlejerk present in various unspecified internet ecosystems, you, like I, know the agentic revolution is coming.

#### What is the agentic revolution?

The agentic revolution is the u/dystopian vision of a near future where a team of agents can do somewhere between most and all of common knowledge worker tasks.

A thought experiment:

1. Given that the agentic revolution has already happened
2. We no longer have accountants/lawyers/software engineers
3. What does the workflow of a SWE agent look like?

Likely not that different from the workday of a human engineer. It needs to write code, interact with colleagues, get business directives, deploy code changes, monitor the health of the system, etc...

Humans use separate tools for most of these tasks. We communicate with slack, use github for persisting code changes, grafana/datadog/pagerduty for monitoring, etc...

All this to say that I can not just function with text in/text out.

This is precisely what the MCP does. It allows you to give your LLM access to a set of preconfigured "tools", that it then decides autonomously when to invoke and with what parameters.

So, if you wanted to create a software engineering agent, you'd need to hook it up with an MCP server for each of the above mentioned examples in order for it to carry out all the tasks you'd expect a software engineer to carry out.

Of course, given today's agents, I really doubt this is a sufficient advancement, but I do think it's a required precondition.

## The project

Alright, so I knew I wanted to vibe code something. And I knew that the MCP is interesting, and likely to become more and more relevant as LLMs continue to develop. 1 + 1 = vibe coded MCP server.

Doing what?

### Can LLMs reason?

I'd spent some time reading through "The Illusion of Thinking: Understanding the Strengths and Limitations of Reasoning Models via the Lens of Problem Complexity" by Shojaee et. al., and its rebuttal "The Illusion of the Illusion of Thinking" by A. Lawsen. I've summarized and added my thoughts on both papers, [here](https://github.com/jacobhm98/interesting-research-papers/blob/main/the_illusion_of_thinking/the_illusion_of_thinking_summary.md) and [here](https://github.com/jacobhm98/interesting-research-papers/blob/main/the_illusion_of_thinking/the_illusion_of_the_illusion_summary.md), but a TLDR:

- A team of apple researchers attempted to test the ability of cutting edge LLM's to reason through various toy problems, like the [tower of hanoi](https://en.wikipedia.org/wiki/Tower_of_Hanoi), and measured its ability to solve increasingly more complicated starting configurations
- They found that LLMs can reason their way to answers of simple starting configurations accurately
- However, there was a critical point where a problem became sufficiently complicated causing the LLM to be able to make essentially zero meaningful progress towards an answer, regardless of how much compute you allotted it
- Critically, even if the researchers provided the models with an algorithmic solution to the problem, they still could not improve their reasoning performance

Obviously, this is a severe condemnation of true step-by-step reasoning capabilities.

The rebuttal by Anthropic can be summarized as:

- Methodological errors
- The model is smart enough to recognize that it needs to type out a long and convoluted solution, which it does not want to do "The pattern coninues, but to avoid making this too long, I'll stop here"
- If you instead ask the model to produce a code solution that solves the problem, they can do so incredibly well.

### Chess

Well, as a surprise to no-one I've recently gotten really into chess (software engineer becomes a chess fanatic -- more breaking news at 6). While reading through the above papers, I couldn't help but wonder how the LLM's would do at chess. Chess is obviously an incredibly complex game, where logical, multi-step reasoning is required in order to avoid hanging your queen (foreshadowing).

Another aspect of this is that chess is more or less a solved game. Sure, we don't have an exhaustive, irrefutable solution to chess due to the problem complexity, but since Deep Blue beat Kasparov in 1997, computers have been significantly better than humans at chess. Just last week Hikaru, a world top 3 short time control player [lost decisively](https://www.reddit.com/r/chess/comments/1loqt0n/leela_destroys_hikaru_with_rook_odds_32/) to a chess engine _with rook odds_, meaning that Hikaru started the game up a full rook (second most valuable piece after the queen).

Again, 2 + 2 = vibe coded LLM chess MCP server.

### Building the server

Honestly, this part went incredibly smoothly. I fully endorse the vibes. I downloaded claude code, created a fresh repository, and within an hour I'd prompted my way to a functional java MCP server without writing a single line of code myself, allowing me to create a game through the chat interface, and play a game with claude as my opponent on lichess. Spaghetti code is [here](https://github.com/jacobhm98/lichess-mcp), if you'd like to try it out -- though for your own sanity don't look too closely at the actual code (1k LoC file of straight ObjectNode manipulation, anyone?).

### Initial results

The first [game](https://lichess.org/oYRJmKSo7jjV) was encouraging for humanity.

Now I'm not amazing at chess. I've been playing with some consistency for the past two years, but my peak is ~1400 rapid on chess.com. That puts me in the 95th percentile of all active accounts, so not a schlub by any means, but against Magnus Carlsen I'd have approximately a [0.02%](https://www.wolframalpha.com/widgets/view.jsp?id=b2c8a0f7c1a7b589decddef06a1179) chance of winning.

Within 5 moves, claude had hung his queen for 0 compensation.

However, after noticing claude had no idea his king was in check, I realized he couldn't actually see the state of the board. He was just making standard developing moves that sounded sensible, with no regard for what his opponent was doing.

Obviously, seeing the board is a critical component of playing chess.

### Second try

So after a bit more prompting, claude expanded the MCP server to allow him to fetch the state of the chess board. We [tried again](https://lichess.org/5n9W63kQHaEg).

After 14 moves, I was sweating. Claude had entered the mainline of the marshall attack with 100% accuracy, something I can count on one hand encountering from my 1400 rated human opponents.

However, on move 15 came the critical mistake. Claude pushed f3 allowing a devastating bishop sac on g3, after which the evaluation was -3.

Now Claude was out of book, and his severe lack of rational thinking ability was evidenced by his missing of a mistake I made attempting to prematurely bring my knight into the attack, after which the eval swung from -7.3 to +1.4 (I'm 1400 remember), instead allowing mate in 1.

Interestingly Claude was able to attempt legal moves only, suggesting he was able to deduce _some_ internal state of the game, though he proved to be completely unable to rationalize his way to any decent moves when the game branched out from the opening theory that was certainly present in his train set.

### Relating my findings to the illusion of thinking

I think my limited sample roughly supports the conclusions of apple's research team's criticisms. In a problem space of sufficient complexity where the LLMs need to rationalize their way to an out-of-training-set solution, they rapidly fall apart.

### Relating this to the illusion of the illusion of thinking

Maybe claude is absolutely smart enough to beat me at chess, he just cant be assed to reason his way to the best move.

If we instead take the approach of the anthropic rebuttal and allow claude to generate the program logic he needs to beat me, maybe he'll do a better job?

### Vibe coding a chess engine

Only one way to find out -- vibe coding a chess engine. Obviously, this has the possible drawback of there being plenty of chess engines in claudes train set, but that was conveniently glossed over by the anthropic rebuttal, so we'll take the same approach here.

#### First engine game

This was definitely an interesting [game](https://lichess.org/SVvHODVM). The engine could deduce legal moves, and chose to open 1.d4, which is definitely a rarer opening than 1.e4 showing that it wasn't necessarily just pattern matching opening theory any longer. However, the engine evaluation function definitely needed some improvement, as again the queen was hung on move 6.

#### Second game

I suggested claude make his engine evaluation slightly more accurate, seeing as he hung his queen in 6 moves. After much thinking and code manipulation, claude was happy and we played another [game](https://lichess.org/AhjzHBLN). Again, claude was playing an unorthodox opening, knights on the rim, and moving the same piece multiple times, but no obvious errors for 12 moves. However, again, on move 13, claude hung his queen.

I will give him some more credit for this one, it required seeing a knight fork, and realizing the only way to move his king out of check was by giving up the queen, a mistake I could easily see a <400 human player making.

However, still far off a genuinely usable engine.

## Conclusion

To me, theres a few learnings I gained from this project.

1. MCPs are cool. They allow you to extend the toolkit of an LLM, allowing them to interact with essentially any piece of software you program it to. I think this is an area worth continuing to explore.
2. Vibe coding is useful, but extremely limited. I saved so much time, and perhaps more importantly, mental energy by vibe coding the MCP from start to finish. It created a self contained, useable software component. However, two distinct limitations made themselves apparent. One, the code it created was an absolute abomination. You couldn't pay me to manually go in and fix any bugs claude introduced -- I'd much rather start from scratch. Two, some tasks are still much too complicated for claude to figure out completely on its own. There are open source chess engine implementations that are surely in the claude train set, however a chess engine of any capacity will be a complicated piece of software, and creating a decently capable one falls squarely outside of claude's capabilities, for now.
3. I'm still doubtful that LLMs can actually reason. I err on the side of the apple research team in the debate, when it comes to true, logical, step-by-step reasoning I'm not super impressed, at least if you think chess is a decent proxy for this.

## Closing words

Thanks for reading! Let me know if you have any interesting research papers you think I should read, or let me know if you constructively disagree with anything I've written here!

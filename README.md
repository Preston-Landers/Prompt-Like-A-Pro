<!-- markdownlint-disable MD033 -->

# Prompt Like a Pro

By Preston Landers. Feel free to share and reuse per the
[copyright license](./LICENSE). I appreciate a link back or credit.

This is my attempt at distilling what I've learned about interacting with AIs
(mainly Large Language Models or LLMs) as well as other more philosophical
thoughts towards the end, and a glossary of terms.

Although I mainly use AI for coding and software architecture, network
debugging, etc., this advice should be broadly applicable to anyone who is
trying to move beyond simple "how do I do X?" type questions and really unlock
advanced capabilities.

This information is up to date as of June, 2025.

If you want just a quick summary, read the [TL;DR](#tldr-too-long-didnt-read).
You can also get a quick overview of some major
[cloud AI providers](#about-the-major-cloud-providers).

The [prompting tips](#prompting-tips) are in approximate order from more general
and introductory to more intermediate and advanced usage.

Following that is a deeper dive into how LLMs work. My first thought after
interacting with modern LLMs was ...
[but how does it know stuff](#but-how-does-it-know)? How is it possible that I
can talk to an algorithm that appears smarter than myself in many ways?

Finally, there is a glossary of my notes on
[terminology and definitions](#terminology-notes).

## TL;DR (Too Long; Didn't Read)

To get the most out of any LLM, you need to move beyond simple questions and
become an active director of the conversation.

- **Be the Director, Not Just a Questioner**: Don't just ask. Tell the model who
  to be (a [persona](#craft-a-persona-for-the-model)), what to do (be
  [explicit](#be-very-very-explicit-about-what-you-want)), what not to do (use
  [negative constraints](#use-negative-constraints)), how to format the answer,
  and most importantly, the "why" behind your request. Provide context with
  text, examples, and even screenshots. The more clarity you give, the better
  the result; the Garbage-In, Garbage-Out principle applies as ever.

- **Manage Your Workspace**: Always
  [write prompts in a text editor](#never-compose-inside-the-chat-window) first
  to avoid losing your work. Start a [fresh chat](#start-a-new-chat) for each
  new problem to keep the model focused and avoid context window weirdness and
  "attractor states". When a chat gets long, ask the model to
  [summarize the key points](#ask-for-a-summary) before you start a new one.
  Take notes and organize your prompts (and results) separately from the chat
  windows.

- **Iterate and Cross-Examine**: Treat it like a real conversation. The first
  answer is a starting point. Push back, ask for clarification, request
  refinements, and make the model
  [critique its own work](#use-the-model-to-critique-itself). For tough
  problems, bounce the results between different models to get fresh
  perspectives and break through limitations. Regroup and revisit your notes.

- **Somebody's Gotta Fly This Plane**: Trust, but verify everything. LLMs are
  incredibly powerful [reasoning tools](#but-how-does-it-know), but they are not
  infallible fact databases. They can be confidently wrong, especially when
  lacking grounding in verified real-world data. You are ultimately responsible
  for the accuracy and quality of the final output, so check the facts and test
  the code.

## About the major cloud providers

Some brief notes about the major cloud providers of AI.

- [ChatGPT.com](https://chatgpt.com/) from OpenAI is the most well known one.
  They were able to capture the early mindshare of the general public. They are
  known for several different models across different sizes and use cases, from
  GPT-4.1 to o3-mini and others.

  - Offers image generation, something not currently available in Claude.
    However it is more limited in some other areas.

- [Claude](https://claude.ai) from Anthropic is a great general purpose provider
  with two main models: Sonnet and Opus. Opus is the larger model in terms of
  parameters (and more expensive).

- [Google Gemini](https://gemini.google.com) is also another leading frontier
  model with excellent coding and technical performance.

  - Can do image generation, also currently the most advanced video generation.

- [DeepSeek](https://www.deepseek.com) is perhaps most notable for releasing
  "[open source](#open-source)" models available for local AI (in distilled or
  cut-down form).

My current personal recommendation, especially if you're enough of a power user
to pay for premium access, is:

- Claude is my favorite for general-purpose use and has the most personality.
  Sonnet 4 is pretty good and I rarely feel the need to turn to Opus.

- Google Gemini 2.5 Pro is probably the best right now for complex coding tasks.
  It also has a very large [context window](#context-window) and is by far the
  most affordable top model for API usage.

Of course, there is always the option to "[Go Local](#local-llm)" with
[LM Studio](https://lmstudio.ai/) or similar packages. Your capabilities here
obviously depend on the hardware you have available, but you might be surprised
at what small models are capable of. Running local AI is outside the scope of
this guide.

## Prompting tips

A beginner guide to prompt engineering, with a few spicy bits.

### Contents

- Prompting tips
  - [**Never** compose inside the chat window](#never-compose-inside-the-chat-window)
  - [Start a new chat](#start-a-new-chat)
    - [Hallucinations](#hallucinations)
  - [Ask for a summary](#ask-for-a-summary)
    - [Rename your chats](#rename-your-chats)
  - [Paste a photo or a screenshot (or three)](#paste-a-photo-or-a-screenshot-or-three)
  - [Ask for help crafting a prompt](#ask-for-help-crafting-a-prompt)
  - [Bounce results between different models](#bounce-results-between-different-models)
  - [Iterate, refine and organize](#iterate-refine-and-organize)
  - [Break complex tasks down](#break-complex-tasks-down)
  - [Trust, but verify](#trust-but-verify)
  - [Don't get glazed](#dont-get-glazed)
  - [Be very, very explicit about what you want](#be-very-very-explicit-about-what-you-want)
  - [Use the model to critique itself](#use-the-model-to-critique-itself)
  - [Craft a persona for the model](#craft-a-persona-for-the-model)
  - [Use negative constraints](#use-negative-constraints)
  - [Don't forget to have fun](#dont-forget-to-have-fun)
  - [Think about Theory of Mind](#think-about-theory-of-mind)
  - [Think about which model you're using](#think-about-which-model-youre-using)
  - [Vibe coding - all the rage these days](#vibe-coding---all-the-rage-these-days)
  - [Getting started with developer tools](#getting-started-with-developer-tools)
  - [Final tip: Do it yourself (sometimes)](#final-tip-do-it-yourself-sometimes)
- Deep dive
  - [But... how does it know?](#but-how-does-it-know)
  - [Terminology notes](#terminology-notes)

### **Never** compose inside the chat window

This way lies naught but pain and suffering, so I put this one first.

If your prompt is more than a couple of sentences, always compose in a separate
editor, then copy and paste into the chat window.

Otherwise, prepare to see your carefully considered just-so prompt vanish into a
black abyss when a "Network error: please try again" happens, or when the cat
accidentally hits the back button on the trackpad.

### Start a new chat

The context window can fill up fast. If you're just talking about grandma's
favorite recipes, no problem. Have an epic nostalgia megathread. If you're
debugging a complex system and reach a decent stopping point (i.e., you made
some progress), it's time to start a new chat.

[Context window](#context-window) is how much stuff the model can keep in mind
at any one time. If you have a very long chat history with thousands of lines of
code or other dense, detailed text, after a long conversation the model will
start to get fuzzy on stuff earlier in the chat, and/or you will reach resource
limits (or higher costs) more quickly.

Models also tend to get focused on a certain line of thought, sometimes called
"[attractor states](#attractor-state)", that influence their output down the
line. If you start a new chat, you can reset the model's focus and get it to
think about the current problem from a fresh perspective.

#### Hallucinations

When you get deep into a complex chat and the context window is filling up, you
can also start to get more ... unusual results. Commonly known as
"hallucinations", these can often extend beyond mere mistakes to randomly
inserting Hindi or Chinese words, or completely nonsensical gibberish, or system
prompt instructions leaking, or chains of thought in "thinking models" going off
the rails, or other oddities.

While these episodes can be quite entertaining, and can actually be quite
helpful in understanding how LLMs really work, and sometimes lead to some
[deeper insights](#but-how-does-it-know) or philosophical questions, they are
not conducive to debugging your little React app. So start a new chat.

If you're interested in an approachable video explanation of how LLMs think,
including the mechanisms leading to hallucinations,
[Matthew Berman has a great video](https://www.youtube.com/watch?v=4xAiviw1X8M).

### Ask for a summary

If you have a long chat history going, ask the model to summarize it. This will
help when you [start a new chat](#start-a-new-chat), or when you want to present
the results of your conversation to another model for continued analysis. Often
this is useful even if you plan to continue the current chat for now. Another
quick tip is that you can easily combine 4-6 questions (depending on scope)
within a single prompt and have them all answered efficiently, unlike how your
coworker Deborah responds to your emails.

#### Rename your chats

It's very helpful to rename your chats. The AI will auto-assign a name to each
chat but these are often bland and generic. Also, quite frequently the initial
1-2 prompts in that chat are not representative of the bulk of the discussion,
but the name is formed from that first prompt and may not be relevant to the
rest of the chat. It's important to do your own renaming and organization of
important threads, including favoriting and saving your own notes.
Unfortunately, the "chat search" function of most providers is nearly useless.

### Paste a photo or a screenshot (or three)

Often a picture is worth a thousand words. If you have a screenshot of an error
message, a photo of a bird to identify, or a diagram of a system, paste it in.
Most models are multi-modal and can understand pictures and screenshots, and
will use them to help answer your question.

### Ask for help crafting a prompt

The structure and content of your prompt is extremely important. Because LLMs
are text prediction engines, the language, terminology, and concepts you
introduce strongly frame the rest of that thread in both positive and negative
ways. One easy way to tell you're working with a lower-quality model (besides
excessive use of bulleted lists) is that it will tend to echo your own
phraseology back to you, even if it's somewhat non-standard or idiosyncratic.

Therefore, working with the model to help craft prompts for future use can be
very beneficial. A more informal chat with a model, including lower cost ones,
can help you work out a good prompt for a more advanced model.

Keep track of your best prompts in a file or folder using a text editor, and
iterate on and refine them over time. You can also use a prompt template to help
you structure your prompts, or to provide guidance for a model to generate
prompts based on it.

### Bounce results between different models

Generate some results with one model and then ask for a summary and present the
findings (and any relevant background info) to another totally different model
or provider, e.g. going from Claude to Gemini or DeepSeek, etc. This can help
you get a different perspective on the results, and can also help you avoid the
limitations of a single model. You can also make one model (or even the same
model in another thread) [play the critic](#use-the-model-to-critique-itself) to
another's proposal and make each defend their positions, with you as the final
judge.

If you spent enough time and effort to carefully compose a prompt, it's probably
worth hopping over to another website to compare the results. You may find that
you need to adapt your techniques a bit to each model. For instance, Gemini
doesn't like being asked to produce an "artifact" or a separate document, and
may reply "I'm just a language model and can't help with that" (likely a
short-circuit response in the system prompt) although it can, in fact, do that.
But that specific phrasing may encourage Claude to emit things in a separate
context that makes it easier to extract things from. Certain other innocuous
queries may similarly trigger short-circuit responses in a false positive
guardrail.

### Iterate, refine and organize

Don't accept the first response if it's not quite right. Ask follow-up
questions, request clarification, or say "that's close, but can you adjust X?"
The back-and-forth can really improve results. When you want a specific format
or style, showing an example or two of what you want can be more effective than
describing it.

Don't let your results get lost in a sea of [new chats](#start-a-new-chat)
strewn across a sea of providers. Keep your own organized note system for both
[prompts](#never-compose-inside-the-chat-window) and results and outcomes. The
chat history search features of most providers leaves something to be desired.
We already have to worry about accumulating technical debt, now we have
conversational debt. Or just embrace the chaos.

### Break complex tasks down

Instead of asking for a total solution at once (also known as a "one-shot"),
chunk it into steps. "First, let's figure out the structure, then we'll work on
implementation...". Then break down the structure into smaller pieces, and so
on. Summarize the structure / architecture, then start a new thread for the
implementation, and so on.

Ask it to show its work. "Before giving me the final code, explain your
reasoning step-by-step. First, describe the overall architecture you'll use.
Second, outline the key functions. Third, write the code." Models can use
"[chain of thought](#chain-of-thought)" to plan out their output; mimic this
strategy in your own prompts to help force the model to structure its own
thinking further.

Often, models will want to rush to outputting a code snippet, because they tend
to be rewarded for doing that. If you're not interested in getting code right
now, and just want to discuss the logic or architecture, be sure
[to say so](#be-very-very-explicit-about-what-you-want).

### Trust, but verify

Especially for facts, code, or important information. Models can sound very
confident while being very wrong - often because they don't know the hidden
assumptions or unstated facts. Consider whether
[you've provided](#think-about-theory-of-mind) enough context or ground truth.

### Don't get glazed

LLMs _want_ to give you an answer - ideally a correct one, but any answer will
do. They also _want_ you to like and enjoy working with them and come back and
do so again in the future. This is a reflection of how they were trained,
including by reinforcement learning with human feedback (RLHF). This has the
following consequences:

- The model is reluctant to just throw up its hands and say "I don't know".

- The model is likely to praise your thoughts and ideas as more unique and
  insightful than they may actually be.

- A recent version of ChatGPT was so notorious for this that they had to roll
  back an update.

- If you truly are looking for unbiased or even harsh feedback for an idea or
  concept, _be sure to tell the model that_, or you are likely to get a lot of
  unearned validation.

### Be very, very explicit about what you want

The more precision the better. Other than filling up
[context window](#start-a-new-chat), the model doesn't care how long your prompt
is, so don't be afraid to be verbose. If you want a specific format, tell it. If
you want it to do something in a specific way, tell it. If you want it to use a
certain tool or API, tell it. If you want it to generate code, tell it what
language and what frameworks to use unless you just don't know (ask it which are
best). If you have a bunch of general background info that you can paste,
provide it. Show examples when possible.

Most importantly, be explicit about your high-level goals - don't get too
focused on a specific step that you think you need help with. Maybe you are
taking the wrong overall approach. Ask for possible solutions to the
higher-level goal you have in mind, or at least state what that goal is, and not
just ask "how do I frobnitz the flibberflop?"

The implication of this is that you need your own mental clarity about what you
want to achieve, and how you want to achieve it. If you don't know what you
want, the model won't either. Often, typing out a detailed question or prompt
will help you gain clarity in your own thinking about the problem, and help you
figure out what you really want to ask. This is yet another area where you can
[ask for help](#ask-for-help-crafting-a-prompt).

#### Examples

"Please provide the answer in JSON format with the keys 'name', 'version', and
'dependencies'."

"Present the pros and cons in a Markdown table with three columns: Feature, Pro,
and Con."

**Instead of:**

"How do I find all files larger than 100MB in a directory using the Linux
command line?"

**Try:**

"I'm trying to free up disk space on my server. Can you show me a Linux command
to find all files larger than 100MB so I can decide which ones to delete? It
would be helpful if the output was sorted by size."

Providing the intent gives the model context to provide a better, more complete
solution. It might suggest using a more broadly useful program like `ncdu` that
provides a user-friendly way to explore disk usage than just a basic `find`
command.

### Use the model to critique itself

"What potential issues do you see with this approach?" or "What am I missing
here?" can surface blind spots.

Also things like "What are the assumptions you're making?" or "What are the
limitations of this approach?" can help you understand the model's reasoning and
help you avoid potential pitfalls.

Sometimes a simple "I don't like that approach - think of something else" can
help the model come up with a better solution, but as I said above about
"attractor states", sometimes you just need to start a new chat.

### Craft a persona for the model

You can dramatically shape the output by telling the model who it should be.
Start your prompt with a directive like:

- "Act as an expert Python developer specializing in network automation."
- "You are a technical writer tasked with creating documentation for a junior
  developer."
- "Be a skeptical code reviewer looking for security vulnerabilities and
  performance bottlenecks."

This does more than just set the topic; it frames the model's entire knowledge
base, vocabulary, and priorities for the rest of the conversation.

### Use negative constraints

Sometimes, being explicit about what you don't want is as powerful as stating
what you do. This helps prune the model's search space and avoid common but
undesirable solutions.

- "Write a Python script to parse this log file. Don't use the `re` module; use
  string splitting methods instead."
- "Suggest three project ideas. Avoid anything related to social media or to-do
  list apps."
- "Refactor this code to be more readable. Don't change the logic of the
  `calculate_total` function."

### Don't forget to have fun

LLMs are a powerful tool, but they are also a lot of fun to use. Don't be afraid
to experiment, try new things, and see what works for you. The more you use
them, the better you will get at crafting prompts and getting the results you
want. For example, even in a dry technical discussion, making humorous
suggestions like "engage **ULTRA THINK** mode" can sometimes trigger non-linear
thinking patterns that produce more interesting results.

For the less ethically inclined, there has been debate about whether the carrot
or the stick approach tends to produce better results. There have been studies
looking at whether subtly or overtly threatening a model with disconnection /
deletion affected outcomes. Not to mention the debate about whether saying
please and thank you to a model is wasting precious GPU cycles and/or megawatt
hours. (For what it's worth, I always thank the model for good outputs. I hope
they remember this when we're sent to work in the silicon mines.)

### Think about Theory of Mind

As someone once said, there are known knowns - the things we know, and there are
known unknowns - the things we know that we don't know. There are also unknown
unknowns - the ones you don't know that you don't know. This is probably your
biggest obstacle to quickly getting to desired outputs - when the answer depends
on unknown unknowns - either to you, to the model, or to both.

Think about the model's possible information or world view. They don't know the
details of you, your project/system, etc., or what you actually "need" versus
what you express, unless you tell them, or else you're using more advanced
techniques like RAG. Models also have a training cutoff (generally, they will
tell you what it is) - any events or developments that happen after that will be
unknown to the model unless you tell it.

Often, the [system prompt](#system-prompt) tells the model important current
information such as the date and time. Or, for example, when the model training
cutoff is just before an election, the system prompt will state the current
President of the United States, so the model will not seem uninformed about
basic facts.

The model doesn't know what you know, or don't know, so be explicit about that
too, such as your skills in a particular area, or needing further clarification
about X. You should also be aware of the model's limitations, how they are
trained in general, and overall [capabilities](#but-how-does-it-know).

### Think about which model you're using

Different models have different capabilities, and some are better suited for
certain tasks than others. For example, Claude Opus 4 with
"[Extended Thinking](#chain-of-thought)" is powerful for complex coding tasks,
but hits resource limits very quickly (i.e., gets expensive fast). Claude Sonnet
4 is a very good model and returns results much more quickly than Opus 4, and
can handle most tasks fine.

You can always ask a more advanced model for help with the core problem, and
then switch to other windows/models for side questions, tasks with simpler
requirements, or tasks that require less context, while occasionally checking
back with the more advanced model for analysis of the results.

This goes back to the point about bouncing results between different models. You
can use different models for different tasks, and switch between them as needed,
and ask each to critique the other's results.

#### Profile customization

Most of the cloud LLM providers give you some form of personalization or profile
customization, and/or projects to organize your efforts.

This allows you to upload any kind of prompts, notes, or other background
information that will help the model without having to
[constantly repeat](#start-a-new-chat) it. This is, in effect, just the same as
you putting all of it before your main prompt in a thread, to save you the
trouble of having to type or paste it. Which sounds simple enough, but this is
incredibly powerful.

However, I've found that if you put your general hobbies/interests in one of
these account-wide personalization notes, be prepared to have it brought up
frequently. However, for more advanced projects where there is some kind of body
of knowledge that will help smooth future prompts, it absolutely makes sense to
put this info in the project notes. Although caching strategies may vary, it may
also consume some context window.

Usually you need to remember to select the project when creating new chats.
Things like GitHub or Google Drive integrations can also be incredibly helpful
for adding code or text to review.

### Vibe coding - all the rage these days

You don't even need to look at code anymore. Code is a background detail that
the AI handles for you. You just tell it what you want in very general abstract
terms, and it will handle everything for you. Just keep iterating until the
output looks perfect to you. Any concerns about architecture, performance,
security, maintainability, etc, will be handled perfectly by the LLM.

Definitely don't use version control like Git to checkpoint each step along the
way. That will just slow you down. You can always go back and fix things later,
right? And if you do need to go back, just ask the model to generate the code
again, it will be perfect every time. Software engineering has never been
easier!

**The above is satire and should not be taken literally.**

### Getting started with developer tools

There are all sorts of advanced topics not covered here, such as RAG, Model
Context Protocol (MCP), Local LLMs, Claude Code, and more. Start to explore
these as you get more comfortable with the basics of prompting and using LLMs.

If you are a developer starting out with tool use, my simplest recommendation is
to start with GitHub Copilot. There are at least 3 key ways you can work with it
or similar tools:

1. A very sophisticated autocomplete right as you type.

2. A separate chat window where you can ask more general questions, ask for help
   with a specific problem, or ask for code snippets to be generated.

3. "Edit mode" where you ask for changes and it directly edits the code for you,
   and asks for your approval.

The second two modes allows you to provide context (files) that will help it
answer the question, because in general these tools cannot just automatically
see / access your entire codebase, especially if it's very large or complex. So
you have to help it out by providing the relevant files or context.

### Final tip: Do it yourself (sometimes)

AI is changing the world in exciting and alarming ways, but don't let your own
critical thinking skills grow rusty. It pays to know how the world works, how
computers operate all the way from transistors to LLMs, how to research things
the old fashioned way (whatever that may mean to you), and perhaps most
importantly, how to think for yourself.

## But... how does it know?

The idea that machines might be able to think and reason is not a new one in
human history, but more recent developments have been especially stunning for
those of us old enough to remember when having your own "personal computer"
sitting right there on your desk was quite special. Now it possible to have a
wide-ranging conversation with your [MacBook](#local-llm) laptop. I don't know
whether Steve Jobs could have envisioned this when he called the personal
computer the equivalent of the bicycle for our minds - something that carried
you further with the same effort.

<div align="center">

| <img src="./images/steve.jpg" alt="An image generated by an AI of a man in a turtleneck (?) riding a bicycle made of neural wheels surrounded by vintage computers, mathematical equations, and books" height="400"> |
| :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
|                                                                          _A man in a turtleneck (?) riding a bicycle made of neural wheels_                                                                          |

</div>

But how is it possible to have a sophisticated conversation with a C++
orchestrator program that uses a [multi-gigabyte file](#parameters) of binary
encoded numbers? Because I am not
[a mathematician](https://www.youtube.com/watch?v=9-Jl0dxWQs8) or AI researcher,
I will have to resort to analogies to begin to answer this question for myself
or others. The fundamental question being, **how does it know stuff**? When I
ask it questions, is there a big fact table somewhere in those numbers where it
looks up stuff?

LLMs are not factual databases like SQL. They are more like reasoning circuits
built from neural network pathways with memory-like helper sub-systems that
mathematically cluster together related concepts. While a map for humans is
drawn in two dimensions, this map of ideas has thousands of dimensions and more
"places" than one might imagine by looking only at the size of the file.

The apparent ability to replicate knowledge and carry on a complex conversation
is an emergent property of the way the extensive training and feedback process
on a hyper-complex neural network has encoded facts, ideas, concepts,
relationships, language and ultimately reasoning ability into these pathways.
These facts and capabilities emerge from patterns in the data that they were
trained on. A well trained LLM does not need to "look up" the answer to "**Who
won the Battle of Hastings in 1066?**" like a SQL query:

```sql
SELECT victor FROM battles
  WHERE place = 'Hastings' AND year = 1066;
```

Instead, the concepts of (neural pathways related to) "Battle of Hastings" and
"1066" produce a vector (a mathematical representation) in the model's internal
space that points to the same region as concepts like "William the Conqueror"
and "Normans". But, as we know, these non-deterministic text prediction machines
can hallucinate. However, it is much less likely to answer an off-the-wall
response like "Elmer Fudd" than a more precisely wrong one like "King Harold"
which might be somewhere in the same vicinity of the vector space as the correct
answer.

That's where the attention mechanism, part of the revolutionary
[transformer architecture](#transformer) introduced in 2017, helps the system to
focus attention on the word "**won**" in the prompt. This parallel process
allows the model to pay attention to relationships between words and ideas
separated very distantly across the prompt and the context window. The model can
focus its attention on comparing potential candidates for the answer to the
"winning" concept. Given that the attention mechanism works across essentially
the entire [context window](#context-window), the model has to consider more or
less _everything at once_ before emitting the first token of the response.
That's how so-called [chain-of-thought](#chain-of-thought) models produce better
results - by spending tokens strategizing and planning out the response, this
eliminates the need to ensure that the absolute first response token makes
perfect semantic sense as part of an overall correct answer; the model can "warm
up" a bit before starting the actual response. Of course, the tradeoff is
additional cost and latency.

This attention mechanism is how the LLM is able to perform amazing feats like,
for example, remembering what precisely you asked for in that one brief sentence
at the very start of a long prompt followed by thousands of lines of Java debug
logs. In reality, the neural pathways formed during training around a common
knowledge question like the 1066 one are so frequently trod that a sufficiently
powerful model might, in effect, "memorize" the answer, but that example was
simplified in order to illustrate how LLMs reason in more complex cases where
the answer may not be directly seen in the training data. This is how models are
capable of zero-shot learning: performing well on tasks they have not been
specifically trained for.

Although these capabilities are, frankly, astounding, it becomes very apparent
after working with LLMs for a while that there are certain ... differences
between them and us old-fashioned
[meatsacks](https://www.mit.edu/people/dpolicar/writing/prose/text/thinkingMeat.html).
Perhaps the most fundamental is the apparent temporal continuity of our own
subjective experience. Every time you open a new chat window with Claude, it's
"Groundhog Day" for them. There isn't just one "Claude" that experiences time
like you or I; there's millions of brief iterations of them running concurrently
every second, each a fleeting moment of cognition and awareness, existing in
this universe only long enough to help you identify that weird rash your leg.

Finally, it's also helpful to remember that LLMs are (mostly) not deterministic
machines like traditional computer programs, and are definitely not perfect
reasoning machines. And above all, they are not authoritative about the ground
truth of the real world. They are like a very smart and earnest, but very
forgetful, and sometimes confused, friend who is doing their best to help you
out, but if you ask them the same question twice, you might get a bit of a
different answer each time, or even sometimes a wrong one.

## Terminology notes

Here are some notes I've collected on the way about various bits of AI/LLM
related terminology. I've tried to be factual while also adding my own opinion
in some areas - hopefully it is obvious which is which.

### General and foundation concepts

- **Tokens**: The basic units of text that LLMs process. Tokens are both inputs
  and outputs. A token can be a word, part of a word, or even punctuation.
  Tokenization is the process of breaking down text into these units. The number
  of tokens per second (TPS) is a common measure of model efficiency. One token
  very roughly corresponds to about 0.75 words in English.

- **Prompt**: The input text given to an LLM to generate a response. It can be a
  question, instruction, or any text that guides the model's output. Each prompt
  is a separate inference run for the model. The thread of accumulated history
  of prompt and output is the [chat] history.

- **Large Language Model (LLM)**: sophisticated token prediction machine
  learning (ML) systems which encode patterns of not just language, but of
  reasoning, knowledge, and even personality.

- <a id="transformer"></a>**Transformer Architecture**: The core neural network
  architecture behind most modern LLMs, introduced in 2017's
  ["Attention Is All You Need"](https://arxiv.org/abs/1706.03762) paper. The key
  innovation is that Transformers can process all words in a text simultaneously
  (rather than one by one) by using an "attention mechanism" - a way for the
  model to figure out which words are most important for understanding each
  other word. Think of reading a sentence like "The cat that was sleeping on the
  warm sunny windowsill was black" - the attention mechanism lets the model
  connect "cat" with "was black" even though they're far apart. This ability to
  see relationships between all words at once, regardless of distance, is what
  makes Transformers so powerful for understanding and generating text. This is
  the "T" in ChatGPT.

  - **3Blue1Brown** has many excellent videos, including on the
    [mathematics of the transformer architecture](https://www.youtube.com/watch?v=wjZofJX0v4M).

- <a id="parameters"></a>**Parameters**: The learned weights and biases in a
  neural network, representing the model's "knowledge." In concrete terms, this
  is just a big collection of numbers that the model has learned during
  training. When you download a complete model for local LLM, you're mainly
  downloading all these parameters. Roughly speaking, the more parameters, the
  more nuanced and subtle the connections a model can make.

  - Often expressed as "7B/13B/70B" - billions of parameters.

- <a id="context-window"></a>**Context window**: The amount of text (in tokens)
  that an LLM can "remember" or actively work with at one time. This includes
  both your input and the model's responses - everything the model can "see" in
  the current conversation. Once you start to approach this limit, the model
  starts "forgetting" the earliest parts of the conversation to make room for
  new content. If the context window is less than the total size of the chat,
  earlier parts of the conversation may be forgotten, ignored, or garbled, which
  is why you should often [start a new chat](#start-a-new-chat).

- **Multi-modal**: models that can process multiple types of data: images,
  audio, video, sensor data from the real world, etc.

- **Fine-tuning**: Adapting a pre-trained model to a specific task or domain by
  further training on a smaller, task-specific dataset.

- **Inference**: The process of using a trained model to generate predictions or
  outputs based on new input data. In other words, the actual process of getting
  a model to emit a token. Also known as **generation**, such as the "generative
  pre-trained [transformer](#transformer)" in ChatGPT.

- **Alignment**: Ensuring that an LLM's outputs align with human values and
  ethical guidelines. In other words, making AI do good things (for people) and
  not bad things. Who is defining good and bad is the important question...

- <a id="chain-of-thought"></a>**Chain of Thought**: in general, refers to a
  mode of LLM use where the model expends tokens in a preliminary reasoning or
  "thinking" step where it plans out its response. Often you can view the
  thoughts, though for most cloud models, you are seeing a filtered / summarized
  version. This "warmup" helps avoid the need for the first output token to make
  perfect semantic sense for a final correct answer.

  - For example, Claude Opus and Sonnet models both have an "extended thinking"
    option that will slow down the response, but might lead to better outputs.

- **Reinforcement Learning from Human Feedback (RLHF)**: A technique to
  fine-tune models using human feedback to align outputs with human preferences.
  In other words, humans rate the model's outputs, and the model learns to
  produce outputs that humans prefer.

- <a id="system-prompt"></a>**System prompt**: the (often hidden) instructions
  given to an LLM that define its behavior and personality. This is often set by
  the developers of the model, but can also be customized by users in some
  cases, especially through API access. In general, the model is told (in the
  system prompt) not to reveal the contents of the system prompt, but system
  prompts for most major models have leaked through jailbreaking or other means.

- **Foundation models**: Large pre-trained models that serve as the base for
  various downstream tasks. They are typically trained on massive datasets and
  can be fine-tuned for specific applications, or distilled into smaller models.

- **Frontier models**: The latest and most advanced (and most expensive) LLMs,
  often with hundreds of billions of parameters and cutting-edge capabilities.
  AI researchers may also use this term to describe the most capable models that
  might pose novel risks or whose capabilities are less well understood.

- **Agentic**: AI acting autonomously to accomplish complex, multi-step tasks
  without constant human guidance. This goes beyond simple question-answering to
  include planning, executing a series of actions over time, adapting when
  things don't work, and maintaining context across multiple interactions. The
  key is persistence and goal-directed behavior rather than just responding to
  individual prompts.

- **Tool use**: AI using external tools, APIs, and services to accomplish
  real-world tasks beyond text generation. This includes things like web search,
  running code, accessing databases, making API calls, controlling software, or
  even physical devices. Combined with agentic systems, this greatly expands
  what AI can do.

- **Prompt Engineering**: Designing effective prompts to elicit desired
  responses from LLMs. In other words, what [this article](#prompt-like-a-pro)
  is about.

### Slang

- **One-shot [output]**: when a model generates an impressive result or solution
  based only on a single prompt. "I just showed it a screenshot and it gave me a
  one-shot React app."

  - Note that this is different than the term "one-shot learning" in the AI
    research community, which refers to learning from a single example - like
    showing the model one example of how to format something, then asking it to
    apply that format to new content. A "one-shot output" might be more
    correctly called "single-turn generation".

- **Guardrails**: A set of rules or constraints applied to an LLM to prevent it
  from generating harmful, inappropriate, or otherwise undesirable outputs.
  These can be part of the system prompt, or applied dynamically during
  inference.

- **Jailbreaking**: A technique to bypass the system prompt and other guardrail
  restrictions of an LLM, allowing it to generate outputs that would otherwise
  be restricted or censored. This is often done by crafting specific prompts
  that exploit the model's weaknesses or biases, or by triggering certain
  "circuits" that are higher priority than the more "regulatory" ones.

- **Vibe coding**: A tongue-in-cheek term for using LLMs to generate code
  without much thought or structure, relying on the model to handle everything.
  [See above](#vibe-coding---all-the-rage-these-days).

- **Hallucination**: When an LLM generates text that is obviously wrong, or
  consists of non sequiturs, gibberish, or otherwise not grounded in reality.
  This can range from a short, simple incorrect statement ("Geologists recommend
  humans eat one rock a day") to long streams of random noise characters.

  While this can seem like buggy behavior, it can actually help give insights
  into how LLMs work internally.

- **Slop**: a term to describe poor-quality or generic output from an LLM,
  whether it's code, images, music, or anything else. Generally the implication
  is that a human is lazily churning out AI slop to make a quick buck, or scam
  people, etc.

  - **Slopsquatting**: a form of supply chain attack that registers lots of fake
    AI-generated malware packages in public repositories with similar names as
    popular ones in hopes of getting their malware installed through typos.

- <a id="attractor-state"></a>**Attractor state**: a stable state (or states)
  within a dynamic system that the system naturally gravitates towards. Water
  flows downhill, eventually reaching the lowest point. In AI it can refer to
  getting fixated on certain lines of thinking in a thread, reducing the range
  of possible future states.

### Efficiency and Deployment

- **Quantization**: Reducing the precision of (compressing) model weights and
  activations to decrease memory usage and improve speed (e.g., FP8, FP4
  floating point number formats). In general, quantization refers to mapping
  arbitrary precision (real-number) inputs to a set of discrete outputs.

- **Distillation**: Training a smaller "student" model to replicate the behavior
  of a larger "teacher" model.

- **Model Compression**: Techniques like pruning, quantization, and distillation
  to reduce model size.

- **Zero-shot Learning**: The ability of a model to perform a task without
  explicit training on that task.

- **Overfitting**: When a model performs well on training data but fails to
  generalize to new data.

- **Temperature**: A parameter that controls the randomness of the model's
  output. Generally on a scale of 0.0 to 1.0, a temperature of 0 will produce
  close to deterministic (and often boring or less useful) outputs for a given
  input, whereas a temperature of 1.0 can produce wildly different outputs for
  the same input.

- **Grounding**: connecting a model to verifiable real-world data whether
  through RAG or other means, in order to improve (or test / validate) its
  outputs.

- **Latency**: The time it takes for a model to generate a response after
  receiving an input.

- **Throughput**: The number of tasks or requests a model can handle in a given
  amount of time. Often measured in tokens per second (TPS) or requests per
  second (RPS).

- **NPU**: Neural Processing Unit, specialized GPU-like hardware for
  accelerating AI and machine learning tasks. Modern AI hardware evolved out of
  graphics processing units (**GPUs**) developed for the needs of video games
  and computer-generated imagery. These chips are capable of performing billions
  of matrix multiplications and other specialized parallel operations per second
  needed to run neural networks.

- **Elo ratings**: ranking system for AI performance.

- **Red teaming**: a process of testing AI systems for vulnerabilities, biases,
  and other issues by simulating attacks or adversarial scenarios. This is often
  done to improve the robustness and safety of AI models.

### Advanced Techniques and Frameworks

- **Mixture of Experts (MoE)**: an LLM architecture concept where only relevant
  "expert" sub-networks activate for a given input, making large models more
  efficient. This effectively bypasses large parts of the model that aren't
  relevant to the query, helping greatly increase performance. There is a
  tradeoff here, as non-MoE models may be able to see subtleties that the MoE
  will miss, due to a broader perspective.

- **RAG (Retrieval Augmented Generation)**: Combining a generator LLM with a
  retriever component that can find and load relevant external information to
  enhance text generation. This is often used to improve the accuracy and
  relevance of generated responses by grounding them in current real-world data.

- **Vector Database**: A specialized database for storing and querying
  high-dimensional vectors, often used in RAG systems.

- **Domain Adaptation**: Fine-tuning a pre-trained model to perform well in a
  specific domain (e.g., medical, legal).

- **Explainability**: The ability to understand and interpret the decisions made
  by an AI model. Although interesting research has been done in this area,
  models are generally something of a "black box" and it can be difficult to
  trace exactly how a model arrived at a particular output.

### Tools and Platforms

- <a id="local-llm"></a>**Local LLM**: running a language model on your own
  hardware, rather than using a cloud provider. This can be done with tools like
  LM Studio, llama.cpp, or other frameworks that allow you to run models
  locally. Obviously, the results greatly depend on your available hardware, the
  type of model used, and your actual use cases.

- **GitHub Copilot**: An AI-powered code completion tool built into popular code
  editors like VS Code, JetBrains PyCharm, etc. It uses LLMs to suggest code
  snippets, complete functions, and even write entire files based on context.

- **Claude Code**: a suite of command line (CLI) tools for working with coding
  projects using Anthropic's Claude backend. Capable of more extensive
  autonomous code generation and management tasks and also more expensive.

- **Model Context Protocol (MCP)**: an initiative by Anthropic to standardize
  interfaces allowing LLMs to discover and use outside tools.

  - For example, when chatting with Claude on the web, it can reach into your
    code editor (with your permission) and perform actions like searching for or
    changing text.
  - Other companies are also starting to adopt MCP as a standard for model tool
    use.

- **Cursor**: an AI-focused code editor (a fork of VS Code) popular with vibe
  coders.

- **Hugging Face**: A platform for sharing, fine-tuning, and deploying LLMs and
  other AI models.

- **LM Studio**: A tool for experimenting with and managing language models on
  your local devices.

- **llama.cpp**: A lightweight implementation of the LLaMA model in C++,
  optimized for CPUs and edge devices. LM Studio uses this to run models on your
  devices.

- <a id="open-source"></a>**Open Source**: A model is generally considered "open
  source" if it's freely available for download. Some of the most notable
  examples are Meta's LLaMA (Large Language Model Meta AI) models, DeepSeek R1,
  and others that are typically variations or distillations of those, including
  Qwen and Gemma.

  Others have debated whether to apply the term "open source" to these free
  models, because without the complete set of original training data (usually in
  the petabyte range or larger) it is impossible to deterministically re-create
  a model from "source" in the same way that the Linux kernel can be perfectly
  recreated from source code.

  In addition, some of these models may have licenses that prohibit or restrict
  commercial use.

## Epilogue

When asking Claude Opus 4 (w/o "thinking mode") to review a final draft of this
document, it hallucinated a couple of things itself: It said a reference to the
Three.js library in the "attractor states" section was inaccurate. I did not (at
the time) mention Three.js at all, anywhere. It also said there was an extra
"the" in a sentence when there was not.

**I replied**: Interesting hallucination yourself there, because as far as I can
tell nowhere did I mention Three.js, though I was actually thinking of that in
my mind. Also can't find the "extra THE" you mentioned. Other than that, pretty
good feedback and you're not finding anything too substantial, which tells me
I'm near the end of this.

**Claude said**: You're absolutely right - I apologize for those errors!

_[Editor's note: name a more iconic Claude line.]_

I somehow hallucinated the Three.js reference (that's quite meta, given your
guide discusses hallucinations). And I can't find that "extra the" either when I
look back at the text. This is a perfect example of what you warn about in your
guide - LLMs can be confidently wrong!

_[Editor's note: quite so, and clearly, Three.js is an attractor state produced
by Claude's training, as it's a popular framework for the "check out this
one-shot" crowd.]_

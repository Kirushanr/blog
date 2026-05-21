---
title: 'Getting started with AI with KI : Part I'
category: AI
---

![Getting started with AI with KI Cover](https://gist.github.com/user-attachments/assets/9f664815-7cbc-4dbc-aa24-0e2708a273e3)

## Introduction 

Getting your head around AI can be confusing, and keeping up with the changes is even harder. AI is a tool. We all learned to crawl, walk, and then run; the same principles apply to learning about a new technology or tool. I am writing this hoping that it will help someone get started and open a new door for them.

These resources helped and are still helping me understand the actual nuts and bolts behind these technologies and the jargon. In addition, they helped me differentiate between fluff and fact so that I can make grounded engineering decisions when building, using, and securing AI technology. I keep going back to these resources as needed (you can't remember all of this because there is way too much stuff to remember).




*** I have referenced individuals and their work, I have no commercial or personal association with them.


## Foundations of AI Engineering 

<img width="674" height="667" alt="image" src="https://gist.github.com/user-attachments/assets/94fc5c9c-4fb6-459b-8b09-f3fd2fa3ca22" />

Before I could get into the nuts and bolts of AI security, I wanted to understand the basics, so I found these resources useful for building a theoretical understanding of the topic.

1) [AI Engineer Road Map](https://www.aihero.dev/ai-engineer-roadmap) [FREE]
Matt Pocock distilled the entire AI Engineering concept into a free mini course, that is super easy to understand.
This resource covers all fundamental concepts of AI Engineering.

2) [AI Engineering](https://www.oreilly.com/library/view/ai-engineering/9781098166298/) written by Chip Huyen  [PAID]
This book is a great reference material that you can keep going back to understand AI engineering concepts in depth.
Also the author has created a companion GitHub repository https://github.com/chiphuyen/aie-book that includes summary of chapters and resources that can be handy.

```
Cool tip:  if you are reading this in Australia ( I am not sure about other countries), 
your local library may offer free request a book option/interlibrary loan that allows
you to request a book if it is not in their catalog ***
Support your local library, when you request a book you also support an author ! 

***(Terms and conditions apply check with your local library).
```

## AI Agents  

<img width="825" height="413" alt="Krang from Teenage Mutant Ninja Turtles" src="https://gist.github.com/user-attachments/assets/e96f1a71-f965-464a-8280-7e4d27b33314" />


Now we have the foundational theoretical understanding of AI topics.
If we define agent by an equation it takes the following form [[1]](https://addyosmani.com/blog/agent-harness-engineering/)[[2]](https://www.langchain.com/blog/the-anatomy-of-an-agent-harness) : 

Agent   = Model + Harness 

Model   = Foundational models OR Open models  OR your own model

Foundational models = Secret sauce big Frontier labs don't share publicly how the models are made

Open Models  = Think of them as how to make sauce as per your needs, you can tweak it 

Harness = Tools + Memory + Runtime + Guardrails 

Tools   = Think of them as function calls, think of them as hands of AI

Memory  = Brain of AI, so that AI can remember things 

Guardrails = To prevent your agent from falling off the balcony 


1)[This Agentic AI course](https://www.deeplearning.ai/courses/agentic-ai) (FREE) by Andrew Ng covers the theoritical building blocks of Agentic AI workflows, architecture and so on. You can audit this course for free, but the coding excercises requires access to a paid subscription !

2)[Agent Patterns](https://docs.cloud.google.com/architecture/choose-design-pattern-agentic-ai-system) (FREE) If you need a reference to different Agent architecture patterns this article from Google may come in handy.

## Building your own agents 

<img width="1612" height="1038" alt="Shows a scene Gilfoye's AI Deleted All Software from the TV Show, Silicon Valley" src="https://gist.github.com/user-attachments/assets/39444d94-6e65-4c87-84a7-f2cf621a1699" />

Now this is a part that gets trickier, there are lots of frameworks out there including not limited to 
Crew AI, Pydantic AI, LangGraph, AWS Strands and the list grows on .... 

All frameworks have their own strength and weaknesses but they rely on the same fundamental principles and patterns I referenced in the previous sections. 

Anyways, I started using AWS Strands because I felt their documentation and getting to started guide is easier to setup . I used Anthropic as my model provider instead of Amazon Bedrock. I have linked to all getting started and free training that is available for all the frameworks I have listed above.

1) [AWS Strands](https://strandsagents.com/docs/user-guide/quickstart/overview/)

2) [LangGraph Training](https://academy.langchain.com/courses/intro-to-langgraph) and [LangGraph Documentation](https://docs.langchain.com/oss/python/langgraph/quickstart)

3) [Pydantic AI](https://pydantic.dev/docs/ai/overview/)

4) [CrewAI](https://docs.crewai.com/en/quickstart) and [Building Multi Agent systems - Deeplearning.ai](https://learn.deeplearning.ai/courses/design-develop-and-deploy-multi-agent-systems-with-crewai/)


## AI Evals 

<img width="1600" height="900" alt="This image is sourced from Hamel's blog, that shows XKCD's comic Machine Learning and the new AI version of that" src="https://gist.github.com/user-attachments/assets/57166020-41b9-4ae2-bffb-1e70c72e5adb" />

Now this is section i am yet to dive deeper, but I have understood the basics of what AI Eval is (this is also covered in Andrew Ng's course that I linked before). AI Evals is about evaluating the quality and accuracy of your AI Systems/Products. 

1) [LLM Evals: Everything You Need to Know](https://hamel.dev/blog/posts/evals-faq/evals-faq.pdf) and [What Are LLM Evals](https://hamel.dev/blog/posts/evals-faq/what-are-llm-evals.html) This is probably the good free resources that that is out there on this topic, a guide written by Hamel and Shreya. I, also suggest following Hamel's [blog](https://hamel.dev/) and his [YouTube channel](https://www.youtube.com/@hamelhusain7140/playlists) 


In Part II and future posts(I am being overly optimistic here), I will cover some of the defensive AI security resources and example implementations that can be very useful !. I am yet to dive into some of them but I will write about them once I get there.

Thank you for reading this post. Hopefully you learned something today! I will see you in my next post….

<img width="220" height="165" alt="skeletor-running-away" src="https://gist.github.com/user-attachments/assets/2d8214b5-ad8e-4e09-af75-15ebc330018d" />
 
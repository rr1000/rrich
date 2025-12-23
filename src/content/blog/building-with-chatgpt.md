---
title: "Building with ChatGPT"
date: 2023-04-16
description: "A guide for non-experts on building apps with AI assistance."
---

In this blog post, I will guide "people who know enough coding to be dangerous" on build applications using ChatGPT. Follow the steps below to create a solid foundation for your application development process.

### Choose a Framework and Learn the Basics:

Select a framework like Rails, Django, or Express, and familiarize yourself with it. Complete one or two tutorials to gain a basic understanding of how things work. There's no need to become an expert at this stage, just get comfortable with the framework.

### Define Clear Requirements:

Write down every detail about how you want your application to function. Create a list of features, and break each feature down into sub-lists. This will make it easier for you to communicate your expectations to GPT.

### Refine Your Requirements:

Your initial requirements may not be detailed enough. Be as specific as possible, e.g., "a user can create a team, a user can only be a member of one team (the one they created) (ex: has_one_team). A team can have many users (has_many_users)." Spend at least 50% of your overall build time refining your requirements outside of ChatGPT.

### Set Expectations with ChatGPT:

Inform ChatGPT of your expectations by telling it to act as a world-class programmer familiar with the chosen framework. Explain that you will act as the product manager and provide requirements.

### Break Features into Smaller Parts:

Feed ChatGPT one feature at a time or even parts of features. This allows it to focus on each task more effectively. For example, "First, we'll build the authentication mechanism with Devise, starting with basic email and password login. We'll add OAuth and login with Google next..."

### Write Tests as You Go:

Always instruct ChatGPT to write tests alongside the code it generates. This ensures the reliability of the code and makes it easier to identify issues. Frameworks like Rails and Django work well with Test-Driven Development (TDD).

### Keep Pushing and Refining Prompts:

ChatGPT's output depends on the quality of your prompts. If the output isn't correct, it's either because your prompt wasn't clear enough, or ChatGPT doesn't have up-to-date information on a specific technology. In the latter case, you can feed it relevant sections of new documentation.

### Additional Considerations:

- *Security:* Ask ChatGPT to perform a security audit on the code it writes.
- *Deployment:* Discuss your deployment plans with ChatGPT and set up a pre-production environment in the cloud as early as possible.

By following these steps, intermediate coders can effectively build applications using GPT. Clear requirements, thorough planning, and constant refinement of prompts are key to successfully leveraging ChatGPT in your development process.


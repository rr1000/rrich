---
title: "The AI Early Adoptorrrrr: Code Synthesis"
date: 2022-12-29
description: "Early experiments using ChatGPT for app generation and scripting."
---

Isn't it delightful that we can now rely on AI to do all our coding for us? No longer do we have to worry about those pesky coding problems — just let the machines take care of it!

...I wouldn't get too excited just yet. You still need to know how to run, deploy, and make changes to the code. Then again, that will all become obsolete soon enough I suppose. Just imagine, a non-technical but savvy founder could create an MVP entirely with an [AI coding tool](https://debuild.app/). How convenient. 

While not an AI coding tool specifically, as a general purpose AI tool, [ChatGPT](https://chat.openai.com/chat) can produce decent code right out of the box. Additionally, given ChatGPT's "chat" like nature, it's easy to ask follow up questions and debug simply by asking ChatGPT to fix an issue. I've found ChatGPT to be quite successful at two primary coding tasks. 1.) app generation, and 2.) simple script writing.

App generation example:

**Input:**
> Write me a Ruby on Rails application that functions as forum software

**Output:**

![ChatGPT](/assets/images/posts/chat-01.png)

This response is not particularly useful. Google likely would have provided a more helpful result. I requested that ChatGPT write a Ruby on Rails app for me, not simply explain how to build one. Let's see what other results it produced.

**Second Output** _(using the same prompt)_

![ChatGPT](/assets/images/posts/chat-02.png)

This time, we received a more helpful response. The beginning of the output was similar to the first response, but this time ChatGPT provided us with some code. Following the instructions and using the provided example code, it would be possible to create a simple forum using this information. Some other tasks that you could ask GPT to perform include:

- Write an authentication system for this application (if you already have some knowledge of the Rails ecosystem, you could be more specific like "add devise to this application so users can log in")
- Allow image uploads, and give me the exact steps to add that functionality to this application
- Create a new model for this forum called "tweeeeets" that has time, author, and body fields
- Provide a way to charge users by integrating Stripe into this app, I want to charge people $1 every time they post in the forum

It is not hard to imagine that in the near future, a developer might spend a few hours directing an AI to create an app rather than writing the code themselves.

Now let's try something a bit more immediately useful — script writing.

**Input:**
> Write a shell script I can run on my mac that uses the haveibeenpwned.com API to detect when I've been in a breach. When that happens, send me an email to example@email.com

![ChatGPT](/assets/images/posts/chat-03.png)

Woo! This is so much more useful than the previous response to the Ruby on Rails input. Likely because my input was better this time around. This is a clear example of how ChatGPT is only as good as the input you give it.

Alright, so let's give this a spin. As you can see below, I've run into a small issue (intentionally to illustrate ChatGPT's debugging capabilities):

![ChatGPT](/assets/images/posts/chat-04.png)

I told ChatGPT about the problem, and it correctly did a few things:
- Remembered I'm on a Mac
- Gave me the fastest solution to the problem (installing jq with Homebrew)

![ChatGPT](/assets/images/posts/chat-05.png)

Now I can run the script, and when I do so, nothing happens. Alright, back to ChatGPT to help debug. I told ChatGPT that the code it gave still isn't working. It gave me a bunch of things to try. Here's a small sampling of that:

![ChatGPT](/assets/images/posts/chat-06.png)

One command ChatGPT told me to try was:

```
curl -s https://haveibeenpwned.com/api/v3/breachedaccount/example@example.com
```

I reformatted that command to use jq like it had me do when I initially created the script.

```
curl -s https://haveibeenpwned.com/api/v3/breachedaccount/rrichrs@gmail.com | jq .
```

Here's what we get:

```json
{
  "statusCode": 401,
  "message": "Access denied due to missing hibp-api-key."
}
```

Nice, we're getting somewhere. Let's tell ChatGPT what's going on:

![ChatGPT](/assets/images/posts/chat-07.png)

Okay, so there's at least one problem with this. haveibeenpwned API keys are not free. They haven't been free to a while (if ever?). This means that even when ChatGPT was under training (before 2021 I believe), haveibeenpwned API keys were not free. I would have expected a better answer here. However, it did give us some code to add to our script. Here's what the updated breach_detector.sh script looks like now:

```shell
#!/bin/bash

# Store your email address in a variable
EMAIL=••••••••••
API_KEY=••••••••••••••••••••

# Check if you have been involved in a data breach using the haveibeenpwned.com API
BREACH=$(curl -H "Authorization: Bearer $API_KEY" -s https://haveibeenpwned.com/api/v3/breachedaccount/•••••••••• | jq -r '.Title')

# If a breach is detected, send an email notification
if [ -n "$BREACH" ]; then
  echo "You have been involved in a data breach: $BREACH" | mail -s "Data Breach Notification" $EMAIL
fi

```

The script still isn't doing what I want it to. I'm missing what to do when there aren't any breaches, and some automation to run this on some cadence, but I can just continue asking ChatGPT to help me implement those features. There really is no end to what I can ask it to do, and eventually it'll respond with exactly what I need (hopefully). I'm going to stop there because we could do this all day.


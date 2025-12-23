---
title: "OpenAI Assistant (GPT) Example"
date: 2023-11-07
description: "Walkthrough of creating a custom AI assistant using OpenAI's platform."
---

### Video
<iframe width="560" height="315" src="https://www.youtube.com/embed/SvMdqJU8O50?si=VuiEtR5TLBMbIpD9" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

### Transcript

Hey, so I wanted to show you how to create one of OpenAI's new AI assistants, or GPTs. Even if you don't have access to it in ChatGPT, you can create it inside the platform. So, the first thing you want to do is just go ahead and hit the create button. It's over here in the platform. Go to assistants, hit create, and then name your bot.

So, for this example, since I have a ton of data on cloud security, I'm just going to call it cloud security bot. And then down here is where we're going to provide instructions. So I wrote an initial version of the instructions that I wanted, and then I had ChatGPT basically make it better. I asked it to improve it, such that the answers are more technical and more precise.

So this is what it gave me. So this is what I'm going to use for the instructions. Now, this can be quite lengthy. It doesn't have to be short like this. You can make your instructions a lot longer. But I'm just going to use what I have here for this example. So I'm just going to clean this up real quick.

And so here are our instructions. And then next, we're going to want to select our model. So we're going to use the new preview. Not going to add any functions right now. This could be an external API that we call. But I'm not going to do that here. We do want it to be able to interpret code, and we do want it to retrieve files.

So, the next thing we want to do is provide it with our dataset. So, one of the easiest ways to do this, instead of having to manually upload a bunch of PDFs and it only accepts 20 separate PDF files, is just to create a combined PDF. So, I have this little script here which will combine PDFs from a directory.

So I'm going to go ahead and punch that in. And let me actually find the directory first.

Cool. So this is the directory we want to use. And we're going to go back to our script. And then we're going to copy this again. Now we have our path and our output. We can just say today's date and we'll say dataset.pdf. Now, what that's going to do is go into this directory, find all the PDFs, and just combine them into one PDF. Again, not a required step, but as you can see here, I have several PDFs, and I don't want to have to upload those individually.

Plus, if I add more, I can just run this script again, and it's just going to create that dataset. So now you see I have the dataset right here. And you can see it's all of the PDF content combined. And so this is what we're going to provide within the context window in the assistant.

So we're going to go ahead and hit upload, and I'm going to find my dataset, there it is. So this is going to take a few minutes to upload. Maybe not, depending on the size. This one's decently large. So, it might take, oh, so it's up. Alright, so now we're just going to hit save. And so, this will now be a new assistant that you can use.

You can't publish it yet, I don't think. And there's no store concept yet, and you can't use it in the chat GPT interface unless you have access to it. But it's here. And you can always go here, test in the playground. And now you have a cloud security bot that has this data in context that it can retrieve information from.

And then we have our instructions here. Now, you can test this out and update the instructions based on ways you want it to answer. But if we were just to say something simple like, you know, how do I configure an RDS database instance with AWS best practices in mind as well as a requirement to be HIPAA compliant.

So if we hit add and run, that's going to hit the ChatGPT Assistant APIs with these instructions, and it's going to use this dataset that we provided. So I didn't really talk about the dataset, but it's a bunch of PDFs on AWS best practices, architecture diagrams, things from NIST, etc.

So now we have our response, and as you can see, this is far better than what stock chat GPT is going to provide you again mostly because of the files that we've uploaded and the instructions that we've provided. And so again, this is great for personal use right now. Hopefully, they'll open up the store and you'll be able to publish these


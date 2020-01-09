---
layout: post
image: /img/hello_world.jpeg
title: My Experiments with patience: Bare metal programming of an x86 PC
---

Hi. Welcome to my first blog!  I assume that a reader has basic knowledge about a computer, or at least seen one. If you haven't, I can't understand how you used a time machine to come from the past without ever seeing a computer. Go back to your stone house.

Let me start with a really cool analogy. You go to a shop to buy a digital watch. Here's how the conversation would go...
>Me: Yo! Give me a cool watch.  
>Shopkeeper: Okay! Take this one *Hands over one cool looking watch*  
>Me: Okay.. How do I use it? **I need an instruction manual**. Give me the *How to use* book of this watch.  
>Shopkeeper: Here you go. *Gives the manual.*  
Now I can use all the cool features of the watch and use it properly. I can keep an alarm, set different times and whatnot

Here, we bought a device. The manufacturer wanted it to be used in some fashion - *Tell time, set alarms,etc.* and gave us a book to tell us how to use the device to do the those things

Then you see a computer.It's a really cool device. People use it for so many things - Reading, Entertainment, and whatnot. The same device used for so many purposes. It almost seems magical when you think about it. But then, that's kind of all that a computer is... something that you can *program*. You can tell it what you want it to do! Kind of like your personal genie ;)

But we never see computers this way because of all the *bloat* (Ouch! Sorry Microsoft, Google and every other person who spends hours writing programs for us). This functionality of computers is often masked by this *bloat*: Operating systems, user applications, etc. make our lives easy and that's all we think of computers. However, this device has only one true purpose: To be programmed. 

![The way of the gods](https://i.redd.it/c0p0se5bhwp01.jpg)

Remember we just bought a watch a while ago? It's a device. It's made by a manufacturer. It also has a specific use. It has an instruction manual.  
Moving on to computers: They too are devices. They too are made by manufacturers. They have a specific use: To be programmed. **They must have an instruction manual!!** Here's the crux of the discussion: **This is called the ISA (Instruction Set Architecture)**.(Google it!)

### Instruction Set Architecture
This is kind of like the *How to Use* guide for your brand new computer. There are a few issues. So many companies make these CPUs - the main players being Intel and AMD. You want an instruction manual (**ISA**) that works for both of them. There's a name for this instruction manual - It's called the **X86 ISA**. All desktop machines use this. Smartphones use something called **ARM**. You must be getting an idea now :)

This is the first in a series of blogs that describe my experiments with *Bare Metal Programming*,ie. You buy a *blank* PC - with a motherboard, hard drive, RAM, and processor, and nothing else. No software on the hard disk. You want to program this device - *A Bare Metal* to make it do what you want :)


Note: I don't like having tutorials with hyperlinks because I think that it really distrupts the flow of the reader. At the end, I'll put a section of terms that you may wish to Google, or any cool resources that may be helpful.

### Buying our  device
>Me: Yo! Give me a computer.  
>Shopkeeper: What? This guy must be nuts... Okay... I'll give you a processor.*Throws an x86 processor like the Intel Pentium/i3/i5/i7.* Here take this *motherboard, RAM, hard disk, a VGA monitor, keyboard, etc.*  
>Me: Thanks.

Note: This is a *highly exagerrated conversation*. Don't throw your processor like a retard!

### Conclusion  
In my next blog, I'll describe what happens when you press the *Power On* key on your PC, the tools that we will be using to do this, and get started with our first ever program - The way the gods meant it to be - without any bloat. What the processor does is purely what we want it to do.

On a serious note: Operating systems are *not* bloat!! In our daily life, you need one that works well. They do so much for application developers and users, and it's unfeasible to manage without them. This blog was just meant to appeal to the curiosity of the reader, and is meant to be read to get a deeper understanding of how computers work, and to show why it's stupid to do such low programming for every single task. Feel free to ask any questions that you may have.

Things to Google: ISA, Computer Organization, Computer Architecture

Until next time
Suraaj


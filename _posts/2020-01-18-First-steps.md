---
layout: post
image: /img/Computer.png
title: Life is simple
subtitle: And so are assemblers!
published: true
---
Hello and welcome! In this blog, we'll see how Goku and an x86 processor are related and
maybe even create the first **Hello world** bare metal program! I really don't understand the world's obsession with *Hello world* though... I think I'll get it to print something like *Hello Daddy* or something...

The last blog introduced us to the toolset (Boss fight). Much like a Rajnikant movie: We'll take a few hits from this boss before giving the final blow and stealing the show ;).

We'll start of with some history, and quickly dive into hardcore development.

### What Goku??? How is a processor related to Goku????
![Evolution]( /img/Blog3/8086.jpg)

This is actually pretty interesting.... You know how Goku can go into some Super Saiyan God Blue with multicoloured eyes and hair (with a pinch of Ultra Instict), but always starts fights in base form? That's the same with our processor: Be it Pentium/i3 or even i7, it always starts in base form: like an intel 8086 processor (Man, you get the name x86 from this processor. It really is the *BigDaddy* of processors). If it didn't start in base form something like this would happen:

>You: Hey bro, I just bought this cool i3 processor! It's super fast and has registers that are 64 bits in size and all. *\*Flexes*

>Me (A noob who still uses a 16 bit 8086): Oh wow, let my run all my games on yours..

>You: Ummmm.... No you can't do that...

>Me: What lol, stop joking bro... You have a processor, So do I... What's the problem?

>You: You see my registers are not the same size...

>Me: Go away you noob, always making excuses. You just don't want to play with me. *\*Cries in corner*

>You: What just happened? That escalated quickly...

Well you get the idea. The 8086 was a 16 bit processor. There was a slow evolution from then... There was the 16 bit 80186 with more address lines, and so on... Gradually we got the 32 bit 80386, and even this was escalated by 64 bit modern day processors like the i3/i5/i7. The central idea here is **backwards compatibility**. Whatever the x86 processor, it always starts off in a simple 8086-like mode with 16 bit registers.  
So, your friend can still execute his 16 bit code on your processor which is acting like the 8086. (It's kinda like using a lamborghini like an autorickshaw, but well, anything for your friend). It has a nice philosophical touch too:
 However high you go, always know your roots... This mode is the **real** mode. It is a mode where your processor acts like a 16 bit one, and all x86 processors boot into this mode.

![Iroh loves us]( /img/Blog3/Iroh.jpg)


### Hello World... Er... Hello Daddy!
Now we know that the processor starts off in 16 bit real mode, like an 8086/80186. (80186 is like a buffed up version of 8086, but not super saiyan yet... It's still a 16 bit processor, but it has 20 bits for addresses, while the 8086 had 16. (Duh!))

Well, let's get started with some Bare Metal!

What do we have to do? See what instructions that have to be executed, look at the ISA, get the 16 bit encoding of that instruction (basically look at how the 8086 would read that instruction). We would then have to write the encoding manually using some hex editor (Hello **ghex**). And all this was for one instruction. Perhaps you are getting an idea how difficult God level programming is... 

But most of the work seems very mechanical.... See the ISA and get the encoding for the instruction. Oh I wish we could have this automated. Remember **need based tool usage**? You really have a need for something specific. All that you need to do is ask, and you will receive.

Turns out this is the tool that we require the most! This tool is called an **assembler**. It's basically a portable *ISA Book*, that reads the instructions that you want to execute as input, and outputs the binary encoding of that instruction. 
In my case I use the **netwide assembler**, lovingly called **nasm**.

![The assembler](/img/Blog3/Assembler.jpg)
Moving on to programming... I want to write something to the screen.. Oh damn.... The monitor is an I/O device, and we know what a pain that it can be to deal with I/O. Let's follow the thought process of a processor in this case.

>Processor: Man... I just booted into memory (address 0x7c00). I'm not even in super saiyan form. I'm still in 16 bit mode, and this programmer wants me to write something to I/O. Kill me already.... I've to look at manuals and all again....

>*BIOS has entered the conversation randomly*
>BIOS: Hey kid, what's up? Why are you looking so depressed?

>Processor: Cut to the chase already bro... The readers know that you're going to help me. Just do it already.

>BIOS: No need to be so rude. I'll help you man. (Of course the BIOS will help, why else will he enter the conversation randomly? This is called breaking the fourth wall,isn't it? ;))

Remember how BIOS was *the one who came before*? When it does its thing, it turns out that it was really considerate and placed routines into memory that help the programmer get started with basic services like printing to the screen, reading from the disk, and so on...
So how does one use these services?  
Well, during the execution of the code, there is a special instruction called an *interrupt* that interrupts (duh!) the current program execution and jumps to the service routine corresponding to that interrupt. (This is called a software interrupt, because the program calls it.)
We just need to input the interrupt number so that we go to the correct routine. After finishing the reuested service, we are brought back to original program. A simple google search says that in 16 bit mode, **int 0x10** corresponds to video services. Specifically, if **register ah was equal to 0x0e** when the interrupt is called, then the ascii letter corresponding to the value in al would be displayed at cursor location, and the cursor would be incremented. How do I know this? Simple: The BIOS manual. Anything that was made for you to use has a manual. This should be firmly imprinted in your memory now. 
Now it looks like the Final Boss of this level has punched us enough... Let's belt him!

Note: You must be thinking... what is this **al,ah** and all. In 16 bit mode, the registers that belong to the *cool gang* are ax,bx,cx and dx. They are the most famous, everyone uses them, everyone loves them. They are the poster boys of 16 bit mode I guess. There are other registers of course. We might deal with a few of them later. The *cool gang* consists of 16 bit general purpose registers (ax,bx,cx,dx). As for **ah**, it's just a name for the upper byte of ax, while al is the lower byte. Thus, if ax = 0x1234, ah = 0x12 ; al = 0x34. Simple!
### Actual assembly code

![Code](/img/Blog3/Code.png)
Go ahead and save this file (*file.asm* maybe?). Convert to binary (assemble)  with:
> nasm *file.asm* -o *file.bin*

Go ahead and emulate as you did before:
> qemu-system-i386 -hda *file.bin* 


![Output](/img/Blog3/Blog3.png)

Feel free to experiment with the hex dump. Knock yourself out!

### Conclusion and the Chicken and Egg problem
Well here's some food for thought: We are using an existing OS to create a kind of OS for a bare metal system... So where did the first OS come from? Reading about this might give you something cool about the history of computers themselves.

Alright, we are ready to really get serious about bare metal programming. There's a lot one can do from now... It's up to the reader to start exploring! Feel free to contact me for any query/suggestion. 

Things to Google: 
* BIOS interrupts (Eg. int 0x10)
* Intel processor history: what comes after 8086?
* 16-bit real mode
* NASM assembler
* Interrupts: software and hardware

Until next time  
Suraaj

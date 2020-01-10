---
layout: post
image: /img/Computer.png
title: You see a big red button
subtitle: So you push it
published: true
---
Hello and welcome! This is jumping right off from my first blog, into all of its technical goodness. By the end, equipped with this knowledge, the reader can hopefully create his first bare metal program.  
This blog introduces the first villian(s) of this series: The toolchains that I used and the environment that I worked in. That's the final boss of this level, and we'll deal with the smaller issues first.
I have a chip (the x86 processor) and its instruction manual (ISA). What are the next steps? Where do you instruct it? How do you instruct it? Do you say the instructions? Do you write them? Do you pat the processor on the shoulder for a job well done? Does a processor even have a shoulder? Getting started in itself seems like such a pain - May the gods have mercy on us. :)

### Computers: As the Gods see them
![Who knew that a computer was this simple?](/img/VonNeumann.png)  


Where did *memory* and *I/O* come from? I thought processors were complex enough!
#### Memory
Here's what a conversation between a processor and memory would look like:

>Processor: Hey bro, I need to do someth....

>Memory: Again.... What do you want now? Do you want to **read** me or **write** into me

>Processor: I want to read the *byte* that you have stored in location 102.  

>Memory: Here you go. *Gives the byte.*

>Processor: What would I do without you bro =( 

That's it: pure unadultrated bromance. Seriously, the processor reads/writes data which is stored in memory in locations/addresses. It's that simple.

![Mando : Processor; Baby Yoda: Memory](/img/bro.jpg)  


Note: Maybe you're familiar with all this and are starting to associate memory with RAM... Well, you'd be partially correct. RAM is a part of the memory system, but not really all of it. For example, in the memory system (give me an address and I give you the data - remember the bromance?) the first 1000 locations might be reading from/writing to RAM, but address 1001-2000 maybe writing to video memory - the memory associated with your display device (screen). But it's mostly RAM and this is just a subtlety. **But hard disks/SSD is not memory**. Why? No bromance. The processor interacts with the hard disk in some other way, meaning there's no *'give me an address, take a byte'* relationship. It's more like an I/O device

#### I/O Devices
A little thought shows how difficult dealing with these buggers can be. *There are so many kinds of I/O devices!*. In order to handle the variety of these devices, there are other chips(*microcontrollers*) that are present in the motherboard/the device. The microcontroller communicates with the system(processor), understands the commands that the system wants the device to do, and instructs the I/O device to perform the task.
This requires another address space: an I/O address space, where every such microcontroller's *registers* are mapped. The registers are kind of like the microcontroller's mini memory. Each register is given a port ID, which is exactly like an address.

Here's what a conversation between a processor and the I/O subsystem would look like: 

>Processor: OMG....Is this a challege?: I need to receive a character from the keyboard  
Where's my book of I/O mappings, I need to find which microcontroller deals with keyboard functionality  

>Keyboard: *Notice me senpai*

>Processor: Wait bro... Found it, the I/O *port* number. *Realizes that the keyboard is a device, it must have a manufacturer, recalls the whole watch analogy again*  
**So the device must have a _how to use guide_ published by the manufacturer!**  

>Keyboard: *Notice me senpai*

>Processor: Refers manual, sends what is required  to the port (the command maybe? It all depends on the manufacturer. Maybe you need to greet the microcontroller first and then ask it to do something politely... maybe not. It all depends on the manual) 

>Keyboard: *Notice me senpai*

>Keyboard microcontroller: Lol, I got something. I need to read a keypress from the keyboard? Easy  
>Keyboard: Finally, I have been waitin...

>Keyboard microcontroller: Done

>Processor: That was painful.  

That's what it is: painful. That's why modern operating systems install something called a device driver (it's a piece of software), that takes care of all this I/O business for you .As a programmer, you only interact with the driver to deal with I/O.

### The Von Neumann Model
Remember these questions:
What are the next steps? Where do you instruct the processor? How do you instruct it? Do you say the instructions?, and so on... 
The answer is the Von Neumann Model. This is simply the model of computing that all computers adopt. 
This seems like a mouthful, but it simply answers all the questions that we asked before. It is simply charecterized by two things:

* Instructions stored in *memory*. 
* Sequential execution of those instructions. (Presence of an instruction pointer)

The instructions are stored in the form of bits... The _encoding_ of an instruction (What sequence of bits corresponds to what instruction) is specified by the *ISA* (of course). The instruction pointer is simply a *register* that points to the current instruction, ie, it has the address value of the current instruction stored in it.
Thus, if you have a program, read the ISA and encode all your instructions into corresponding bit representation. Store that huge sequence of 1's and 0's in memory (in some address) and point your instruction pointer (IP) there (Store the address of the starting instruction in IP), there's your program being executed by the CPU! That is the **most hardcore way of programming**, hands down. If you can do that, you're God. I will build a temple for you. I would put a _real men code in binary_ meme here or something, but you get the idea.

### The Boot Process
Well, you can write a program in binary, that's cool: Look at the ISA and write the bits on paper, but how will you place that in memory?? How will you change the instruction pointer? Of the three tasks that are required to become **hardcore programmer**, two already seem impossible. So, checkmate: no temple for you.  
Moreover, remember how we thought that we would be the only software on the system? Wrong again. It's all because of that motherboard. The manufacturer placed basic software in some memory structure (this is part of the memory system). This is responsible for checking external devices, checking whether memory works properly, etc.  
This software is called the **BIOS(Basic Input Output System)**.
So you power on the PC, there's something called a **POST(Power On Self Test)**, at the end of which control passes to the BIOS.
The BIOS chills around for a while (doing something useful of course) and here comes the important part... It checks the connected hard disks (or floppy or CD... but who uses those nowadays??) to see whether they are **bootable**. If they are bootable,BIOS loads the first 512 bytes (that's a sector: disks are made up of sectors which are 512 bytes each) of that hard disk into memory address **0x7c00** (That's hexadecimal: In decimal that would be 31744) and sets the instruction pointer at 0x7c00. Finally we have control!!
 Our 512 byte sector is called the **boot sector** and would be present from memory address 0x7c00 to 0x7e00 (0x7e00-0x7c00 = 0x200 = 512).

Note: This is how BIOS traditionally worked. This is called Legacy BIOS. Nowadays, there's the super saiyan version of BIOS called UEFI BIOS that carries out the boot process a little differently and simplifies matters in some ways. I was lucky to have a real PC that usedLegacy BIOS lying around. Most old PC's use this. However, our x86 PC emulator(we'll emulate our bare metal programs to save the trouble of restarting the PC multiple times) will use legacy BIOS so it's all chill. 

### Our target
Now that we know how a computer boots, first we need to make our disk *bootable* (Bootable means that the first sector will be read into memory address 0x7c00). Then, we'll write our program in binary (hex) by referring to the ISA (that's the way god wanted), and write the bits into the first sector of the hard disk (so that it'll load into memory address 0x7c00) and our program will execute.

Making a hard disk bootable: How the BIOS identifies that a disk is bootable is by checking the last 2 bytes of the boot sector (byte 510 and byte 511). If they are 0x55 and 0xaa respectively, the BIOS believes that the disk is bootable. (0x55AA are called magic bytes)

### Boss Fight: The required tools
I've always hated this part. Learning any new skill often bogs you down with lots of new software that you will have to learn and it becomes very disheartening to keep watching tutorials. When you should be solving the problem, you'd be spending time learning stuff like programming lanuages (want to do machine learning - learn python first), programming environments (want to do android dev - learn android studio), etc. I understand that it is absolutely necessary to do so. However, my aversions to learning software has made me put this as the boss fight for this blog ;).
I am using Linux, where it gets crazier with tons of command line software. However, Linux is the most suitable platform for serious 
development work. All of my work was done in Linux.
I always believed in a need based tool usage. I want a hexadecimal editor program (to write in binary/hex) and a x86 PC emulator. You 
can use **ghex/bvi** as the hex editor and **qemu** as the x86 pc emulator
### Code at last: Wait, does binary/hex count as code?
My program is simply a jump instruction that jumps to itself, making an infinite loop. I refer to the ISA, and see that the encoding for the instruction is 0xEBFE (1110101111111110 in binary: I dare you, check it!). I place it as the first two bytes of our file and make bytes 510 and 511 0x55AA. **This is our first bare metal program**. A hex dump (you can use **xxd** to hex dump) should look like this:


00000000: ebfe 0000 0000 0000 0000 0000 0000 0000  ................
00000010: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000020: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000030: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000040: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000050: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000060: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000070: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000080: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000090: 0000 0000 0000 0000 0000 0000 0000 0000  ................
000000a0: 0000 0000 0000 0000 0000 0000 0000 0000  ................
000000b0: 0000 0000 0000 0000 0000 0000 0000 0000  ................
000000c0: 0000 0000 0000 0000 0000 0000 0000 0000  ................
000000d0: 0000 0000 0000 0000 0000 0000 0000 0000  ................
000000e0: 0000 0000 0000 0000 0000 0000 0000 0000  ................
000000f0: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000100: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000110: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000120: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000130: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000140: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000150: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000160: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000170: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000180: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000190: 0000 0000 0000 0000 0000 0000 0000 0000  ................
000001a0: 0000 0000 0000 0000 0000 0000 0000 0000  ................
000001b0: 0000 0000 0000 0000 0000 0000 0000 0000  ................
000001c0: 0000 0000 0000 0000 0000 0000 0000 0000  ................
000001d0: 0000 0000 0000 0000 0000 0000 0000 0000  ................
000001e0: 0000 0000 0000 0000 0000 0000 0000 0000  ................
000001f0: 0000 0000 0000 0000 0000 0000 0000 55aa  ..............U.

Finally save the file as anything (file.bin maybe?). To emulate using qemu, this is what you need:
> qemu-system-i386 -hda file.bin

qemu-system-i386 is our x86 processor simulator. -hda means consider file.bin as the first hard disk in your emulated PC.
You should get nothing, as expected. Try removing the magic bytes and emulate. You'll see that the BIOS doesn't recognize any bootable disk. 
### Conclusion  
Phew, that was a lot! In the next blog, I talk about how life is simple, and so are assemlbers.

Things to Google: BIOS, UEFI, qemu, Hexadecimal, Binary, Von Neumann Model, registers, Program counter/Instruction pointer

Until next time  
Suraaj

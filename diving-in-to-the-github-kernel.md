# Diving into the Windows NT Kernl

> Originally published on 10/21/2016

I've been bit by the operating system bug. After taking an intro to computer architecture course last semester, I've been itching to dig deeper and really understand things. After first trying MIPS assembly and learning the (very) basics of hardware, the magic and intimidation of computers was gone. Up until that point, there was always another layer of abstraction between me and the bits - in most cases many. Finally learning about interrupts, virtual memory, registers, and the like removed the intimidation factor. Now, I'm finding that my OS course is one of the only CS courses that really excites me.

In my current OS course, I'm in a group doing a presentation on the basics of Windows & its history. In my research, I was struck by the disparity between the information available for Windows NT and the info available for Linux. The resources that do exist for NT consist of an excellent book (Windows Internals) and scattered forum posts of various quality. For Linux, one can read the whole source, countless wikis, tutorials, discussions, and many books while only scratching the surface of the available resources.

With this known, it isn't surprising that the discourse, especially in academia, is so heavily centered around Linux. Here at RIT, Windows is dismissed without a second thought - with near consensus that it is a relic inferior Linux in every way. That is really unfortunate for a few reasons. Certainly, lack of diversity in operating systems is concerning - the entire community benefits from legitimate competition. More concerning is that the perception that it is impossible to learn about the NT kernel has promoted intellectual laziness. I regularly hear erroneous statements like Linux is better than Windows because it supports multiple hardware architectures (NT does, too);That only Linux supports custom file systems (NT does); that it is impossible to see the NT kernel source.

That last misconception is interesting because it seems so obvious; Windows is closed source and Microsoft would of course never share it. Fortunately for those in the academic community however, there was a project to release the kernel source for use in academic research and teaching of OS concepts. The Windows Research Kernel was released around 2006 for academic use and was bundled with resources to support OS courses. In my initial research for my class presentation, I found out about the WRK and realized that there is a golden opportunity.

If only I could get my hands on the WRK, I could create a lasting resource for the CS department at RIT. With the right documentation, there is an opportunity to make it as easy to learn about Windows NT as it is to learn about Linux. After students build and run OS/161 for class, they could build Linux and then build the WRK. They could explore the implementations of Virtual Memory in all three. They could implement cool new features in NT and see how that compares to implementation for other kernels.


With sights set on this goal, I emailed Microsoft and received a kind response from Dave Probert, who was responsible for the WRK project when it was active. I received his blessing to use the project according to its license with one catch: he no longer had the source, so I had to find someone who still did. Luckily, with a little sleuthing, I was able to find that the Hasso-Plattner Institut  still had it. An email later and I had the full WRK and the Curriculum Resource Kit.

Now that I have the resources available, my plan is to develop an internal website with all of the resources needed to learn, build, and modify Windows NT (5.2 because that's all I have) in a modern computing environment. They say that teaching is the best way to learn, so I expect this effort will be an excellent way to structure my own learning.

Stay tuned for more as this project develops.

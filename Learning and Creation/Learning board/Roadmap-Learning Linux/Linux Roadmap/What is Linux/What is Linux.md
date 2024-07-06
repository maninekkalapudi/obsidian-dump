---
tags: Done
Start Date: 2022-01-18
End Date: 2022-01-31
---
Hello! Hope you’re doing well. In this post we’ll talk about Linux. Linux is a free, opensource software that is and highly customizable and ubiquitous in the computing world. Large parts of the internet as we know it is runs on Linux based operating systems(OS). So, knowing Linux and working with Linux command line will take us a long way in the software industry and others as well.

Have you ever wondered whey there is no “Linux OS” out there? I’m sure you heard of MacOS, Windows and even Linux “distros” but never Linux OS. Let find out in this blog post.

**Topics covered in this post**:

1. Types of software development and distribution
2. What is Linux?
3. What is a Linux “distro”?
4. What is a Linux command line?
5. Linux commands

### 1. Types of software development and distribution

Lets understand the following terms to get a better idea how software is developed:

1. Opensource- The source code in the software is available to view and modify. A community of developers contribute to build and maintain it.
2. Free- Software that is free to use for the individual. One may not be able to view the source code and in some cases the code cannot be modified or distributed
3. Closed source- The source code is not visible and the end user cannot modify anything in the product or even redistribute it. Users have to pay to obtain it

### 2. What is Linux?

Linux is a free and opensource, unix-like operating system(actually a kernel) developed by Linus Torvalds as a “free operating system” [in 1991](https://www.cs.cmu.edu/~awb/linux.history.html). It was based of(not a copy) Unix operating system, which was developed by AT&T(Bell Labs) as a proprietary OS(some versions).

Linux on the other hand was developed as a free and opensource alternative for Unix. We can get the publicly available source code for Linux, modify it and even redistribute it without any cost involved. Also, developers across the globe participate in contributing to the Linux development.

Opensource nature of Linux allowed to modify it for all kinds of purposes ranging from microcontrollers to a massive supercomputers and even in space vehicles there are on moon and mars.

Linux as your probably might think, is not a full-fledged OS but it is a kernel. A kernel is a part of the OS which talks to the hardware components like CPU, RAM etc., and other components in the OS as shown below:

![[Learning and Creation/Attachments/Untitled 4.png|Untitled 4.png]]

Source: [Top Linux Interview Questions and Answers (2022) - InterviewBit](https://www.interviewbit.com/linux-interview-questions/)

### 3. What is a Linux “distro”?

When the first version of the Linux kernel was developed, it was distributed with the with a set of [GNU](https://en.wikipedia.org/wiki/GNU_project) utilities and tools for setting up a file system, the Graphical User Interface(GUI) and apps like terminal. This is where it gets the name “Linux distribution”(Linux distro).

![[Untitled 1 3.png|Untitled 1 3.png]]

Linux distribution. Source: [How SUSE builds its Enterprise Linux distribution – PART 2 | SUSE Communities](https://www.suse.com/c/how-suse-builds-its-enterprise-linux-distribution-part-2/)

This is still the case to this day and there [hundreds of Linux distributions](https://distrowatch.com/dwres.php?resource=popularity) are available and every distro has Linux kernel and GNU components. One can download for free and even customize them to their heart’s content. There are distros that are derived from other distros. Needless to say this is a huge hobby among enthusiasts to try out various distros and tinker them.

### 4. What is a Linux command line?

A Linux command line is a text interface which allows us to interact with the computer using commands. It is often referred to as shell, terminal, console or various other names and definitions are mentioned below:

1. **Terminal**:
    
    A text based environment where you input the commands and see the output. A terminal will pass the input commands to shell for execution and display the output from it.
    
    Examples: [Windows Terminal](https://github.com/microsoft/terminal)
    
2. **Shell**:
    
    A shell is the program that the terminal sends user input to. The shell generates output and passes it back to the terminal for display
    
    Examples:
    
    - bash, fish, zsh, ksh, sh, tsch
    - PowerShell, pwsh

The popular shell among the Linux distributions is “Bourne Again SHell” or bash.

1. **Console**:
    
    A console is a physical device that had the terminal with screen and keyboard. In the software world a terminal and a console are referred interchangeably
    

Now you might ask, how is this useful? Well, in a normal desktop environment you will get all the GUI components installed. Which would look like in the below picture

![[Untitled 2 3.png|Untitled 2 3.png]]

Ubuntu Linux distro GUI

This is great for personal use and everything in the GUI seems to be laid out perfectly. But when it comes to [servers](https://en.wikipedia.org/wiki/Server_(computing)), where Linux is a primary choice; there will be no GUI and all the work should be done through the terminal.

![[Untitled 3 3.png|Untitled 3 3.png]]

When you login to a server or open terminal app in your Linux distro, you’ll see the above window. What does the text `me@linuxbox:~$` mean?

- `me`: username which you logged in
- `linuxbox`: name of the machine or server
- `~`: home directory(folder) for the user. At any point one terminal session will be on one single directory. Likewise, multiple terminals can point to different directories
- `$`: represents that you are normal user. In few shells, the admin/root user will see `#` instead of the `$` sign. `#` also represents elevated privileges to the system.

You can enter any command after this window appears and hit `Return` key to display the output. We will discuss about the user types, permissions and other topics in the future posts.

  

I’m using [WSL with Ubuntu](https://www.youtube.com/watch?v=Owrk9UxnMdI) going further and it looks like the below picture

![[Untitled 4 2.png|Untitled 4 2.png]]

`/mnt/c/Users/manik` is the home directory for the user.

### 5. Linux commands

Commands are reserved keywords that signifies an action in the system. Here are the few example commands that we can try out in a Linux command line.

- `date`- displays current date
- `cal`- displays calendar with only current month and current date is highlighted
- `ls`- lists all the directories and files in the current folder
- `pwd`- present working directory
- `clear`- clears(hides) all the contents on the terminal window

![[Untitled 5.png]]

  

Meanwhile if you type some random gibberish into the command line, it will throw error saying `command not found`. We can use the arrow keys to go through the command history(up arrow key) and also navigate within the command(left and right arrow keys)

![[Untitled 6.png]]

  

We can perform almost any action within the operating system using the commands. We will further explore all the important commands in the future blog posts

### References:

1. [Linux Command Line Books by William Shotts](https://linuxcommand.org/tlcl.php)
2. [The Linux command line for beginners | Ubuntu](https://ubuntu.com/tutorials/command-line-for-beginners#1-overview)
3. [What's the difference between a console, a terminal, and a shell? - Scott Hanselman's Blog](https://www.hanselman.com/blog/whats-the-difference-between-a-console-a-terminal-and-a-shell)
4. [Difference between Terminal, Console, Shell, and Command Line - GeeksforGeeks](https://www.geeksforgeeks.org/difference-between-terminal-console-shell-and-command-line/)
5. [What is the difference between Terminal, Console, Shell, and Command Line? - Ask Ubuntu](https://askubuntu.com/questions/506510/what-is-the-difference-between-terminal-console-shell-and-command-line)
6. [LinuxCommand.org: Learn The Linux Command Line. Write Shell Scripts.](http://linuxcommand.org/)
7. [How SUSE builds its Enterprise Linux distribution – PART 2 | SUSE Communities](https://www.suse.com/c/how-suse-builds-its-enterprise-linux-distribution-part-2/)
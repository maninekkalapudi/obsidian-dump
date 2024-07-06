---
tags: Done
Start Date: 2022-01-24
End Date: 2022-02-06
---
Hello! Hope you’re doing well. In this post we’ll talk about Command Line Interface(CLI) in Linux. In the [last post](https://maninekkalapudi.com/what-is-linux) we discussed about what Linux is. In this one we will get a taste of working with Linux command line. Lets go!

  

**Topics covered in this post**:

1. What is a Shell?
2. Terminal Emulators
3. Linux Filesystem
4. Navigating Linux filesystem in CLI
5. Linux command behavior

  

### 1. What is a Shell?

When we refer to the command line what we really mean is “shell”. The shell is a program that takes keyboard commands and passes them to the operating system to carry out. Almost all Linux distributions supply a shell program from the GNU Project called **bash**.

The name “bash” is an acronym for “Bourne Again SHell”. It is an enhanced replacement for Shell(sh), the original Unix shell program written by Steve Bourne.

The popular shells used in linux are:

1. C Shell (csh)
2. Kron Shell (ksh)
3. Z Shell(zsh)

Linux shell offers a way to interact with the kernel through commands. Ex: `ls` lists all the files and folders in a directory. Each command represents a task to be performed.

Every shell bash or zsh will offer the similar functionality with same commands for the most part with some additional functionality of their own. One such example is the difference between [bash and shell](https://askanydifference.com/difference-between-bash-and-shell/). The original shell didn’t offer command history(list of previously executed commands) and bash has it.

### 2. Terminal Emulators

A terminal is a program which passes the user input commands to the shell and displays the command output from shell to the user. A number of terminal emulators are available for Linux, but they all basically do the same thing; give us access to the shell.

![[Untitled 8.png|Untitled 8.png]]

Terminal offers a way to customize the appearance of the text(commands, progress bars, icons and output) displayed and much more. The possibilities for customization are endless. One such example is [here](https://itsfoss.com/customize-linux-terminal/).

### 3. Linux Filesystem

Linux is a hierarchical directory structure. This means that every directory can have files and other directories in them. When represented pictorially, it looks like a tree(data structure). The “Root” directory is the first directory in the filesystem and it has the various folders that are assigned for a purpose mentioned below:

![[Untitled 1 5.png|Untitled 1 5.png]]

Linux Filesystem. Source:[Linux File Hierarchy Structure - GeeksforGeeks](https://www.geeksforgeeks.org/linux-file-hierarchy-structure/)

When we first login to the Linux machine as a typical user we will be logged into the `/home/<username>` directory. Only a root user will be logged into root (`/`) directory.

### 4. Navigating Linux filesystem in CLI

One of the fundamental actions we perform in any operating system is navigating the filesystem. We are familiar with the graphical file manager in Windows and MacOS and even in the desktop linux distros. But, how about a server with only CLI? Lets dive in.

As soon as you login to the linux machine(ex: a server) as a user, you will be logged in to `~`(`/home/<username>`) directory. In the below picture, I’ve logged into ubuntu using [WSL on Windows](https://pureinfotech.com/install-windows-subsystem-linux-2-windows-10/). We will be using this going further.

![[Untitled 2 5.png|Untitled 2 5.png]]

At a given time, we are inside a single directory per terminal session. We can see the files contained in the directory and the pathway to the directory above us (called the parent directory) and any subdirectories below us. The directory we are standing in is called the _current working directory_.

Lets try two basic commands after logging in to linux terminal

- `whoami`- shows the username
- `pwd`- gives current working directory that we are in
- `ls`- gives the list of files and directories in the current directory

![[Untitled 3 5.png|Untitled 3 5.png]]

This gives us a pretty good understanding about who we are(`whoami`), where we are(`pwd`) and what do we have in our current directory(`ls`). To navigate the filesystem we use `cd` command.

`cd` is short for “change directory”, which allows us to change the current working directory. `cd` command expects a folder or a path in the filesystem.

![[Untitled 4 4.png|Untitled 4 4.png]]

From the above example:

- `cd /usr/bin`- change the current working directory to `/usr/bin`
- `cd ~`- `~` is notation for user’s home directory, which is `/home/<username>/`
- `cd ./test`- `.` represents current directory and `/test` represents test directory under the current one.

Notice that the path is provided in two different ways. We have two ways to define a path in linux.

1. **Absolute path****:**
    
    An absolute pathname begins with the root directory(`/`) and follows the tree branch by branch until the path to the desired directory or file is completed.
    
    Ex: `/usr/bin` or `/home/mani`
    
2. **Relative path****:**
    
    A relative pathname starts from the working directory. To do this, it uses a couple of special notations to represent relative positions in the file system tree. These special notations are `.` (dot) and `..` (dot dot).
    
    - `.`- current directory
    - `..`- parent directory to the current directory
    
    Ex: `./test`
    

Few Tips:

1. While navigating the subdirectories() within the current working directory, the `.` can be ommited from the `cd` command. For example: We are in pdir. pdir contains dir1 and dir1 has dir2. To navigate to dir2 from pdir, we can use `cd dir1/dir2`
2. Just enter `cd` command and hit return to navigate to user’s home directory from any directory.
3. `cd ~username`- changes the working directory to the home directory of username

### 5. Linux command behavior

`ls` command gives the list of files and directories in the current working directory. But, what if we want more info while displaying the info? This is where command options and arguments comes handy. The options for a command will modify its behavior thus giving different results.

- `ls -a`- displays all the files even the hidden files in the current working directory. Hidden files and directories start with `.`. For example: `.profile`is a hidden file and `.landscape` is a hidden directory in the below example.

![[Untitled 5 3.png|Untitled 5 3.png]]

- `ls -altr`-displays all the files in the current working directory(`a`), in long list(`l`), sorted by modification date(`t`) and in reverse order(`t`). We can use multiple options for a command for a desired behavior.

![[Untitled 6 3.png|Untitled 6 3.png]]

- `ls /usr . -altr` command takes two paths as arguments (`/usr` and `.` ) and displays the results for both the paths as shown below

![[Untitled 7 2.png|Untitled 7 2.png]]

An important question here is... do we have to remember all of the commands and their options available in linux? NO! You can’t possibly do that but we have a command for that as well.

`man` short for “manual” gives the full documentation for almost any command in linux. We can get documentation for any command using `man <command>`. Lets try for `ls` command.

![[Untitled 8 2.png|Untitled 8 2.png]]

In the upcoming posts we will dive deep into working with files and directories in linux. Stay tuned!

  

Sources:

1. [Linux Command Line Books by William Shotts](https://linuxcommand.org/tlcl.php)
2. [What are the Different Types of Shells in Linux? - JournalDev](https://www.journaldev.com/39194/different-types-of-shells-in-linux)
3. [https://www.cs.dartmouth.edu/~campbell/cs50/shell.html#:~:text=The shell is the Linux,shell executes the ls command.](https://www.cs.dartmouth.edu/~campbell/cs50/shell.html#:~:text=The%20shell%20is%20the%20Linux,shell%20executes%20the%20ls%20command.)
4. [22 Useful Terminal Emulators for Linux Desktop (tecmint.com)](https://www.tecmint.com/linux-terminal-emulators/)
5. [Difference Between Bash and Shell (With Table) – Ask Any Difference](https://askanydifference.com/difference-between-bash-and-shell/)
6. [Linux File Hierarchy Structure - GeeksforGeeks](https://www.geeksforgeeks.org/linux-file-hierarchy-structure/)
7. [How to install WSL2 (Windows Subsystem for Linux 2) on Windows 10 • Pureinfotech](https://pureinfotech.com/install-windows-subsystem-linux-2-windows-10/)
8. [Classic SysAdmin: The Linux Filesystem Explained - Linux Foundation](https://www.linuxfoundation.org/blog/blog/classic-sysadmin-the-linux-filesystem-explained)
---
tags: Done
Start Date: 2022-04-01
End Date: 2022-05-08
---
Hello! Hope you’re doing great. In my [last post](https://maninekkalapudi.com/files-and-directories-in-linux-cli) I have written about working with files and directories in linux CLI. In this post, lets discuss what actually is a “command” and how to create a command of our own.

  

Topics covered in this post:

1. What is a “command”?
2. Identifying a command
3. Know your commands via CLI
4. Create your own command using alias

  

### 1. What is a “command”?

Command(s) in general means an instruction or a set of instructions given to a machine to perform an action. A command in linux world can be any of the following:

1. **An executable program**- `/usr/bin` in linux has all the compiled binaries(installed programs). These are written in C, C++, Python, Shell and etc.
2. **Shell bulit-in**- bash shell supports a number of commands called as _shell built-ins._ Ex: `cd` command
3. **Shell function**- Shell scripts that are included in the environment.
4. **Alias**- Aliases, like the name suggests, we can give an alias to the built-in functions

### 2. Identifying a command

1. `type` command
    
    `type` command is a shell bult-in that displays the kind of the command. For example: `type cd` will displays “**cd is a shell builtin**". It means when we use `cd` command, it will refer to the shell built-ins in the bash shell.
    
    ![[Untitled 17.png|Untitled 17.png]]
    
      
    
    `type ls` command on the other hand shows that `ls` is in fact an alias to the command `ls --color=auto`. When we use `ls` command, the results will be displayed with color coding as above. An alias will work just like any command and when we use the alias it will invoke the command it is pointing to.
    
      
    
2. `which` command
    
    A common practice in software development is using different versions of same software in one environment(machine) simultaneously. For example: testing a website on two different versions of nodejs runtimes.
    
    `which <command>` command will show the paths of one or more versions of an installed program. For example: `which java` will show `usr/bin/java` path
    
    It works only for executable programs, not built-ins nor aliases that are substitutes  
    for actual executable programs. In the below example, there is no result displayed for the  
    `which cd` command as it is a shell-built-in.
    
    ![[Untitled 1 7.png|Untitled 1 7.png]]
    

  

`type` and `which` commands are two ways we can determine the type of a command and where it is referenced(installed) from.

### 3. Know your command

- **_`--help`_**

To know more about any command we can use `--help` option for any command. `<command> --help` will show all the options for the command. In the below example, we can see the documentation for `mv` command.

![[Untitled 2 7.png|Untitled 2 7.png]]

Each option will give additional functionality to the command. For example: `mv` command with option `-u` will only move those files from source directory that are new or updated than the destination directory.

  

- **manual (****`man`** **command)**

_man_ short for _manual_ will provide the formal documentation for any executable programs. man command will provide all the information for the command in different sections like name, synopsis, description and others.

![[Untitled 3 7.png|Untitled 3 7.png]]

  

- apropos command

`apropos <search_term>` command will show the appropriate commands by scanning the man pages based on the search term.

![[Untitled 4 6.png|Untitled 4 6.png]]

The results of the apropos command covers a wide range of cases from man pages thus very different results. It is recommended to use those commands that are suitable for a scenario. A brief description of the command is given after the command in the results.

### 4. Create your own command using alias

Till now we saw the examples that had only one command. We can use semicolon(`;`) between each command to run all of them at once(`cmd1; cmd2; cmd3;`).

For example: `echo "Hi, there!"; ls; ls destdir`. `echo` command is the print statement of the linux cli and `ls` command will list the files and directories.

![[Untitled 5 5.png|Untitled 5 5.png]]

Now, we can use these commands and create an alias and use the alias to perform the same action every time. Note that the user-defined alias is specific to machine.

  

`alias <name> = '<command_string>'` will create an alias with the supplied name. Now, let’s create the alias and see it in action.

![[Untitled 6 5.png|Untitled 6 5.png]]

  

After creating alias with `alias mycommand="echo \"Hi, there\"; ls; ls destdir”` command, we can invoke the alias like any linux command shown above. When we check the type of the alias using `type mycommand`, it shows `mycommand is aliased to `echo "Hi, there"; ls; ls destdir'`.

  

To list all the aliases that are currently in the system, use `alias` command and to remove any alias use `unalias <alias_name>`. For example, `unalias mycommand`

![[Untitled 7 4.png|Untitled 7 4.png]]

### References

1. [**Linux Command Line Books by William Shotts**](https://linuxcommand.org/tlcl.php)
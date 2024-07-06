---
tags: Done
Start Date: 2022-05-13
End Date: 2022-06-20
---
Hello! Hope you’re doing great. In my [last post](https://maninekkalapudi.com/know-your-linux-commands), I have written about commands in linux CLI. In this post we will understand how to play with any command’s output, store it in files or even connect multiple commands together into command pipelines. Let’s go!

  

**Topics covered in this post:**

1. What is Standard Input, Output and Error in Linux?
2. File Descriptors
3. Redirecting stdout
4. Redirecting stderr
5. `cat` command
6. Command pipelines

  

## 1. What is Standard Input, Output and Error in Linux?

Lets say we use a command like `ls` in the linux cli. `ls` will list all the files and directories in a given path and if the path is non-existent or incorrect path it will throw an error. This is shown as below:

![[Untitled 9.png|Untitled 9.png]]

Now, every linux command like `ls` is designed produce the following:

1. Command output
2. Status messages and error messages

The output and the error messages are displayed on the screen and we should know that [everything is a file in linux](https://www.tecmint.com/explanation-of-everything-is-a-file-and-types-of-files-in-linux/). Which means that a command will(not necessarily) send its output to a special file called **_Standard Output_**(_stdout_) and error to **_Standard Error_**(_stderr_).

By default, both _stdout_ and _stdout_ are linked to the screen and not saved into a disk file. Also, many commands or programs take input from _**Standard Input**_ (_stdin_) which is by default attached to the keyboard.

So stdout, stderr and stdin are files that are attached to screen and keyboard respectively. When a command is used in the cli, these files receive the output, errors and input.

![[Untitled 1 6.png|Untitled 1 6.png]]

Source: [How to Redirect stderr to stdout in Bash (linuxhint.com)](https://linuxhint.com/redirect-stderr-stdout-bash/)

  

## 2. File Descriptors

A file descriptor is a unique number that identifies an open file in an operating system. It has a record of all the files opened and their locations stored in a global table along with the permissions.

![[Untitled 2 6.png|Untitled 2 6.png]]

Source: [What is a File Descriptor? (computerhope.com)](https://www.computerhope.com/jargon/f/file-descriptor.htm#:~:text=A%20file%20descriptor%20is%20a,Grants%20access.)

  

On a Unix-like operating system, the first three file descriptors, by default, are STDIN (standard input), STDOUT (standard output), and STDERR (standard error).

|Name|File descriptor|Description|
|---|---|---|
|Standard input(stdin)|0|The default data stream for input. In the terminal, this defaults to keyboard input from the user.|
|Standard output(stdout)|1|The default data stream for output, for example when a command prints text. In the terminal, this defaults to the user's screen.|
|Standard error(stderr)|2|The default data stream for output that relates to an error occurring. In the terminal, this defaults to the user's screen.|

The file descriptors are used to input/output(I/O) redirection in the linux cli. Lets discuss this in the next one.

## 3. Redirecting stdout

I/O redirection basically means that we can define where the output ends up finally with the redirection operator `>`.

To redirect the stdout to a file, we will use `<command> > <output_file_name>`. Lets take a look at this with an example:

Here we are using the command `ls -l > ls_op.txt` to long list the files and write the output to `ls_op.txt` file. `cat` command will display the output on the screen

![[Untitled 3 6.png|Untitled 3 6.png]]

What happens if we provide a non-existing path to the ls command?

![[Untitled 4 5.png|Untitled 4 5.png]]

Here, `/bn/usr/` path doesn’t exist and we anticipated an error message written to the `ls_op.txt` file. Instead the error is shown on the screen. How about the output file?

![[Untitled 5 4.png|Untitled 5 4.png]]

`ls_op.txt` file is empty and the error is displayed on the screen. What happened here?

First, we received an error message for the non-existing path. The error will be sent to _stderr_ instead of _stdout._ Next, the redirection operator(`>`) will overwrite the data on the output file. Since we didn’t receive any output, the file was overwritten with nothing and the error was redirected to screen.

Intrestingly, we can use `>` to create a new file or even truncate an existing file.

`> <filename>` will create a new file and if this file exists, its contents will be truncated.

![[Untitled 6 4.png|Untitled 6 4.png]]

What if we want to just append the output to an existing file? `>>` will append the data to the existing file and create a new file if it is not present.

![[Untitled 7 3.png|Untitled 7 3.png]]

## 4. Redirecting stderr

Redirecting the stderr must need its file descriptor. As discussed above, the file descriptor for the stderr is “2” and this will be used along with redirection operator for stderr.

The stderr can be redirected to a file as mentioned below:

`ls -l /bn/usr 2> ls-err.txt`. Here 2 is the file descriptor for stderr followed by the redirection operator. Let’s see the output

![[Untitled 8 3.png|Untitled 8 3.png]]

  

- **Redirecting both stdout and stderr to single file**
    
    In most scenarios we may want to capture both the output and error to a single file. Suppose, we have a job that is scheduled to run everyday and write it’s output to a file. Writing both the stdout and stderr will help us identify the issues whenever the job fails
    
    `ls -l /bin/usr > ls-output.txt 2>&1`. This command helps us do that and it has two parts:
    
    - `ls -l /bin/usr > ls-output.txt`- This is normal stdout redirection and the stdout is redirected to `ls-output.txt` file.
    - `2>&1`- the stderr(2) is again redirected to stdout(1). This should happen in the same order else it will not work.
    
    ![[Untitled 9 2.png|Untitled 9 2.png]]
    

  

## 4. cat command

cat, which is short for concatenate, can perform multiple operations like:

1. Display file contents
2. Concate multiple files into a new file using redirection operator
3. Create a new file using cat command and redirection operator

  

1. **Display file contents**

`cat <filename>` will display the contents of the given file. If multiple files were to be displayed, then add the option `-n` and pass the file names one after the other.

![[Untitled 10.png]]

  

1. **Concate multiple files into a new file using redirection operator**

What `cat` commad essentially did in the above step is that it read the file contents and passed it to stdout file which is attached to the screen. We can use the I/O redirection technique to redirect the output of `cat` command to a file. Let’s see this in action.

![[Untitled 11.png]]

`cat testfile > catfile` command put the “testfile” contents to “catfile” using `>` operator. Also, the “catfile” is created by the command on the go.

  

This can be easily applied in a scenario where we want to write multiple file contents to a single file. `cat <input1> <input2> > <ouputfile>`

![[Untitled 12.png]]

  

1. **Create a new file using cat command and redirection operator**

What happens when we don’t pass a file name for the `cat` command? `cat` command expects a file name and when not provided, it will read from the stdin. This is provided in the manual(`man cat`)

![[Untitled 13.png]]

Here, the `cat` command will continuously accept the input from keyboard and displays it back on the screen as the stdout is still attached to the screen. By pressing `ctrl+d` or `cmd+d` keys will exit the prompt.

![[Untitled 14.png]]

  

What if we redirect the above example’s output to an output file? The command for this scenario would be `cat > <filename>`.

![[Untitled 15.png]]

The input will be accepted continuously from the keyboard and output will be written to the file. Again, by pressing `ctrl+d` or `cmd+d` keys will exit the prompt.

  

## 6. Command Pipelines

Till now, we have used single commands in the cli and played with the output from those commands(`>`). Piplines in the shell help utilize the stdout of one command to be piped into stdin of other command with the help of pipe(`|`) operator.

`ls -l /usr/bin | less`. `ls -l` command will list all the contents in the path `/usr/bin`. Later the output of this command will be passed to `less` command. `less` command will display the contents page(one screen) by page.

**Filters**

- `ls -l /usr/bin | sort`- This command will provide the sorted lists
- `ls -l /usr/bin | sort | uniq | less`- This command will provide the sorted, unique list in pages. The `uniq` command is often used in conjunction with `sort` command. `uniq` command accepts either stdin or a single filename argument.
- `ls -l | sort | uniq | grep zip`- This command will search for a pattern in the sorted, unique list using `grep` command. `grep` command accepts a pattern within a file or a sdtin. A “pattern” here means a word or a regex pattern.
    
    ![[Untitled 16.png]]
    
- `ls /usr/bin | tee ls.txt | grep zip`- The `tee` command reads the stdin from the previous command and copies it to the stdout and also to one or more files. This allows to capture the output from the intermediate steps in the process

## Conclusion

The I/O redirection is a powerful technique and pairing that with the pipelines will help us build a quick script or first versions of data pipeline. Commands like `grep` will help trigger other commands based on the seach results or even help understand the output.

  

### Resources

1. [**Linux Command Line Books by William Shotts**](https://linuxcommand.org/tlcl.php)
2. [What is a File Descriptor? (computerhope.com)](https://www.computerhope.com/jargon/f/file-descriptor.htm#:~:text=A%20file%20descriptor%20is%20a,Grants%20access.)
3. [Cat Command in Linux {15 Commands with Examples} | phoenixNAP KB](https://phoenixnap.com/kb/linux-cat-command)
4. [less command in Linux with Examples - GeeksforGeeks](https://www.geeksforgeeks.org/less-command-linux-examples/#:~:text=Less%20command%20is%20a%20Linux,accesses%20it%20page%20by%20page.)
5. [How to Redirect stderr to stdout in Bash (linuxhint.com)](https://linuxhint.com/redirect-stderr-stdout-bash/)
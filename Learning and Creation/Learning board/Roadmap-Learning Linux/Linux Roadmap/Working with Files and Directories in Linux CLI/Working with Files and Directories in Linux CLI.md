---
tags: Done
Start Date: 2022-02-05
End Date: 2022-02-13
---
Hello! Hope you’re doing great. In my [last post](https://maninekkalapudi.com/the-linux-command-line-experience) I have written about the how to get started with linux command line(cli) and terms like shell, terminal and etc. We also tried few basic commands to list the files and directories in a path. In this post, we will take this further and discuss about how we can interact with files and directories. Lets dive in!

  

**Topics covered in this post**:

1. Files and directories in Linux
2. Create and edit files in cli
3. Create directories with cli
4. File permissions in linux
5. Manipulating files and directories in cli

  

### 1. Files and directories in Linux

Files are the basic entities in linux which can store some data, text or a script/program. Directories (folders in other operating systems) contain either files or other directories. Both files and directories are common among all the operating systems.

Linux filesystem, shown in the below diagram; has different files and directories for various operations.

![[Untitled 19.png|Untitled 19.png]]

Linux Filesystem Source: [ICS 240: Operating Systems by William McDaniel Albritton (hawaii.edu)](http://www2.hawaii.edu/~walbritt/ics240/materials/module2-session07.htm)

When a user is logged in, they will land in `/home/<username>`(shown as `~` in cli). The user can create and delete files and directories within their home directory. There will be some files and directories that are created by the sysadmin(`root` or admin user) in your user directory which cannot be modified or deleted.

`ls` command shows contents in the current directory. When we run `ls` command with “all” option(`-a`), it will show hidden files/directories(filenames starting `.`) along with the regular content. Hidden files can be the configuration files(`.bashrc`), environment files(`.profile`) and etc. More on this in upcoming posts.

![[Untitled 1 9.png|Untitled 1 9.png]]

  

Also, there is no concept of file extensions in linux. The type of the file is determined by the contents of the file(or file header) rather when not provided. Operating system like Windows do rely upon the file extension to determine the file type. For example, `.txt` is a text file, `.exe` is an executable program and `.jpg` is an image file and etc.

To check the type of file, we can use `file <filename>` command. In the below example, We’ve `OMENCity` without any file extension mentioned. When we run the command `file OMENCity`, we get the file metadata(file information).

![[Untitled 2 9.png|Untitled 2 9.png]]

### 2. Create and edit files in cli

1. Creating a file with `touch` command:

`touch <filename>` will create a new file in the current working directory. Optionally, we can pass the `<path>/to/<file>/<filename>` to the touch command to create a file in the specific location(shown in the next example).

![[Untitled 3 9.png|Untitled 3 9.png]]

  

Notice that we didn’t pass any file extension with the `touch` command like this `touch <filename.extension>`. We can do that as well. For example, `touch index.html`. Lets try this example.

![[Untitled 4 8.png|Untitled 4 8.png]]

  

1. Editing a file with Vim(`vi`) editor:

Now that we have created the files, lets edit the file in the command line. We can use command line editors like Vim(personal preference) or nano. The command to open a file using vim editor is `vim <filename>`(we can use `vi` only instead of `vim` for the command). We can use the similar command for nano editor as well, `nano <filename>`.

Lets edit the `testfile` in home directory. Once we say `vim testfile` command and press the return(enter) key, the below screen is presented. We can’t just edit the file yet. Alternatively we can use `vim /path/to/<filename>` to edit the file in a different path than the current working directory.

![[Untitled 5 7.png|Untitled 5 7.png]]

  

So, to edit the file we need to press `esc` key and then `I` key. This will turn the vim editor to “insert” mode. We can observe the “INSERT” at the bottom of the screen. Now, we can write text to the file.

![[Untitled 6 7.png|Untitled 6 7.png]]

  

After entering the text, we need to save the file with latest changes. Press `esc` key and type `:wq` and press enter to save the latest changes. `:wq` is the command to save the file(`w`) and quit the vim editor(`q`).

![[Untitled 7 6.png|Untitled 7 6.png]]

c. Viewing the file content with `cat` command

To view the contents of the file, we can use `cat`(concatenate) command. `cat <filename>` will spit out the contents of the file directly to the command line.

![[Untitled 8 5.png|Untitled 8 5.png]]

  

d. Creating files with Vim editor:

We have seen an example on creating a file with `touch` command. We can also use the vim editor to do the same and we can eliminate the file creation step altogether. Lets see this with an example.

Previously, we have used the `vim <filename>` or `vim /path/to/<filename>` commands to edit the file in vim editor on an existing file. We can use the same `vim <filename>` command to create a non-existing file as well. Lets see this in action.

![[Untitled 9 4.png|Untitled 9 4.png]]

![[Untitled 10 3.png|Untitled 10 3.png]]

![[Untitled 11 3.png|Untitled 11 3.png]]

Following steps were performed in the above example:

1. List files using `ls` command
2. Open vim editor for the new file `vi vinewfile`
3. Change the vim editor mode to Insert using `esc` key and `I` key
4. Edit the contents of the file and save it using `:wq` command
5. `cat` command to view the contents of the file.

We can use the above steps to create a hidden file. For example `vim .vimhiddenfile`

**Note**: While using `vim` command to create a file on the go, we must save(`:wq`) it to appear in the path. Else the file will not be created.

### 3. Create directories with cli

Creating a directory is pretty straightforward. `mkdir`, short for “make directory”, is the command to create the directories. Lets see this in action.

![[Untitled 12 3.png|Untitled 12 3.png]]

Following steps were performed in the above example:

1. `ls` command to list all the files
2. `mkdir <dirname>`(`mkdir newdir`) command to create the directory in the current working directory
3. `mkdir ./realtive/path/<dirname>` command to create a directory in the specified path
4. To create an empty folder path i.e., creating a directory within a non-existing directory in a path, we should use `-p` option with the `mkdir` command. Else, the error(similar one) `mkdir: cannot create directory ‘./newnewdir/dir1’: No such file or directory`. For example: `mkdir -p ./newnewdir/dir1` will create both `newnewdir` and `dir1` within it,
5. To change the directory we use `cd /path/to/dir` command

  

### 4. File permissions in linux

Now, lets run `ls -al` or `ll` (long list) command in the home directory and check the output. The output of both the commands are similar and they display following info in the columns respectively

1. File permissions
2. [File's number of hard links](https://www.redhat.com/sysadmin/linking-linux-explained)
3. File owner username
4. name of the group that owns the file
5. Size of the file in bytes
6. Date and time of the file's last modification
7. Name of the file

![[Untitled 13 2.png|Untitled 13 2.png]]

In the above list the directories are marked with `d` for the first letter in the File permissions and similarly for files it is `-`. Each file and directory in linux will have read(`r`), write(`w`) and execute(`x`) permissions for the below categories of users:

1. Owner - Who own a file or directory
2. Group - A group of user with same permissions provided by the owner
3. World - Any user who is granted with some permissions provided by the owner

![[Untitled 14 2.png|Untitled 14 2.png]]

The first 3 letters after the directory/file indicator are the permissions for the Owner, followed by Group and finally World. Lets take 3 examples of the files/directories highlighted in the above picture.

- `.bashrc` file(-rw-r--r—) - The owner has read and write permissions for the file. No execution permissions for the owner. The group and the world share same permissions i.e., read only
- `newdir` directory(drwxr-xr-x) - The owner has all 3 permissions for this directory i.e., read, write and execute. The group and the world share the same permissions which is read and execute

File permissions, changing the permissions for file and user access in linux is a pretty interesting topic and we have barely touched the surface. Will dive deep in an upcoming post.

  

### 5. Manipulating files and directories in cli

The basic operations we perform in a filesystem(graphical or cli based) are create, copy, move, delete and rename the files and directories. lets see how that happens in a cli.

1. Creating a file with `touch` and `vim` commands:
    
    We have discussed the file creation in cli in the above section. One more thing to add to that is creating multiple files at once using `touch` command. `touch <file1> <file2> <file3>` will create 3 files in the current working directory. Additionally, we can mention the path for each file.
    
    ![[Untitled 15 2.png|Untitled 15 2.png]]
    
2. Copy files and directories with `cp` command
    
    1. `cp <source> <destination>` will copy the files/directories from source path to a destination path. For example, We’re copying a file in the following example using `cp ./index.html destdir/` command. Alternatively we can use wildcard(`*`) to specify the certain types of files or string in the filename. Ex: `cp *.html /destdir` command will copy all the html files and `cp *test* /destdir` command will copy all the files with “test” in their filename
        
        ![[Untitled 16 2.png|Untitled 16 2.png]]
        
    2. Copying a file to another file will overwrite the file in destination path. For example, the command`cp /path/to/sourcefile /path/to/destinationfile` will overwrite the contents of destination file with source file’s contents
        
        ![[Untitled 17 2.png|Untitled 17 2.png]]
        
    3. To copy a directory to a path, we need an additional option `-r` which stands for recursive and it will allow us to copy a directory and its contents recursively. For example `cp -r ./srcdir ./destdir`.
        
        ![[Untitled 18 2.png|Untitled 18 2.png]]
        
    
    At this point one might wonder why do we need a cli to perform these simple tasks which could be done in GUI very easily. Like drag and drop a file/directory to copy. The answer is power and flexibility.
    
    Let say we have thousands of files common between two directories(`src` and `dest`). We need to copy only those files that are not in to `dest` path from the `src` path. Doing this in the GUI is a tedious task and if we have to repeat it everyday or even every hour, it would be nearly impossible to finish the task with consistent results. But in cli it is just a simple command.
    
    `cp -u srcdir/* destdir/` command will copy only the files that are not present in `destdir` directory from `srcdir`and also the files that are modified recently.
    
    ![[Untitled 19 2.png|Untitled 19 2.png]]
    

  

1. Move files/directories with `mv` command:
    
    `mv srcpath destpath` command will move files from `srcpath` to `destpath`. if a file already exists in the `destpath` it will be overwritten. The following example shows how to use `mv` command to move files and also rename the file using the `mv` command.
    
    ![[Untitled 20.png]]
    
2. Remove/delete a file or directory with `rm` command:
    1. `rm /path/to/file` command will delete the file from the path. The following shows how `rm` command works
        
        ![[Untitled 21.png]]
        
    2. `rm -d /path/to/dir` command will delete empty directory from the path. if we try the same command with non-empty folder , we will see `Directory not empty` error. To remove a directory along with its contents, we use `-r`(recursive) option which will delete the contents recursively.
        
        ![[Untitled 22.png]]
        
    3. To check how to delete process is carried out we can use `i` option with `rm` command. This will show which file or directory is being deleted. Example shown below. For every file and subdirectories in the directory will ask for prompt to delete it (yes-y, no-n)
        
        ![[Untitled 23.png]]
        

**Note:** To perform copy, delete or move operations the users should have necessary permissions(rwx) as discussed in the above section.

### Resources:

1. [**Linux Command Line Books by William Shotts**](https://linuxcommand.org/tlcl.php)
2. [Unix / Linux - File Management (tutorialspoint.com)](https://www.tutorialspoint.com/unix/unix-file-management.htm)
3. [Vim Editor Modes Explained (freecodecamp.org)](https://www.freecodecamp.org/news/vim-editor-modes-explained/)
4. [Hard links and soft links in Linux explained | Enable Sysadmin (redhat.com)](https://www.redhat.com/sysadmin/linking-linux-explained)
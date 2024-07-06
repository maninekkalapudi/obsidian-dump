---
tags: Done
Start Date: 2022-07-19
End Date: 2022-08-17
---
Hello! In my last post I have written about [permissions in linux](https://maninekkalapudi.com/permissions-in-linux). In this post we will see explore about **processes in linux** and how to manage them from cli. Lets go!

  

**Topics covered in this post:**

1. Intro to modern systems
2. What is a process?
3. Processes in Linux
4. Interacting with processes in cli
5. Signals
6. Shutting down the system with cli

## 1. Intro to modern systems

Many modern systems are usually multitasking, meaning they can perform more than one tasks at once or at least they pretend to do this so well. In reality, the kernel in the operating system rapidly switches from one process to another and provides an impression that the system is multitasking.

This is true for linux as well. When we switch from a text document to a terminal, the linux kernel also switches the underlying processes as well. Switching a process means allotting the CPU time execution cycle and resources like memory to the processes

## 2. What is a process?

A program or a script is stored in a file and a process is nothing but a program in motion. Whenever we execute a program, the linux kernel has to allocate certain resources like CPU and memory to that program. The kernel also tracks the execution by assigning an ID called PID or Process ID.

This is shown graphically in operating systems like windows as below

![[Untitled 18.png|Untitled 18.png]]

## 3. Processes in Linux

When a linux system boots up, the kernel starts few processes using `init`. `init` is the first program that is launched and it also launches a series of shell scripts (located in `/etc`) called “_**init scripts**_” which start all the system services. The following image shows these init scripts in my system.

![[Untitled 1 8.png|Untitled 1 8.png]]

Many of these services will run as “**deamon programs**”, programs that runs in the background and they generally doesn’t have any UI or require user interaction.

Even when we just login to the system and not necessarily performing any tasks, it manages few processes to keep the system up.

The `init` process here becomes the “**_parent_**” or “_**grandparent process”**_ to all the process launched through it. The processes launched by other process are called as “**_child processes_**”.

## 4. Interacting with processes in cli

The following commands are widely used in monitoring and managing the processes in linux.

1. `ps`- provides the snapshots of all the processes running to the terminal
2. `top`- provides an dynamic view of all the processes running on the system within the terminal. The output looks similar to the GUI of a modern task manager
3. `<command/script> &`- runs the process in the background
4. `jobs`- shows the list of processes running in the background
5. `fg %<job_number>`- returns the process to the foreground
6. `kill`- command to kill the process

  

1. **`ps`** **command:**
    
    It is the most commonly used command to view processes for the user. It shows the snapshot of the processes running on the system at that time. The result looks like the below image
    
    ![[Untitled 2 8.png|Untitled 2 8.png]]
    
    “_**PID**_” or Processes ID, is the number assigned by the kernel for that process to track resources allotted. Ex: CPU time(TIME) and memory. “_**TTY**_” or “teletype,” refers to the controlling terminal for the process. “_**TIME”**_, the amount of CPU time consumed by the process.
    
    - `ps aux` command will display the process from all the users(`a`) that can be attached to any terminal(`x`) along with it’s user/owner info(`u`)
        
        ![[Untitled 3 8.png|Untitled 3 8.png]]
        
2. **`top`** [**(t**](https://phoenixnap.com/kb/top-command-in-linux#:~:text=The%20top%20(table%20of%20processes,including%20CPU%20and%20memory%20usage.)[able](https://phoenixnap.com/kb/top-command-in-linux#:~:text=The%20top%20(table%20of%20processes,including%20CPU%20and%20memory%20usage.) [**o**](https://phoenixnap.com/kb/top-command-in-linux#:~:text=The%20top%20(table%20of%20processes,including%20CPU%20and%20memory%20usage.)[f](https://phoenixnap.com/kb/top-command-in-linux#:~:text=The%20top%20(table%20of%20processes,including%20CPU%20and%20memory%20usage.) [**p**](https://phoenixnap.com/kb/top-command-in-linux#:~:text=The%20top%20(table%20of%20processes,including%20CPU%20and%20memory%20usage.)[rocesses](https://phoenixnap.com/kb/top-command-in-linux#:~:text=The%20top%20(table%20of%20processes,including%20CPU%20and%20memory%20usage.)[**)**](https://phoenixnap.com/kb/top-command-in-linux#:~:text=The%20top%20(table%20of%20processes,including%20CPU%20and%20memory%20usage.) **command:**
    
    It displays a real-time view of the running processes, their resource consumption and also displays kernel-managed tasks. The output is refreshed every 3 seconds by default.
    
    ![[Untitled 4 7.png|Untitled 4 7.png]]
    
    To exit the `top` command output prompt and get back to the terminal, press `q`.
    
      
    
3. `<command/script> &` command
    
    When we run a programs with GUI like notepad (gedit) from the cli, it will open a window and the terminal will be busy until the window is closed.
    
    ![[Untitled 5 6.png|Untitled 5 6.png]]
    
      
    
    Any command/script that is suffixed with `&` operator will run in the background and the terminal is available for the user. For example, `gedit &` command will launch the program and terminal will be available to the user right after it.
    
    ![[Untitled 6 6.png|Untitled 6 6.png]]
    
    Notice that there is a number printed on the terminal after the command. It is the PID assigned a shell feature called “_**job contol**_” which shows the PID of the process.
    
4. `jobs` command
    
    It will show the list of background processes/jobs that are launched from the terminal as shown below
    
    ![[Untitled 7 5.png|Untitled 7 5.png]]
    
    The `ps` command will also show info about the above process
    
    ![[Untitled 8 4.png|Untitled 8 4.png]]
    
      
    
5. `fg %<job_number>` command
    
    Our process (job id 1) is running in the background and any process running in the background is immune from terminal keyboard input, including any attempt to interrupt it with Ctrl-c. `fg %<job_number>` command will bring the process to the foreground
    
    ![[Untitled 9 3.png|Untitled 9 3.png]]
    
    When we press `Ctrl+C`, the process will terminate
    
    ![[Untitled 10 2.png|Untitled 10 2.png]]
    

## Signals

- `kill` command is used to “kill” the processes. Here’s an example:
    
    ![[Untitled 11 2.png|Untitled 11 2.png]]
    
    `gedit &` command will launch the program in the background and we PID is available on the terminal. Next, we use `kill <PID>` command to terminate the process.
    
    The `kill` command here doesn’t actually “kill” the program, rather it will send a signal to the process to terminate. This gives the process to save the work in progress and the processes will also listen to these signals.
    
    When the process is running on the foreground, `CTRL+C` and `CTRL+Z` will send a signal to interrupt and terminate the process respectively.
    

  

- `killall` command will terminate multiple processes with same or matching name or by username.
    
    - `killall -u <username>`- will kill all the processes under the username provided
    - `killall name`- will kill all the processes which matches the provided name
    
    ![[Untitled 12 2.png|Untitled 12 2.png]]
    
    In the above example, multiple instances of `gedit` program is launched in the background and will `killall` command, we can terminate all those instances at once.
    
    When we want to terminate the processes that doesn’t belong to us, we need superuser privileges.
    

## 6. Shutting down the system with CLI

Yes, we can do it! Shutting down a system involves orderly termination of all the processes in the system. It also requires admin privileges to perform this action.

- `sudo reboot`- restart the system
- `sudo shutdown -h now`- shuts down the system without any delay
- `sudo shutdown -r now`- reboots the system without any delay

  

`shutdown` command options:

1. `-h`- specifies to shut down the system
2. `now`- The time string may either be in the format "hh:mm" for hour/minutes(24h format) specifying the time to execute the shutdown at. We can also specify minutes with `+m` in place of “now”. Example, +0 means now and by default the values is “+1” when not specified in the command.

## Conclusion

Process management is a task that is usually maintained by the sysadmins and devops engineers to name a few. This helps in maintaining the health of the machines or servers and resource monitoring as well. Managing a linux system using cli is effective and swift.

  

### Resources

1. [**Linux Command Line Books by William Shotts**](https://linuxcommand.org/tlcl.php)
2. [Linux Command Basics: 7 commands for process management     | Enable Sysadmin (redhat.com)](https://www.redhat.com/sysadmin/linux-command-basics-7-commands-process-management)
3. [linux - What does aux mean in `ps aux`? - Unix & Linux Stack Exchange](https://unix.stackexchange.com/questions/106847/what-does-aux-mean-in-ps-aux)
4. [How to Use the top Command in Linux (phoenixnap.com)](https://phoenixnap.com/kb/top-command-in-linux#:~:text=The%20top%20(table%20of%20processes,including%20CPU%20and%20memory%20usage.)
5. [8 Linux commands for effective process management | Opensource.com](https://opensource.com/article/18/9/linux-commands-process-management)
---
tags: Done
Start Date: 2022-06-23
End Date: 2022-07-16
---
Hello! In my [last post](https://maninekkalapudi.com/io-redirection-in-linux) I have written about how I/O redirection works in linux. When we think of a command, which is a file in linux; it will be assigned with a set of permissions. Only users with right access will be able to run the command. All of this will be detailed in the following post. Let’s go!

  

**Topics covered in this post:**

1. Preface
2. Permission Groups
3. Permission Types
4. Modifying Permissions
5. Executing Commands with sudo

  

## 1. Preface

Linux or any UNIX-like operating system is built to the core with multi-user model in mind. Before the computers were personal, they filled up buildings as often seen in universities. A practical way to utilize the computer was to connect it with multiple [terminals](https://maninekkalapudi.com/the-linux-command-line-experience) for say, each department.

This is a multi-user model in a nutshell. Multiple users can connect to the same computer via terminals using relevant credentials and each user will have certain permissions for certain actions only.

In a modern system like cloud, multiple users connect to a remote server using SSH(Secure SHell). Each user accessing the server will have a separate account and permissions that are relevant to the role. For example, developer can have SSH access to the dev server but a tester may not have the same access.

## 2. Permission Groups

The way permissions are assigned can be grouped into 3 major categories.

1. Owner - Users may own files and directories and they have control over their access. The owner level access will not impact the actions of other users.
2. Group - Users can be grouped based on their role, say developers or admins; and assign access to files and directories to all the users under the specific group.
3. All Users - In addition to the above two groups, Any user who can access the system will have access to some files and directories granted by the owner.

Let’s see all of this in action. First, lets see what permissions are assigned to current user in the cli. We use `id` command and it shows

  

![[Untitled 7.png|Untitled 7.png]]

  

- uid - user ID. A number(1000) is assigned when user is created and it is mapped to the user ID.
- gid - primary group ID. User is assigned a primary group ID (gid) and may belong to additional groups
- groups- Different groups that the user is part of. Example: 27 is the sudo(root) user group

  

`id` command will show how the permissions are enabled to a user using different permission groups. Access to any resources within the system can be assigned using the groups which has a common function. Ex: SSH access to a machine for only developers.

  

**Note:** The uid and gid starts with 1000 for ubuntu and it might be different for other linux operating systems. Ex: Fedora starts with 500

## 3. Permission Types

Every file or a directory in linux has 3 basic permissions:

- Read(`r`)- To read the contents of a file or a directory (if execute(`x`) permission is also set for the directory)
- Write(`w`)- Create, Edit, rename or delete the contents of a file or a directory (if execute(`x`) permission is also set for the directory)
- Execute(`x`)- Run or execute a file or view the contents of a directory. This allows a file to be treated as a program and executed

Lets take a look at these permission in the cli. When we run the long list command i.e., `ls -l` command on a file or a directory, the first column in the resulting list are the permission to that object (highlighted in the below image).

![[Untitled 1 4.png|Untitled 1 4.png]]

The first character in the permissions is the object indicator, directory is `d` and file is `-`. The rest of the characters represents permission groups.

The first set of three characters after the object indicator are Owner permission, next set are Group permissions and last set are All User permissions.

![[Untitled 2 4.png|Untitled 2 4.png]]

For example, we can understand the permission in the above long list as follows:

- `dir1`- This is a directory(`d`). The owner has all the permissions(`rwx`), the group and all users has read and execute permissions(`r-x`)
- `logs.txt`- This is file(`-`). The owner has only read and write access(`rw-`). The group and the all users han only read permissions(`r—`)

## 4. Modifying Permissions

`chmod` command is used to change the permissions of a file or a directory. Only the file’s owner or the superuser can change the mode of a file or directory.

The permission groups for the `chmod` commands will be mentioned as

- `u`- Owner
- `g`- Group
- `o`- Owner
- `a`- All users

Additionally, `+` and `-` assignment operators are used to add or remove permissions respectively. Again, the permission here are read, write and execute(`rwx`).

The syntax for `chmod` command is as follows:

`chmod <permission_group><assignment_operator><permission> <file_name/directory_name>`

  

**Scenario 1**:

Remove the execution permission for all users for `dir1` (from previous example).

![[Untitled 3 4.png|Untitled 3 4.png]]

  

**Scenario 2**:

Assign execution permission to only group for `dir1`.

![[Untitled 4 3.png|Untitled 4 3.png]]

  

**Scenario 3**:

Add execute permission for the owner and set the permissions for the group and others to read and execute. Multiple specifications may be separated by commas

![[Untitled 5 2.png|Untitled 5 2.png]]

_**Note**_: Assigning the right permissions to the relevant users is very necessary activity to maintain best security practices. Generally the less permission given to an user or a group, the better to avoid any mishaps.

## 5. Executing Commands with sudo

Sudo (su “do”) allows a system administrator to delegate authority to give certain users (or groups of users) the ability to run some (or all) commands as root or another user while providing an audit trail of the commands and their arguments.

In linux, some resources that are very fundamental to the system are managed by administrators only to ensure the integrity. The simplest example to use a sudo is update command.

In ubuntu, `apt-get update` will update the operating system. `apt` is the package manager here and `update` is the option to tell the system to fetch the latest version of the software.

![[Untitled 6 2.png|Untitled 6 2.png]]

The current user does not have the necessary permission to run the command successfully. The update command refers to the systems files(`/var/lib/apt/lists/lock`) that only admin users have permissions.

To execute the update command successfully, `sudo` will be prefixed to the update command. This will temporarily allow us to execute the commands as an admin. This is demonstrated in the second half in the above example image.

**Thoughts on using** **`sudo`** **command:**

`sudo` command gives the privileges of a sysadmin to any user. In a well designed system, admin privileges will not be provided to any user. As a general rule of thumb, less privileges to an user are better to avoid any change of full system compromise under an attack.

## Conclusion

Multi-user systems like UNIX/linux are designed to ensure that multiple users would be able to use one machine. This also brings interesting questions on who gets to access what data on that machine.

Linux has always been developed with this in mind. Using `chmod` command in cli to assign right permission is a lot easier especially in server environments.

  

### Resources

1. [**Linux Command Line Books by William Shotts**](https://linuxcommand.org/tlcl.php)
2. [Classic SysAdmin: Understanding Linux File Permissions - Linux Foundation](https://linuxfoundation.org/blog/classic-sysadmin-understanding-linux-file-permissions/)
3. [How to Create Users in Linux (useradd Command) | Linuxize](https://linuxize.com/post/how-to-create-users-in-linux-using-the-useradd-command/)
4. [How To Create a Sudo User on Ubuntu | Linuxize](https://linuxize.com/post/how-to-create-a-sudo-user-on-ubuntu/)
5. [Changing file permissions (xinuos.com)](http://osr507doc.xinuos.com/en/OSUserG/_Changing_file_permissions.html)
6. [Sudo](https://www.sudo.ws/)
7. [Linux command line basics: sudo | Enable Sysadmin (redhat.com)](https://www.redhat.com/sysadmin/sudo)
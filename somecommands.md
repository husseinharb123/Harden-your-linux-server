# common linux commands 

## 1. ssh 
- ### install openssh server on linux 
    ```
    sudo apt-get install openssh-server
    ```

## 2. chown  (Change of Ownership)


- ### Change the owner of a file:

    ```
    chown user1 file.txt
    ```
    `    This command changes the owner of file.txt to user1.`

- ### Change the owner and group of a file:

    ```
    chown user1:group1 file.txt
    ```
    `This command changes the owner of file.txt to user1 and the group to group1.`

- ### Change the owner of a directory and its contents:

    ```
    chown -R user1 /path/to/directory
    ```
    `This command changes the owner of the directory /path/to/directory and all its contents to user1. The -R flag makes the operation recursive, so it affects all files and subdirectories in the directory.`

- ### Change the owner of a symbolic link:

    ```
    chown -h user1 symlink
    ```
    `    This command changes the owner of the symbolic link symlink itself, rather than the file it points to. The -h flag is used to modify the link rather than the file it points to.`

- ### Change the owner of multiple files at once:

    ```    
    chown user1 file1.txt file2.txt file3.txt
    ```
    ``This command changes the owner of file1.txt, file2.txt, and file3.txt to user1.``

`Note that in order to use chown, you need to have appropriate permissions to change the ownership of the file or directory.`

## 3. chmod (change mod)

- ### Changing the permissions of a file to allow read, write, and execute access for the owner, and read access for everyone else:

    ```
    chmod 744 myfile.txt

    ```
- ### Adding execute permission to a script file:

    ```        
    chmod +x script.sh
    ```
- ### Removing write permission for a group from a directory and its contents:

    ```
    chmod -R g-w mydirectory
    ```
- ### Changing the owner and group of a file to user1 and group1, respectively:

    ```
    chmod u=user1,g=group1 myfile.txt
    ```
- ### Removing all permissions for the group and others from a file:

    ```
    chmod go= myfile.txt
    ```
`In each of these examples, the chmod command is used to change the permissions of the specified file or directory. The numerical value or symbolic representation of the permissions is used to specify which permissions should be added or removed. The + or - symbol is used to indicate whether the permissions should be added or removed, respectively.`


## 4. adduser 
   

- ### To add a new user with default options, simply type:

    ```
    sudo adduser username
    ```


- ### To create a new user and specify a home directory, use the following command:

    ```
    sudo adduser --home /path/to/home/dir username
    ```


- ### To create a new user and assign them to a specific group, use the following command:

    ```
    sudo adduser --ingroup groupname username
    ```


- ### To create a new user with a custom comment, use the following command:


```sudo adduser --comment "Custom Comment" username```


- ### To create a new user and specify a custom shell, use the following command:


    ```
    sudo adduser --shell /path/to/shell username
    ```




## 5. useradd

Sure, here are some examples of how to use the useradd command in Linux:

- ### To create a new user with default options, simply type:

    ```
    sudo useradd username
    ```

- ### To create a new user with a home directory, use the following command:

    ```
    sudo useradd -m username
    ```

- ### To create a new user and assign them to a specific group, use the following command:


```sudo useradd -G groupname username```

- ### To create a new user with a custom comment, use the following command:

    ```
    sudo useradd -c "Custom Comment" username
    ```

- ### To create a new user with a custom shell, use the following command:

    ```
    sudo useradd -s /path/to/shell username
    ```


`These are just a few examples of how to use the useradd command in Linux. For more options and details, you can refer to the useradd man page by typing man useradd in the terminal.`

## `adduser vs useradd `


 - ### adduser is a higher-level command that is designed to be more user-friendly and interactive. It prompts the user for additional information, such as full name, password, and group membership. It also creates a home directory and sets up some default settings, such as creating a user group with the same name as the user.

 - ### useradd, on the other hand, is a lower-level command that provides more flexibility and control over the user creation process. It does not create a home directory by default and requires additional flags to specify other settings, such as the home directory, shell, and group membership.

 - ### Here are some key differences between adduser and useradd:

    1. adduser is interactive, while useradd is not. adduser prompts the user for additional information, while useradd requires all options to be specified on the command line.

    2. adduser creates a home directory by default, while useradd does not. With useradd, you need to use the -m flag to create a home directory.

    3. adduser sets up default settings, such as creating a user group with the same name as the user, while useradd does not. With useradd, you need to use the -g flag to specify the primary group and the -G flag to specify additional groups.

    4. adduser is more user-friendly and easier to use, while useradd provides more control and flexibility.

        `Overall, the choice between adduser and useradd depends on your specific needs and preferences. If you are looking for a more user-friendly and interactive experience, adduser is a good choice. If you need more control over the user creation process and prefer to specify all options on the command line, useradd is a better option.`

## 6. to show the users use cat or getnet 

Each line in the /etc/passwd file represents a user account on the Linux system and contains several fields separated by colons (:). The fields in each line of the /etc/passwd file are as follows:

```
username:password:UID:GID:gecos:home directory:default shell
```

`username`: This is the name of the user account, which is used to log in to the system.

`password`: This field used to contain the encrypted password for the user account. However, in modern Linux systems, this field is typically set to an x character, indicating that the encrypted password is stored in the /etc/shadow file instead.

`UID`: This is the user ID (UID) number, which uniquely identifies the user account on the system.

`GID`: This is the group ID (GID) number, which specifies the primary group for the user account.

`gecos`: This field is used to store additional information about the user, such as the user's full name, phone number, and email address. This field is also known as the "comment" field.

`home directory`: This is the path to the user's home directory, which is where the user's files and settings are stored.

`default shell`: This is the default shell that is used when the user logs in to the system. This can be a command-line interface, graphical user interface, or other type of user interface.

It's worth noting that some of the fields in the /etc/passwd file may contain special values, such as nologin, which indicates that the user account is not allowed to log in to the system.


- To view the usernames of all the users on the system
    ```
    cat /etc/passwd | cut -d: -f1
    ```


- Alternatively, you can use the getent command to retrieve a list of all the users on the system. The getent command searches a variety of databases, including /etc/passwd, to retrieve information about system users and groups. 

    ```
    getent passwd | cut -d: -f1
    ```




`cut -d`: -f1 - This part of the command uses the cut command to extract the first field from each line of the output from the previous cat command. The cut command is used to extract columns of data from text files, and the options used in this command are:

`-d`: - This specifies the delimiter that is used to separate the fields in the /etc/passwd file. In this case, the delimiter is a colon (:).

`-f1` - This specifies the field that we want to extract from each line of the output. In this case, we want to extract the first field, which contains the username of each user account.

## 7 . remove a user 

To remove a user from Linux, you can follow these steps:

-  Log in as the root user or as a user with sudo privileges.

    ```
    sudo deluser johndoe
    ```
- If you want to remove the user's home directory and mail spool, add the --remove-home option to the command:

    ```
    sudo deluser -r johndoe
    ```
- Note: If the user is currently logged in or a process is running under this account, you may need to log them out or force them to log out before you can delete their account. You can use the pkill command followed by the username to force them to log out:

    ```
    sudo pkill -u johndoe
    ```
- list to show only the processes that are running under the user account.

    ```
    sudo ps -u username 
    ```
- Note the process ID (PID) of the process that you want to stop.To stop a process, you can use the kill command followed 

    ```
    sudo kill 16349
    ```


## 8 . remove a group 

To remove a group in Linux, you can use the groupdel command. Here are the steps to remove a group:


Open a terminal or command prompt.

- Use the groupdel command followed by the name of the group you want to remove. For example, to remove a group named "developers", you would type:

    ```
    sudo groupdel developers
    ```

- Note: If there are any users that are still a part of the group you want to delete, you will need to remove them from the group first using the gpasswd command. For example, if the user "johndoe" is a member of the "developers" group, you can remove them from the group using the following command:
    ```
    sudo gpasswd -d johndoe developers
    ```
After removing all users from the group, you can proceed with step 3 above to delete the group.


## 9. usermod 
## usermod is a command in Linux and Unix operating systems that is used to modify user accounts. Here are some examples of how you can use usermod:

- Change a user's home directory:


    ```
    usermod -d /new/home/directory username
    ```
    This will change the home directory of the user "username" to "/new/home/directory".

- Add a user to a group:


    ```
    usermod -a -G groupname username
    ```

- Change a user's primary group:

    ```
    usermod -g groupname username
    ```

- Lock a user's account:

    ```
    usermod -L username
    ```
`This will lock the account of the user "username" by adding an exclamation mark before the encrypted password in the /etc/shadow file.`

- Unlock a user's account:

    ```
    usermod -U username
    ```
`This will unlock the account of the user "username" by removing the exclamation mark from the encrypted password in the /etc/shadow file.`


## 10. managing packages in ubunto using apt-get 
- apt-get: This is the default package manager for Debian and Ubuntu based distributions. It can be used to install, upgrade, and remove - packages. Here are some examples:
- `sudo apt-get update`: Update the package list
- `sudo apt-get upgrade`: Upgrade all packages to their latest version
- `sudo apt-get install <package_name>`: Install a package
- `sudo apt-get remove <package_name>`: Remove a package
- `sudo apt-get autoremove`: Remove any unnecessary dependencies 

## 11. managing packages in ubunto using apt
- `sudo apt update`: This command updates the local package index, so you have the most up-to-date information on available packages.

- `sudo apt upgrade`: This command upgrades all installed packages to their latest version.

- `sudo apt install <package-name>`: This command installs the specified package and any dependencies it requires.

- `sudo apt remove <package-name>`: This command removes the specified package but does not remove any dependencies that are not needed by other packages.

- `sudo apt autoremove`: This command removes any unused dependencies that were installed as part of other packages and are no longer needed.

- `sudo apt search <search-term>`: This command searches for packages that match the specified search term.

- `sudo apt show <package-name>`: This command provides detailed information about a specific package, such as its version, description, and dependencies.

- `sudo apt list --installed`: This command shows a list of all the installed packages on your system.

- `sudo apt full-upgrade`: This command performs a more aggressive upgrade, which may remove packages if necessary to upgrade others.

- `sudo apt edit-sources`: This command opens the sources.list file, which contains a list of repositories where packages can be downloaded from. You can use this file to add, remove, or modify repositories.


## 11. user management, including login, logout, shutdown, and sleep. 

- Login: `login johndoe`
- Logout: `logout`
- Shutdown imed: `shutdown -h ` 
- restart : `shutdown -r`
- Sleep:  `sleep 10 `  suspend the system after 10 seconds.

## 12. passwd 

- `passwd`: This command allows a user to change their own password. Simply run passwd and follow the prompts to enter and confirm a new password.

- `sudo passwd`: This command allows a system administrator to change the password of another user. Replace "username" with the name of the user whose password you want to change.

- `passwd -l`: This command locks a user's password and prevents them from logging in. This is useful for temporarily disabling a user's account. Replace "username" with the name of the user whose password you want to lock.

- `passwd -u`: This command unlocks a user's password and allows them to log in again. Replace "username" with the name of the user whose password you want to unlock.

- `passwd -d`: This command deletes the password of a user. This means that the user can log in without a password. This is not recommended for security reasons. Replace "username" with the name of the user whose password you want to delete.

- `passwd -S`: This command displays the status of a user's password. It shows whether the password is locked or unlocked, and when the password was last changed. Replace "username" with the name of the user whose password status you want to check.

12. Check the system logs 
- ` cat  /var/log/auth.log `
-  `cat /var/log/secure`


13. list all groups
    ```
    sudo cat /etc/group
    ```
14. create new group 
    ```
    sudo addgroup groupname
    ```

## 15 . systemctl 

### systemctl is a powerful command-line tool used for controlling the systemd system and service manager, which is used in many modern Linux distributions. Here are some useful commands that can be used with systemctl:

- Start a service: `systemctl start <service>`
This command starts a service that is currently inactive.

- Stop a service: `systemctl stop <service>`
This command stops a running service.

- Restart a service: `systemctl restart <service>`
This command stops and then starts a service.

- Reload a service: `systemctl reload <service>`
This command reloads the configuration of a service without stopping it.

- Check the status of a service: `systemctl status <service>`
This command shows the current status of a service, including whether it is running or not.

- Enable a service to start at boot: `systemctl enable <service>`
This command configures a service to start automatically at system boot.

- Disable a service from starting at boot: `systemctl disable <service>`
This command removes a service from the list of services that start automatically at system boot.

- View the logs of a service: `systemctl status <service> -l`
This command shows the logs of a service, including any errors or warnings.

- List all active units: `systemctl list-units --type=service`
This command shows a list of all active services on the system.

- List all units and their status: `systemctl list-units`

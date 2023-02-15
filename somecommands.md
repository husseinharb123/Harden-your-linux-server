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
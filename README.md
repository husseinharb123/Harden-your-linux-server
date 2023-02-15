# Document Linux host information
Each time you work on a new Linux hardening job, you need to create a new document that has all the checklist items listed in this post, and you need to check off every item you applied on the system. Furthermore, on the top of the document, you need to include the Linux host information:
Machine name
- IP address
- Mac address
- Name of the person who is doing the hardening (most likely you)
- Date



# common used commands 
- Show the location where the package is installed. The -S (capital S) stands for "search" : sudo dpkg -S {package_name}
- curl ifconfig.co : show public ip 

# enable and enforce secure password policy 

## A strong password should contain:
- min length of 12 char
- Upper case letters
- Lower case letters
- Digits
- Symbols

##  To enforce a secure password policy in Ubuntu, we will use the pwquality module of PAM 
1. install this module, launch the Terminal by using Ctrl+Alt+T shortcut. Then run this command in Terminal:
    ``` 
        sudo apt install libpam-pwquality 
    ``` 
2. copy `/etc/pam.d/common-password ` file before configuring any changes. for backup reason
3. edit it for configuring password policies using  
   ```
    sudo nano /etc/pam.d/common-password  
   ```
4. look for the following line  `Password   requisite   pam_pwquality.so retry=3 `
5. replace it with the following

    ``` 
    password requisite pam_pwquality.so retry=4 minlen=9 difok=4 lcredit=-2 ucredit=-2 dcredit= -1 ocredit=-1 reject_username enforce_for_root 
    ```
6. The parameters in the above command mean:

- `retry`: The number of consecutive times a user can enter an incorrect password.
- `minlen`: The minimum length of the password.
- `difok`: The number of characters that can be similar to the old password.
- `lcredit`: The minimum number of lowercase letters.
- `ucredit`: The minimum number of uppercase letters.
- `dcredit`: The minimum number of digits.
- `ocredit`: The minimum number of symbols.
- `reject_username`: Rejects the password containing the username.
- `enforce_for_root`: Also enforces the policy for the root user.

7. reboot the system to apply the changes in the password policy
    ```
    sudo reboot
    ```
## Configure password expiration period

### Password expiration policy can be configured through ` /etc/login.defs`
1.  edit this file: 
    ```
     sudo nano /etc/login.defs
    ```
2. add the following values per your requirments :
    ```
    PASS_MAX_DAYS 120   # maxium number of days a pasword may be used
    PASS_MIN_DAYS 0     # minium number of days allowed between pass change
    PASS_WARN_AGE 8     # numbers of days warning given before a passowrd expires
    ```
### above-configured policy will only apply on the newly created users. To apply this policy to an existing user, use `chage` command
1. to use the `chage` command ,syntax is :
   ```
   chage [options] username
   ```
2. To view the current password expiry/aging details, the command is:
   ```
   sudo chage –l username
   ```
3. configure the maximum No. of days after which a user should change the password :
   ```
   sudo chage -M <No./_of_days> <user_name>
   ```
4. To configure the minimum No. of days required between the change of password
   ```
   sudo chage -m <No._of_days> <user_name>
   ```
5. To configure warning prior to password expiration:
    ```
    sudo chage -W <No._of_days> <user_name>
    ```
# Disable Root Login on Ubuntu 20.04

## Good security practices recommend that you disable the root login over SSH to prevent unauthorized access to your Linux-based machine by any other user. Disabling root login prevents root access over SSH to your Linux-based machine, which means that no one will have unlimited privileges. Following the recommended security practices, you should create an additional user with almost all superuser privileges to access the account.

## Prerequisites

### created a new user and added that user to the sudo group to grant administrative privileges. You will use this sudo user to access your machine because you won’t be able to SSH as a root user after disabling the root login

1. Open the terminal and run the following command to create a new user:
```
sudo adduser <username>
```
2. set password for the user 
   ```
   sudo passwd <username>
   ```
3.  To add the newly created user to the sudo group, run the following command:
```
sudo usermod -aG sudo <username>
```
## To disable root login on Ubuntu 20.04, follow these steps:

1. Log into your Ubuntu 20.04 server with a non-root user with sudo privileges.

3. Edit the SSH configuration file located at /etc/ssh/sshd_config with the following command:
   ```
     sudo nano /etc/ssh/sshd_config.
   ```

4. Find the line that starts with `PermitRootLogin yes` and change it to `PermitRootLogin no`.

5. Save and close the file.

6. Restart the SSH service to apply the changes: 
    ```
    sudo systemctl restart ssh.
    ```


Now, root login over SSH will be disabled and unauthorized access to your Ubuntu 20.04 machine will be prevented.

## Check the authentication attempts log 

1. To monitor login attempts on an Ubuntu server, you can use the `auth.log ` file located in the `/var/log1` directory. 
    ```
    sudo cat /var/log/auth.log
    ```
2. You can also use the `grep` command to search the file for specific keywords, such as "login" or "failed":

    ```

    grep 'login' /var/log/auth.log

    grep 'failed' /var/log/auth.log

    This will show you all log entries related to logins and failed login attempts, respectively.
    ```
# Update the  Ubuntu Server Before Production

## To ensure the security, stability, compatibility, and ability to take advantage of new features and improvements, it is important to fully update an Ubuntu server before production. Here's how to do it:
`Note: Make sure to back up important data before updating your server to avoid any potential data loss.`

## Steps 
1. Connect to the server via SSH or through a terminal console.
2. Run the command `sudo apt-get update` to update the package list.
3. Run the command `sudo apt-get upgrade` to upgrade all installed packages to their latest version.
4. Run the command `sudo apt-get dist-upgrade` to upgrade the distribution and handle dependencies.
5. After the upgrade process is complete, it is recommended to reboot the server using the `sudo reboot` command.

# Set Up Automatic Security Update (Unattended Upgrades)

## One of the most fundamental ways to keep the Ubuntu server secure is by installing security updates on time to patch vulnerabilities. By default, the unattended-upgrades package installed, but you still need to configure a few options. It will automatically install software updates, including security updates. 

## steps
1. Update the Ubuntu 20.04 LTS server for security patches, run:
   ```
   sudo apt update && sudo apt upgrade
   ```
2. Install unattended upgrades on Ubuntu if not installed.
   ```
   sudo apt install unattended-upgrades 
   ```
3. You need to install  the update-notifier-common package in order to set up automatic reboot.

    ```
    sudo apt install update-notifier-common
    ```
4. Configure automatic unattended updates:
   ### you can Configure automatic unattended updates from this file  `/etc/apt/apt.conf.d/50unattended-upgrades`
    ```
    sudo vi /etc/apt/apt.conf.d/50unattended-upgrades
    ```

   ### Set up alert email ID uncomment :
    ```
        Unattended-Upgrade::Mail "vivek@server1.cyberciti.biz"
        Unattended-Upgrade::MailReport "only-on-error";
    ```

   ### Automatically reboot Ubuntu box WITHOUT CONFIRMATION for kernel updates uncomment :

    ```
    Unattended-Upgrade::Automatic-Reboot "true"
    ```

   ### Recommended: remove unused kernel packages and dependencies and make sure the system automatically reboots if needed by uncommenting and adapting the following lines You may have to add a semicolon at the end of this line:
    ```
        Unattended-Upgrade::Remove-Unused-Kernel-Packages "true";
        Unattended-Upgrade::Remove-New-Unused-Dependencies "true";
        Unattended-Upgrade::Remove-Unused-Dependencies "true";
        Unattended-Upgrade::Automatic-Reboot "true";
        Unattended-Upgrade::Automatic-Reboot-Time "02:38";
    ```
   ###  Save and close the file.

5.  Turn on unattended security updates, run:
    ```
    sudo dpkg-reconfigure -plow unattended-upgrades
    ```


### Verify that it is working by running the following command:
```
    sudo unattended-upgrades --dry-run --debug
```
### Check the status of the unattended-upgrades service using systemctl:
```
    sudo systemctl status unattended-upgrades.service
```

## It is time to see logs . Hence, we can use command such as grep command or cat command or more command/egrep command as follows:
```
    sudo cat /var/log/unattended-upgrades/unattended-upgrades.log
```
##  you can control the schedule for the apt-daily.service where the system checks for update, which performs automatic system updates on Ubuntu-based systems.by default  it is twice a day with a random delay after 6 AM and 6 PM
1. you can in edit the apt-daily.timer file :
   ```
   sudo nano /lib/systemd/system/apt-daily.timer
   ```
2.  you can make changes to the file, save it and have it take effect.




`note : You learned how to configure automatic unattended updates for your Ubuntu Linux based server up-to-date. It is a simple and easiest way to protect your server from vulnerabilities. This method is also beneficial when you administrate multiple servers. Manually updating the system and applying patches can be a very time-consuming process. However, for a large number of servers/VMs, I would recommend something like Ansible:`

# locking down openssh

## Use SSH public key based login

### Using SSH public key based authentication is considered a best practice for several reasons:
- Increased security: Public key authentication is more secure than password-based authentication as the private key must be paired with the public key and kept secure by the user.

- Convenience: Once the public key is set up, you do not need to enter a password each time you log in, making it a more convenient way to access your servers.

- Auditing: SSH public key authentication allows you to log and monitor all access to your servers, making it easier to track and audit who has accessed your servers and when.

- Scalability: SSH public key authentication is easily scalable, allowing you to add and remove users without changing passwords for every user.n_
- 
### steps
1. Generate a key pair:
```
ssh-keygen -t rsa -b bits -C "COMMENT"
```
2. add passphrase if you want for more security 
3. Store the public key on the server: 
    ```
    ssh-copy-id -i keyname.pub user@server_ip_address
    ``` 
3.   now ssh into the server and edit the ssh service configuation 
   ```
   sudo nano /etc/ssh/sshd_config
   ```
4. within `/etc/ssh/sshd_config` 
   - Enable public key authentication:
     Find the line `PubkeyAuthentication no` and change it to `PubkeyAuthentication yes`.
   - Disable password authentication:
     Find the line `PasswordAuthentication yes` and change it to` PasswordAuthentication no`.
   - change the default port for ssh service :
     Find the line `Port 22` and change it to` Port <any port number between 1 and 65535>`.
   - change the ListenAddress of ssh service :
     Find the line `ListenAddress 0.0.0.0` and change it to` ListenAddress <your ip that you  try to ssh from >
   - Disable RootLogin :
     Find the line `PermitRootLogin <anyvalue>` and change it to` PasswordAuthentication no`.

5. restart the ssh service : 
   ```
   sudo systemctl restart ssh 
   ```
    
6. Set permissions for the private key (only the owner):

    ```
    chmod 600 ~/.ssh/authorized_keys
    ```
7. Test the connection:
    ```
    ssh -i ~/.ssh/private_key_file user@server_ip_address
    ```
8. Monitor and audit:
To monitor and audit access to your servers, you can use tools such as `log analyzers or intrusion detection systems`. Additionally, you can view the SSH logs in the `/var/log/auth`.log file on a Linux server.


# monitor ssh logs and lock ssh 

## Fail2ban is a tool that helps prevent unauthorized access to a server by monitoring log files for repeated failed login attempts, and then banning the IP addresses responsible for the attempts. When used with SSH on Linux, it helps secure the server by preventing brute-force attacks where an attacker tries multiple username and password combinations in rapid succession. By banning IP addresses that have exceeded a configured number of failed login attempts, fail2ban helps to reduce the risk of successful attacks on the SSH service.

## steps 
1. Install fail2ban:

    ```
    sudo apt-get install fail2ban
    ```

2. Copy the default SSH configuration file:

    ```
    sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
    ```
3. Configure the jail.local file:

    ```
    sudo nano /etc/fail2ban/jail.local
    ```
4. In the `jail.local `file, uncomment the line that starts with [ssh] and change the enabled value to true.
You can also adjust the other values, such as `port`, `maxretry`,`ignoreip`, and `bantime` to fit your requirements.
    ```
    port	    Port specification
    filter	    Service specific filter (Log filter)
    logpath	    What log to use
    maxretry	Number of attempts to make before a ban
    findtime	Amount of time between failed login attempts
    bantime	    Number of seconds an IP is banned for
    ignoreip	IP to be allowed
    ```
5. Restart fail2ban:
    ```
    sudo systemctl restart fail2ban
    ```
6. Check the status of fail2ban:

    ```
    sudo fail2ban-client status ssh
    ```
7. To ensure that Fail2ban runs on system startup, use the following command:
    ```
    sudo systemctl enable fail2ban.service 
    ```
   
`Now, fail2ban is monitoring your SSH log file for failed login attempts and automatically banning the IP addresses of malicious actors.`

# use firewall to block unused ports or sockets 
## ufw stands for Uncomplicated Firewall and is a front-end for the iptables firewall on Linux systems. It provides a simpler way to manage the firewall compared to using iptables directly, making it easier to configure and maintain. Some of the reasons why ufw is commonly used include:
- Simplicity: ufw provides a simple and easy-to-use interface for managing firewall rules, making it easy for even novice users to manage their firewall.
- Flexibility: ufw allows you to define firewall rules using a variety of methods, including allowing or denying traffic based on the source or destination IP address, port, and protocol.
- Logging: ufw provides logging capabilities, allowing you to monitor firewall activity and track any attempts to bypass the firewall.
- Security: ufw can be used to secure your system by blocking incoming traffic from unauthorized sources, as well as outgoing traffic to potentially dangerous destinations.

## steps :

1.  list  all TCP and UDP sockets, including their process, that are in the listening state, with numerical addresses using this command  
    ```
    sudo ss -tupln
    ```
2. then you can use ufw chose which sockets you want to  block these :
    ```
    sudo apt install ufw
    ```
3. Check the status of UFW:

    ```
    sudo ufw status
    ```
4. Enable UFW:

    ```sudo ufw enable```
5. Allow incoming traffic to a specific port:

    ```
    sudo ufw allow [port number]
    ```
6. Deny incoming traffic to a specific port:

    ```sudo ufw deny [port number]```
7. llow incoming traffic to a specific port for a specific IP address:

    ```
    sudo ufw allow from [IP address] to any port [port number]
    ```
8. Delete a rule:

    ```
    sudo ufw delete [rule number]
    ```
9. Disable UFW:

    ```
    sudo ufw disable
    ```
10. Reset UFW to default state:

    ```
    sudo ufw reset 
    ```  
11. reload UFW service:

    ```
    sudo ufw reload 
    ```  
## block incoming ICMP (ping) requests 
1. enter the following command:

    ```
    sudo ufw deny in to any from any proto icmp
    ```
2. to apply the changes, enter:
    ``` 
    sudo ufw reload
    ```

`This will prevent incoming ping requests to your server and ensure that pinging is not allowed.`




# Limiting User Privileges:

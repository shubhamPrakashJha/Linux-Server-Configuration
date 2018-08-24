# Linux Server Configuration for Web App
_Configure Amazon Lightsail Linux Server To host Web App_

## SERVER INFORMATIONS

**PUBLIC IP :**  `13.232.118.216`

**INITIAL LOGIN URL FOR SHELL :** `ssh ubuntu@13.232.118.216 -i ~/.ssh/LightsailDefaultPrivateKey.pem`

**SSH PORT AFTER STEP 4 :** `2200`

**UPDATED LOGIN URL FOR SHELL AFTER STEP 4 :** `ssh ubuntu@13.232.118.216 -i ~/.ssh/LightsailDefaultPrivateKey.pem -p 2200`

**USERNAME :**  `grader`

**PASSWORD :**  `grader`

**Login command for Grader :**  `ssh grader@13.232.118.216 -i ~/.ssh/graderKeyPair -p 2200`

**PASSPHRASE :**  `grader`




## PROCEDURE

### _Getting Your Server________________________

#### STEP 1 : Start a new Ubuntu Linux server instance on Amazon Lightsail
* Log in to Lightsail
* Create an instance
* First, choose "OS Only" (rather than "Apps + OS"). Second, choose Ubuntu as the operating system.
* Choose your instance plan
* Give your instance a hostname
* Wait for it to start up
* Once your instance has started up, you can log into it with `Connect using SSh`
* `public IP` address of the instance is displayed along with its `User name`
* Download `private key` of SSH KEY PAIR.
* save this .pem file to `~/.ssh` directory

#### STEP 2 : SSH into your server
* Use these commands to SSH into your server from your console
    ```commandline
    ssh ubuntu@13.232.118.216 -i ~/.ssh/LightsailDefaultPrivateKey.pem
    ```

### _Securing Your Server________________________

#### STEP 3 : Update all currently installed packages
* Use these commands to update installed packages
    ```commandline
    sudo apt-get update
    sudo apt-get upgrade
    ```
* If *** System restart required *** is displayed at login, run:
    ```commandline
    sudo reboot
    ```

#### STEP 4 : Change the SSH port from 22 to 2200
* Firstly, Open port 2200 in `Lightsail > Networkind > Firewall` to avoid locking yourself out by adding:
    ```textmate
    Application   Protocol    Port range
    Custom        TCP	  2200
    ```
* Open `/etc/ssh/sshd_config` file using :

    ```commandline
    sudo nano /etc/ssh/sshd_config
    ```
* Change the line `Port 22` to `Port 2200`
* Then restart the SSH service:
    ```commandline
    sudo service ssh restart
    ```
* New command to login to the server:

    ```commandline
    ssh ubuntu@13.232.118.216 -i ~/.ssh/LightsailDefaultPrivateKey.pem -p 2200
    ```
    
#### STEP 5 : Configure the Uncomplicated Firewall (UFW) 
Allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP 
(port 123)
* Check UFW status
    ```commandline
    sudo ufw status
    ```
* Block all incoming connections on all ports
    ```commandline
    sudo ufw default deny incoming
    ```
* Allow outgoing connection on all ports
    ```commandline
    sudo ufw default allow outgoing
    ```
* Allow incoming connection for SSH on port 2200
    ```commandline
    sudo ufw allow 2200/tcp
    ```
* Allow incoming connections for HTTP on port 80
    ```commandline
    sudo ufw allow www
    ```
* Allow incoming connection for NTP on port 123
    ```commandline
    sudo ufw allow ntp
    ```
* check the rules that have been added before enabling the firewall
    ```commandline
    sudo ufw show added
    ```
* enable the firewall
    ```commandline
    sudo ufw enable
    ```
* Check UFW status again
    ```commandline
    sudo ufw status
    ```
    
### _GIVING GRADER SERVER ACCESS________________________
Giving `grader` access to log in to my server for reviewing my project.
#### STEP 6 : Create a new user account named `grader`
* Create user `grader`
    ```commandline
    sudo adduser grader
    ```
#### STEP 7 : Give `grader` the permission to `sudo`
* If you are signed in using a non-root user with sudo privileges, type
    ```commandline
    sudo visudo
    ```
* Search for the line that looks like this:
    ```commandline
    root    ALL=(ALL:ALL) ALL
    ```
* Below this line, copy the format you see here, changing only the word 
"root" to reference the new user that you would like to give sudo privileges to:
    ```commandline
    root    ALL=(ALL:ALL) ALL
    newuser ALL=(ALL:ALL) ALL
    ```
* Sign In to user `grader`
    ```commandline
    sudo su - grader
    ```
* Sign Out of user `grader`
    ```commandline
    exit
    ```

#### STEP 8 : Create an SSH key pair for `grader` using the `ssh-keygen` tool
Enable Key Based Authentication
* Generate SSH Key Pairs locally on your system using application 
`ssh-keygen`, type:
    ```commandline
    ssh-keygen
    ```
* Enter file in which to save the key (/home/user/
.ssh/id_rsa):
    ```commandline
    /home/user/.ssh/graderKeyPair
    ```
* Two Files will be created inside `~/.ssh/` i.e `graderKeyPair` `graderKeyPair.pub`

* Place the Public Key on our remote server so that SSH can use it to log in
    1. make .ssh dir inside `/home/grader/`
        ```commandline
        sudo mkdir /home/grader/.ssh
        ```
    2. create `authorized_keys` file inside `/home/grader/.ssh`
        ```commandline
        sudo touch /home/grader/.ssh/authorized_keys
        ```
        this is a special file will store all the public keys this account 
        is allowed to use for authentication.
    3. Copy the contents of `/home/user/.ssh/graderKeyPair.pub` from the local machine 
    
    4. open `authorized_keys` file inside `/home/grader/.ssh`
        ```commandline
        sudo nano /home/grader/.ssh/authorized_keys
        ```
    
    5. paste Copied Content into `/home/grader/.ssh/authorized_keys` file
        

* Set permission of `.ssh` & `authorized_keys` so that other user can not gain access to your account
    ```commmandline
    sudo chmod 700 /home/grader/.ssh
    sudo chmod 644 /home/grader/.ssh/authorized_keys
    ```
* Changer ownership of `.ssh` & `authorized_keys` grader so that grader can gain access to these file
    ```commmandline
    sudo chmod 700 /home/grader/.ssh
    sudo chmod 644 /home/grader/.ssh/authorized_keys
    ```

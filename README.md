# Linux Server Configuration for Web App
_Configure Amazon Lightsail Linux Server To host Web App_

## SERVER INFORMATIONS

**PUBLIC IP :**  `13.232.118.216`

**INITIAL LOGIN URL FOR SHELL :** `ssh ubuntu@13.232.118.216 -i ~/.ssh/LightsailDefaultPrivateKey.pem`

**UPDATED LOGIN URL FOR SHELL AFTER STEP 4 :** `ssh ubuntu@13.232.118.216 -i ~/.ssh/LightsailDefaultPrivateKey.pem -p 2200`

## PROCEDURE

### _Getting Your Server________________________

### STEP 1 : Start a new Ubuntu Linux server instance on Amazon Lightsail
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

### STEP 2 : SSH into your server
Use these commands to SSH into your server from your console
```commandline
ssh ubuntu@13.232.118.216 -i ~/.ssh/LightsailDefaultPrivateKey.pem
```

### _Securing Your Server________________________

### STEP 3 : Update all currently installed packages
Use these commands to update installed packages
```commandline
sudo apt-get update
sudo apt-get upgrade
```
If *** System restart required *** is displayed at login, run:
```commandline
sudo reboot
```

### STEP 4 : Change the SSH port from 22 to 2200
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
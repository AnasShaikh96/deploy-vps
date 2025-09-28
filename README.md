# Deploy-VPS
This Markdown is an offshoot of Hitesh Choudhary sir's deploy to production series but, the twist is that it focuses on how to deploy on your local server/VPS.


# Prerequisites
1. An extra working machine (secondary) apart from main machine (primary), make sure the secondary machine has atleast 2 CPUs, 8GB RAM, 50Gbs Storage and a working battery.
2. Secondary machine should have Ubuntu installed.
3. Both the machines should be in the same LAN(In the same wifi).
4. If main machine is Windows then go ahead and install WSL (Windows Subsystem for Linux).
5. That's pretty much it, We're ready to rock!
   

# Secondary Machine Settings: These Steps are mandatory before Hitesh Sir's steps 
Why? Because your secondary machine currently is not allowing anyone to access to itself. Openssh-server is going to change that and you would be able to access it within the same LAN.

Why in the same LAN and not on public internet? Search for Private vs Public IPs, I promise it is a cool concept and you'll learn something new ;)  

1. Access your Secondary machine as admin.
2. Run these commands
```sudo apt update
sudo apt upgrade -y
```
3. Install OpenSSH-server and enable it
```
sudo apt install -y openssh-server
sudo systemctl enable --now ssh
```
4. Simply check the status of ssh server, if it shows live then we're good to go.
```sudo systemctl status sshd```

5. Run below command to check the ip address of your Secondary machine and copy it somewhere.
```hostname -I```


# Accessing local VPS on Main Machine. 

1. Windows : Install WSL on your machine if haven't done already.
2. open terminal on your **main machine** and run following command
```ssh root@192.ip.ip.ip```
**Note**: device name root can be different in your context.
3. If done properly a password prompt will appear. Add password and Viola, We're in!!


# Continue Hitesh Sir's series till [Nginx Configuration](https://docs.chaicode.com/youtube/chai-aur-devops/setup-nginx/).
Once you successfully setup Nginx and can see raw html output on the ip, We can move forward and try to access this html from public internet.






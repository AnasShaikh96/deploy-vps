# Deploy-VPS
This Markdown is an offshoot of Hitesh Choudhary sir's deploy to production series but, the twist is that it focuses on how to deploy on your local server/VPS.
**Note:** This guide is supposed to be a beginner's guide and a fun learning experiment, if you're someone who is looking for full fledged VPS which can serve enterprise level apps concurrently, this is not for you. 



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




# Accessing VPS from public internet.
While we have setup a VPS, we'll only ever be able to access it in the LAN due to current restraints. To fix it, We can do 3 things as of now:

1. Easiest ✅: : Use Cloudflare tunnel to expose public IP under a TCP tunnel.
2. The Dev way ✔️: Use vpn service (wireguard, openVPN, tailscale) to mask our private IP under their public IP
3. Not Recommended ❎:  Go to your router dashboard and enable port forwarding to 22, 80, 443.

## Easiest way
I'm considering you have ssh-ed your way into the VPS and using the VPS from your main machine (because that is the way to do it, think it as it is situated in antarctica).

We're going to use Cloudflared, a cloudflare service to view apps/static websites hosted on our local VPS.
**What is it?** Think of it as a middleman/connector between your private IP and Cloudflare.
**How does it works?**  It creates an outbound tunnel between your VPS's private IP and Cloudflare Edge which then exposes it's own public ip, assigns a subdomain and forwards requests back to your local VPS.
**Why does it works?** Cloudflare is doing a lot of heavy lifting here. It does proxying, encryption, routing traffic, Ddos measures etc. Learn more about them [here](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/) 

Lets get into the setup!
1. Install Cloudflared
```
wget https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb
sudo dpkg -i cloudflared-linux-amd64.deb
```

2. Boot up a Quick Tunnel (no domain, free subdomain)
```
cloudflared tunnel --url http://192.ip.ip.ip:3000
```
**Notes**: 
1. I used IP instead of localhost because of configurations, it might be different for you.
2. This command is going to assign you a temporary subdomain eg. `randomtext.trycloudflare.com`.
3. If you want a permanent running domain you can login to Cloudflare Zero Trust Dashboard where you have to add your **Domain** and setup proper tunnels via dashboard. [Read More](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/get-started/create-remote-tunnel/)


## The Dev way
WIP




## Not Recommended
You can manually access your router's Admin panel and enable port forwarding to your required ports. This is not recommended because of the security concerns it comes with. Anyone accessing this can severly attack your domain and it can have security consequences.




 

# To set up

## Links:

### Tutorials (in-order):

In: https://www.kali.org/docs/cloud/digitalocean/

1. Get the netboot ISO: http://http.kali.org/kali/dists/kali-rolling/main/installer-amd64/current/images/netboot/

2. Create your Virtual machine with the ISO, follow screenshots in part 3.

3. Follow the screenshots here to set up the manual partitions: https://medium.com/@hackthebox/how-to-deploy-a-kali-linux-distribution-in-digital-ocean-cloud-c556edf17741

4. Reboot when done

5. Follow the remaining steps to apt-update and setup for Digital Ocean. If you encounter issues with apt-update, likely that your internet is not set up. Unlike an xfce GUI, eth0 may not be properly/automatically configured to connect to ethernet. Use the following commands to check if the state is down. (https://askubuntu.com/questions/13993/how-to-connect-a-wired-internet-connection-through-terminal)
   > ip a
   > nano /etc/network/interfaces

So if your network card appears as eth0 for example then you would leave the file like this:
auto eth0  
 iface eth0 inet dhcp

> ip link set eth0 up
> /etc/init.d/networking restart
> ping 8.8.8.8

6. Continuing from step 5; to update and set up for Digital Ocean. (https://www.kali.org/docs/cloud/digitalocean/)

   > sudo su
   > apt update
   > apt full-upgrade -y
   > apt install -y cloud-init
   > echo 'datasource_list: [ ConfigDrive, DigitalOcean, NoCloud, None ]' > /etc/cloud/cloud.cfg.d/99_digitalocean.cfg
   > systemctl enable cloud-init --now
   > apt install -y openssh-server
   > systemctl enable ssh.service --now
   > passwd -d root
   > mkdir -p /root/.ssh/

7. Cleanup
   apt autoremove
   apt autoclean
   rm -rf /var/log/\*
   history -c

8. Poweroff
   poweroff

9. After setting up Digital Ocean, create a new droplet.
10. Generate the public/private key using puttygen to get the pkt file. Copy the public key to Digital Ocean.

11. Find the .vmdk file, upload to Digital Ocean. Wait for 'pending' to finish. Proceed with starting a droplet once it's done. I used:

    > Droplet Type > Basic
    > CPU options > Regular with ssd > $48/mo. 8GB/4CPUs, 160GB SSD, 5TB transfer.
    > Extra options > Enable Monitoring

12. Set up putty config. IP address + pkt file (ssh>auth>browse)
13. SSH into Digital Ocean Kali
14. Get the tools using metapackages.
    > https://www.kali.org/docs/general-use/metapackages/ > https://www.kali.org/tools/kali-meta/#kali-linux-headless
    > I chose to use 'apt install kali-linux-headless'.

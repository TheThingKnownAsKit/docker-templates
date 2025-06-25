# Downloads
Download Docker for ARM64 on Linux: https://learn.arm.com/install-guides/docker/docker-desktop-arm-linux/ 
After following those instructions you also have to run `systemctl --user enable docker-desktop` to get it to work

Sometimes it can be hard to find the right place to download VSCode for ARM64, so here's the link: https://code.visualstudio.com/download

Open VSCode and install the Remote Development extension pack from Microsoft

# Post Installation Docker Setup
Copy the .devcontainer folder into the top level of your repository

Use the search feature to find any instance of PROJECT_NAME or CHANGEME and switch them. Replace all is fine

Ctrl(or CMD) + Shift + P to open commands, then select Dev Containers: Rebuild and Reopen in Container

It should build correctly though it might take FOREVER to build it for the first time. Open the Dev Container logs (there will be a button prompt) and just make sure it isn't stuck on one thing forever

# Host Machine Setup
Some things need to be configured on the host machine to give Docker access to things like cameras or serial devices

This was written on an Ubuntu based system

To give Docker access to your network, open a new terminal and run: `xhost +local:`

Docker also can’t get through your firewall right now (assuming you have one), so run the below commands

one at a time in the host computers terminal
```
sudo ufw allow in  proto udp to   224.0.0.0/4
sudo ufw allow out proto udp from 224.0.0.0/4
sudo ufw reload 
```
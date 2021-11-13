# Outlines the steps needed to set up an ubuntu machine (/ VM in virtualbox)

Download Ubuntu .iso file and move it to a known location
Create virtual box VM with at least 1GB memory (or better: 1/4 of the total host machine RAM)
Note: to host AIRFLOW on Ubuntu, you need to set at least 8GB of RAM.
Use dynamic storage (set maximum at least 10GB)
In settings, on storage tab, change the Controller IDE, adding the .iso file from the folder it's stored.
Start the VM and create admin user. open the terminal.

Update package lists, then upgrade any packages that need it:  
`sudo apt update` `sudo apt list --upgradable` `sudo apt upgrade`

Install zsh improved shell for terminal, this will close the terminal:  
`sudo apt install zsh && chsh -s $(which zsh) && exit` 

From a new terminal, install a few more packages needed for repository connection, cloud services, and terminal file editing:  
`sudo apt install git-all, awscli, vim`

Set Up SSH, so you can connect to this VM from your local machines command line:  
- Install SSH server on the VM: `sudo apt update && sudo apt install openssh-server`
- Get the VMs IP address: `hostname -I`
- Enable SSH from VirtualBox: Right-click on the VM, settings, networking tab, adv settings, port forwarding, fill out: SSH TCP 2200 <vm_ip_address_here eg 10.0.2.15> 22
- Test the SSH connection: from your local/host machine: `ssh -p 2200 <yourusername>@localhost`

Now you can shutdown the VM, and restart it as "headless" (no UI), because you will connect from the terminal via ssh from now on.

Create a shortcut for the ssh connection:
1. Add the connection to your ssh config file: `vi ~/.ssh/config` then type:
```
Host vm_ubuntu
  HostName localhost
  Port 2200
  User <yourusername> eg george
```
2. Close the file with `:wq` then close the terminal windown and open a new one.
3. Test the new alias, connecting using: `ssh vm_ubuntu`

Now set up the colour theme, highlighting and plugins that will make life in the terminal far easier:
1. Inside the VM, install "oh-my-zsh" following [steps](https://github.com/ohmyzsh/ohmyzsh) on GitHub:  
`sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"`  
2. On your machine (not the VM), install the [powerline](https://github.com/powerline/fonts) fonts + set up your terminal to use one of the "Meslo" fonts.
3. Install [powerline10k](https://github.com/romkatv/powerlevel10k#oh-my-zsh) theme:  `git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k`
4. Set ZSH_THEME in .zshrc config file to the new theme:  `sed -i 's/^ZSH_THEME=/ZSH_THEME="powerlevel10k\/powerlevel10k" #default ZSH_THEME=/g' ~/.zshrc`
5. Download custom [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting/blob/master/INSTALL.md) AND [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions/blob/master/INSTALL.md) plugins:  `git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting` and `git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions`
6. Set all the plugins:  `sed -i 's/^plugins/plugins=(git aws compleat systemadmin zsh-autosuggestions zsh-syntax-highlighting) #default plugins/1' ~/.zshrc`

TO DO : 
- installing docker + rootless/best practice
- cronjob on docker and processes etc
- postgres with docker + health checks
- airflow+ celery checks etc + UI up and down + memory allocation


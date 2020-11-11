![SSHing in](https://github.com/zperk028/100DaysofCloud/blob/main/Journey/010/sshsuccess.JPG)

# Day Ten 

## MS learning path: [AZ-104: Deploy and manage Azure compute resources](https://docs.microsoft.com/en-us/learn/paths/az-104-manage-compute-resources/) 

*Jumping platforms from Scott Duffy's Udemy course over to MS Learn, and jumping topics from resource monitoring to compute resources, I'm scratching the itch to dive ahead and go a little deeper on VMs today to keep things interesting.*

### Module 1: [Create a Linux virtual machine in Azure](https://docs.microsoft.com/en-us/learn/modules/create-linux-virtual-machine-in-azure/) 

Alright! We're gonna create a Linux VM, SSH into it, and then poke around to install software and change the network configuration. Fun stuff! 

- When creating a Linux VM, the following resources also must be created (or already exist) in an RG:
1. The VM (CPU and memory)
2. An AZ Storage account (that holds the virtual hard drives used by the VM)
3. The virtual HD (VHD) itself that will contain the OS, apps and data used by the VM
4. Vnet, to connect the VM to other AZ services/on-prem networks
5. Network Interface (NIC) to connect the VM to the Vnet
6. An optional public IP address for remote access to the VM
- By default, a VM will deploy with two VHDs:
1. One for the OS itself (the operating system disk). This will be the VM's primary drive, and a Linux deployment will be labeled as `/dev/sda` by default. 
2. A Temp disk for temporary storage for the OS and any apps. On Linux deployment it will be labeled as `dev/sdb` by default, and formatted and mount to `/mnt` by the Azure Linux Agent (wtf does this mean??) The temporary disk is not persistent. You should only write data to this disk that is not critical to the system.
- Managed v Unmanaged Disks: Unmanaged disks make the management of storage accounts used for VHDs the responsibility of the user, and there is a limit to the equivalent of 40 fully throttling HDs per storage account. Scaling out past that requires the creation of additional storage accounts to hold more VHDs for use with the VM we're creating. Managed disks are the "preferred" (business term) model, and can autoscale to the users needs.

#### Configuring SSH for our Linux VM
- SecureShell (SSH) is an encryption connection protocol used for secure sign-ins over unsecured connections.
- Authentication methods: Uname and PW vs SSH Key Pair.  Uname and PW leaves the VM open to potentially brute forcing the PW. SSH key pairs, aka: public-private key pairs, are more secure, but best if you only plan to access the VM from a few devices. If you need unfettered access to the VM from any device, a uname/pw is best.
- Two Parts to the SSH Key Pair:
- Public Key, which is placed on the Linux VM, and can be shared with anyone.
- Private Key, which is what you present to the VM to verify your identity.
- Creating the SSH Key Pair on Linux, Win10 and OSX, use the built in command `ssh-keygen`. 
- For the exercise, we use the bash command `ssh-keygen -t rsa -b 4096`, the "minimum command necessary" to generate an SSH key pair in Azure. This command line generates a SSH protocol 2 (SSH-2) RSA public-private key pair with a length of 4096. This then creates two files, `id_rsa` and `id_rsa.pub` which we place in the default `~/.ssh` directory, and associate a password to the key pair, which then encryptes the key itself with 128-bit AES. This then spews out a SHA256 'key fingerprint' and some pretty rad randomart for the key. 

![SSH keygen in sandbox](https://github.com/zperk028/100DaysofCloud/blob/main/Journey/010/ssh.JPG)

*Since this is a sandbox exercise in a temporary environment that exists outside of my actual Azure directory, I'm less than concerned about providing a screencap here.*

- With the SSH key pair generated, we then have to configure the VM with it. This can be done during the VM's creation or after. 
- We then have to `cat` (we're in bash, despite my declaration to learn more PS, but that's what the module calls for) the `.pub` file of the key pair and copy of the output value. This output is the value of the public key in the key pair, and it's what we'll use to set up the VM for SSH. 
- If we already had a VM created (we don't because this is a fresh sandbox) we could then install the public key in CLI using `ssh-copy-id`. The complete command would be `ssh-copy-id -i ~/.ssh/id_rsa.pub myusername@VMname` which should then be followed by a prompt for the key passphrase. 

#### Creating a Linux VM in AZ Portal

- We start (in the sandbox via the portal) by grabbing a Ubuntu Server 18.04 LTS Canonical from the marketplace and hitting create to start some basic configuration, which is all dictated by the module. 
- One configuration point of note is the "best practices naming convention" of the VM resource. `test-web-eus-vm1` for the environment (test), the role (web), location (east US), service (VM) and instance number (1). Fun stuff. 
- Getting down to business. In the Administrator Account section of the setup, we opt for the SSH public key source to be 'Use existing public key' instead of the default 'Generate new key pair' or the alternate 'Use existing key stored in Azure'.  With this set, we then paste in the `cat` output of the public key file from the last section. 
- Lastly, we ensure that 'Allow Selected Ports' option is set, and specify Port 22 (SSH). We run through a few more defaults, and away we deploy! 

#### Connect to the VM with SSH
- First we'll need the public IP for the Linux VM we created in the sandbox. We head to the VM's blade in the portal to find this. In this (sandbox) case, it's 52.255.184.219. 
- From the MS Learning Path's sandbox CLI Cloud Shell (where the key pairs exist, which I confirm by `cd ~/.ssh` and `ls`), we start the connection using the command `ssh azureuserzperk@52.255.184.219` (with my completely legit and not at all lame temporary sandbox uname). 
- Because this is the first connecting, SSH spits an output challenging the authenticity of the host, ans asks if we want to continue connecting and permanently add my current IP to the list of known host.
- At this point it *should* prompt be for the private key passphrase, but it does not... interesting...
- Regardless, we're SSH'd into the VM now. Running a few basic bash commands confirms this, with `ls -la` showing the contents of the home directory and `ps -l` bringing up the VM's current processes! 


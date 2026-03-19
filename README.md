# GCloud VM with VSCode
This guide walks you through setting up a virtual machine on Google Cloud and connecting it to Visual Studio Code using Remote SSH. It covers everything from selecting a project and configuring a VM instance to generating SSH keys, connecting securely, and transferring files.

The goal is to provide a simple, end-to-end workflow so you can quickly spin up a cloud VM and start developing or running workloads (like GPU training) directly from VSCode using this repo (`https://github.com/Pixels2bytes/VM_Setup.git`).

## Getting Started
- [Check Credentials](#check-credentials)
- [Setup VM Instance in GCloud](#setup-vm-instance-in-gcloud)
- [Setup SSH Configuration File](#setup-ssh-configuration-file)
- [Link VSCode to VM](#link-vscode-to-vm)
- [Run VM](#run-vm)
- [[BONUS] Transfer Files From Local Computer to VM](#bonus-transfer-files-from-local-computer-to-vm)
- [Helpful Links](#helpful-links)

Go to Google Cloud > Select Project Picker (`Left-hand corner next to the GCloud Title`) > Find Project:
<img src="docs/tuto-gcloud-project-select.png" width="600">
```
https://console.cloud.google.com/
```

## Check Credentials
- Get your username and ensure your role is Editor

<img src="docs/tuto-gcloud-check-permissions.png" width="600">

## Setup VM Instance in GCloud
- Go to Compute Engine > VM Instances

<img src="docs/tuto-vm-instances.png" width="400">

- Select `Create Instance` from VM Instances Dashboard

You will be taken to Machine Configuration. Name your virtual machine `project-name-gpu-vm`.Ten select the machine type fit for your project. If you are unsure, use the Gemini AI Agent to help select the best fit for you. 

Once completed, ensure the VM status is `ON` then copy the `External.IP.Address`. If the VM is not on, navigate to the hamburger (`...`) symbol and select `Run/Resume`. The External IP should show.

## Setup SSH Configuration File
- Open VSCode and clone VM_Setup: git clone 
- Download Extension - Remote SSH
- Command Palette > Remote-SSH: Open SSH Configuration File...

```
# VM Template [Reason - $Price/hr]
Host project-name-remote-ssh
HostName External.IP.Address
User your_username_from_GC_IAM
IdentityFile C:\Users\Username\Desktop\VM_SETUP\storekeys\project-name-remote-ssh
```

- Generate SSH keygen: In VSCode go to Terminal > New Terminal and type:

```ssh-keygen -t rsa -f project-name-remote-ssh -C your_username_from_GC_IAM -b 2048```

*PRESS* Enter, Enter

<img src="docs/tuto-vmkey-setup.png" width ="600">

- Paste the private file `project-name-remote-ssh` location in SSH Configuration IdentityFile
- Open the public file `project-name-remote-ssh.pub` and copy the key + username

## Link VSCode to VM
Go to the VM Instances Dashboard > Select the VM

<img src="docs/tuto-create-vm-instance.png" width="600">

- Go to Edit > SSH Keys section
- Press `Add Item` and paste the key + username in the SSH Public Key field then save

<img src="docs/tuto-ssh-demo-paste-public-key.png" width="600">

## Run VM
- Command Palette > Remote-SSH: Connect to Host... > Select Host Project `project-name-remote-ssh`

## [BONUS] Transfer Files From Local Computer to VM
Outside of VSCode, open Powershell and type:

```
scp -i "C:/Users/Username/Project/Location/Of/StoreKeys/PRIVATE-SSH-FILE-Here-ssh" -r "C:/Users/Username/Location/Of/File/Folder/To/Copy/From/Here" username@ExternalIP:/home/username/Folder/To/Copy/To/Here
```

## Helpful Links
- *YOUTUBE VIDEO:* SSH into Remote VM with VS Code | Tunneling into any cloud | GCP Demo:
https://www.youtube.com/watch?v=0Bjx3Ra8PRM
- *ARTICLE:* Getting Started - First-Time Git Setup: https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup
- *ARTICLE:* Boot Disk Storage Increase: https://docs.cloud.google.com/compute/docs/disks/resize-persistent-disk
- *ARTICLE:* Kernal Crashes: https://github.com/microsoft/vscode-jupyter/wiki/Kernel-crashes

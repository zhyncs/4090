# Hardware

ROG 7 Plus RTX4090

# Software

## Install NVIDIA Driver
```
# https://www.nvidia.com/download/driverResults.aspx/211709/en-us/
# 537.42
```

## Enable Windows Subsystem Linux
```powershell
# https://learn.microsoft.com/en-us/windows/wsl/install-manual
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

## Set Version
```powershell
# https://learn.microsoft.com/en-us/windows/wsl/basic-commands
wsl --set-default-version 2
```

## Install Ubuntu 22.04 Server
```powershell
# https://canonical-ubuntu-wsl.readthedocs-hosted.com/en/latest/guides/install-ubuntu-wsl2/
wsl --install -d Ubuntu-22.04
```

## Enable Hyper V on Windows Home Edition
```powershell
pushd "%~dp0"
dir /b %SystemRoot%\servicing\Packages\*Hyper-V*.mum >hyper-v.txt
for /f %%i in ('findstr /i . hyper-v.txt 2^>nul') do dism /online /norestart /add-package:"%SystemRoot%\servicing\Packages\%%i"
del hyper-v.txt
Dism /online /enable-feature /featurename:Microsoft-Hyper-V-All /LimitAccess /ALL

# https://learn.microsoft.com/en-us/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v#enable-hyper-v-using-powershell
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All
```

## Setup networkingMode for WSL2
```
# Create a virtual switch (external, name Bridge)
# https://learn.microsoft.com/en-us/windows-server/virtualization/hyper-v/get-started/create-a-virtual-switch-for-hyper-v-virtual-machines?tabs=powershell#create-a-virtual-switch

# C:\Users\username\.wslconfig

[wsl2]
networkingMode = bridged
vmSwitch = Bridge
```

## Setup OpenSSH
```
# https://learn.microsoft.com/en-us/windows-server/administration/openssh/openssh_install_firstuse?tabs=powershell#install-openssh-for-windows

# set up authorized_keys
$HOME/.ssh/authorized_keys

# set up /etc/ssh/sshd_config
Port 8413
PermitRootLogin yes
ListenAddress 0.0.0.0
PasswordAuthentication yes
```

## Install Docker on Ubuntu 22.04
```bash
# https://docs.docker.com/engine/install/ubuntu/

# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

## Install CUDA Toolkit on Ubuntu 22.04
```bash
# https://developer.nvidia.com/cuda-12-2-0-download-archive?target_os=Linux&target_arch=x86_64&Distribution=WSL-Ubuntu&target_version=2.0&target_type=runfile_local
wget https://developer.download.nvidia.com/compute/cuda/12.2.0/local_installers/cuda_12.2.0_535.54.03_linux.run
sudo sh cuda_12.2.0_535.54.03_linux.run
```

## Setup Proxy
```bash
# Use Surge
export https_proxy=http://192.168.124.14:6152;export http_proxy=http://192.168.124.14:6152;export all_proxy=socks5://192.168.124.14:6153

# Docker
# https://docs.docker.com/network/proxy/
# ~/.docker/config.json
{
 "proxies": {
   "default": {
     "httpProxy": "http://192.168.124.14:6152",
     "httpsProxy": "http://192.168.124.14:6152"
   }
 }
}
```

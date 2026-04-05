# Install-Docker-Desktop-in-Ubuntu-22.04
For OS161

Step 1: Check your system (IMPORTANT)
Docker Desktop needs virtualization (KVM).
Run:
egrep -c '(vmx|svm)' /proc/cpuinfo
If result is:
0 =>virtualization NOT supported
>0 =>good to go

Step 2: Enable KVM
Load modules:
sudo modprobe kvm
sudo modprobe kvm_intel   

Verify:
lsmod | grep kvm
You should see kvm_intel or kvm_amd

Fix permission:
sudo usermod -aG kvm $USER

Then logout + login again

You must log out of the whole system session

Lazy option (also works)
If you don’t mind:

sudo reboot

Restarting your PC does the same thing
Once you log back in, your kvm permission will work properly.

If you want, after login you can run:
groups

You should see:
... kvm ...
If kvm is there → perfect

Step 3: Install dependencies
sudo apt update
sudo apt install ca-certificates curl gnupg gnome-terminal

Step 4: Add Docker repository
run
sudo install -m 0755 -d /etc/apt/keyrings

run
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

run
echo \
"deb [arch=amd64 signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
$(lsb_release -cs) stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

run
sudo apt update

Step 5: Download Docker Desktop
wget https://desktop.docker.com/linux/main/amd64/docker-desktop-amd64.deb

Step 6: Install
sudo apt install ./docker-desktop-amd64.deb

If you see this warning:
Download is performed unsandboxed as root...
Ignore it (normal)

Step 7: Start Docker Desktop
systemctl --user start docker-desktop

OR open from app menu:
Activities → Docker Desktop
Accept terms → it will start

Step 8: Enable auto-start (optional)
systemctl --user enable docker-desktop

Step 9: Verify
docker --version
docker compose version



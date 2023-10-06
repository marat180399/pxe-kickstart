# pxe-kickstart
Here we have 2 VMs on KVM.
<br />1st one is PXE server with DHCP, FTFP and HTTP daemon installed. 
<br />2nd one is VM created by virt-install command with following getting IP and URL for automatic OS installtion.

<img src="https://raw.githubusercontent.com/marat180399/pxe-kickstart/main/schema.svg" width="100%">

1) Prepare internal network with internal.xml by performing
   <b>virsh net-create --file internal.xml</b>
2) Run xming on your workstation, run putty, allow X11 forwarding there and connect to your KVM HOST. Run virt-manager, x11 window with VM manager will be displayed on your workstation.
3) On LOCALHOST create a guest VM (this will be your PXE server) and connect it to your newly created network.
4) Deploy a PXE server on KVM-based VM with REDOS 7.3.2 OS as per instruction & video below:
   <br />a) https://redos.red-soft.ru/base/server-configuring/other-utilites/setting-server-pxe/pxe-redos-uefi-or-legacy/
   <br />b) https://www.youtube.com/watch?v=BTo-9lw5QXo&feature=youtu.be
6) Install ansible on PXE server.
7) Change mod to executable for <b>armConfigInfo</b> bash script and run it. Answer all it's questions. This script puts users variables (http address, admin name, passwords, selected version) into <b>roles/pxeOsInstall/vars/main.yml</b>
8) Run "<b>ansible-playbook playbook</b>" for generating ks.cfg config file and pushing it into local tftp server directory.
9) On LOCALHOST create another guest VM with following command: <b>virt-install --name redos-arm --memory 4096 --vcpus 2 --disk size=30 --network network=internal --pxe --os-variant rhel7.7</b>
10) Newly created VM will be connected to internal network and get IP from DHCPD with other configurations. REDOS 7.3.2 will be automatically installed in a time.


P.S. I know that using bash for generating a file containing variables for ansible is not a quite good solution. I could use only bash for this simple purpose, but the main reason I did that is just to remember how to use ansible:)

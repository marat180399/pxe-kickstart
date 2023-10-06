# pxe-kickstart
Here we have 2 VMs on KVM.
1st one is PXE server with DHCP, FTFP and HTTP daemon installed. 
2nd one is VM created by virt-install command with following getting IP and URL for automatic OS installtion.

<img src="https://raw.githubusercontent.com/marat180399/pxe-kickstart/main/schema.svg" width="100%">

1) Prepare internal network with internal.xml by performing
   virsh net-create --file internal.xml
2) Run xming on your workstation, run putty, allow X11 forwarding there and connect to your KVM HOST. Run virt-manager, x11 window with VM manager will be displayed on your workstation.
3) Create a VM and connect it to your newly created network.
4) Deploy a PXE server on KVM-based VM with REDOS 7.3.2 OS as per instruction below: https://redos.red-soft.ru/base/server-configuring/other-utilites/setting-server-pxe/pxe-redos-uefi-or-legacy/
5) Install ansible.
6) Change mod to +x of armConfigInfo bash script and run it. Answer all it's questions.
7) Run "ansible-playbook playbook" for generating ks.cfg config file and pushing it into tftp server.
8) On host create another VM with following command: virt-install --name redos-arm --memory 4096 --vcpus 2 --disk size=30 --network network=internal --pxe --os-variant rhel7.7
9) Newly created VM will be connected to internal network and get IP from DHCPD with other configurations. REDOS 7.3.2 will be automatically installed in a time.


P.S. I know that using bash for generating a file containing variables for ansible is not a quite good solution. I could do use only bash for this simple purpose, but the main reason I did that is just to remember how to use ansible:)

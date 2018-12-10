# Is Dockerless world a reality ?
Workshop on 15-December with Prathidhwani


## Prerequisites
- Laptop should be capable enough to run atleast one Fedora/Centos 7 VM instance.
    - 2 GB of RAM should be allocated for the VM
    - 1 vCPU should be allocated for the VM
    - Atleast 7GB of free space should be available on the partition where the VM image is downloaded or resides. 
- Laptop should have Hypervisors installed Like VirtualBox,HyperV, KVM or Vmware Workstation
- Download the prebuilt image or download Fedora/Centos cloud image or install the required packages in your existing Virtual  
  machine. Choose one of the below methods
    
  
  
  [Prebuilt Image Downloads](#Prebuilt Image downloads) 
  
  
  

## Prebuilt Image downloads

Image format| OS  | hypervisor | Image download Link
------------| ----|------------|-------
Qcow2| Linux | KVM| https://cloud.debian.org/images/cloud/OpenStack/9.5.6-20181013/debian-9.5.6-20181013-openstack-amd64.qcow2
VDI|Windows|VirtualBox|https://goo.gl/g9TxN7
VDI|Linux|VirtualBox|https://goo.gl/g9TxN7

## Package installation

~~~
yum install podman buildah docker skopeo


## Steps for Linux distribution configured with KVM

- Create Cloud-init ISO file

You can use a pre-built cloud-init iso image by clicking [here](https://github.com/ranjithrajaram/debutsav/blob/master/debiancloudinit.iso?raw=true) . Password for the `debian` user will be set as `passw0rd`. 

Create a directory and two files should be created with the following contents. For example, path shown here is `/vm`. Replace the path as per your system configuration

        mkdir /vm/debustav
        cd /vm/debutsav

The first file should be named as `user-data`. Include the following contents. Note password for the debian user is set as `passw0rd`. Change the password as required.

~~~
#cloud-config
password: passw0rd
chpasswd: { expire: False }
ssh_pwauth: True
~~~

Create another file called `meta-data`. Include the following contents. Hostname of the VM is set as `debian-node-0`

~~~
instance-id: debiannode0
local-hostname: debian-node-0
~~~
- Create an ISO image using the two files. Execute the following command. This will create an iso image called `debiancloudinit.iso`. `genisoimage` package should be installed on the system.
~~~
genisoimage -output debiancloudinit.iso -volid cidata -joliet -rock user-data meta-data
~~~
- Create the VM instance using command line tool. Package `virt-install` should be installed on the node. For GUI steps, refer the youtube video. In the below command, modify the path mentioned after `--disk`. You have to modify two paths.
~~~
virt-install --import --name debianvm --memory 1024 --vcpus 1 --disk /vm/new-debian-9.5.6-20181013-openstack-amd64.qcow2,format=qcow2,bus=virtio --disk /vm/debutsav/debiancloudinit.iso,device=cdrom --network bridge=virbr0,model=virtio  --noautoconsole
~~~
- Execute `virt-manager` to access the VM or `virsh console debianvm`. In the virsh command, `debianvm` was the name of the VM that was created using the `virt-install` command. To exit from `virsh command`, use `CTRL+]`
- To login to the virtual machine, use the following credentials
~~~
username: debian
password: passw0rd
~~~

Note incase if you have mentioned a different password in user-data file, then use the same one. `admin` user has sudo access.

## Steps for Windows laptops configured with VirtualBox hypervisor

- Step 1: Download the pre-built cloud-init iso image by clicking [here](https://github.com/ranjithrajaram/debutsav/blob/master/debiancloudinit.iso?raw=true)

- Step 2: Download the VirtualBox Image by clicking on the following [link](https://goo.gl/g9TxN7)

- Step 3: Go to the VirtualBox GUI and create the debian virtual image. Do not start the virtual Image until the cloud init iso is attached.



- Step 4: Attach cloud init iso image


- Step4: Login to the VM using the follwing credentials
~~~
username: debian
password: passw0rd
~~~

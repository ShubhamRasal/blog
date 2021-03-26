---
title: "Create network setup using subnetting."
toc: true
toc_label: "Content"
tags:
  - Linux 
  - Networking
  - Arth-Tasks
date: March 06, 2021
categories: Linux
header:
  image: /assets/images/thumbnails/hadoop-lvm.png
  overlay_image: /assets/images/thumbnails/linux.png
  teaser: "/assets/images/arth-task-14//b553c63c2b2348158a3f6ee2788cd309.png"
permalink: /:categories/:title/
excerpt: "Create a network Topology Setup in such a way so that System A can ping to two Systems System B and System C but both these systems should not be pinging each other without using any security rule e.g firewall etc."
comments: true
---
Linux Networking

In this tutorial, we are going to create above setup that is mentioned in title.

![Green Round Hot Neons Gradients (Cont.) Pitch Deck Presentation (3).png]({{ site.url }}{{ site.baseurl }}/assets/images/arth-task-14//b553c63c2b2348158a3f6ee2788cd309.png)

I have provided a step-by-step guide to create this environment using Virutal Box. You can do in anywhere but we all the 3 machines need to be in same subnet.

1.  ## Setup Host-only-adapter
    

**virtual Box > File  Menu > Host Network Manager**

**![01516e02b9ecb30a674994c0d55e98af.png]({{ site.url }}{{ site.baseurl }}/assets/images/arth-task-14/6f6ee1525a2b41f79b627ccbfcd6dd91.png)**

**Create new adapter**

**![6dffa8622cb0444ea7136a4779b9d11b.png]({{ site.url }}{{ site.baseurl }}/assets/images/arth-task-14/d4ee0a7ee7ac4baf8b7c8a82134d8fd0.png)**

**Select Configure Adapter Manually**

Add the IPv4 address: 192.168.58.1

ipv4 network mask 255.255.255.0

uncheck the DHCP server checkbox

![754eb2c324ec9b76918ca2469f520d15.png]({{ site.url }}{{ site.baseurl }}/assets/images/arth-task-14/68ed4990d63c4b2dbabf3c374bcebbdd.png)

click Apply and close.

* * *

## 2\. Launch the VM's and attach above created network adapter

![4064b6b9b999a26fe1a08a90f99e452e.png]({{ site.url }}{{ site.baseurl }}/assets/images/arth-task-14/afcdf7d0c800482b927eb16e792acdac.png)

Also refresh the Mac Address

Do the same setup for all 3 VM's that we want to use in this tutorial.

* * *

Now, We have launched 3 VM's under 1 switch,

So to achieve given problem statement, we are going to use subnetting concept.

But first understand what is Subnetting?

Subnetting: It is the process of dividing the network into smaller sections. This can be useful for different purposes and helps isolate the groups of hosts together and deal with them easily.

By default, each network has only one subnet, which contains all the host address defined within.

A netmask is basically a specification of the amount of address bits that are used for the network portion. A subnet mask is another netmask within used to further divide the network.

We divide the larger network into subnetworks. In our case we have larger network 192.168.58.0/24. (1 Network and 256 hostss)

We took above network and divide them into multiple networks.

let's understand using below table how we can do this.

|     |     |     |     |
| --- | --- | --- | --- |
| 192.168.58.0/24 | 11111111.11111111.11111111.00000000 | (255.255.255.0) | 1 network of 256 hosts |
| 192.168.58.0/25 | 11111111.11111111.11111111.10000000 | (255.255.255.128) | 2 subnets of 128 host |
| 192.168.58.0/26 | 11111111.11111111.11111111.11000000 | (255.255.255.192) | 4 subnets of 64 hosts |
| 192.168.58.0/27 | 11111111.11111111.11111111.11100000 | (255.255.255.224) | 8 subnets of 32 hosts |
| 192.168.58.0/28 | 11111111.11111111.11111111.11110000 | (255.255.255.240) | 16 subnets of 16 hosts |

and so on...

Subnetting divides one network into smaller networks

```text
11111111   .   11111111    .     11111111    . 11110000
    N              N	             N          SN   H
```

N =  Network

SN =  SubNetwork

H = Hosts

Here, In our case we have 3 systems, so we will launch this 3 machines in different subnets.

And I am going to divide network into 4 subnets and from them, will use first 3 subnets.

so we will assign System A into first subnet, System B into second subnet and System C into subnet 3

- 4 Subnets of 64 hosts(minus 2 for network and brodcast addresses)
    - 192.168.58.0-63/26           255.255.255.192
    - 192.168.58.64-127/26       255.255.255.192
    - 192.168.58.128-191/26     255.255.255.192
    - 192.168.58.192-255/26     255.255.255.192

So let's start doing this setup.

## Step 3: System configurations

### System A

Update the IP address and Add System A into first subnet. It will update netmask and broadcast IP address

`$ ifconfig enp0s3 192.168.58.2 netmask 255.255.255.192`

![d9bdc7135fc5ae2cc72cde6292f606f1.png]({{ site.url }}{{ site.baseurl }}/assets/images/arth-task-14/01cc8705f907438b9c6fe2c37bc0621b.png)

* * *

### System B

Update the IP address and Add System B into second subnet. It will update netmask and broadcast IP address

`$ ifconfig enp0s3 192.168.58.65 netmask 255.255.255.192`

![03fb84c348e8991ab75eb5a4644d36f2.png]({{ site.url }}{{ site.baseurl }}/assets/images/arth-task-14/c6db502f583a4d23b1cbdfee48510cdf.png)

* * *

### System B

Update the IP address and Add System C into third subnet. It will update netmask and broadcast IP address

`$ ifconfig enp0s3 192.168.58.129 netmask 255.255.255.192`

![b69114dd81a9ddb6c4901b50a6cab117.png]({{ site.url }}{{ site.baseurl }}/assets/images/arth-task-14/ec614f4102864edcbe0b63ac550b6924.png)

* * *

Till, now we have created and launched 3 systems in different subnets. They can connect with the only their subnet. But, in our case, we want connectivity between system A to System B and System C. and Vise versa.

### System A

To do this, we have to add route in System A so it can connect to subnet 2 and subnet 3

We have only default route only for within the subnet, so it will not create packets for subnet2 and subnet 3 ad gives error as network unreachable.

![0288e8585af9df4f8274e5f6caf876dd.png]({{ site.url }}{{ site.baseurl }}/assets/images/arth-task-14/9c9d266b23bf4be5bc06b890c3ed5c4f.png)

Now you can see here, Ping command does not received any packets from System B and System C. For that we have to add route in System B and System C so that this systems can ping back to System A.

### System B

![70bcd6faba01e10a594acb894c66aab4.png]({{ site.url }}{{ site.baseurl }}/assets/images/arth-task-14/44374b0d1f584da88c6bce61211cd602.png)

![fd5fda643c805958c7ea3b0b0d702033.png]({{ site.url }}{{ site.baseurl }}/assets/images/arth-task-14/818e8708a2804c3582425fe78a30a0b8.png)

System C

Now, we  have to do the same setup for System C

![2a214a0b52089ed3c8549b1834fde58e.png]({{ site.url }}{{ site.baseurl }}/assets/images/arth-task-14/82568343b804415e8a9b94dded7c8d9c.png)

![d31120bdbf4943765aaed4d3604a43bb.png]({{ site.url }}{{ site.baseurl }}/assets/images/arth-task-14/b23732f6901a4b7ab02bef0c96f10029.png)

### System A

Test that, we are able to ping System B and System C

![dc39144320b4b9191efe6611c1f7026e.png]({{ site.url }}{{ site.baseurl }}/assets/images/arth-task-14/391cb699964d411d82e04521f0d3e307.png)

## Conclusion

We have successfully created a Network Topology Setup in such a way so that System A can ping to System B,but System B and System C are not able to ping each other using subnetting.

Completed Arth Task 14.1

# Thank you…

*About the writer:*

*Shubham is DevOps Engineer. Shubham has hands-on experience in implementing automation, CI/CD pipelines and DevOps processes. He loves to write blogs on developers’ technical issues in day-to-day work and guide them through his tech blogs.*

Resources:

- [Learning Subnetting](https://www.youtube.com/watch?v=fTTfbxrYRE4)
- [How to set host-only adapter on VM](https://dev.to/isabolic99/how-to-set-host-only-adapter-on-vm-virtual-box-2jka)
- [Understanding IP Addresses, Subnets, and CIDR Notation for Networking](/F:/NOTES/Joplin/resources/app.asar/Understanding%20IP%20Addresses,%20Subnets,%20and%20CIDR%20Notation%20for%20Networking)
- [Specifying a subnet mask for a network interface](https://library.netapp.com/ecmdocs/ECMP1155586/html/GUID-B02ACB37-C1CD-44E7-9AF7-ABB50F2E42BB.html)
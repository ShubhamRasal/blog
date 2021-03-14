---
title: "Integrating LVM with Hadoop and providing Elasticity to DataNode Storage"
toc: true
toc_label: "Content"
tags:
  - Linux 
  - LVM
  - Arth-Tasks
date: March 1, 2021
categories: Linux
header:
  image: /assets/images/thumbnails/hadoop-lvm.png
  overlay_image: /assets/images/thumbnails/linux.png
  teaser: "/assets/images/arth-task-7.1/hadoop-lvm-teaser.png"
permalink: /:categories/:title/
excerpt: "We will make datanode contributing storage more elastic with help of LVM"
comments: true
---
## Pre-requisites: 

I have assumed that you had installed hadoop before. if not please you can refer the below blog for installing hadoop cluster using ansible playbook.

[Install Hadoop using Ansible](https://developer-shubham-rasal.medium.com/how-to-configure-hadoop-cluster-using-ansible-58d942c59ac0)

Many time we want to attach more or decrease the storage that our datanode is contributing to hadoop cluster.

In this case we can take help of Linux LVM concept.

We will solve above tasks step by step. so lets start.

## 🔅Integrating LVM with Hadoop and providing Elasticity to Data Node Storage

In this task we have to create one logical volume and mount that volume on data node contributing directory.

### Step1: Attach the external hard disk.

Here I am using virtualbox for demo.

shutdown the os. and attach new storage.

![19e25dae0e90372770f80dc5210ddf28.png]({{ site.url }}{{ site.baseurl }}/assets/images/arth-task-7.1/a93148cf76e342b39d7c98726ccf9b40.png)

![b4ebc36f2d22ced9dca1d2caf2a4fd1c.png]({{ site.url }}{{ site.baseurl }}/assets/images/arth-task-7.1/954ad8fa15234d58b9f875ab7db6ded0.png)

![7a4da47faa4795a47e57a1af1c5ccfdf.png]({{ site.url }}{{ site.baseurl }}/assets/images/arth-task-7.1/4010e02d6cb649679678fa584095cef3.png)

![93cb677c473a0da15bc76522b3d1aa53.png]({{ site.url }}{{ site.baseurl }}/assets/images/arth-task-7.1/a68a0fd333624ab9b088e687f7cc79f4.png)

![273435bc07d4a4a43afbf0183c42a908.png]({{ site.url }}{{ site.baseurl }}/assets/images/arth-task-7.1/89719c766ace4c13bdb427ad087227a7.png)

![a2bf9607541d4b42d27a4b357e7bef95.png]({{ site.url }}{{ site.baseurl }}/assets/images/arth-task-7.1/984c266cbeb84adfb7ff2d3c5501abbf.png)

![b52a1956880815ffecdaf8532fbc38dc.png]({{ site.url }}{{ site.baseurl }}/assets/images/arth-task-7.1/c9091ce6203e48b89aa5d0bc9dc6111c.png)

Now we have attached new hard disk, we can confirm using fdisk command.

![7b64d571e6b9f861c85c79cb873885ed.png]({{ site.url }}{{ site.baseurl }}/assets/images/arth-task-7.1/1ddba2356afc4feb9466320bb032fa58.png)

### Step 2: Create physical volume (PV)

Now we have attach the physical storage, we have to convert it into physical volume to use it.

![4e0c8a7c29c336f25a87b5259d9aae01.png]({{ site.url }}{{ site.baseurl }}/assets/images/arth-task-7.1/bd93faac509e47d08511d32c56acc1dc.png)

### Step 3: Create Volume group.

we can create one volume group from many physical device and also add more storage into it as needed.

![a304ae804024df6a509339166ed871bd.png]({{ site.url }}{{ site.baseurl }}/assets/images/arth-task-7.1/a3dd38a7b76f4ebdb13e41e9346b14f0.png)

### Step 4: Create Logical Volume.

We can create many logical volume as per need from volume group. Also increase and decrease the size as per need.

Here we are selecting the size we want to contribute to the hadoop storage as datanode.
I have created Logical volume of 5GiB.

![5a800e7af56837808ff0c69d37897da3.png]({{ site.url }}{{ site.baseurl }}/assets/images/arth-task-7.1/904264ea8a024411b94f1d75e40fc9b3.png)

### Step 5: Format the Storage and Mount

Now we want to use the logical volume, for that we first have to format the file system on it.

![bd56d4082b15cfd02227d0643581fb4d.png]({{ site.url }}{{ site.baseurl }}/assets/images/arth-task-7.1/3e6801917de14bdf9c7435f02be36afa.png)

### Step 6: Start the datanode services and check the contributing storage.

`hadoop-daemon.sh start datanode`

Above command  will start the service of datanode and connect to the hadoop cluster.

![fd27b152efcffdd66dce62adf0d84780.png]({{ site.url }}{{ site.baseurl }}/assets/images/arth-task-7.1/46423227764f4def86c1a850b436ecca.png)

## Providing Elasticity to data node storage.

we can increase and decrease the datanode contributing storage in hadoop cluster using LVM.

1.  ### Increase the storage
    

![fae7de7df810e0d320e295efa275c04c.png]({{ site.url }}{{ site.baseurl }}/assets/images/arth-task-7.1/be94d3ff386440b689c833b93978efe0.png)

###  2\. Decrease the storage

![2cce79163b0ce01135111acaeec5053b.png]({{ site.url }}{{ site.baseurl }}/assets/images/arth-task-7.1/77aafa3130e64a5eb4a36f390b681fc5.png)

![a879fd87b4a931b577463d59a13cb686.png]({{ site.url }}{{ site.baseurl }}/assets/images/arth-task-7.1/e647bab5e2734f7ab3c96a47cf7cfd22.png)

Now you can see we have successfully increase and decrease the LVM partitions.

Arth Task 7.1A completed
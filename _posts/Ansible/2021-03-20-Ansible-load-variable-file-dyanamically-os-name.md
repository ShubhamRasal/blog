---
title: "How to load variable dynamically according to os in ansible?"
toc: true
toc_label: "Content"
tags:
  - Ansible
  - Arth-Tasks
date: March 20, 2021
categories: Ansible
header:
 # image: assets\images\arth-task-14.3\header.png
  #overlay_image: /assets/images/thumbnails/linux.png
  teaser: assets/images/arth-task-14.3/header.png
permalink: /:categories/:title/
excerpt: "Create an Ansible Playbook which will dynamically load the variable file named the same as OS_name and just by using the variable names we can Configure our target node."
comments: true
---
![Init (8).png]({{ site.url }}{{ site.baseurl }}/assets/images/arth-task-14.3/header.png)
## Problem Statement:

Create an Ansible Playbook which will dynamically load the variable file named the same as OS_name and just by using the variable names we can Configure our target node.
(Note: No need to use when keyword here.)

* * *

Let's understand the use case of this problem. There will be many cases when some of the package names are different Linux OS distributions. Also, there will be a case that they are different file formats only such as deb or rpm.

To solve this problem, Ansible offers us one module named "[include_vars](/F:/NOTES/Joplin/resources/app.asar/%22%F0%9F%94%B0%2014.1%20Create%20a%20%20network%20Topology%20Setup%20in%20such%20a%20way%20%20so%20that%20System%20A%20can%20%20ping%20to%20two%20Systems%20System%20B%20and%20System%20C%20but%20both%20these%20systems%20should%20%20not%20be%20pinging%20each%20other%20without%20using%20any%20security%20rule%20e.g%20firewall%20etc.%20%20%F0%9F%94%B0%2014.2%20Further%20in%20ARTH%20-%20Task%2010%20have%20to%20create%20an%20Ansible%20playbook%20that%20will%20retrieve%20newContainer%20IP%20%20and%20update%20the%20inventory.%20So%20that%20further%20Configuration%20of%20Webserver%20could%20be%20done%20inside%20that%20Container.%20%20%F0%9F%94%B0%2014.3%20Create%20an%20Ansible%20Playbook%20which%20will%20dynamically%20%20load%20the%20variable%20file%20named%20same%20as%20OS_name%20and%20just%20by%20%20using%20the%20variable%20names%20we%20can%20Configure%20our%20target%20node.%20%28Note:%20No%20need%20to%20use%20when%20keyword%20here.%29%20%22)" which load variables from files dynamically within task. This module can load the YAML or JSON variables dynamically from a file or directory, recursively during task runtime.

To detect on which operating system we are performing tasks, we can use the "setup" module. This module is automatically called by playbooks to gather useful variables about the remote hosts that can be used in the playbook. This module is also available for windows target.

Let's take an example of installing an Apache web server in CentOS and Ubuntu Linux.  We do not want a hard-coded playbook installing web servers for a different distribution, OS, etc.

* * *

First, find out which variables setup module offers use by which we can detect remote host distribution and accordingly we can load the variable file on the fly.

First check, can we connect both the system using the ping module.

![23643bef40f7805a4cd3062e70b5b0a9.png]({{ site.url }}{{ site.baseurl }}/assets/images/arth-task-14.3/a6af8c7ab4f94806a87803daa358b7d4.png)

Now, we have connectivity to both the servers, we have to get that variable name which gives us a remote host type variable. The setup module gives many variables under the "ansible_distribution" section. so lets see what variable is gives.

![435ff0615c0606823b5b5542f1a52ebd.png]({{ site.url }}{{ site.baseurl }}/assets/images/arth-task-14.3/11ea8027ce2d4dd795c2e1530200709b.png)

Now, we know the distribution name and major version, we will create variables files and give this distribution name and version name to that files. eg: Ubuntu-18.yml, CentOS-8.yml

Also,  you can use the os\_family variable which gives the os\_family of remote hosts.

Create Ubuntu-18.yml file and add the below variables in that file

![666689a79ba30aea0878532635c885af.png]({{ site.url }}{{ site.baseurl }}/assets/images/arth-task-14.3/7a6f0065cc5c4f8bbdc9d359c6fb9f15.png)

```yml
# Ubuntu-18.yml
package_name: apache2
service_name: apache2
document_root: /var/www/html
```


Create CentOS-8.yml file and add below variables

![229e46671e6e4149a72deaa07676ae95.png]({{ site.url }}{{ site.baseurl }}/assets/images/arth-task-14.3/0e6ed3d4ba764e718514817571cbd8f6.png)

\# CentOS-8.yml package\_name: httpd service\_name: httpd document_root: /var/www/html/

```yml
# CentOS-8.yml
package_name: httpd
service_name: httpd
document_root: /var/www/html/
```

Now we have to write a playbook that will install a webserver and read variables dynamically according to distribution type.

Let's first make sure, we are using selecting proper files using ansible facts. so for that, we will use debug module to create a final string.
{% raw %}
```yml
- name: "Install webserver"
  hosts: webserver
  tasks:
  - name: "Test variables"
    debug:
      msg: "{{ ansible_distribution }}-{{ ansible_distribution_major_version }}.yml"
```
{% endraw %}
Run this playbook, `$ ansible-playbook main.yml`

![b6140ceaff53a08534560ec2baa8267b.png]({{ site.url }}{{ site.baseurl }}/assets/images/arth-task-14.3/35ab2a742e8b46c49524db661cd6a7b7.png)

Now let's write a playbook that will read the variables according to OS type of remote host and install the webserver.
{% raw %}
```yml
- name: "Install webserver"
  hosts: all
  vars_files:
     - "{{ ansible_distribution }}-{{ ansible_distribution_major_version }}.yml"
  
  tasks:
    - name: "Install the web server"
      package:
              name: " {{ package_name }}"
              state: present
    
    - name: "Create document root directory"
      file: 
          path: "{{document_root }}"
          state: directory
          recurse: yes

    - name: "Create index.htm page in document root"
      copy:
              content: "<h1> Welcome to {{ ansible_distribution }} server !! </h1>"
              dest: "{{ document_root }}/index.html"
    
    - name: "Start the service"
      service:
              name: "{{ service_name }}"
              state: started


- name: "Test the servers"
  hosts: localhost
  tasks:
          - name: "HealthCheck the servers"
            uri:
               url: "http://{{item}}"
               return_content: yes
            with_items: "{{ groups['webserver'] }}"
            register: output
            failed_when: '"Welcome" not in output.content'
```
{% endraw %}
The above playbook will install and test our web server is working properly or not.

`$ ansible-playbook main.yml`

We can test the installation is successful or not using the curl command.![5da6f7edfb17d205a872b44a11ea24ab.png]({{ site.url }}{{ site.baseurl }}/assets/images/arth-task-14.3/1431a99832074aae8e22f65cab81a6c8.png)

![8664a00a712b09981cd3c9edc79e536e.png]({{ site.url }}{{ site.baseurl }}/assets/images/arth-task-14.3/85f4dc3a2c9c4b9383adf031ff475689.png)![b9bd0062d4802edef31dd384be2f4c88.png]({{ site.url }}{{ site.baseurl }}/assets/images/arth-task-14.3/8a1e289b17b048c396134bd378ad4492.png)

## Conclusion:

We have successfully made our playbook variables dynamically according to distribution and major version number without using the when keyword.

# Thank you…

*About the writer:*
*Shubham** loves technology, challenges, continuous learning, and reinventing himself. He loves to share his knowledge and solve daily problems using automation.*

*Visit him : shubhamrasal.me/blog to know more.*
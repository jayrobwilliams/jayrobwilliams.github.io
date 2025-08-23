---
title: Basic sandbox network with three (3) virtual machines
output:
  md_document:
    variant: gfm+footnotes
    preserve_yaml: TRUE
knit: (function(inputFile, encoding) {
  rmarkdown::render(inputFile, encoding = encoding, output_dir = "../_posts") })
date: 2024-12-04
permalink: /posts/2023/03/nest-map
excerpt_separator: <!--more-->
always_allow_html: true
toc: true
header:
 og_image: "posts/nest-map/fig-1.png"
tags:
  - VirtualBox
  - Ubuntu 22 desktop, Bitnami opencart, and Ubuntu gateway 
  - Networking
  - Sandboxing
---

## A basic network sandbox portfolio
Network sandboxing refers to creating an isolated environment within a network to test and evaluate network traffic, applications, or systems without affecting the broader infrastructure or production environment.

A sandboxed network provides an essential learning environment and platform, offering a secure, isolated space to simulate real-world network scenarios without introducing risks to live systems. For this Portfolio, i will create a walk through of my own private sandboxed virtual network using VirtualBox. The network will consist of multiple virtual machines (VMs) configured within a private IP address range. The aim is to gain an applied understanding of networking concepts, IP subnetting, network interface configuration, and a basic server setup, design, planning and organisation strategies.
<!--more-->
Furthermore, creating a basic sandbox environment with a ubuntu desktop, ubuntu gateway, and Bitnami OpenCart is to provide a secure, isolated space for testing and experimentation without affecting production systems or real-world data. This type of sandbox setup is typically used to evaluate software, analyze vulnerabilities, or experiment with configurations in a controlled, risk-free manner. 

Here's a breakdown of each component's role in achieving this aim:

### Desktop Environment (Local Workstation)
The desktop serves as the local machine where the sandboxing process begins. It's a safe and isolated environment from which users can interact with the sandboxed components without compromising the underlying system or other networked systems.

### Aim:
- Isolation: The desktop acts as the point of interaction with the sandbox environment, allowing users to control and monitor the sandbox without directly interacting with the host operating system or network.
- Testing: Allows developers and testers to execute software, make changes, and test functionality without impacting critical systems.
- Configuration and Deployment: The desktop is typically where the OpenCart application (via Bitnami's pre-packaged software) is configured and deployed, setting up a virtualized or containerized environment.

### Gateway (Network Bridge)
The gateway acts as the network entry point, providing connectivity between the sandboxed environment and external networks (such as the internet or other isolated networked environments). This is typically managed via a firewall or network segmentation strategy, ensuring traffic flow is controlled and can be monitored.

### Aim:
- Controlled Connectivity: The gateway allows the sandbox to connect to the internet or other external resources (for example, to access APIs, external databases, or perform updates), while also maintaining security and isolation from the broader network.
- Network Monitoring: Traffic passing through the gateway can be closely monitored to detect any unusual activity, vulnerabilities, or potential threats.
- Access Control: The gateway enforces access controls, ensuring that the sandboxed environment only communicates with allowed external systems or services, preventing any unwanted or malicious data leaks.

### Bitnami OpenCart (Application Layer)
Bitnami OpenCart is an easy-to-install, pre-packaged version of the OpenCart e-commerce platform that can be run on various environments (virtual machines, containers, or cloud platforms). In this sandbox, it serves as the application being tested or experimented with.

### Aim:
- Testing Application Configurations: OpenCart can be used in the sandbox to test new plugins, themes, updates, or integrations, such as payment gateways or shipping modules. These tests are done in a safe environment, preventing any potential issues from affecting a live production store.
- Performance and Load Testing: Sandboxing OpenCart in an isolated environment allows the simulation of real-world traffic or user interactions to evaluate the platform’s performance under various conditions.
- Security Analysis: This setup can be used to evaluate potential security vulnerabilities, such as testing OpenCart’s response to security exploits (e.g., SQL injections, cross-site scripting, etc.), and harden the system before deploying it to production.
- Simulated eCommerce Operations: The sandbox environment allows administrators to simulate real-world business operations—such as inventory management, payment processing, and order fulfillment—without any real financial or operational risks.

### Steps 
- Understand the network analysis
  
  <img src="/images/posts/nest-map/p2.png" style="display: block; margin: auto;" />
  
- Make a sketch on packet-tracer:

  Draw a rough sketch of your network diagram using either a cisco newtwork packet-tracer, pen and paper, or gns3 on ubuntu
  <img src="/images/posts/nest-map/packetracer.PNG" style="display: block; margin: auto;" />
- Create an IP table for the machines according to the network diagram and sketch

| Device     | Role     | IP Address     | Subnet Mask     |
|--------------|--------------|--------------|--------------|
| Desktop VM | Management and deployment | 192.168.126.2  | 255.255.255.0 |
| Gateway Router VM (enp0s3)  | Internet Access to Ubuntu Gateway  | 10.0.2.15  | 255.255.255.0 |
| Gateway Router VM (enp0s8)  | Subnet 01- Access to internet, Gateway and Bitnami Opencart| 192.168.126.1  | 255.255.255.0 |
| Gateway Router VM (enp0s9)  | Subnet 02- Access to internet, Gateway and Desktop | 192.168.26.1  | 255.255.255.0 |
| Application Server VM  | Server (Bitnami-Opencart)  | 192.168.26.2  | 255.255.255.0 |

- #### Desktop Configuration:
  
  Go to setting and then click NETWORK and set ADPATER 1 to INTERNAL NETWORK, then clicked OK and click START to bootup the machine on the VM.

<img src="/images/posts/nest-map/st1.PNG" style="display: block; margin: auto;" />

After starting up the machine, go to Settings and click on WIRED SETTINGS then click on IDENTITY to setup the MAC Address to (08:00:27:97:75:31 (enp0s3)) then click IPV4 and set to Manual to set the ADDRESS, NETMASK, GATEWAY AND DNS ADDRESS: 192.168.126.2	NETMASK: 255.255.255.0 GATEWAY: 192.168.126.1	and DNS: 8.8.8.8, 1.1.1.1 
Then click on APPLY and then DISCONNECT and RE-CONNECT the WIRED CONNECTION

<img src="/images/posts/nest-map/ubsp1.PNG" style="display: block; margin: auto;" />

Open a command line terminal using “Ctrl + alt + T” and type “ip a” to see if IP address is set, if IP address is set continue, else re-do the above step and restart the Ubuntu desktop machine. 

<img src="/images/posts/nest-map/ubip.PNG" style="display: block; margin: auto;" />

- #### Gateway(Router) Configuration:
  Go to settings and then click NETWORK and set ADAPTER 1 to NAT, ADAPTER 2 for (desktop) to INTERNAL NETWORK and ADAPTER 3 for (opencart) to INTERNAL NETWORK
<img src="/images/posts/nest-map/g1.PNG" style="display: block; margin: auto;" />
<img src="/images/posts/nest-map/g2.PNG" style="display: block; margin: auto;" />
<img src="/images/posts/nest-map/g3.PNG" style="display: block; margin: auto;" />

Then click OK and click START to bootup the machine on the VM  

After starting up the machine use:  

**student@router:~$   sudo nano /etc/netplan/00-installer-config.yaml**  
to enter the network interface to configure the ADDRESS, NETMASK, and GATEWAY for all three (3) ADAPTERS. 
<img src="/images/posts/nest-map/setup.PNG" style="display: block; margin: auto;" />
Then use: student@router:~$ **sudo netplan apply** 	       (to apply new configuration) 

Then use: student@router:~$ **ip a**				(to view all IPs) 
<img src="/images/posts/nest-map/g5.PNG" style="display: block; margin: auto;" />

- #### Bitnami Opencart Server Configuration:
  Go to settings and then click NETWORK and set ADPATER 1 to INTERNAL NETWORK, then click OK and click START to bootup the machine on the VM
<img src="/images/posts/nest-map/st2.PNG" style="display: block; margin: auto;"/>

After starting up the machine use:  
bitnami@debian:~$   **sudo nano /etc/network/interfaces**  
to enter the network interface to configure the ADDRESS, NETMASK, and GATEWAY.

ADDRESS: 192.168.26.2	NETMASK: 255.255.255.0	GATEWAY: 192.168.126.1

<img src="/images/posts/nest-map/opc1.PNG" style="display: block; margin: auto;"/>

Then use **Ctrl + X** to save by pressing Y when asked do you want to save settings? 
Use: bitnami@debian:~$  **sudo reboot now**  

After reboot use: bitnami@debian:~$ **ip a** 	to see if IP Address is set. 

<img src="/images/posts/nest-map/opcst.PNG" style="display: block; margin: auto;"/>              

### Machines Functionality
This is the interaction and communication between all three (3) machines

- #### Desktop Funtionality
Ping from desktop (192.168.126.2) to bitnami opencart application server (192.168.26.2)

<img src="/images/posts/nest-map/1to2.PNG" style="display: block; margin: auto;" />

Ping from desktop (192.168.126.2) to ubuntu gateway server (192.168.126.1)

<img src="/images/posts/nest-map/1tog.PNG" style="display: block; margin: auto;" />

Access the opencart e-commerce site using the desktop web browser and IP address 192.168.26.2

<img src="/images/posts/nest-map/opencart.PNG" style="display: block; margin: auto;" />

Access the google site for internet connect on the desktop web browser using www.google.com 

<img src="/images/posts/nest-map/broswer.PNG" style="display: block; margin: auto;" />

Using ssh@192.168.126.1 to access the gateway from the desktop

<img src="/images/posts/nest-map/sshrouter.PNG" style="display: block; margin: auto;" />

- #### Gateway Functionality
Ping from ubuntu gateway (192.168.126.1) to desktop application (192.168.126.2)

<img src="/images/posts/nest-map/gto2.PNG" style="display: block; margin: auto;" />

Ping from ubuntu gateway (192.168.26.1) to bitnami opencart application server (192.168.26.2)

<img src="/images/posts/nest-map/gto1.PNG" style="display: block; margin: auto;" />

Using nslookup bbc.co.uk to resolve bbc's ramdom IP addresses 

<img src="/images/posts/nest-map/bbc.PNG" style="display: block; margin: auto;" />

- #### Bitnami Opencart Functionality
Ping from opencart (192.168.26.2) to desktop application (192.168.126.2)

<img src="/images/posts/nest-map/2to1.PNG" style="display: block; margin: auto;" />

Ping from bitnami opencart application server (192.168.26.2) to ubuntu gateway (192.168.26.1) 

<img src="/images/posts/nest-map/opc3.PNG" style="display: block; margin: auto;" />

### Video
If you find it difficult to play or watch this video directly, please click the **Watch Video Link** below
<video controls>
  <source src="images/posts/nest-map/video.mp4" type="video/mp4",
</video>
  
[Watch Video](https://roehamptonprod-my.sharepoint.com/:v:/r/personal/nkereisg_roehampton_ac_uk/Documents/Networking%20and%20Security%20Practices/video.mp4?csf=1&web=1&nav=eyJyZWZlcnJhbEluZm8iOnsicmVmZXJyYWxBcHAiOiJPbmVEcml2ZUZvckJ1c2luZXNzIiwicmVmZXJyYWxBcHBQbGF0Zm9ybSI6IldlYiIsInJlZmVycmFsTW9kZSI6InZpZXciLCJyZWZlcnJhbFZpZXciOiJNeUZpbGVzTGlua0NvcHkifX0&e=PW3q7j)

### Summary of Sandboxing
Sandboxing is a security mechanism used to execute code or run applications in a restricted and isolated environment. The primary goal is to limit the access of potentially untrusted or malicious programs to the system, thereby protecting the host environment from threats such as malware, data breaches, or system crashes.

## Bonus

### Key Features of Sandboxing
- Isolation:
  The sandbox operates as a separate environment from the host system, preventing direct interaction with critical system resources.
- Controlled Access:
  Programs inside the sandbox have restricted permissions, allowing them to access only predefined files, network resources, or system processes.
- Testing and Debugging:
  Developers use sandboxes to test untrusted code, software updates, or configurations without risking the stability or security of the main system.
- Temporary Nature:
  Changes made within a sandbox are usually discarded after the sandbox is closed, preventing persistent system modifications.
### How Sandboxing Works
- Virtualization: Sandboxes often rely on virtualization technologies to create isolated environments.
- Containerization: Tools like Docker use sandboxing to isolate applications within lightweight containers.
- Restricted APIs: Sandboxes limit the APIs and system calls available to the application.
### Applications of Sandboxing
- Web Browsers:
  Modern browsers like Chrome and Firefox use sandboxing to isolate tabs and plugins to prevent malware from spreading.
- Mobile Applications: In the modern technology of iOS and Android, sandboxes are used to ensure apps cannot access each other's data or system resources 
  directly.
- Security Testing: Cybersecurity professionals use sandboxes to analyze malware behavior in a controlled environment.
- Development Environments: Developers use sandboxes to test and debug applications without affecting the host system.
### Benefits
- Protects sensitive data and system integrity.
- Minimizes the risk of malware infections and exploits.
- Enables safe testing and experimentation with untrusted code.
### Limitations
- Performance overhead compared to running applications natively.
- Sophisticated malware can sometimes escape sandboxes.
- Requires proper configuration to ensure effectiveness.
### Conclusion
Sandboxing is a critical tool for enhancing system security by isolating and containing potential threats. Its use is widespread in software development, cybersecurity, and operating systems, making it a cornerstone of modern secure computing practices.

[^1]: Yes, I know this is a perfect situation to use LASSO. Sometimes
    people (reviewers) want certain models run, and you just have to run
    them.

[^2]: There’s a very real chance that someone else is me in six months.

[^3]: Things get a lot more complicated if your `split()` call produces
    a list of dataframes that aren’t one row each, so make sure that’s
    what you’re getting before you proceed.

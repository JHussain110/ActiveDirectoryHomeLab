# Active Directory Home Lab
# Active Directory Home Lab (VirtualBox)

## Project Overview

In this project I built a small Active Directory environment on my personal computer using VirtualBox.

The purpose of the lab was to understand how enterprise Windows networks work and how system administrators manage users, devices, and networking services.

The environment simulates a small corporate network and includes:

• Windows Server 2019 Domain Controller  
• Active Directory Domain Services  
• DNS  
• DHCP  
• NAT routing for internet access  
• Automated user creation with PowerShell  
• A Windows 10 client machine joined to the domain  

This project helped me understand how users and computers are managed centrally in organisations.

---

# Lab Architecture

The lab consists of two virtual machines running inside VirtualBox.

1. Domain Controller (Windows Server 2019)
2. Client machine (Windows 10)

The domain controller manages the network services and authentication for the environment.

The server uses **two network adapters**:

Adapter 1  
Connected to NAT to access the internet.

Adapter 2  
Connected to an internal VirtualBox network used by domain clients.

Clients communicate with the domain controller through the internal network.

![Lab Overview](screenshots/00_lab_overview.png)

---

# Tools and Technologies Used

• Oracle VirtualBox  
• Windows Server 2019  
• Windows 10  
• Active Directory Domain Services  
• DHCP  
• DNS  
• Routing and Remote Access (NAT)  
• PowerShell  

---

# Step-by-Step Setup

## Installing the Domain Controller

First I created a virtual machine in VirtualBox and installed Windows Server 2019.

The VM was configured with:

• 2GB RAM  
• Virtual hard disk  
• Two network adapters  

![Server Install](screenshots/01_server_download.png)

After installation I logged into the server and confirmed the Server Manager dashboard.

![Server Manager](screenshots/02_server_manager.png)

---

# Configuring Network Adapters

The server has two network interfaces.

Internet Adapter  
Connected to NAT to access the internet.

Internal Adapter  
Used for the internal lab network.

I identified both adapters and renamed them to make configuration easier.

![Network Adapters](screenshots/03_network_adapters.png)

---

# Static IP Configuration

The internal network adapter was configured with a static IP address.

IP Address  
172.16.0.1

Subnet Mask  
255.255.255.0

DNS Server  
127.0.0.1 (the domain controller itself)

This server will later act as the DNS server for the domain.

![Static IP](screenshots/04_static_ip.png)

---

# Installing Active Directory Domain Services

Next I installed the **Active Directory Domain Services** role using Server Manager.

After installation I promoted the server to a **Domain Controller** and created a new forest.

Domain name used in this lab:

mydomain.com

![Domain Creation](screenshots/05_domain_creation.png)

After the promotion process the server restarted and the domain was created successfully.

![Domain Created](screenshots/06_domain_created.png)

---

# Creating a Domain Administrator

Instead of using the default administrator account, I created a dedicated domain admin user.

This account was placed in a new organisational unit called:

MyAdmins

The account was then added to the **Domain Admins** group.

![Admin User](screenshots/07_admin_user.png)

---

# Configuring Routing and NAT

To allow internal clients to access the internet I installed the **Routing and Remote Access** role.

This enables NAT on the domain controller.

The server forwards traffic from the internal network to the internet through the external adapter.

![RRAS Console](screenshots/10_rras_console.png)

![NAT Setup](screenshots/11_nat_setup.png)

---

# Installing DHCP

Next I installed the DHCP server role.

DHCP automatically assigns IP addresses to client machines joining the network.

![DHCP Install](screenshots/12_dhcp_install.png)

I configured a DHCP scope with the following range:

Start IP  
172.16.0.100

End IP  
172.16.0.200

Subnet  
255.255.255.0

![DHCP Scope](screenshots/13_dhcp_scope.png)

The DHCP server was then authorised and activated.

![DHCP Authorised](screenshots/14_dhcp_authorised.png)

---

# Automating User Creation with PowerShell

To simulate a realistic environment I used a PowerShell script to automatically create many user accounts.

The script reads names from a text file and generates accounts inside Active Directory.

![Names File](screenshots/15_names_file.png)

The script loops through the list of names and creates users with a default password.

![PowerShell Script](screenshots/16_powershell_script.png)

After running the script over 1000 user accounts were created in Active Directory.

![Users Created](screenshots/17_users_created.png)

---

# Creating the Client Machine

Next I created a Windows 10 virtual machine.

This machine only uses the **internal VirtualBox network adapter** so it communicates with the domain controller.

![Client VM](screenshots/18_client_vm.png)

After installation I verified that the client received an IP address from DHCP.

![IP Configuration](screenshots/19_ipconfig.png)

The DHCP server also shows the lease assigned to the client.

![DHCP Lease](screenshots/20_dhcp_lease.png)

---

# Joining the Client to the Domain

The Windows 10 machine was renamed to:

Client1

I then joined it to the domain **mydomain.com**.

Once joined, the computer object appeared inside Active Directory.

![Computer in AD](screenshots/21_computer_in_ad.png)

---

# Logging in with a Domain User

Finally I logged into the client machine using one of the domain accounts created earlier.

Running the command:

whoami

confirmed the user was authenticated through the domain.

![Domain Login](screenshots/22_domain_login.png)

---

# What I Learned

This project helped me understand how enterprise networks manage users and devices centrally.

Key things I learned:

• How Active Directory controls authentication  
• How DNS supports domain services  
• How DHCP automatically assigns IP addresses  
• How NAT allows internal networks to access the internet  
• How PowerShell can automate system administration tasks  

Building the lab also improved my troubleshooting skills when configuring networking services and verifying connectivity.

---


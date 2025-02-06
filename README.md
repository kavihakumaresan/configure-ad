# configure-ad
<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services

<h2>Operating Systems Used </h2>

- Windows Server 2022, 2vCPUs, 8GM RAM
- Windows 10 Pro (22H2), 2vCPUs, 8GB RAM

<h2>High-Level Deployment and Configuration Steps</h2>

 - Install Active Directory
 - Create a Domain Admin user for the domain
 - Join client-1 to domain

<h2>Deployment and Configuration Steps</h2>

<p>
Before deploying Active Directory, we need to set up two virtual machines (VMs) on Azure:
  - DC-1 – A Windows Server VM that will function as the domain controller.
  - Client-1 – A Windows 10/11 Pro VM that will act as a client machine.
Both VMs must be on the same virtual network to ensure proper communication. Once the setup is complete, we will log into DC-1 to proceed with the Active Directory installation.
</p>

![image](https://github.com/user-attachments/assets/a359fcf0-6ccf-4b66-9c9d-f26d9c1b428e)


<p>
The Server manager should automatically load up if not click on Start and you should see the Server Manager icon. Click on Add roles and features and accept the defaults until Select Server Roles. We'll select Active Directory Domain Services.
</p>

![image](https://github.com/user-attachments/assets/b9e4e44d-1f00-4b43-b112-508b993c7ca4)

<p>
In the deployment configuration, click on Add a new forest and input mydomain.com. 
</p>

![image](https://github.com/user-attachments/assets/b3783a09-7c80-4ca2-aa17-63120821cd06)

<p>
Click Next until you reach the Directory Services Restore Mode (DSRM) password section. Enter a password of your choice—this is primarily used for recovery purposes, though it is unlikely to be needed in this setup. Proceed with the configuration, and once the Active Directory installation is complete, the system will automatically sign you out as the changes take effect.
</p>

<p>
Now, we'll try to log into our Client-1 machine. If you try to login with your usual credentials, it will fail since no domain is specified as we just set up DC-1 as the domain controller. So to log into the machine, we need specify the domain like so: mydomain.com\\(your username), and then enter the password. 

Once that's confirmed, we jump back log back into the DC-1 machine. The next step is to create a Domain admin user within the domain. The person will be able to administer the entire domain of users and is quite an important task. So we can click on Start -> Windows Administrative Tools -> Active Directory Users and Computers. Now right click on mydomain.com -> New -> Organizational Unit (OU).
</p>


<p>
We can name the new OU _EMPLOYEES and we can create another one called _ADMINS. Then we'll create an admin user. Right click on _ADMINS -> New -> User. We'll name this admin user Jane Doe, with as username of jane_admin. Set whatever password that you wish. 

Although Jane Doe is in a OU called _ADMINS, she technically is not yet a Domain admin since we have not given those privileges to her. So to do this, we click on _ADMINS and see that Jane Doe is there. Right click on Jane Doe -> Properties -> Member Of then type in Domain Admins in the box. Click on Check names, then OK and apply these settings.
</p>

![image](https://github.com/user-attachments/assets/6584807d-72dd-4e6b-8d4e-54355b1c570e)


![image](https://github.com/user-attachments/assets/27654747-5a26-4ee1-beaf-b1801a86d7fe)



<p>
Log back into Client-1 as Jane_admin, be sure to do mydomain.com\ before the username. Right click on the Start menu -> Settings -> Rename this PC (advanced). Then under Computer Name click on Change and in Domain, type mydomain.com and OK.
</p>


<p>
You will have to restart Client-1 for these changes to take effect. So do that and go back to DC-1. We'll verify if Client-1 has joined the domain. While still on Active Directory Users and Computers, click on mydomain.com and the Computers and you should see Client-1 there.
</p>




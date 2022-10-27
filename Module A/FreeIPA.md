#Установка и настройка FreeIPA
vim /etc/selinux/config
SELINUX=disabled

apt -y install rng-tools;
echo HRNGDEVICE=/dev/urandom >> /etc/default/rng-tools;
systemctl enable rng-tools --now;
apt -y install freeipa-server;
ipa-server-install;
To accept the default options shown in square brackets, just press Enter key.…

Do you want to configure integrated DNS (BIND)? [no]: Enter

Enter the fully qualified domain name of the computer

on which you're setting up server software. Using the form

<hostname>.<domainname>

Example: master.example.com.

Server host name [ipa.computingforgeeks.com]: Enter
The domain name has been determined based on the host name.
Please confirm the domain name [computingforgeeks.com]: Enter
The kerberos protocol requires a Realm name to be defined.
This is typically the domain name converted to uppercase.
Please provide a realm name [COMPUTINGFORGEEKS.COM]: Enter
Certain directory server operations require an administrative user.
This user is referred to as the Directory Manager and has full access
to the Directory for system management tasks and will be added to the
instance of directory server created for IPA.
The password must be at least 8 characters long
Directory Manager password: <secure password>
Password (confirm): <secure password>
The IPA server requires an administrative user, named 'admin'.
This user is a regular system account used for IPA server administration.

IPA admin password: <secure password>
Password (confirm): <secure password>

The IPA Master Server will be configured with:
Hostname:    ipa.computingforgeeks.com
IP address(es): 192.168.58.121
Domain name: computingforgeeks.com
Realm name:  COMPUTINGFORGEEKS.COM

The CA will be configured with:

Subject DN:   CN=Certificate Authority,O=COMPUTINGFORGEEKS.COM
Subject base: O=COMPUTINGFORGEEKS.COM
Chaining:  self-signed
Continue to configure the system with these values? [no]: yes
...output cut…
Client configuration complete.
The ipa-client-install command was successful
==============================================================================
Setup complete
Next steps:
1. You must make sure these network ports are open: TCP Ports: * 80, 443: HTTP/HTTPS * 389, 
  636: LDAP/LDAPS * 88, 464: kerberos UDP Ports: * 88, 464: kerberos * 123: ntp 
2. You can now obtain a kerberos ticket using the command: 'kinit admin' 
  This ticket will allow you to use the IPA tools (e.g., ipa user-add) and the web user interface.

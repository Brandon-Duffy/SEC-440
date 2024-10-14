---
description: AD01
---

# Windows Admin Center

sconfig, change IP to 10.0.5.5/24

Change hostname, standard work.





On AD01 - Open Server Manager and install ADDS and DNS and include the tools. Create a new forest as well, with PTR records through AD Users and Computers

Setting up users is also done easily through Users and Computers, accessible from Server Manager.



Confiure FS01 as normal with sconfig, AD join it.





```
powershell
Invoke-WebRequest -Uri https://go.microsoft.com/fwlink/p/?linkid=2194936 -UserBasicParsing -outfile windowsAdminCenter-xxx.msi

```

Above command allows for the installation of WAC on FS01





To add extensions on WAC, hit the gear icon in the top right corner of the screen, scroll down to Extensions, and install ADDS and DNS.



GPO

On AD01, open AD U\&C, create new OU and move WKS1 into it. Open Group Policy Management -> Domains -> brandon.local -> WinRM -> Create new GPO.

<figure><img src=".gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>




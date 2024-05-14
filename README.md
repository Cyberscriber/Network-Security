<h1>Network Security & Compliance Lab</h1>

 <h2>Description</h2>
Project consists of firewall management and compliance efforts, configuring UFW and firewalld for robust network security in simulated e-commerce scenarios.  vulnerability assessments using Nmap and deployed Snort for intrusion detection, ensuring
adherence to PCI DSS standards and enhancing threat detection capabilities.
<br />
<h2>Environments Used </h2>

- <b>Security Onion</b>
- <b>Ubuntu<b>

<h2>Walk-through:</h2>
UFW (Uncomplicated Firewall) isn't started by default.  Admins must enable it themselves. <br />
Helpful commands are as follows:<br />
sudo ufw status  which shows if UFW is active or inactive.<br />
sudo ufw status verbose which shows more detail<br />
sudo ufw enable which starts UFW<br />
It's best practice to deny all incoming and outgoin traffic by default. <br />
sudo ufw default deny incoming to block all incoming connections<br/>
sudo ufw default allow outgoing to allow all outgoing connections<br/>
sudo ufw allow to open specific ports<br/>
sudo ufw deny to close specific ports<br/>
sudo ufw delete to delete rules<br/>
sudo ufw disable to shut down the firewall<br/>
<br/>
<br/>
<p align="center">
Drop Zone <br/>
Firewalld is very similar to UFW, but it is a bit more complicated and provides greater flexibility and most importantly does not disrupt services when managing firewall updates.  Firewalld uses zones to divide network interfaces.  Zones are assigned sets of rules depending on needs.

<br />
<br />
To run the command that checks whether or not the `firewalld` service is up and running: <br/>
Systemctl status firewalld <br/>

<br/>
<br/>
<img src="https://imgur.com/MwazUv1.png" height="80%" width="80%" alt="Drop Zone Steps"/>

<br />
<br />
To run the command that lists all currently configured firewall rules: <br/>
Sudo firewall-cmd  - - list-all
<p align="center">
 <br/>
<img src="https://imgur.com/45VZk43.png" height="80%" width="80%" alt="Drop Zone Steps"/>
<p align="center">
 Note: the above screenshot was taken after the firewall rules were configured, and for that reason, it 
 does not show pre-configuration rules.  Pre-configuration rules are shown in the currently configured 
 zones screenshots below
 <br />
 <br />
 <p align="center">
To remove unneeded services:<br />
sudo firewall-cmd  - -zone=public  - -remove-service=dhcpv6-client  - -permanent
    <br/>
  Rules are erased when the firewall reboots, so to save the configuration, add the --permanent option to the end of the rich rule.
  <br/>
<img src="https://imgur.com/QjdM6Rs.png" height="80%" width="80%" alt="Drop Zone Steps"/>
<br />
<br />
The Command that lists all currently supported services: <br />
Sudo firewall-cmd  - - get-services  <br/>
<p align="center">
 Based on your needs, disable the ones that are not critical to business operations as a form of system hardening.
  <br/>
<img src="https://imgur.com/W6aFMTg.png" height="80%" width="80%" alt="Drop Zone Steps"/>
<br />
<br />
The command that lists all currently configured zones: <br />
Sudo firewall-cmd  - -list-all-zones
<p align="center">
  <br/>
<img src="https://imgur.com/mqRYYiB.png" height="80%" width="80%" alt="Drop Zone Steps"/>
<br />
<br />
<br/>
<img src="https://imgur.com/Cuq2IH3.png" height="80%" width="80%" alt="Drop Zone Steps"/>
 <br />
<br/>
<img src="https://imgur.com/kkrTOa1.png" height="80%" width="80%" alt="Drop Zone Steps"/>
<br />
 To run the commands to create new zones:
<br />
 <br />
 Sudo firewall-cmd  - - permanent  - - new-zone=web
<br/>
 <br />
 <img src="https://imgur.com/DGzlrem.png" height="80%" width="80%" alt="Drop Zone Steps"/>
<br />
<br />
 To run the command to reload the firewall service in order to apply new settings, the command is:  firewall-cmd --reload
 <br />
<p align="center">
The process of binding the interface to the zone causes the interface to inherit all the rules from the zone.<br />
To run the command that sets your `eth` interfaces to zones (to bind the zone to the interface) :<br />
Sudo firewall-cmd  - - zones=sales  - - change-interface=eth2  - - permanent 
<br />
<br/>
 <img src="https://imgur.com/TqP4kft.png" height="80%" width="80%" alt="Drop Zone Steps"/>
<br />
<br />
<br/>
 <img src="https://imgur.com/XcS8I4B.png" height="80%" width="80%" alt="Drop Zone Steps"/>
<br />
<br />
The command that adds services to zones:<br />
Sudo firewall-cmd  - -zone=public  - - permanent  - -add-service=http<br />

<br/>
 <img src="https://imgur.com/auEoyAF.png" height="80%" width="80%" alt="Drop Zone Steps"/>
<br />
<br />
The command that will add all blacklisted IPS to the drop zone:<br />
Sudo firewall-cmd  - -zone=drop  - -permanent  - -add-rick-rule=”rule family=’ipv4’ source address=’IP address removed’ reject”<br />

<br/>
 <img src="https://imgur.com/k58bkfU.png" height="80%" width="80%" alt="Drop Zone Steps"/>
<br />
<br />
<br/>
 <img src="https://imgur.com/rbuKqgM.png" height="80%" width="80%" alt="Drop Zone Steps"/>
<br />
<br />
<br/>
 <img src="https://imgur.com/IckMGPd.png" height="80%" width="80%" alt="Drop Zone Steps"/>
<br />
<br />
The command that reloads the `firewalld` configurations and writes it to memory:<br />
 firewall-cmd --reload<br />

<p align="center">
The command that displays all zone services:<br />
firewall-cmd  - -get-services<br />

<br/>
 <img src="https://imgur.com/X4tkCGk.png" height="80%" width="80%" alt="Drop Zone Steps"/>
<br />
To run a rich-rule that blocks the IP address `IP address removed` on the public zone:<br />
sudo firewall-cmd  - -zone=public  - -permanent  - -add-rich-rule=”rule family=’ipv4’ source address=’IP address removed’ reject”<br />

<br />
<br/>
 <img src="https://imgur.com/RL6GQst.png" height="80%" width="80%" alt="Drop Zone Steps"/>
<br />
<br />
<br/>
 <img src="https://imgur.com/W6N7qEO.png" height="80%" width="80%" alt="Drop Zone Steps"/>
<br />
Blocking the ICMP pings from entering the network help mitigate against Dos attacks. The command that blocks `pings` and `icmp requests` in the `public` zone to harden the network:<br />
sudo firewall-cmd  - -zone=public  - -permanent  - -add-icmp-block=echo-reply  - -add-icmp-block=echo-request<br />

<br />
<br/>

 <img src="https://imgur.com/do9UaLR.png" height="80%" width="80%" alt="Drop Zone Steps"/>
<br />
<br />
<br/><img src="https://imgur.com/0sNvy2c.png" height="80%" width="80%" alt="Drop Zone Steps"/>
<br />
The command that lists all of the rule settings for each zone:<br />
sudo firewall-cmd  - -list-all  - -zone=block <br />

<br />
<br/>
<img src="https://imgur.com/L1nplJv.png" height="80%" width="80%" alt="Drop Zone Steps"/>
<br />
<br />
<br/>
<img src="https://imgur.com/E8bx3HV.png" height="80%" width="80%" alt="Drop Zone Steps"/>
<br />
<br />
<br/>
<img src="https://imgur.com/jVHXs37.png" height="80%" width="80%" alt="Drop Zone Steps"/>
<br />
<br />
<br/>
<img src="https://imgur.com/2Troz2Y.png" height="80%" width="80%" alt="Drop Zone Steps"/>
<br />
<br />
<br/>
<img src="https://imgur.com/6iF5fQ8.png" height="80%" width="80%" alt="Drop Zone Steps"/>
<br />
<br />
<br/>
<img src="https://imgur.com/naKw8uU.png" height="80%" width="80%" alt="Drop Zone Steps"/>
<br />
<br/>
<img src="https://imgur.com/OBl7KVl.png" height="80%" width="80%" alt="Drop Zone Steps"/>
<br />
<br />
<br/>
<img src="https://imgur.com/jIVRTjV.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br/>
<img src="https://imgur.com/rsCBACM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br/>
<img src="https://imgur.com/XqWl4YY.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br/>
<img src="https://imgur.com/M0eLhXS.png" height="80%" width="80%" alt="Drop Zone Steps"/>
<br />
<br />
<br/>
<img src="https://imgur.com/JnPwiMr.png" height="80%" width="80%" alt="Drop Zone Steps"/>
<br />
<br />
<br/>
<img src="https://imgur.com/XqMJsGx.png" height="80%" width="80%" alt="Drop Zone Steps"/>
<br />
<p align="center">
Security Onion Lab

What was the indicator of an attack?<br />
   Alert indicator is categorized as Red/Critical, possible data breach <br />
   How did they attacker locate the victim? <br />
   Attack was targeting Italian users<br />
What was it that was downloaded? <br />
a JavaScript downloader called JS/Nemucod, which is attached directly to the emails inside a ZIP file<br />
How was it downloaded? <br />
When the user opens the zip file and double clicks the JavaScript, the default file type associations in Windows will cause Internet Explorer to open and execute the JavaScript<br />
What was it that was downloaded? <br />
malware<br />
What does the exploit do? <br />
Command & Control:  the executable files downloaded by Nemucod are used to retrieve a Trojan Downloader called Fareit or Pony Downloader, which in turn downloads another set of executable files containing the Gozi infostealer<br />
Ref:  https://www.certego.net/en/news/italian-spam-campaigns-using-js-nemucod-downloader/



<br />
<img src="https://imgur.com/aAZ3kXe.png" height="80%" width="80%" alt="Drop Zone Steps"/>
<br />
<br />
<br/>
<br/>
</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>

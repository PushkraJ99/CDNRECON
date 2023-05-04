# 💫 About Me
👾 Bug Hunter | Student <br>
<p align="center">
<img src="https://media.giphy.com/media/WUlplcMpOCEmTGBtBW/giphy.gif" width="500"> 
</p>

# CDNRECON: Peel back the layers of the web
CDNRECON is a tool that tries to find the origin or backend IP address of a website protected by a CDNs reverse proxy. This tool can be useful in penetration testing to identify the true target IP and in reconnaissance to identify the hosting provider or data center of a website. You can also simply use it to test your own website for any leaks.

It's important to note that CDNRECON is a tool for educational purpose and should not be used for any illegal activities. Additionally, it's not guaranteed to be 100% accurate and some websites may have additional security measures in place to prevent this information from being revealed. The author will not be responsible for any misuse of the above information.

Short summary of it's functionality:
- Identify the origin IP behind a CDN's reverse proxy
- Display information about the hosting provider and data center of the website
- Support for both HTTP and HTTPS

A more in-depth chronological order of what it does:
- Checks the target domain nameservers
- Dumps DNS records with DNSDumpster
- Gets subdomains from the target domains SSL certificate
- Gets subdomains with SecurityTrails API
- Checks common subdomains and gets their IP addresses
- Checks if any of the IP addresses belong to Cloudflare
- Checks if any of the IP addresses belong to Akamai
- Checks if any of the IP addresses are using the AkamaiGHost server
- Optionally returns data from Shodan for possibly leaked IP addresses
- Optionally writes the results to target.com-results.txt file

[Installation and usage for Linux](https://github.com/PushkraJ99/CDNRECON#installation-and-usage-for-linux) <br>
[Installation and usage in Windows CMD](https://github.com/PushkraJ99/CDNRECON#installation-and-usage-in-windows-cmd)

 Checking the nameservers, common subdomains and their IP addresses
 ```
    __________  _   ______  ________________  _   __
   / ____/ __ \/ | / / __ \/ ____/ ____/ __ \/ | / /
  / /   / / / /  |/ / /_/ / __/ / /   / / / /  |/ / 
/ /___/ /_/ / /|  / _, _/ /___/ /___/ /_/ / /|  /  
\____/_____/_/ |_/_/ |_/_____/\____/\____/_/ |_/   
                                                   

[i] Checking cloudflare.com nameservers . . .
[+] cloudflare.com is pointing to Cloudflares nameservers
[+] Nameservers: ['ns3.cloudflare.com.', 'ns7.cloudflare.com.', 'ns4.cloudflare.com.', 'ns5.cloudflare.com.', 'ns6.cloudflare.com.']
==================================================
[i] Checking common subdomains . . .
[+] www.cloudflare.com is a valid domain
[+] mail.cloudflare.com is a valid domain
[+] blog.cloudflare.com is a valid domain
[+] support.cloudflare.com is a valid domain
==================================================
[i] Getting subdomain IP addresses . . .
[+] www.cloudflare.com has an IP address of 104.16.124.96
[+] mail.cloudflare.com has an IP address of 216.58.210.147
[+] blog.cloudflare.com has an IP address of 172.64.146.82
[+] support.cloudflare.com has an IP address of 104.18.39.119
==================================================
````
Getting subdomains from the target domains SSL certificate
````
[i] Getting subdomains from juuso.computer's SSL certificate . . .
[i] This might take a while, hang tight
[+] found *.juuso.computer from the SSL certificate
[+] found www.juuso.computer from the SSL certificate
````
<b>Dumping DNS records with DNSDumpster</b>
````
[i] DNSDumpster output for juuso.computer
[+] juuso.computer seems to be valid
````
 Checking if the IP addresses belong to Cloudflare
````
==================================================
[i] Checking if 104.16.124.96 is Cloudflare . . .
[+] 104.16.124.96 is Cloudflare
[+] Ray-ID: 7556c47a2d879914-ARN
[+] Country: Canada
[i] Checking if 216.58.210.147 is Cloudflare . . .
[!] 216.58.210.147 is NOT cloudflare
[i] Checking if 104.18.41.174 is Cloudflare . . .
[+] 104.18.41.174 is Cloudflare
[+] Ray-ID: 7556c47c8bb615dc-ARN
[+] Country: Canada
[i] Checking if 104.18.39.119 is Cloudflare . . .
[+] 104.18.39.119 is Cloudflare
[+] Ray-ID: 7556c47e0d3afe2c-HEL
[+] Country: Canada
  
````
Checking if the IP addresses belong to Akamai and if they're using the AkamaiGHost server
```
[i] Checking if 23.61.197.234 is Akamai . . .
[+] 23.61.197.234 Server detected as AkamaiGHost
[+] Country: Sweden
[i] Checking if 95.101.93.134 is Akamai . . .
[+] 95.101.93.134 Server detected as AkamaiGHost
[+] Country: Sweden
==================================================
````
Returns data for non Cloudflare IP addresses from Shodan
````
[i] Shodan results for 23.61.197.234
[+] ISP: Akamai Technologies, Inc.
[+] Country: Sweden
[+] Hostname(s): ['a23-61-197-234.deploy.static.akamaitechnologies.com', 'kbb.com']
[+] Domain(s): ['akamaitechnologies.com', 'kbb.com']
[+] Open port(s): [80, 443]
[i] Shodan results for 95.101.93.134
[+] ISP: Akamai Technologies, Inc.
[+] Country: Sweden
[+] Hostname(s): ['a95-101-93-134.deploy.static.akamaitechnologies.com', 'kbb.com']
[+] Domain(s): ['akamaitechnologies.com', 'kbb.com']
[+] Open port(s): [80, 443]
````
## Installation and usage For Linux

Requires atleast python version 3.6 since it uses f-strings.
>Tested on Arch Linux. It should work on any Linux distribution and Windows.

Clone the repository
```
$ git clone https://github.com/PushkraJ99/CDNRECON
```
Install the required depencies
```
$ cd CDNRECON
$ pip install https://github.com/PaulSec/API-dnsdumpster.com/archive/master.zip --user
$ pip3 install -r requirements.txt
```
Sample usage guide

```
$ nano config
$ put shodan=API-KEY-HERE on line 1
$ put securitytrails=API-KEY-HERE on line 2
$ CTRL + X to save
$ python3 main.py example.com -c config
```
For more in-depth usage info, supply the -h flag (python3 main.py -h).
````
usage: main.py [-h] [--write] domain [shodan]

CDNRECON - A Content Delivery Network recon tool

positional arguments:
  domain      Domain to scan
  shodan      Your Shodan API key

options:
  -h, --help  show this help message and exit
  --write     Write results to a target.com-results.txt file
````

## Installation and usage in Windows CMD
Requires atleast python version 3.6 since it uses f-strings.
>Tested on Arch Linux. It should work on any Linux distribution and Windows.
Clone the repository
```
$ git clone https://github.com/PushkraJ99/CDNRECON
```
Install the required depencies
```
$ cd CDNRECON
$ pip install https://github.com/PaulSec/API-dnsdumpster.com/archive/master.zip --user
$ pip3 install -r requirements.txt OR pip install -r requirements.txt
```
Sample usage guide
```
$ Open config.txt and Edit With Your API KEYS
$ put shodan=API-KEY-HERE on line 1
$ put securitytrails=API-KEY-HERE on line 2
$ CTRL + S to save config.txt
$ python main.py example.com -c config.txt
```
For more in-depth usage info, supply the -h flag (python3 main.py -h).
````
usage: main.py [-h] [--write] domain [shodan]
CDNRECON - A Content Delivery Network recon tool
positional arguments:
  domain      Domain to scan
  shodan      Your Shodan API key
options:
  -h, --help  show this help message and exit
  --write     Write results to a target.com-results.txt file
````


## How to get a Shodan API key
<b>1. Register an account at https://account.shodan.io/ (it's totally free).<br>
<b>2. Head over the to the "Account" page and see the "API key" field.<br>
<img src="https://a.pomf.cat/nvdiap.png"></img>

## How to get a SecurityTrails API key
<b>1. Register an account at https://securitytrails.com/app/signup (it's totally free).<br>
<b>2. Head over to your account page and see the "API keys" section.<br><br>
<img src="https://a.pomf.cat/phbfgo.png"></img>

## To do
- Add more CDNs
- Add Censys support
- Add certificate search [ DONE ]
- Add IPv4 range bruteforcer
- Add favicon hash search
- Add html body hash search


## 🌐 Socials : Follow Me Here For More

[![Instagram](https://img.shields.io/badge/Instagram-E4405F?style=for-the-badge&logo=instagram&logoColor=white)](https://instagram.com/you_are_not_goodlooking_but_he)

[![Twitter](https://img.shields.io/badge/Twitter-1DA1F2?style=for-the-badge&logo=twitter&logoColor=white)](https://twitter.com/intent/follow?screen_name=PushkraJ99) 

[![Github](https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white)](https://github.com/PushkraJ99)

[![TryHackMe](https://img.shields.io/badge/TryHackMe-%23D42029.svg?logo=TryHackMe&logoColor=white)](https://tryhackme.com/p/PushkaraJ)



## [Snake4Readme](https://github.com/PushkraJ99/Snake4Readme)

<p align="center">
<img src="https://github.com/PushkraJ99/Snake4Readme/blob/main/Snake4Readme/grid-snake.svg">
</p><br>

[![](https://visitcount.itsvg.in/api?id=PushkraJ99&icon=8&color=12)](https://visitcount.itsvg.in)

<p align="center"> 
  <b> Visitor count</b><br>
  <img src="https://profile-counter.glitch.me/PushkraJ99/count.svg" />
</p><br>

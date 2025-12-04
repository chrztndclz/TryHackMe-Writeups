## Introduction

---
#### The Story

The unthinkable has happened - McSkidy has been kidnapped. Without her, Wareville’s defenses are faltering, and Christmas itself hangs by a thread. But panic won’t save the season. A long road lies ahead to uncover what truly happened. The TBFC (The Best Festival Company) team already brainstorms what to do next, and their first lead points to the **tbfc-web01**, a Linux server processing Christmas wishlists. Somewhere within its data may lie the truth: traces of McSkidy’s final actions, or perhaps the clues to King Malhare’s twisted vision for EASTMAS.

#### Learning Objectives

- Learn the basics of the Linux command-line interface (CLI)
- Explore its use for personal objectives and IT administration
- Apply your knowledge to unveil the Christmas mysteries

---

## Linux CLI

---
#### Working With the Linux CLI

_- But, there is no graphical interface (GUI) on the server! How will we look for clues?  
__- Who needs a GUI when we have a Linux command-line terminal? It’s even better!_

Linux has a powerful command-line interface, allowing you to use and manage the system simply by typing commands on your keyboard. It’s not as hard as it sounds - once you get used to it, maybe you’ll like the CLI more than the graphical interface. Not only that, but most experienced IT and cyber security experts work with the CLI every day, so let's start learning

- To run your first CLI command, type `echo "Hello World!"` and press Enter. This will "echo" the text back.

- Then type `ls` to list the contents of the current directory. This command will show you McSkidy's files.

- After that, type `cat README.txt` to display the file contents. You will see its content in the output below.


<img width="695" height="408" alt="image" src="https://github.com/user-attachments/assets/2df7c44e-35b7-4280-925c-c5ecadea7d74" />


**Navigating the Filesystem**

Looks like McSkidy left a security guide before being kidnapped - it would definitely help! You might have noticed the "Guides" directory when you ran ls last time - that's likely the directory we need. Your CLI journey began at McSkidy's home directory (you can verify this by running pwd), but now let's switch to the guides directory.

- Switch the directory by running cd Guides. You will appear at /home/mcskidy/Guides.
  
- Run the ls command again to list the content of the guides directory (it will be empty).

<img width="1064" height="195" alt="image" src="https://github.com/user-attachments/assets/768d4b8b-29e6-4aa2-b2c1-c419c20bdd95" />


**Looking for the Hidden Guide**

Oh-oh, it looks like the guides aren't there. Or are they? In Linux, files and directories can be hidden from plain view if they start with a dot symbol (e.g., .secret.txt). Such a feature is often used by IT administrators to hide system files, by attackers to hide malware, and now by McSkidy to hide the precious guide from bad bunnies!

- View the directory again by running ls -la. The -a flag shows the hidden files. The -l flag shows the additional details, such as file permissions and file owner.
  
- Read the hidden guide by running cat .guide.txt. Don't forget the leading dot.

<img width="802" height="533" alt="image" src="https://github.com/user-attachments/assets/27aceed1-555c-45f0-8acb-9c18becea9f2" />


**Grepping the Logs**

In her guide, McSkidy refers to /var/log/, a Linux directory where all security events (logs) are stored. Indeed, every SOC analyst at TBFC will confirm that the best way to find evil bunnies is to check the logs. Log files are usually very big, and looking through them with cat is not ideal. Thus, let's use grep, a command to look for a specific text inside a file.

- Navigate to the logs directory with cd /var/log and explore its content with ls.
  
- Run grep "Failed password" auth.log to look for the failed logins inside the auth.log.

<img width="1053" height="193" alt="image" src="https://github.com/user-attachments/assets/43dcb7f9-d178-42bf-89d6-34ccf8a8b129" />


**Finding the Files**

You can see a lot of failed logins on the "socmas" account, all from the HopSec location! They were clearly trying to break into SOC-mas, Wareville's Christmas ordering platform. What if bad bunnies left some malware there? Let's follow McSkidy's guide and look for Eggsploits and Eggshells with find - a command that searches for files with specific parameters, such as -name:

- Run find /home/socmas -name *egg* to search for "eggs" in the socmas home directory.
  
- Note that find is a powerful command. Check out its documentation for more details.


<img width="1062" height="198" alt="image" src="https://github.com/user-attachments/assets/8c08f522-c685-48b3-a447-d730f912a86b" />


**Analyzing the Eggstrike**

Looks like you found something, eggstrike.sh! Files with the .sh extension contain CLI commands and are called shell scripts. Such scripts are used both by IT teams to automate things and by attackers to quickly run malicious commands. Let's display the suspicious script's content and try to understand it:


<img width="1066" height="373" alt="image" src="https://github.com/user-attachments/assets/c390563f-ce29-4713-8e05-7316bf6bc34f" />


1. The lines starting with # are just comments and are not the actual commands.
2. The cat wishlist.txt | sort | uniq lists unique items from the wishlist.txt.
3. The command then sends the output (unique orders) to the /tmp/dump.txt file.
4. The rm wishlist.txt deletes the wishlist file (containing Christmas wishes).
5. The mv eastmas.txt wishlist.txt replaces the original file with eastmas.txt.


**CLI Features**

The Eggstrike script you read seems to be stealing Christmas wishes and replacing them with the fake ones! You might have noticed that the commands in the script are a bit complex, but that's not unusual since the script author is no other than Sir Carrotbane, the leader of HopSec's red team. Let's explore the special symbols below:


**Pipe symbol (|)**
Send the output from the first command to the second
cat unordered-list.txt | sort | uniq

**Output redirect (>/>>)**
Use > to overwrite a file, and >> to append to the end
some-long-command > /home/mcskidy/output.txt

**Double ampersand (&&)**
Run the second command if the first was successful
grep "secret" message.txt && echo "Secret found!"

---

Sir Carrotbane Attacks

Now it is clear that the server has been breached, and the Christmas wishlist has been replaced with an EASTMAS one. Although you found no clue of what happened to McSkidy, at least you know the attackers were there. You can see how Sir Carrotbane replaced the wishlist by visiting http://10.49.177.240:8080 from the VM's web browser. You can open it by clicking the Firefox icon on the Desktop.


**System Utilities**

There are hundreds of CLI commands to view and manage your system. For example, uptime to see how much time your system is running, ip addr to check your IP address, and ps aux to list all processes. You may also check the usernames and hashed passwords of users, such as McSkidy, by running cat /etc/shadow. However, you'd need root permissions to do that.


<img width="1064" height="241" alt="image" src="https://github.com/user-attachments/assets/334f66fe-57a7-4869-aee4-2343d9be33a4" />


**Root User**

Root is the default, ultimate Linux user who can do anything on the system. You can switch the user to root with sudo su, and return back to McSkidy with the exit command. Only root can open /etc/shadow and edit system settings, so this user is often a main target for attackers. If at any moment you want to verify your current user, just run whoami!

- Switch to the root user by running the sudo su command.
  
- You can verify your current user by running whoami.


<img width="1062" height="243" alt="image" src="https://github.com/user-attachments/assets/0d1c88c1-c6d3-4fe1-9e02-30983673581c" />


**Bash History**

Did you know that every command you run is saved in a hidden history file, also called Bash history? It is located at every user's home directory: /home/mcskidy/.bash_history for McSkidy, and /root/.bash_history for root, and you can check it with a convenient history command, or just read the files directly with cat. Let's check if Sir Carrotbane with his bad bunnies left their traces in history!

- Familiarize yourself with Bash history by running the history command.
  
- Note that your commands are also saved to a file (cat .bash_history).


<img width="733" height="471" alt="image" src="https://github.com/user-attachments/assets/71f7a98f-0063-446c-a584-e3e941af766d" />


---

#### Objectives

Which CLI command would you use to list a directory?

`ls` 


<img width="804" height="554" alt="image" src="https://github.com/user-attachments/assets/25808d77-5580-49fd-b78b-849b5db02e75" />

`THM{learning-linux-cli}`

---

Which command helped you filter the logs for failed logins?

`grep` 


<img width="1057" height="363" alt="image" src="https://github.com/user-attachments/assets/26dbfd17-78c2-4574-85d7-eac678a3b4ec" />

`THM{sir-carrotbane-attacks}`

---

Which command would you run to switch to the root user?

`sudo su`

---

Finally, what flag did Sir Carrotbane leave in the root bash history?

<img width="1059" height="381" alt="image" src="https://github.com/user-attachments/assets/b1427421-af02-4cd8-8fc3-f1590d5c788e" />

`THM{until-we-meet-again}`



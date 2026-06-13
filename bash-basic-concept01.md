# My research and basic understanding of bash </br>  
I came across an social media post / article (sadly, I did not bookmark) with this bash code **find / -xdev -printf '%h\n'** </br>  
I wanted to find out what this command will do and I explored more about what **'%h\n'** will do. </br>  

## The questions and the learning that followed are documented here: </br>  
I ran the command **find / -xdev -printf %h \n** without the single quotes and leave a blank space betwee **%h** and **\n**, and the command resulted in error. </br>  
The question why **bash** is throwing this error, and the search that followed made me realise that it is not **bash** that is throwing the error, it is the **find** command that is. </br>  
I understood that, **bash** treats the any content encapsulated within single quotes as text - plain simple text - meaning **bash** does not act on it. </br>  

## Bash does not validate the command: </br>  
I understood that bash does not validate **%h \n** - instead it just reads **%h**, followed by a blank space, followed by a **\n** which bash interprets as **n**. </br>  
Bash sends **find** **-printf %h n** and the **find** program does NOT understand why there are two arguments to **-printf** namely **%h** and **n**. </br>  
Hence, it is actually the **find** command that throws the error. </br>  
This lead to another question. </br>  

## If it is the **find** program that throws the error, what exactly does **bash** do? </br>  
The search for this answer lead to some important learning. </br>  
  ### The sequence of events: </br>
  1. When I type a command - for example -  **find /home/as/Documents**, the **Shell** (bash in this case) parses the line.
  2. Then **bash** has to find where the command **find** lives, and bash finds it using the **PATH** variable. So now **bash** knows **find** lives at **/usr/bin/find**. </br>
  3. Now that **bash** knows where **find** is, **bash** does NOT directly invoke the command **find** and do the job. </br>
  4. What happens next is called a **Kernel System Call** called **fork()**. **Bash** makes a clone of itself and make the clone a **Child Process**. </br>
  5. The **Child Process** now becomes the **find** program and executes the line **find /home/as/Documents**. </br>
  6. While this is happening, **bash** executes another **System Call** called **wait()** and waits for the **Child Process (find)** to complete and exit. </br>
  7. The **Child Process (find)** finishes the task, **prints** the output to **stdout** and **exits** with an **exit code**. </br>  
  8. Once the **Child Process (find)** exits, **kernel** tears down **find**.s memory, **bash** comes out of the **wait()** state, stores the **exit code** in **$?** and prints a new prompt. </br>  
  9. **SO THIS IS WHAT EXACTLY HAPPENS WHEN WE RUN A COMMAND** </br>
This lead to another question. </br>

## What does it mean by **bash** executes **wait(). What is bash waiting for? </br>  
This question lead to another important learning. </br>  
1. When I open a **terminal/terminal emulator** it actually executing the program **/usr/bin/bash**. </br>  
2. Whenerver a program is executed there is a **process** that is running until the program is closed. </br>
3. Every **process** has a **process ID (PID)**. </br>
4. So whenever I open a **terminal window**, the **Shell (bash)** is running and expecting me to type something. </br>
5. Say I typed the same command **find /home/as/Documents**, as explained earlier, **bash** created a **Child Process**, executes **wait()**, **Child Process** exits, and finally, **bash** prints a prompt. </br>
   ### The sequence of these events is explained using the example below: </br>
   root           1       0  1 18:10 ?        00:00:01 /sbin/init splash  
   as          1690       1  3 18:11 ?        00:00:00 /usr/lib/systemd/systemd --user  
   as          2658    1690  8 18:11 ?        00:00:00 /usr/libexec/gnome-terminal-server  
   as          2665    2658  0 18:11 pts/0    00:00:00 bash  
   as          2677    2665  0 18:12 pts/0    00:00:00 ps -ef </br>  
Let's take a look at **line 3** - That is the process when I open the **terminal window**. The process ID **PID** is **2658**. </br>  
When I opened the **terminal** it has executed **bash (/usr/bin/bash)** as a **Child Process** with process ID **PID** **2665** and becomes the parent of this **Child Process**. </br>  
As displayed in **line 4** - **bash** - the **Child Process** has a parent process ID **PPID** of **2658**. </br>  
Now I run the command **ps -ef** and the **Shell (bash)** creates a clone of itself, makes the clone its **Child Process** with process ID **PID** **2677** and goes to **wait()**. </br>  
The **Child Process** is going to execute **/usr/bin/ps** and become the **ps** command, executing the ps command with the options **-ef**. </br>  
Note that the **Child Process** now has the parent process ID **PPID** of **2665**. </br>  
Once the **ps -ef** is executed, the **Child Process** prints the output to **stdout** and exits with **exit code 0**. </br>  
The parent process **bash** which has been in **wait()** comes back, stores the **exit code 0** in **$?** and prints a new prompt. </br>
Just because the **ps** command has finished running does **NOT** mean that the **Shell (bash)** is also done. </br>
**Bash** is still running as a process and waits for the user's input. </br>

The image below shows the tasks that were explained above: </br>  
<img width="723" height="610" alt="image" src="https://github.com/user-attachments/assets/d1d0d28b-2354-439d-97cd-e1a1d56fb1a5" />  

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


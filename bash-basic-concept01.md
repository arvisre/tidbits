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

## Scenario: Let's say my current working directory is /home/as/Downloads. There is a sub-directory and files inside Downloads. </br>  

## Action: I'm tryin to run the **"rm -r ."** command and expect it to delete both the Downloads directory and its sub-directory and files. </br>  

## Result:</br>  
### 1) as@asdfed:~/Downloads$ rm -r .
rm: refusing to remove '.' or '..' directory: skipping '.'  
as@asdfed:~/Downloads$ ls -l  
total 36  
-rw-r--r--. 1 as as 13600 May 14 21:25  001.png  
-rw-r--r--. 1 as as 13594 May 14 21:25  002.png  
drwxr-xr-x. 2 as as  4096 May 16 18:11 'Telegram Desktop' </br>  

### 2) as@asdfed:~/Downloads$ rm -r ./  
rm: refusing to remove '.' or '..' directory: skipping './'  
as@asdfed:~/Downloads$ ls -l  
total 36  
-rw-r--r--. 1 as as 13600 May 14 21:25  001.png  
-rw-r--r--. 1 as as 13594 May 14 21:25  002.png  
drwxr-xr-x. 2 as as  4096 May 16 18:11 'Telegram Desktop' </br>  

## 1 and 2 - The Documents directory itself will NOT be deleted — because I'm currently inside it, the OS keeps it open/in-use, so rm cannot remove the directory entry itself. </br>  

### 3)as@asdfed:~/Downloads$ rm -r ./*  
as@asdfed:\~/Downloads$ ls -l  
total 0  
as@asdfed:\~/Downloads$ </br>    

### 4)as@asdfed:\~/Downloads$ ls -l  
total 4  
-rw-r--r--. 1 as as    0 May 20 14:20  001.png  
-rw-r--r--. 1 as as    0 May 20 14:20  002.png  
drwxr-xr-x. 2 as as 4096 May 20 14:22 'Telegram Desktop' </br>  
### as@asdfed:\~/Downloads$ rm -r /home/as/Downloads  
as@asdfed:\~/Downloads$ ls -l  
total 0  
as@asdfed:\~/Downloads$ cd ..  
as@asdfed:~$ ls -l  
total 40  
drwxr-xr-x.  2 as as 4096 May  6 21:55 Desktop  
drwxr-xr-x.  3 as as 4096 May 20 14:05 Documents  
drwxr-xr-x.  2 as as 4096 May 11 13:50 ISOs  
drwxr-xr-x. 50 as as 4096 May 11 23:46 MEGA  
drwxr-xr-x.  2 as as 4096 May  6 21:55 Music  
drwxr-xr-x.  3 as as 4096 May 19 14:35 Pictures  
drwxr-xr-x.  2 as as 4096 May  6 21:55 Public  
drwx------.  2 as as 4096 May 10 20:28 snap  
drwxr-xr-x.  2 as as 4096 May  6 21:55 Templates  
drwxr-xr-x.  2 as as 4096 May  6 21:55 Videos </br>  

## 4 - Linux allows me to delete a directory I'm currently inside by providing its absolute path as the argument.

## I'm new to GitHub and Markdown file format. I just learned that a single tilde (representing the home directory) can be processed as a strike-through operator.  
## I learned that to workaround this, I have to prefix the tile with a backslash character. Uffff.  

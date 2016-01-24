[back to overwiev](/../..)

# Command Line Cheatsheet

##### Table of Contents  
[Basics](#basics)  
[Showing & Navigating](#showing--navigating)  
[Copying/Moving/Removing](#copyingmovingremoving)  
[Input & Output Redirects](#input--output-redirects)  
[Sorting & Filtering](#sorting--filtering)  
[Environment](#environmen)  

## Basics
**clear** –– clears the terminal
**nano** filename –– opens the nano text editor for specific file (optional)
^ –– in nano stands for ctrl
~ –– usually represents the users home directory

## Showing & Navigating
**ls** –– lists all files (folders are called directories. files and directories are structured in a file system)  
**ls -a** –– lists all files + hidden files (-a is called an option)  
ls -t –– orders by time last added  
ls -l –– with long description (access-rights, n# of hard links, owner, group-name, size in bytes, date-last-modified, name)  
ls -alt –– -a, -t & -l together  
**pwd** –– print working directory path (the directory you’re currently in)  
**cd directory-name** –– change directory  
cd folder/childfolder –– two down  
cd .. –– one up – cd folder/folder –– two down  
**mkdir name** –– make directory  
**touch name** –– creates a new file inside the working directory  
**cat** filename –– outputs the content of a file to the console  
**wc** filename –– outputs the number of lines, words, and characters in file  

## Copying/Moving/Removing
**cp** file-to-copy place-to-paste –– copies files and paste them somewhere else  
cp file1 file2 file3 place-to-paste –– you can copy multiple files at once  
cp * place-to-paste –– copy all files from working directory  (special characters like * are called wildcard, when you combine it with .txt for example would mean any file that ends with .txt)    
**mv** file-to-move-1 place-to-move –– moves a file  
mv file-to-rename new-filename –– renames a file  
**rm** file-to-remove –– removes a file or directory !permanently!  
rm -r dir-to-remove –– removes a dir and all its children !permanently!  

##Input & Output Redirects
echo "Hello" > world.txt –– **>** redirects the standard output (Hello) into something else (world.txt)  
cat file1.txt > file2.txt –– content of file1 is redirected into file2 which overrides the content of file2 by the content of file1  
cat file1.txt >> file2.txt –– **>>** appends the output of file1 to file2  
cat < file.txt –– takes the output from the right and inputs it into the left  
cat file.txt | wc –– **|** is a "pipe". The | takes the standard output of the command on the left, and pipes it as standard input to the command on the right. You can think of this as "command to command" redirection.  
cat text1.txt | wc | cat > text2.txt –– multiple pipes can be combined here the output of text1.txt is given to wc whose output is given to cat which places it into text.txt

##Sorting & Filtering
**sort** filename –– outputs the sortet content of the file  
cat filename | sort > nameofsortedfile –– outputs the content of filename binds it to the sort function that then redirect that sorted output into a file  
**uniq** filename –– filters out adjacent, duplicate lines in a file  
sort filename | uniq > uniqfilname –– sorts a file "pipes" (|) the sorted content to uniq which filters out all adjacent duplicates and gives the result back to another file  
**grep** keyword filename –– searches a file for lines that have the keyword and output the result (it is case sensitive)  
grep -i keyword filename –– here grep is case insensitive  
grep -R keyword folder –– searches all files in a directory and outputs filenames and lines containing matched results  
grep -Rl keyword folder –– searches all files in a directory and outputs only filenames with matched results  
**sed** 's/keyword/replacement' filename –– find in file and replace s = substitution. So sed 's/find/replace' in_this_file (only replace the first finding)  
sed 's/keyword/replacement/**g**' filename –– same as above but replaces all instances not only the first finding  

##Environment
**nano** ~/.bash_profile –– creates a file to store environment settings. Whenever a session starts it will load it's content.
(within bash_profile) **alias** pd="pwd" –– create keyboard shortcuts, for commonly used commands.
(within bash_profile) **export** USER="username" –– create a variable that can be accessed by $USER
(within bash_profile) export PS1=">> " –– to change the command prompt from $ to >>
**source** ~/.bash_profile –– makes changes available without the fuzz to open a new console.
**env** –– return a list of environment variables
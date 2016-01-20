# Command Line Cheatsheet

## Basics

### Showing & Navigating
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

### Copying/Moving/Removingm
**cp** file-to-copy place-to-paste –– copies files and paste them somewhere else  
cp file1 file2 file3 place-to-paste –– you can copy multiple files at once  
cp * place-to-paste –– copy all files from working directory  (special characters like * are called wildcard)  
cp a*.txt place-to-paste –– copy all files starting with a ending with .txt  
**mv** file-to-move-1 place-to-move –– moves a file  
mv file-to-rename new-filename –– renames a file  
**rm** file-to-remove –– removes a file or directory !permanently!  
rm -r dir-to-remove –– removes a dir and all its children !permanently!  
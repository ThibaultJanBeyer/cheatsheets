[back to overwiev](/../..)

#Ruby Cheatsheet

##### Table of Contents  
[Vars & Arrays](#vars-arrays)  
[Calculation](#calculation)  
[Comment](#comment)  
[Conditions](#conditions)  
[Printing](#printing)  
[User Imput](#user-imput)  
[Loops](#loops)  
[Removing](#removing)  
[Ignoring](#ignoring)
[Renaming](#renaming) 

*$ irb –– to write ruby in the terminal*

##Basics
##Vars & Arrays:
my_variable = “Hello”  
my_variable.capitalize! –– ! changes the value of the var same as my_name = my_name.capitalize  
my_array = &lsqb;1,2&rsqb;
####Functions to create Arrays
"bla,bla".split(“,”) –– takes sting and returns an array (here  &lsqb;"bla","bla"&rsqb;)

##Calculation:
Addition (+)  
Subtraction (-)  
Multiplication (*)  
Division (/)  
Exponentiation (**)  
Modulo (%)  
you can do 1 += 1 –– which gives you 2 but 1++ and 1-- does not exist in ruby

##Comment:
**=begin**  
Bla  
**=end**  
**&num;** bla

##Conditions:
**if** 1 < 2  
puts “one smaller than two”  
**elsif** 1 > 2 –– *careful not to mistake with else if. In ruby you write elsif*  
puts “elsif”  
**else**  
puts “false”  
**end**  

**unless** false –– unless checks if the statement is false (opposite to if).  
puts “I’m here”  
**else**  
puts “not here”  
**end**  

**&&** –– and  **||** –– or  **!** –– not  
**(x && (y || w)) && z** –– use parenthesis to combine arguments  
problem = false  
print "Good to go!" unless problem –– prints out because problem != true  

##Printing:
**print “bla”**  
**puts “test”** –– puts the text in a separate line

##String Methods
“Hello”**.length** –– 5  
"Hello”**.reverse** –– “olleH”  
"Hello”**.upcase** –– “HELLO”  
"Hello”**.downcase** –– “hello”  
“hello”**.capitalize** –– “Hello”  
“Hello”**.include? “i”** –– equals to false because there is no i in Hello  
“Hello”**.gsub!(/e/, “o”)** –– Hollo  

##User Imput
**gets** –– is the Ruby equivalent to prompt in javascript (method that gets input from the user)
gets.chomp –– removes extra line created after gets (usually used like this)

##Loops
**While loop:**  
i = 1  
**while** i < 11  
  puts i  
  i = i + 1  
**end**  

**Until loop:**  
i = 0  
**until** i == 6  
puts i  
i += 1  
**end**  

**For loop**  
**for** i **in** 1**...**10 – ... tells ruby to exclude the last number (here 10 if we .. only then it includes the last num)  
  puts i  
**end**  

**Loop iterator**  
i = 0  
**loop do**   
i += 1  
print "I'm currently number #{i}” –– a way to have ruby code in a string   
**break** if i > 5  
**end**  

**Next**  
**for** i **in** 1**..**5  
**next if** i % 2 == 0 –– If the remainder of i / 2 is zero, we go to the next iteration of the loop.  
print i  
**end**  

**.each**  
objects**.each do |**item**|** –– for each object in objects do something while storing that object in the variable item  
  print “#{item}"  
**end**  

**.times**  
10**.times do**  
  print “this text will appear 10 times”  
**end**  


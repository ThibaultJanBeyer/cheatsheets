[back to overwiev](/../..)

#Ruby Cheatsheet

##### Table of Contents  
[Vars, Arrays & Hashes](#vars-arrays--hashes)  
[Methods](#methods)  
[Calculation](#calculation)  
[Comment](#comment)  
[Conditions](#conditions)  
[Printing](#printing)  
[User Imput](#user-imput)  
[Loops](#loops)  

##Basics
*$ irb –– to write ruby in the terminal*  
*don't use ' in ruby, use " instead*  
*you can replace most {} with do end and vice versa –– not true for hashes or #{} escapings*  
*Best Practice: end names that produce booleans with question mark*  
##Vars, Arrays & Hashes
```Ruby
my_variable = “Hello”  
my_variable.capitalize! # ! changes the value of the var same as my_name = my_name.capitalize
```
```Ruby  
my_array = [a,b,c,d,e]  
my_array[1] –– b  
my_array[2..-1] # c , d , e  
multi_d = [[0,1],[0,1]]
```
```Ruby  
hash = { "key1" => "value1", "key2" => "value2" } # same as objects in JavaScript
my_hash = Hash.new # same as my_hash = {} # set a new key like so: pets["Stevie"] = "cat"
pets["key1"] # value1
pets["Stevie"] # cat
```

####Functions to create Arrays
```Ruby
"bla,bla".split(“,”) # takes sting and returns an array (here  ["bla","bla"])
```

##Methods
```Ruby
def greeting(hello, *names) # *name is a splat argument, takes several parameters passed in an array
  return "#{hello}, #{names}"
end

start = greeting("Hi", "Justin", "Maria", "Herbert") # call a method by name
puts start
```

##Calculation
Addition (+)  
Subtraction (-)  
Multiplication (*)  
Division (/)  
Exponentiation (**)  
Modulo (%)  
you can do 1 += 1 –– which gives you 2 but 1++ and 1-- does not exist in ruby

##Comment
```Ruby
=begin  
Bla
Multyline comment  
=end
```  
```Ruby
# single line comment
```

##Conditions
**IF**
```Ruby
if 1 < 2  
puts “one smaller than two”  
elsif 1 > 2 # *careful not to mistake with else if. In ruby you write elsif*  
puts “elsif”  
else  
puts “false”  
end
```  
**unless**
```Ruby
unless false # unless checks if the statement is false (opposite to if).  
puts “I’m here”  
else 
puts “not here”  
end
```  

**&&** –– and  **||** –– or  **!** –– not  
**(x && (y || w)) && z** –– use parenthesis to combine arguments  
problem = false  
print "Good to go!" unless problem –– prints out because problem != true  

##Printing
```Ruby
print “bla”  
puts “test” # puts the text in a separate line
```

##String Methods
```Ruby
“Hello”.length # 5  
"Hello”.reverse # “olleH”  
"Hello”.upcase # “HELLO”  
"Hello”.downcase # “hello”  
“hello”.capitalize # “Hello”  
“Hello”.include? “i” # equals to false because there is no i in Hello  
“Hello”.gsub!(/e/, “o”) # Hollo
```  

##User Imput
```Ruby
gets # is the Ruby equivalent to prompt in javascript (method that gets input from the user)
gets.chomp # removes extra line created after gets (usually used like this)
```

##Loops
**While loop:**  
```Ruby
i = 1  
while i < 11  
  puts i  
  i = i + 1  
end  
```

**Until loop:**  
```Ruby
i = 0  
until i == 6  
  puts i  
  i += 1  
end
```  

**For loop**  
```Ruby
for i in 1...10 # ... tells ruby to exclude the last number (here 10 if we .. only then it includes the last num)  
  puts i  
end  
```

**Loop iterator**  
```Ruby
i = 0  
loop do
  i += 1  
  print "I'm currently number #{i}” # a way to have ruby code in a string   
  break if i > 5  
end  
```

**Next**  
```Ruby
for i in 1..5  
  next if i % 2 == 0 # If the remainder of i / 2 is zero, we go to the next iteration of the loop.  
  print i  
end  
```

**.each**  
```Ruby
things.each do |item| # for each things in things do something while storing that things in the variable item  
  print “#{item}"  
end  
```
on hashes like so:  
```Ruby
hashes.each do |x,y|
  print "#{x}: #{y}"
end
```

**.times**  
```Ruby
10.times do  
  print “this text will appear 10 times”  
end  
```

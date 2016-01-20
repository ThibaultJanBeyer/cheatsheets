[back to overwiev](..)

#This .md has to be styled

Ruby

$ irb –– to write ruby in the terminal

Basics
Vars:
my_name = “Tibo”
my_name.capitalize! –– ! changes the value of the var
Array:
my_array = [1,2]

Functions to create Arrays
text.split(“,”) –– takes sting and returns an array (here split up text whenever you see a , )

Calc:
Addition (+)
Subtraction (-)
Multiplication (*)
Division (/)
Exponentiation (**)
Modulo (%)
i = i + 1 –– i += 1 –– i *= 2 –– i /= 2 –– i -= 1 (++ and -- does not exist in ruby)

Comment:
=begin
Bla
=end
# bla

Conditions:
if 1 < 2 puts “true” elsif 1 > 2 puts “elsif” else puts “false” end
unless false puts “I’m here” else puts “not here” end –– unless checks if the statement is false (opposite to if).
&& –– and  || –– or  ! –– not
(x && (y || w)) && z –– use parenthesis to combine arguments
problem = false 
print "Good to go!" unless problem –– prints out because problem != true

Printing:
print “bla”
puts “test” –– puts the text in a separate line

String Methods
“Tibo”.length –– 4
"Tibo”.reverse –– “obiT” 
"Tibo”.upcase –– “TIBO” 
"Tibo”.downcase –– “tibo”
“tibo”.capitalize –– “Tibo”
“tibo”.include? “i” –– equals to true because i in tibo
“tibo”.gsub!(/i/, “o”) –– tObo

User Imput
gets –– is the Ruby equivalent to prompt in javascript (method that gets input from the user)
gets.chomp –– removes extra line created after gets (usually used like this)

Loops
While loop
i = 1
while i < 11
  puts i
  i = i + 1
end

Until loop
i = 0
until i == 6
i += 1
end
puts i

For loop
for i in 1…10 – … tells ruby to exclude the last number (here 10 if we .. only then it includes the last num)
  puts i
end

Loop iterator
i = 0
loop do 
i += 1
print "#{i}”
break if i > 5
end

Next
for i in 1..5
next if i % 2 == 0 –– If the remainder of i / 2 is zero, we go to the next iteration of the loop.
print i
end

.each
object.each do |item|
  print “#{item}"
end

.times
10.times do
  print “this text will appear 10 times”
end
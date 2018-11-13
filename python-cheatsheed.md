## Integer and Float Calc

```python
100/6 = 16 # python keeps the number type
100./6. = 16,6666666668 # . transforms the integer in a float
float(100)/6 = 16,6666666668 # same
100/6 = 16,6666666668 # in python3 the conversion happens automatically
```

## Strings

```python
my_string = 'string'

my_string = "string"

my_string = """multy
Line
string
"""
```

### Formatting

```python
day = 6
print "03 - %s - 2019" %  (day)
# 03 - 6 - 2019
print "03 - %02d - 2019" % (day)
# 03 - 06 - 2019
```

`%02d`: The 0 means "pad with zeros", the 2 means to pad to 2 characters wide, and the d means the number is a signed integer (can be positive or negative).

```python
print "Let's not go to %s. 'Tis a silly %s." % (string_1, string_2)
# Let's not go to Camelot. 'Tis a silly place.
```

### Methods

```python
len("foo") #=> 3
"FOO".lower() #=> "foo"
"foo".upper() #=> "FOO"
str(3) #=> "3"
```

```python
"J123".isalpha()
```

`.isalpha()`: check that a string doesn't contain any non-letter characters

```python
"moo"[1:len(new_word)] #=> "oo"
```

## Comments

```python
# I’m a single line comment
"""
I’m a multi
line comment
"""
```

## Booleans

```python
a = True
b = False
```

## Conditions

```python
c = 1 < 2 and 2 < 3 #=> True
d = 1 > 2 or 2 > 3 #=> False
e = not False #=> True
e = not not False #=> False
```

```python
def greater_less_equal_5(answer):
    if answer > 5:
        return 1
    elif answer < 5:
        return -1
    else:
        return 0

print greater_less_equal_5(4) #=> -1
print greater_less_equal_5(5) #=> 0
print greater_less_equal_5(6) #=> 1
```

## Type Conversions

```python
"I’m " + str(13) # => "I’m 13"
3 + int("7") # => 10
3 + int(7.9) # => 10
```

## Console

```python
foo = raw_input("What is your name? ")
```

Prompts for user input in console

## Date & Time

```python
from datetime import datetime

now = datetime.now()
print now.year #=> 2018
print now.month #=> 11
print now.day #=> 8
print '%02d/%02d/%04d' % (now.month, now.day, now.year)
# will print the current date as mm/dd/yyyy => 11/08/2018
print '%02d:%02d:%02d' % (now.hour, now.minute, now.second)
#=> 11:55:46
```

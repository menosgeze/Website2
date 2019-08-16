# Some Basic Solutions

## Exercise 1
This is taken from the book *Cracking the Code Interview*. 
Assume I want to check if a string has a character repeated.
* I could assume there are --M-- different characters. For example, ASCII has 128 characters. 
* I could compute the length --L-- of the string. 

The following solution assumes that we don't wish to change our argument, so it creates an integer variable, which will could be substituted with a list. This integer has binary length less than --M--, so it could be stored in an integer or a big integer, and it is not memory expensive. 
{{<highlight python>}}
def has_unique_chars(mystr):
    test = 0
    for mychar in mystr:
        if test & 2**ord(mychar) == 2**ord(mychar):
            return False
        else:
            test = test | 2**ord(mychar)
    return True
{{</highlight>}}

Alternatively, we can allow to change the string argument, and just do:
{{<highlight python>}}
def has_unique_chars(mystr):
    mystr = sorted(mystr)
    for i in range(1, len(mystr)):
        if mystr[i] == mystr[i-1]:
            return False
    return True
{{</highlight>}}

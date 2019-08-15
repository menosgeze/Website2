# Some Basic Solutions

## Exercise 1
This is the Ch.1.Ex.1 from the book *Cracking the Code Interview*. 
Assume I want to check if a string has a character repeated.
* I could assume there are $M$ different characters. For example for ASCII has
  128 characters. 
* I could compute the length $L$ of the string. 

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

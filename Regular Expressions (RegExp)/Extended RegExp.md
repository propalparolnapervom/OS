# Extended Regual Expression

> NOTE: This extends functionality of `basic regular expressions` from another block

[Example of usage](https://kodekloud.com/topic/extended-regular-expressions/)

[RegExp online](https://regexr.com/)

## Match the previous element 1 or more times: '+'

> **NOTE**: Confusing fact: In `basic regular expressions` we're looking at here some symbols, including '+' has to be escaped by '\' to turn to operator
> 
> Using of `extended regular expressions` eliminates this confusion

```
cat test.txt 
0
1
2
00
0+
0000000


###############
# Basic RegExp
###############

# Here '+' is treated as regular symbol, no the operator
cat test.txt | grep '0+'
0+

# Here '+' is treated as operator with meaning "1 or more times"
cat test.txt | grep '0\+'
0
00
0+
0000000


##############################
# Extended RegExp
#
# (see appropriate document,
# only small example is here)
##############################

# Use '-E' flag to point to extended RegExp use: to eliminate escaping
cat test.txt | grep -E '0+'
0
00
0+
0000000

# Use `egrep` instead of `grep` tool, to point to extended RegExp use: to eliminate escaping
cat test.txt | egrep '0+'
0
00
0+
0000000
```

## Previous element can exist "this many" times: '{}'

The line contains "0" `miminum` "3" times (so, or more)
```
cat test.txt 
asdf 00
fda 000 dnejd
a 00000 kdks
nken 0000 ns
ls asdf ls 0 lllsdf

cat test.txt | egrep '0{3,}'
fda 000 dnejd
a 00000 kdks
nken 0000 ns
```

The line contains "1", followed by "0" `maximum` "3" times (so, or less till the 0 times)
```
cat test.txt | egrep '10{,3}'
```

The line contains "0" `exactly` "3" times
```
cat test.txt | egrep '0{3}'
```

The line contains "0" `min` "3" times, `max` "5" times
```
cat test.txt | egrep '0{3,5}'
```



## Make the previous element optional (so, `0` or `1` times): '?'

```
cat test.txt 
disable
disabled
disables

# The 'd' symbol here might be presend or might be absent
cat test.txt | egrep 'disabled?'
disable
disabled
disables

```


## Match one thing or the other: '|'

```
cat test.txt 
disabled
asdf
enabled
pjene

cat test.txt | egrep 'enabled|disabled'
disabled
enabled
```

```
cat test.txt 
disabled
asdf
enabled
pjene
asdf disables kwnek

cat test.txt | egrep 'enabled?|disabled?'
disabled
enabled
asdf disables kwnek
```

## Ranges and Sets: '[]'

Range - symbol is in the range 
```
cat test.txt 
this is disabled
the amount is 2334
1234 34 433

# The line should contain small symbols
cat test.txt | egrep '[a-z]'
this is disabled
the amount is 2334

# The line should contain numbers
cat test.txt | egrep '[0-9]'
the amount is 2334
1234 34 433

```

Set - symbol is within explicitly specified charater list
```
cat test.txt 
this is disabled
the amount is 2334
1234 34 433

# The line should contain wether `d` or `1`
cat test.txt | egrep '[d1]'
this is disabled
1234 34 433



cat test.txt 
the dog
and cat
and paper cut

# The line should contain `cat` or `cut`
cat test.txt | egrep 'c[au]t'
and cat
and paper cut
```

NOT in the range
```
cat test.txt 
website 1: http://non-encrypted.com
website 2: https://encrypted.com

# Find lines with `http`, but not `https`
cat test.txt  | egrep 'http[^s]'
website 1: http://non-encrypted.com




cat test.txt 
onlysmallcharacters
ONLYBIGCHARACTERS
smallandBIGCHARACTERS
smallcharactersandnumbers1234

# The lines, where present any non-small characters
cat test.txt  | egrep '[^a-z]'
ONLYBIGCHARACTERS
smallandBIGCHARACTERS
smallcharactersandnumbers1234
```

Find devices. It's usual name is something like `/dev/sdd` or `/dev/twa0`
```
egrep -r '/dev/[a-z]*[0-9]?' /etc/
```

## Subexpressions: '()'

```
# Only small symbols, 0 or many times
[a-z]*

# Only small symbols
#    OR
# Only big symbols, 0 or many times
[a-z]|[A-Z]*

# small or big symbols, 0 or many times
([a-z]|[A-Z])*

```



























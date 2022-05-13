

## The line begins with: '^'
The file
```
cat test.txt
asam
samanta
samsa
```

The line begins with `sam`
```
cat test.txt | grep '^sam'
samanta
samsa
```

## The line ends with: '$'

The line ends with `sam`
```
cat test.txt | grep 'sam$'
asam
```

## Match any ONE character: '.'

The file
```
cat test.txt 
cute samanta
samsa cut them
semass  
```

The line matches any 1 character: the whole sequence **can** be inside any other lines
```
cat test.txt | grep 'c.t'
cute samanta
samsa cut them
```

The line matches any 1 character: the whole sequence **must** be separate word
```
cat test.txt | grep -w 'c.t'
samsa cut them
```

## Escape special characters: '\'

The line contains actual '.' instead of "any ONE character", for example.

Just '.' returns whole file, as it any random character on any position.
```
cat test.txt | grep '.'
cute samanta
samsa. cut them
semass  
```

But escaped '\.' returns only line with '.'
```
cat test.txt | grep '\.'
samsa. cut them
```


## Match the previous element 0 or more times: '*'

The file
```
cat test.txt 
let
lett
lettt
lets do this
```

Find `let`, where `t` then appears 0, 1 or 2 times
```
cat test.txt | grep 'let*'
let
lett
lettt
lets do this
```

The line with word inside, which starts with `/`, has some 0 or more amount of random symbols inside, ends with `/`
```
cat test.txt 
this is /usr/bin directory
use // directory
the /root is home dir

cat test.txt | grep '/.*/'
this is /usr/bin directory
use // directory
```


## Match the previous element 1 or more times: '+'

> **NOTE**: Confusing fact: In basic RegExp some symbols, including '+' has to be escaped by '\' to turn to operator

```
cat test.txt 
0
1
2
00
0+
0000000

# Here '+' is treated as regular symbol, no the operator
cat test.txt | grep '0+'
0+

# Here '+' is treated as operator with meaning "1 or more times"
cat test.txt | grep '0\+'
0
00
0+
0000000
```




















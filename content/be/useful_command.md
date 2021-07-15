+++ 
draft = false
date = 2021-02-25T09:50:59+07:00
title = "[Linux] Useful Commands"
description = ""
slug = "" 
tags = []
categories = []
externalLink = ""
series = []
+++

# Useful Command Lines
25/02/2021

## I. Cat
Read a text-like file  
Example:  
```bash
$ cat file.txt | grep "pattern" | -A number_of_after_line -B number_of_before_line -C both_before_and_after_lines
```

## II. Find
Find one or more files base on pattern  
- return all the file and directory locate in a dir
```
$ find ~/Hd7/
```
- return only file (or dir)
```
$ find ~/Hd7/ -type f
```
or
```
$ find ~/Hd7/ -type d
```
- return a file with specific name
```
$ find . -type f -name "test.txt"
```

- return a file match the pattern
```
$ find . -type f -name "*.py"
```
case insensitive: `-iname`

- return files which are modified in from 2 to 10 minutes ago
```
$ find . -type f -mmin -10 -mmin + 2
```
`+10` mean more than 10 minutes ago  
`-mtime ` apply for day instead of minute  

- return files over 5M
``` 
$ find . -size + 5M
```
after that we can list all file in that directory
```
$ ls -lah path 
```

- return all empty file/dir
```
$ find .  -empty
```

- return file/dir have permission
```
$ find . -perm 777
```

- execute another command on result of a find command
```
$ find path -name "*.py" -exec rm {} +
```

- exclude in find
```
$ find ./codeforces -type f ! -name "*.*"
```

- find default apply for all subdirectory, to only apply for 1 level directory, use `-maxdepth`
```
$ find . -type f -name "*.jpg" -maxdepth 1
```

## III. du (Disk Usage)
Example
- show size of current directory in human-readable output
```
$ du -sh
```

## IV. sed (Stream EDitor)
- Find and replace text in a file 
```
sed -i 's/original/new/g' file.txt
```
`-i`: in-place (ie: save back to original file)  
`s`: substitute command  
`g`: global (ie: replace all and not just the firse occurence)  

- insert text in b.txt into a.txt at line 5 (-i to save back to orginal a.txt)
```
sed -i '5r ~/path_to_b.txt' a.txt
```

## V. stat 
- show statistics of a file or folder (size, permission, ...)
```
$ stat file.txt
```

## VI. awk (Aho, Weinberger, and Kernighan)
```
$ cat file.txt | grep "pattern" | awk '{print $1 $2}'
```

## VII. xargs (eXtended ARGuments)
Read streams of data from standard input, then generate and executes command line, meaning it can take output of a command and passes it as argument of another command. If no command is speicified, xargs executes echo by default.
```
$ ps -ef | grep "pattern" | awk '{print $2}' | xargs kill -9
```

---
title: Installing vscode with gdb on Linux
toc: true
tags: linux etc
---

## 1. Installing Compiler

``` 
$ sudo apt-get install build-essential
```

- gcc를 설치한 과정이다. 

## 2. Installing VSCode

```
$ sudo ap-get install curl 
$ sudo sh -c 'curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > /etc/apt/trusted.gpg.d/microsoft.gpg'
$ sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main" > /etc/apt/sources.list.d/vscode.list'
$ sudo apt update
$ sudo apt install code
$ code
```


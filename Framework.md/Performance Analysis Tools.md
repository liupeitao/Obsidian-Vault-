## Meta 

[[2023-11-22]]
motivate by  #njun  #李樾老师 #静态程序分析框架 
https://www.bilibili.com/video/BV1gP4y1d7Jt/?spm_id_from=333.337.search-card.all.click&vd_source=0fd341bc6e29d3d10b5bc23b540cec16


## Tool List
1. pylint
2. flake8
3. coverage
4. memory_profiler
### Install 
install by `pip` 
`pip install [pylint|flake8|...]`

### Usage
`pylint` , `flake8`, `coverage`  is easy to use , just `pylint example.py` or `coverage option example.py`

`memory_profiler` is a little difference. 
1. you need add `@profiler` on the `function` you want to analyse.
2. run `python -m memory_profiler pylinttest.py`

source code 
``` python
@profile
def greet(name):
	print("Hello, " + name)
	greet("Alice")
```
you will see 
```
 python -m memory_profiler pylinttest.py
Hello, Alice
Filename: pylinttest.py

Line #    Mem usage    Increment  Occurrences   Line Contents
=============================================================
     6   20.578 MiB   20.578 MiB           1   @profile
     7                                         def greet(name):
     8                                             """print greet info
     9                                         
    10                                             Args:
    11                                                 name (str): a str will be print
    12                                             """
    13   20.578 MiB    0.000 MiB           1       print("Hello, " + name)

```

## detail

In Python, there are several popular program analysis frameworks available for detecting and improving code quality, performance, and security. Here are some common Python program analysis frameworks:

1. pylint: pylint is a static code analysis tool used to check the quality and potential issues in Python code. It can detect code conventions, errors, unused variables, code complexity, and provide detailed reports and suggestions.
    
2. flake8: flake8 is a framework that integrates multiple static code analysis tools, including pylint, pyflakes, and pep8. It checks for code conventions, errors, unused variables, import problems, and provides customizable reports.
    
3. mypy: mypy is a static type checking tool used to identify type errors and inconsistencies in Python code. It supports type annotations and type inference and can detect type errors, missing type annotations, and more.
    
4. coverage.py: coverage.py is a code coverage tool used to determine the extent of code covered by test cases in Python code. It generates code coverage reports to help developers assess the thoroughness of their tests and the executable paths of their code.
    
5. memory_profiler: memory_profiler is a tool used to analyze the memory usage of Python programs. It can perform line-by-line analysis of code, displaying memory consumption for each line and helping to identify memory leaks and high memory consumption issues.
    

These program analysis frameworks serve as useful tools for Python developers to enhance code quality, performance, and security. They provide automated detection and reporting capabilities and can be integrated into development environments or continuous integration systems, making code analysis more convenient and efficient.


---
title: Functions and modules
---

# Functions and modules

## Functions

Functions are reusable pieces of programs. They allow you to give a name to a block of statements,
allowing you to run that block using the specified name anywhere in your program and any number of
times. This is known as *calling* the function. We have already used many built-in functions such
as `print`, `len` and `range`.

The function concept is probably *the* most important building block of any non-trivial software
(in any programming language), so we will explore various aspects of functions in this chapter.

Functions are defined using the `def` keyword. After this keyword comes an *identifier* name for
the function, followed by a pair of parentheses which may enclose some names of variables, and by
the final colon that ends the line. Next follows the block of statements that are part of this
function. An example will show that this is actually very simple.
Here we define a function called `print_nuc_count` using the syntax as explained above. 
Run the example to see the output.

Example:

```python
!INCLUDE "code/function1.py"
```

Notice that we can call the same function twice which means we do not have to write the same code
again.

### Function Parameters

The function `print_nuc_count` as defined above takes no arguments.
Therefore, there are no parameters declared in the parentheses in the function definition.
Often, we want to supply some information to the function in the form of arguments, 
so that the function can *do* something utilising those values.
In that case we specify parameters in the function definition.
These are basically just variable names that define the input to the function 
so that we can pass in different values to it and get back corresponding results.
The values of these variables are defined when we call the function and are already assigned values
when the function runs.
Parameters are specified within the pair of parentheses in the function definition, separated by
commas. When we call the function, we supply the arguments in the same way.  

> **The difference between arguments and parameters**
>
> Note the terminology used - 
> the names given in the function definition are called *parameters* 
> whereas the values you supply in the function call are called *arguments*.

The function we defined above is not very useful. 
We usually don't want to count the number of nucleotides in the same sequence again and again. 
We will define a function that takes one argument, called `seq`, which is used to determine the number of nucleotides.

```python
!INCLUDE "code/function_parameters.py"
```

**How It Works** 

The first time we call the function `print_nuc_count`, 
we directly supply the sequence as arguments. 
In the second case, we call the function with a variable as argument. 
`print_nuc_count(dna)` causes the value of argument `dna` to be assigned to parameter `seq`. The `print_nuc_count` function works the same way in both cases.



### Local Variables

When you declare variables inside a function definition, they are not related in any way to other
variables with the same names used outside the function - i.e. variable names are *local* to the
function. This is called the *scope* of the variable. All variables have the scope of the block
they are declared in starting from the point of definition of the name.


```python
!INCLUDE "code/function_local.py"
```

**How It Works** 

The first time that we print the *value* of the name *x* with the first line in the function's
body, Python uses the value of the parameter declared in the main block, above the function
definition.

Next, we assign the value `2` to `x`. The name `x` is local to our function.  So, when we change
the value of `x` in the function, the `x` defined in the main block remains unaffected.

With the last `print` statement, we display the value of `x` as defined in the main block, thereby
confirming that it is actually unaffected by the local assignment within the previously called
function.


### Default Argument Values

For some functions, you may want to make some parameters *optional* and use default values in case
the user does not want to provide values for them. This is done with the help of default argument
values. You can specify default argument values for parameters by appending to the parameter name
in the function definition the assignment operator (`=`) followed by the default value.

Note that the default argument value should be a constant.

```python
!INCLUDE "code/function_default.py"
```

**How It Works**

The function named `say` is used to print a string as many times as specified. If we don't supply a
value, then by default, the string is printed just once. We achieve this by specifying a default
argument value of `1` to the parameter `times`.

In the first usage of `say`, we supply only the string and it prints the string once. In the second
usage of `say`, we supply both the string and an argument `5` stating that we want to *say* the
string message 5 times.

> **Caution**
> 
> Only those parameters which are at the end of the parameter list can be given default argument
> values i.e. you cannot have a parameter with a default argument value preceding a parameter without
> a default argument value in the function's parameter list.
>
> This is because the values are assigned to the parameters by position. 
> For example, `def func(a, b=5)` is valid, but `def func(a=5, b)` is *not valid*.

### Keyword Arguments

If you have some functions with many parameters and you want to specify only some of them, then you
can give values for such parameters by naming them - this is called *keyword arguments* - we use
the name (keyword) instead of the position (which we have been using all along) to specify the
arguments to the function.

There are two advantages - one, using the function is easier since we do not need to worry about
the order of the arguments. Two, we can give values to only those parameters to which we want to,
provided that the other parameters have default argument values.

```python
!INCLUDE "code/function_keyword.py"
```

**How It Works** 

The function named `func` has one parameter without a default argument value, followed by two
parameters with default argument values.

In the first usage, `func(3, 7)`, the parameter `a` gets the value `3`, the parameter `b` gets the
value `7` and `c` gets the default value of `10`.

In the second usage `func(25, c=24)`, the variable `a` gets the value of 25 due to the position of
the argument. Then, the parameter `c` gets the value of `24` due to naming i.e. keyword
arguments. The variable `b` gets the default value of `5`.

In the third usage `func(c=50, a=100)`, we use keyword arguments for all specified values. Notice
that we are specifying the value for parameter `c` before that for `a` even though `a` is defined
before `c` in the function definition.

### The `return` statement

The `return` statement is used to *return* from a function i.e. break out of the function. We can
optionally *return a value* from the function as well.

```python
!INCLUDE "code/function_return.py"
```

**How It Works**

The `maximum` function returns the maximum of the parameters, in this case the numbers supplied to
the function. It uses a simple `if..else` statement to find the greater value and then *returns*
that value.

Note that a `return` statement without a value is equivalent to `return None`. `None` is a special
type in Python that represents nothingness. For example, it is used to indicate that a variable has
no value if it has a value of `None`.

Every function implicitly contains a `return None` statement at the end unless you have written
your own `return` statement. You can see this by running `print(some_function())` where the function
`some_function` does not use the `return` statement such as:

```python
def some_function():
    pass

print(some_function())
```

The `pass` statement is used in Python to indicate an empty block of statements.

TIP: There is a built-in function called `max` that already implements the 'find maximum'
functionality, so use this built-in function whenever possible.


## **Exercises**: functions

### **Exercise 8.1** Hamming distance {-}

Write a function called `hamming` that takes two strings `s` and `t` of equal length as arguments and computes the number of differences between them.
The function should return the number of of symbols that differ in `s` and `t`.

~~~{.python}
def hamming(s, t):
    # adapt this function with your own code
    return 0

print(hamming("ACTG", "ACTG"))
print(hamming("ACTG", "GTCA"))
print(hamming("AACC", "AATT") + hamming("CTGA", "TCGA"))
~~~

If your function is defined correctly, the code above should exactly match the following output when run:

~~~{.raw}
0
4
4
~~~

### **Exercise 8.2** Find exact motif matches {-}

Given two strings `s` and `t`, `t` is a substring of `s` if `t`
is contained as a contiguous collection of symbols in `s` (as a result,
`t` must be no longer than `s`).
Write a function called `find_match` that takes two arguments, `s` and `t`,
and returns a `list` of all positions of the substring `t` in `s`.

~~~{.python}
# define your function here
~~~

When you have finished the function run the code below:

~~~{.python}
# Don't change anything here, but update the function definition above!
print(find_match("GATATATGCATATACTT", "ATAT"))
print(find_match("AUGCUUCAGAAAGGUCUUACG", "U"))
~~~

The output above should exactly match the following:

~~~{.raw}
[2, 4, 10] 
[2, 5, 6, 15, 17, 18] 
~~~

## DocStrings - documenting your functions

Python has a nifty feature called *documentation strings*, 
usually referred to by its shorter name *docstrings*. 
DocStrings are an important tool that you should make use of since it helps to
document the program better and makes it easier to understand. 
We can even get the docstring back from, 
say a function, when the program is actually running!

```python
!INCLUDE "code/function_docstring.py"
```

**How It Works**

A string on the first logical line of a function is the *docstring* for that function.
The convention followed for a docstring is a multi-line string where the first line starts with a
capital letter and ends with a dot. Then the second line is blank followed by any detailed
explanation starting from the third line. You are *strongly advised* to follow this convention for
all your docstrings for all your non-trivial functions.

We can access the docstring of the `count_nuc` function using the `__doc__` (notice the *double
underscores*) attribute (name belonging to) of the function.
 
If you have used `help()` in Python, then you have already seen the
usage of docstrings! What it does is just fetch the `__doc__`
attribute of that function and displays it in a neat manner for
you. You can try it out on the function above - just include `help(count_nuc)` in your
program. Remember to press the `q` key to exit `help`.

Automated tools can retrieve the documentation from your program in this manner. 
Therefore, we *strongly recommend* that you use docstrings for any non-trivial function that you write. 
The `pydoc` command that comes with your Python distribution works similarly to `help()` using docstrings.

**From now on, *all*  your functions should be documented using a docstring!** 

## **Exercises**

For all exercises below follow these 5 steps:
  
1. Describe three test cases, including input and expected output, that test normal behavior of your function.

2. Describe two test cases testing a corner case. What should be the expected behavior?

3. Implement the function.

4. Test the function with your test cases. Update the function when necessary.

5. When your function works as expected, ask a fellow student for three of his or her test cases and test your function.
Is there any unexpected behavior?
Update the function if needed.

Be sure to document using your code using comments and
include docstrings for all functions definitions.

### **Exercise 8.3** Translation {-}

Write a function that converts a mRNA sequence (in using the A,C,G,T alphabet) into
a protein sequence.
You can use the codon table that was used in exercises 5.10.

Describe your test cases (step 1 and 2) here:

~~~{.python}
#Describe your test cases (step 1 and 2) here:
~~~

Implement your function (step 3):

~~~{.python}
#Implement your function (step 3):
~~~

Test your function using your test cases (step 4):

~~~{.python}
#Test your function using your test cases (step 4):
~~~

Test the function using other test cases (step 5):

~~~{.python}
#Test the function using other test cases (step 5):
~~~

### **Exercise 8.4** FASTA files {-}

The [FASTA format](https://en.wikipedia.org/wiki/FASTA_format) is a widely used text format to represent nucleotide or peptide sequences.
Write a function that reads a multiple sequence FASTA file.
The function should take the file name as an argument and return a dictionary
with the sequence identifier as key and the actual sequence as value.
Create files for all your test cases.
You can create text files from the jupyter window (`New` -> `Text File`) or use the `Upload` button to upload files you created locally.

Describe your test cases (step 1 and 2) here:

~~~{.python}
#Describe your test cases (step 1 and 2) here:
~~~

Implement your function (step 3):

~~~{.python}
#Implement your function (step 3):
~~~

Test your function using your test cases (step 4):

~~~{.python}
#Test your function using your test cases (step 4):
~~~

Test the function using other test cases (step 5):

~~~{.python}
#Test the function using other test cases (step 5):
~~~

### **Exercise 8.5** Motif conversion {-}

Write a function that converts a consensus sequence into a positional weight matrix.
The consensus sequence can be composed of symbols from the [IUPAC DNA code](http://www.bioinformatics.org/sms2/iupac.html).
The positional weight matrix should be a two-dimensional list.
Every element in the first list is a list containing the relative frequencies of A, C, G and T, in that specific order, that together sum up to `1.0`.

Describe your test cases (step 1 and 2) here:

~~~{.python}
#Describe your test cases (step 1 and 2) here:
~~~

Implement your function (step 3):

~~~{.python}
#Implement your function (step 3):
~~~

Test your function using your test cases (step 4):

~~~{.python}
#Test your function using your test cases (step 4):
~~~

Test the function using other test cases (step 5):

~~~{.python}
#Test the function using other test cases (step 5):
~~~


## Modules

Python functions can be stored together in a *module*. A module, a collection of functions with a certain theme, can be *imported* by another program to make use of its functionality. This is how we can
use the Python standard library as well. First, we will see how to use the standard library
modules.

File `module_using_sys.py`:

~~~{.python}
!INCLUDE "code/module_using_sys.py"
~~~

Let's run this program from the shell. 

~~~{.bash}
$ python module_using_sys.py we are arguments
~~~

~~~
The command line arguments are:
module_using_sys.py
we
are
arguments
~~~

**How It Works**

First, we *import* the `sys` module using the `import` statement. Basically, this translates to us
telling Python that we want to use this module. The `sys` module contains functionality related to
the Python interpreter and its environment i.e. the **sys**tem.

When Python executes the `import sys` statement, it looks for the `sys` module. In this case, it is
one of the built-in modules, and hence Python knows where to find it.

If it was not a compiled module i.e. a module written in Python, then the Python interpreter will
search for it in the directories listed in its `sys.path` variable. If the module is found, then
the statements in the body of that module are run and the module is made *available* for you
to use. Note that the initialization is done only the *first* time that we import a module.

The `argv` variable in the `sys` module is accessed using the dotted notation i.e. `sys.argv`. It
clearly indicates that this name is part of the `sys` module. Another advantage of this approach is
that the name does not clash with any `argv` variable used in your program.

The `sys.argv` variable is a *list* of strings. 
Specifically, the `sys.argv` contains the list of *command line arguments* i.e. the arguments passed to your program using the command line.

Here, when we execute `python module_using_sys.py we are arguments`, we run the module
`module_using_sys.py` with the `python` command and the other things that follow are arguments
passed to the program. Python stores the command line arguments in the `sys.argv` variable for us
to use.

Remember, the name of the script running is always the first argument in the `sys.argv` list. So,
in this case we will have `'module_using_sys.py'` as `sys.argv[0]`, `'we'` as `sys.argv[1]`,
`'are'` as `sys.argv[2]` and `'arguments'` as `sys.argv[3]`. 

### The from ... import statement

If you want to directly import the `argv` variable into your program 
(to avoid typing the `sys.`everytime for it), 
then you can use the `from sys import argv` statement.

In general, the `import` statement is better since your program will avoid name clashes and will be more readable.

~~~{.python}
from math import sqrt
print("Square root of 16 is", sqrt(16))
~~~

### Multiple imports

You can import from many different modules. Imports are **always** put at the top of the file.

File `module_using_imports.py`:

~~~{.python}
!INCLUDE "code/module_using_imports.py"
~~~

Command:

~~~{.bash}
$ python module_using_imports.py test me
~~~

Output:

~~~
The command line arguments are:
code/module_using_imports.py
test
me

The current directory is: /home/simon/git/cfb/day3

The square root of 16 is 4.0

Today is 2015-09-23
~~~

## **Exercises**

### **Exercise 8.6** Reading the command line arguments {-}
  
Make a script called `nuc_arg.py` that prints the nucleotide content of a sequence that you give as argument on the command line.
For example, this command:

~~~{.bash}
$ python nuc_arg.py TGACTCA
~~~

should print the following output:

~~~
2 2 1 2
~~~

*Hint:* if you want, you can use the nucleotide count function from the lecture.

Copy this script to a new file:

~~~{.bash}
$ cp nuc_script.py nuc_arg.py
~~~

Now you can edit the script `nuc_arg.py`.

### **Exercise 8.7** FASTA statistics {-}

Write a script called `nuc_fasta.py`.
This script should accept the name of a FASTA file as argument, read the FASTA sequences in that file and print the nucleotide content of all the sequences.
The script should print the `ID` of every sequence followed by the nucleotide content of that sequence.

## Coding style 

In general Python programmers follow a set of style guidelines. 
These are described in [PEP 0008](https://www.python.org/dev/peps/pep-0008/).
We will highlight a selection of the most important ones here. 
Follow these guidelines. 
You don't have a good reason not to.
In addition, in the exam you will get points for following this coding style.

**Style**

* Use 4 spaces as indentation.
* Always put `import` statements at the top of the file
* Be consistent.
* Avoid whitespace:
    * immediately inside parentheses, brackets or braces;
    * immediately before a comma, semicolon, or colon;
    * immediately before the open parenthesis that starts the argument list of a function call;
    * immediately before the open parenthesis that starts an indexing or slicing.
    

**Comments**

* Be sure to comment your code. 
* Comments that contradict the code are worse than no comments. 
* Comments should be complete English sentences.
* Write docstrings for all modules and functions
* Use double quote characters for docstrings

**Naming Conventions**

* Use meaningful variable names
* Variable, function and module names are all-lowercase. Underscores can be used if it improves readability.


## Using available modules: argparse

The Python standard library, 
which is included with every Python version,
contains a lot of useful modules (see [here](https://docs.python.org/3/library/))
In addition, there are many third-party modules available.
Some of these we will use in this course, such as [pandas](http://pandas.pydata.org/) for data analysis and [matplotlib](http://matplotlib.org/) for making figures.
However, it is not possible to exhaustively cover all these modules in this course.
Therefore, it is very useful to be able to search for modules and for examples and tutorials on how to use tese modules. 
With the subjects covered so far, you have should have enough understanding of basic Python principles.

Let's take an example, the `argparse` module. 
This is a very useful module for scripts that take (a lot of) command-line arguments. Some examples on how to use this module:

https://docs.python.org/3/howto/argparse.html

* [https://docs.python.org/3/howto/argparse.html](https://docs.python.org/3/howto/argparse.html)
* [http://www.pythonforbeginners.com/argparse/argparse-tutorial](http://www.pythonforbeginners.com/argparse/argparse-tutorial)
* [https://pymotw.com/3/argparse/](https://pymotw.com/3/argparse/)
* [https://mkaz.tech/code/python-argparse-cookbook/](https://mkaz.tech/code/python-argparse-cookbook/)

Have a look, Google it, try it out!
We will use the argparse module in our final assignment for today.

## **Exercises**: putting it all together

### **Exercise 8.8** motif scanning {-}

Write a command-line Python script that scans a FASTA file with a IUPAC consensus sequence.
As an optional argument to the script, a use should be able to specify a number of mismatches.
It should be possible to specify these arguments on the command line, use the `argparse` module.
As output, the script should print, for every match, the ID of the sequence and the position of the match within the sequence.

Take care of the following:

* Comment your code
* Use functions
* Follow the style guide
* Test your code

Start small. 
It is better to have a working script that has limited functionality but works well, then a script that tries to do everything and fails.

#### Test data

Two examples of consensus sequences and FASTA files (for c-Myc and STAT3 from [Chen et al. 2008](http://dx.doi.org/10.1016/j.cell.2008.04.043)) are located in `/scratch/cfb/consensus_motif`.

* `consensus.txt`
* `c-Myc.fa`
* `STAT3.fa` 

#### Extension (optional)

Implement the 'Match' algorithm from [Kel et al. 2003](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC169193/) to scan for matches to a positional weight matrix.

The `log` function can be imported from the `math` module. See the definition of this function [here](https://docs.python.org/3/library/math.html#math.log).

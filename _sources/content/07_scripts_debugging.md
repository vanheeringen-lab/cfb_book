---
title: Scripts and debugging
---

# Scripts and debugging

## **Exercises**: scripts

### **Exercise** 7.1: Copying the files required for the exercises {-}

Open a terminal from the Jupyter "Home" page in your webbrowser (the view of your home directory), by clicking on "Terminal" in the "New" pull down menu.

If you haven't already done so, first create a folder/directory named "cfb_2018" in your network folder for this course:

~~~{.bash}
cd                # go to root of your home directory
mkdir cfb_2018    # mkdir creates a directory
cd cfb_2018       # change directory
pwd               # print current working directory. 
                  # In my case, the result is: /home/kalbers/cfb_2018
~~~

Now we are going the copy the files and scripts necessary to perform these exercises to the "cfb_2018" directory. 
The Linux command "cp" copies files and directories.

~~~{.bash}
cp -rp /scratch/cfb/scripts_and_debugging ~/cfb_2018/
~~~

Note that the "~" refers to the root of your homedirectory (whatever your username is), and that the directory "cfb_2018" has to exist for this particular
copy command to work. 

Now change directory (using "cd") to the directory "~/cfb_2018/scripts_and_debugging". 
Validate that you are in the correct directory using the command "pwd" as above.


### **Exercise** 7.2: Comparing the Jupyter notebook with running scripts from the command line {-}

Open a new Python3 Jupyter-notebook for these exercises. Copy the following code into a cell and execute the cell:

~~~{.python}
print("Hello")
my_number = 1
my_number
~~~

Questions: 

1. Describe the exact output. 
2. Which of the three lines in the above code produce output? 
3. Is this output formatted in the same way?

Now create a new text file by clicking "Text File" in the "New" drop-down menu in the file-browser tab-page of your Jupyter notebook. 
Make sure to first go the correct subdirectory ("~/cfb_2018/scripts_and_debugging") by clicking on the respective subdirectories in the
Jupyter file browser window.
Put the same three lines of code in this text file. Then, rename the text file to "exercise2.py" by clicking on the file name at the top of the screen.
(The default file name is "untitled.txt").

If everything went well, the word "print" should now have the color green, and the text "Hello" should be colored red. This is called *syntax highlighting*.

Now, run the script from the command line in the terminal window as follows:

~~~{.bash}
python exercise2.py
~~~

Questions:     

4. Is the output of the script printed to the terminal window exactly the same as the output printed to screen when executing the cell in a Jupyter notebook?
5. How does a Jupyter-notebook differ from a script in terms of outputing information to the screen?


### **Exercise** 7.3: Permanence of Python variables in memory {-}

Copy the following code to a cell in a ***Jupyter notebook*** and execute the cell.

~~~{.python}
my_number = 42
~~~

Copy the following code to a ***new*** cell below the previous one in the same Jupyter notebook, and execute the cell:

~~~{.python}
print("my_number:",my_number)
~~~

Question:

1. What is the output of the second cell in the Jupyter notebook?


Next, create a Python script called "exercise3a.py" with exactly the same code as the first cell (i.e. "my_number =  42").
Create a second Python script called "exercise3b.py" with exactly the same code as the second cell ("print("my_number:",my_number)").

Now go to your Terminal and execute the first script and the second script in that order:

~~~{.bash}
python exercise3a.py
python exercise3b.py
~~~

Questions:

2. Does executing the first script produce an error message? If not, what is the output printed to the screen? If yes, why?
3. Does executing the second script produce an error message? If not, what is the output printed to the screen? If yes, why?
4. What is the state of the Python memory when it starts to execute a Python script?



## **Exercises**: debugging

### **Exercise** 7.4 {-}

The purpose of the following code is to count how often each TF in the network is a target of a TF in the network (it also counts if a TF regulates itself).
In other words, how many TFs in the network bind in the promoter of a given TF.

~~~
The below code is provided in the script "exercise4_with_errors.py". 
Use the terminal and command line to copy the file to a new file named "my_exercise4.py".
Edit the new file by clicking on it in the Jupyter browser.
Every time you change the file, save it.
Execute the Python script from the command line with "python my_exercise4.py". (Don't type the quotes on the command line)
~~~

#### Questions {-}

1. What is the purpose of this code? (This should be obvious from the short description given above and the code itself)
2. What is the input and what is the expected output? 
**Answer this question before running or changing the code, but make use of the network as defined in the dictionary *hsc_network* **.
3. Identify the source of the runtime errors, and fix the mistake into the code so that the script runs without error. 
In that case, the last line of the output should be "Finished analysis of HSC network".
4. After you fixed the errors in the code, does the code produce the expected output? 
5. What is the output (list each TF and its target frequency)?
6. How would you solve this counting problem more elegantly? Hint: e.g. by using a single dictionary to replace the list *list_of_tfs*  and dictionary *tf_to_index*


Advice: make liberal use of print statements to understand 
1. What each line in the code is trying to achieve
2. How to make the code function correctly, and make sure that it indeed functions correctly.

Hints: There are several *bugs* in the code below. Some are obvious mistakes (a typo). 
However, some are mistakes that do not produce runtime errors that produce an incorrect result.


~~~{.python}

# define HSC network from Bonzanni et al, Bioinformatics 2013
hsc_network = {                                # each key is a source TF, the value for each key is a list of target TFs
        "Scl" : ["Scl","Gata1","Gata2","Hhex","Zfpm1","Fli1","Erg","Smad6","Runx1","Eto2"],
        "Pu.1" : ["Pu.1","Runx1","Gata1"],
        "Runx1" : ["Runx1","Pu.1","Erg"],
        "Smad6" : ["Runx1"],
        "Eto2" : ["Erg"],
        "Gata1" : ["Gata1","Scl","Pu.1","Gata2"],
        "Fli1" : ["Fli1","Runx1","Pu.1","Scl","Gata2","Hhex","Erg","Smad6"],
        "Erg" : ["Smad6","Runx1","Erg","Hhex","Gata2","Fli1"],
        "Zfpm1" : ["Gata2"],
        "Gata2" : ["Zfpm1","Runx1","Smad6","Eto2","Scl","Gata2","Hhex","Fli1","Erg"],
        "Hhex" : ["Gata2"]
        }

target_frequencies = [0,0,0,0,0,0,0,0]         # How often is each TF a target of another TF?
list_of_tfs = []                               # list of transcription factors in the network

tf_to_index = {}                               # dictionary linking TF name to numerical index in list

index = 0                                      # initialize index to zero
for tf in hsc_network:
    list_of_tfs.append(tf)                     # append tf to the list of transcription factors in the network
    index = index + 1  # update index

    tf_to_index[tf] = index                    # associate the transcription factor tf with the index


for source_tf in hsc_network:                  # iterate over TFs in the network
    list_of_target_tfs = hsc_network[source_tf]
    for target_tf in list_of_target_tfs:       # iterate over all TFs that are targeted by source_tf
        index_in_list = tf_to_index[target_tf] # retrieve list index associated with this TF
        target_frequencies[index_in_list] = target_frequency[index_in_list] + 1

for index in range(len(list_of_tfs)):
    tf = list_of_tfs[index]
    target_frequency_tf = target_frequencies[ index ]
    print("Transcription factor",tf,"is",target_frequency_tf,"times a target of another TF")

print("Finished analysis of HSC network")
~~~


### **Exercise** 7.5 {-}

The following code (provided in the file "exercise5.py") 
tests whether a particular TF of interest directly regulates itself (in other words, if a TF binds in its own promoter).
The name of the TF of interest is assigned to the variable *my_tf_of_interest*.

The code should print 

~~~
Transcription factor my_tf_of_interest regulates itself
~~~

or

~~~
Transcription factor my_tf_of_interest does not regulate itself
~~~

to screen (with my_tf_of_interest replaced by the actual TF name), depending on whether there is a self-regulation relationship encoded in the
*hsc_network* dictionary variable or not.




Questions

0. This script contains a bug (otherwise it wouldn't be a question here). Can you identify the bug without running the code (try for at least 10 minutes)?
1. Run the code as a script from the command line in a terminal.
 Does the code run without error?
2. Does the code provide the correct output for the TF named 'Scl'?
3. Does the code provide the correct output for all TFs? 
If not, how would you adapt the code (extend or change it) so that it does produce the desired output?

~~~{.python}
# Does a TF regulate itself?

hsc_network = {                                # each key is a source TF, the value for each key is a list of target TFs
        "Scl" : ["Scl","Gata1","Gata2","Hhex","Zfpm1","Fli1","Erg","Smad6","Runx1","Eto2"],
        "Pu.1" : ["Pu.1","Runx1","Gata1"],
        "Runx1" : ["Runx1","Pu.1","Erg"],
        "Smad6" : ["Runx1"],
        "Eto2" : ["Erg"],
        "Gata1" : ["Gata1","Scl","Pu.1","Gata2"],
        "Fli1" : ["Fli1","Runx1","Pu.1","Scl","Gata2","Hhex","Erg","Smad6"],
        "Erg" : ["Smad6","Runx1","Erg","Hhex","Gata2","Fli1"],
        "Zfpm1" : ["Gata2"],
        "Gata2" : ["Zfpm1","Runx1","Smad6","Eto2","Scl","Gata2","Hhex","Fli1","Erg"],
        "Hhex" : ["Gata2"]
        }

my_tf_of_interest = 'Scl'

for target_tf in hsc_network[my_tf_of_interest]:
    if target_tf == my_tf_of_interest:
        my_tf_regulates_itself = True

if my_tf_regulates_itself:
    print("Transcription factor",my_tf_of_interest,"regulates itself")
else:
    print("Transcription factor",my_tf_of_interest,"does not regulate itself")
~~~


### **Exercise** 7.6 {-}

This exercise focuses on problems that may arise when an input file does not have the formatting you expect, contains errors or otherwise unexpected events.

For small networks it is feasible to type your network directly as a dictionary, like in the example above. 
In other realistic scenarios, the network file is produced by another analysis (either your own Python script or another program), and you would like
to analyze the network produced by that program.

The purpose of the below code is to determine which TFs are provided in a flat text file, and count *how many* target genes each TF has. 
To keep things managable, the text file contains only a small network.

The setting is as follows:

* We have an flat text input file containing the network links in the following format:

~~~
source_tf_1 target_1 target_2 target_3
source_tf_2 target_4 target_5
~~~

* We want to parse the input file and count for each source_tf that occurs how many targets it has.
* To test the correctness of our code, we provide you with an input file that has several issues that cause the program 
to misbehave.



The following code is contained in the script "exercise6.py". 
It reads a network from the file "network_exercise_6.txt".  
The code contained in it is as follows:

~~~{.python}
target_tfs_dict = {} # will contain a list of the target genes for each source gene

input_file = open("/scratch/cfb/scripts_debugging/network_exercise_6.txt",'r') # change path/filename if you want to run it on a different file

for line in input_file:
    entries = line.strip().split()
    source_tf = entries[0]   # source TF is always the first string in a line
    targets = entries[1:] # the strings after the first one are the target genes
    target_tfs_dict[source_tf] = targets

input_file.close()

for tf in sorted(target_tfs_dict):
    print("Source TF:\t",tf,"\tNumber of targets:\t",len(target_tfs_dict[tf]))
~~~

Run the script from the command line in a terminal. You can view the network file by clicking on it in the Jupyter file browser. 


#### Expected output {-}

If the script functions correctly, the desired output is:

~~~
Source TF:	 Erg 	Number of targets:	 6
Source TF:	 Eto2 	Number of targets:	 1
Source TF:	 Fli1 	Number of targets:	 8
Source TF:	 Gata1 	Number of targets:	 4
Source TF:	 Gata2 	Number of targets:	 9
Source TF:	 Hhex 	Number of targets:	 1
Source TF:	 Pu.1 	Number of targets:	 3
Source TF:	 Runx1 	Number of targets:	 3
Source TF:	 Scl 	Number of targets:	 10
Source TF:	 Smad6 	Number of targets:	 1
Source TF:	 Zfpm1 	Number of targets:	 1
~~~

#### Question/assignment: {-}

The script "exercise6.py" makes three assumptions about the input file that are not valid (the input file violates the assumption).

Describe at least three assumptions that this script makes about the input file and provide a script that produces the desired output.

For example, describe the assumptions as specifically as in the following examples (this is just an example, not relevant to this script):

* The script assumes that each line has precisely 20 characters
* The script assumes that each line starts with a capital letter

**NOTE** You have the change the Python script; you are **NOT** allowed to change the input file!

#### Hints: {-}

1. Use if statements to check whether a specific assumption that you require for correct execution is satisfied.
For instance, you can use an if statement to check the length of a list, or whether a key is present in the dictionary.
2. Add print statements to understand what each line is doing and what the value is of a variable.
3. You are allowed to change the input file to understand where the problem is, but you have to provide code that produces the correct output 
for the original input file.


[comment1]: # (Assumes at least 1 target on each line)
[comment2]: # (Assumes no duplicates / assumes all targets of a TF are on the same line)
[comment3]: # (Assumes the separator is always a space)


### **Exercise** 7.7 (bonus; fun but non-trivial) {-}

Implement the boolean network dynamics from the [Bonzanni et al paper](https://www.ncbi.nlm.nih.gov/pubmed/23813012). 

Create an additional dictionary that represents the state of each interaction (a square in Figure 1 from Bonzanni et al).

Take into account that some interactions are inhibitory (flat arrow head) and some are excitatory/activating (pointy arrow head).

Update the interactions based on the state of the TFs and then update the target TF. Decide based on the paper on the update schedule.

Can you validate that the stable states/attractors reported by Bonzanni et al are indeed stable?

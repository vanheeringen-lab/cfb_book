---
title: Files
---

# Files

## Introduction to files

Thus far we have used Python without reading input from files and writing output to files. We have created objects like `lists`, `dictionaries`, but nothing has been saved persistently on a disk, meaning that if we close Python, everything is gone. Usually you want to open a file, read its contents (line by line), analyze it, and write it to another file. To work with files we need the Python `file` object. `file` objects serve as a **link to a file** residing on a disk of a computer.

To open a file, write some contents to it and close it:

~~~{.python}
myfile = open('testfile.txt', 'w')
myfile.write('some text\n')
myfile.close()
~~~ 

Note the following:

*  'myfile' is the name of the Python `file` **object** 
*  'testfile.txt' is the name of the **file** written to the disk of the computer
    * extension is not important, but suggests that it contains text
*  The **'w'** argument to the `open` function opens the file for **writing**
*  If the file 'testfile.txt' already exists, you will overwrite its contents
*  If not, the file 'testfile.txt' will be created 
*  You can also use **'a'** instead, which would **append** any output to the existing file called 'testfile.txt'
*  If you use **'r'** instead, you only open the file for only **reading** (it has to exist of course)
*  `write` is a method belonging to the `file` object 
*  `write` does not add a newline character to the end of a line, that is why we add `\n` to our text
*  `close` is a method belonging to the `file` object


Many files used for our analyses are large. We could open them, store their contents in the memory of computer (RAM) and work on the complete contents. However, in most cases we want to avoid loading all contents into memory, and as most of the files we work with contain lines of text, we rather open the file, process its contents **line-by-line**, and write output to another file. 

To open a file and read its contents line-by-line we first open the file using `open`. To step through the file line-by-line we use a `for` or `while` loop to 'iterate' over the resulting `file` object. **Each iteration returns a line of the file, from top to bottom**.

The `for` or `while` loop automatically ends as soon as there are no lines anymore (=after the last line).

At the end of the code that you execute for each line, you should close the file. 

~~~{.python}
infile = open('/scratch/cfb/list-dict-loop/example.bed', 'r')
for line in infile:
    print(line)
infile.close()
~~~

~~~{.raw}
browser position chr7:127471196-127495720
browser hide all
track name="example" description="example_bed_file" visibility=2 itemRgb="On"
chr7	127471196	127472363	Pos1	0	+	127471196	127472363	255,0,0
chr7	127472363	127473530	Pos2	0	+	127472363	127473530	255,0,0
chr7	127473530	127474697	Pos3	0	+	127473530	127474697	255,0,0
chr7	127474697	127475864	Pos4	0	+	127474697	127475864	255,0,0
chr7	127475864	127477031	Neg1	0	-	127475864	127477031	0,0,255
chr7	127477031	127478198	Neg2	0	-	127477031	127478198	0,0,255
chr7	127478198	127479365	Neg3	0	-	127478198	127479365	0,0,255
chr7	127479365	127480532	Pos5	0	+	127479365	127480532	255,0,0
chr7	127480532	127481699	Neg4	0	-	127480532	127481699	0,0,255
~~~

The file `/scratch/cfb/list-dict-loop/example.bed` is an example BED file. BED files are used to store genomic coordinates along with additional information. BED files are for instance used to store genomic coordinates of ChIP-seq peaks. The first three fields (columns) contain the chromosome, start, and end of the features, respectively. The file `/scratch/cfb/list-dict-loop/example.bed` also contains 'header' lines that are used when the file is displayed in the UCSC genome browser. This is not always the case for BED files.

In the next few examples we will see some analysis of this file in action. Each example adds some steps and builds from the previous example. 

Example 1:

* open the `/scratch/cfb/list-dict-loop/example.bed` file
* read the file line-by-line
* ignore header lines starting with 'browser' or 'track'
* split each line into a list
* print each line
* assign the first three elements of this list to the variables 'chr', 'start', and 'end'
* print chr start end

~~~{.python} 
infile = open('/scratch/cfb/list-dict-loop/example.bed', 'r')
for line in infile:        # use a variable called 'line' for each line
    line = line.strip()    # take away the '\\n' character at the end of each line
    # only proceed with lines lacking 'browser' or 'track' at the beginning
    if not line.startswith('browser') and not line.startswith('track'):
        # split the line (`string`) into a `list`, with the '\\t' as a delimiter
        line = line.split('\t')
        print(line)

infile.close()
~~~

~~~{.raw}
['chr7', '127471196', '127472363', 'Pos1', '0', '+', '127471196', '127472363', '255,0,0']
['chr7', '127472363', '127473530', 'Pos2', '0', '+', '127472363', '127473530', '255,0,0']
['chr7', '127473530', '127474697', 'Pos3', '0', '+', '127473530', '127474697', '255,0,0']
['chr7', '127474697', '127475864', 'Pos4', '0', '+', '127474697', '127475864', '255,0,0']
['chr7', '127475864', '127477031', 'Neg1', '0', '-', '127475864', '127477031', '0,0,255']
['chr7', '127477031', '127478198', 'Neg2', '0', '-', '127477031', '127478198', '0,0,255']
['chr7', '127478198', '127479365', 'Neg3', '0', '-', '127478198', '127479365', '0,0,255']
['chr7', '127479365', '127480532', 'Pos5', '0', '+', '127479365', '127480532', '255,0,0']
['chr7', '127480532', '127481699', 'Neg4', '0', '-', '127480532', '127481699', '0,0,255']
~~~

Note how we used `startswith` to check if a string starts with 'browser' or 'track'


Example 2:

* assign the first three elements of this list to the variables 'chr', 'start', and 'end'

~~~{.python} 
infile = open('/scratch/cfb/list-dict-loop/example.bed', 'r')
for line in infile:
    line = line.strip()
    if not line.startswith('browser') and not line.startswith('track'):
        line = line.split('\t')
        chr, start, end = line[0:3]  
        print(chr, start, end)

infile.close()
~~~

~~~{.raw}
chr7 127471196 127472363
chr7 127472363 127473530
chr7 127473530 127474697
chr7 127474697 127475864
chr7 127475864 127477031
chr7 127477031 127478198
chr7 127478198 127479365
chr7 127479365 127480532
chr7 127480532 127481699
~~~

Example 3:

* convert the 'start' and 'end' to an integer `int` (they are strings at first)
* calculate the width (size in bases) of each feature

~~~{.python}
infile = open('/scratch/cfb/list-dict-loop/example.bed', 'r')
for line in infile:              
    line = line.strip()          
    if not line.startswith('browser') and not line.startswith('track'):
        line = line.split('\t')  
        chr, start, end = line[0:3]      
        start = int(start)    # convert start to `int`
        end = int(end)        # convert end to `int`
        width = end-start
        print(width)

infile.close()
~~~

~~~{.raw}
1167
1167
1167
1167
1167
1167
1167
1167
1167
~~~

Example 4:

* calculate how many features with strand '+' and strand '-' are present

~~~{.python}
plus = 0
minus = 0
infile = open('/scratch/cfb/list-dict-loop/example.bed', 'r')
for line in infile:
    line = line.strip()
    if not line.startswith('browser') and not line.startswith('track'):
        line = line.split('\t')
        strand = line[5]
        if strand == '+':
            plus = plus+1
        elif strand == '-':
            minus = minus+1

infile.close()
print(plus, minus)
~~~

~~~{.raw}
5 4
~~~

Thus far we have used the `print` statement to print output to the screen. But if you want to be more explicit in the formatting of the output, you should use `format`, which creates a `string` that can either be printed or written to a file. The standard way of using `format` is like this: 'some text {} more text {}'.format(var1, var2). See the following example in which `format` is used:

~~~{.python}
cell_lines = ['HCT116', 'HeLa', 'HEK293']
for i in range(3):
    args = 'Cell line {} is {}'.format(i+1, cell_lines[i])
    print(args)
~~~

Note that the `print` statement by default adds a newline character "\\n". If you write to a file you need to add the newline character "\\n" to the `string`:

~~~{.python}
outfile = open('cell_lines.txt', 'w')
cell_lines = ['HCT116', 'HeLa', 'HEK293']
for i in range(3):
    args = 'Cell line {} is\t{}\n'.format(i+1, cell_lines[i])
    outfile.write(args)
outfile.close()
~~~

Note how we also used "\\t" here !!

## Glossary of `file` terms

* `open`
* `with`
* `for`
* `while`
* `write`
* `startswith`
* `format`

## **Exercises**: putting it all together

### **Exercise** 6.1 {-}

In this exercise you are going to analyze data from ChIP-seq of several transcription factors in mouse embryonic stem cells (Chen et al. 2008, 13;133(6):1106-17, PMID 18555785). Please have a look at Figure 1 and 2 of this paper. Figure 1 shows the ChIP-seq profiles for the transcription factors.

The ChIP-seq peaks that you see represent *in vivo* binding sites of these transcription factors. In Figure 2 the authors have analyzed the DNA sequence underneath these peaks, and identified specific DNA sequences (**motifs**) for each transcription factor. 

You are going to analyze these sequences as well, to see which motifs you can identify. You will count **k-mers** (short sequences, e.g. 8 bases in length) in the sequences underneath the ChIP-seq peaks of a specific transcription factor, and analyze which k-mers are overrepresented.

DNA (or amino acid) sequence files are usually in FASTA format. Check out https://en.wikipedia.org/wiki/FASTA_format for a FASTA description and an example. Just read the first part.

Before you start off using the FASTA files from the study mentioned above, you will work on a file called "testfile.fasta" (present in `/scratch/cfb/list-dict-loop`), which is a small FASTA file. This allows you to make and test your code quicker.

1. Provide the Python code that allows you to read the FASTA file while ignoring the description lines containing ">". For now it is OK to print the output. 

2. Extend your code with the following: For each sequence, extract all possible k-mers of length 4. Skip sequences that are shorter than 4 bases. Print the output. Make sure your code allows you to easily run the same analysis with different k-mer lengths.

3. Extend your code with the following: Count the number of occurrences for each k-mer that is encountered (in all sequences together)

4. Extend your code with the following: Write the top 10 most abundant k-mer sequences to an output file, along the the number of times they occur

5. Now that you have assembled your code (you can remove the printing statements you used for testing) adapt your code to count kmers of **length 8**. Run your code on 2 files present in the `/scratch/cfb/list-dict-loop` directory:

* GSM288346_ES_Oct4_mm9.peaks.fasta
* GSM288356_ES_c-Myc_mm9.peaks.fasta
* GSM288353_ES_Stat3_mm9.peaks.fasta
* GSM288354_ES_Klf4_mm9.peaks.fasta

For the top most occurring 8-mers that you obtained for each file, do these resemble the motifs for the corresponding transcription factors as displayed in Figure 2 of the paper?

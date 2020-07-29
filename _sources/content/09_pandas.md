---
title: Introduction to pandas
---


# Introduction to pandas DataFrames

In this tutorial you are going to use the module `pandas`.
We will focus on DataFrames, a convenient object type used in pandas.
This tutorial has no exercises, but serves as an introduction to pandas.
It will illustrate what you can do with pandas DataFrames. 
If you want more information on pandas, a (quick) tutorial is found here:

[http://pandas.pydata.org/pandas-docs/stable/10min.html](http://pandas.pydata.org/pandas-docs/stable/10min.html)

## Importing the pandas module

We start by importing the pandas module, for which we use the following command:

~~~{.python}
import pandas as pd
~~~

Note that we imported the pandas module and renamed it to `pd`.
 This allows us to call all pandas commands using `pd` (instead of `pandas`).
This is very common: it saves typing since `pd` is shorter than `pandas`.

## Creating a pandas DataFrame from `lists`

Here we will create a pandas DataFrame containing the following columns:

~~~{.python}
chr_exp1 = ["chr1", "chr11", "chr11", "chr11", "chr13", "chr15", "chr17", "chr17", "chr19", "chr19", "chr2", "chr2", "chr3", "chr4", "chr4", "chr4", "chr5", "chr5", "chr6", "chr6"]

start_exp1 = [43232415, 74459412, 77774406, 131780211, 20806034, 66795152, 7590368, 26903451, 36705003, 53761044, 74880854, 203102822, 14988512, 4543357, 16077241, 20851791, 130970429, 131563056, 111888063, 143889976]

end_exp1 = [43233415, 74460412, 77775406, 131781211, 20807034, 66796152, 7591368, 26904451, 36706003, 53762044, 74881854, 203103822, 14989512, 4544357, 16078241, 20852791, 130971429, 131564056, 111889063, 143890976]

refseq_exp1 = ["NM_024097","NM_001098638","NM_003251","NM_016522","NM_001110221","NR_002441","NM_001126112","NM_005165","NM_007145","NM_173856","NM_004263","NM_003352","NR_046253","NR_037888","NM_006017","NM_147181","NM_001164386","NM_001142599","NM_001164283","NR_027113"]
~~~

In this case we create an empty DataFrame first, and we fill it up with the individual columns

~~~{.python}
df_peaks_exp1 = pd.DataFrame()

df_peaks_exp1['chr'] = chr_exp1
df_peaks_exp1['start'] = start_exp1
df_peaks_exp1['end'] = end_exp1
df_peaks_exp1['refseq'] = refseq_exp1
~~~

You can check what your DataFrame looks like using  `print()`, or simply type `df_peaks_exp1`.

~~~{.python}
print(df_peaks_exp1)
~~~

We make a second DataFrame, which corresponds to a second experiment ('exp2')

~~~{.python}
chr_exp2 = ["chr6_dbb_hap3", "chr6_mann_hap4", "chr6_qbl_hap6", "chr6_qbl_hap6", "chr6_ssto_hap7", "chr7", "chr9", "chrX", "chrX", "chrX"]

start_exp2 = [4324541, 1932656, 3218006, 4038208, 3038042, 98029927, 90497271, 71496641, 73506544, 152162171]


end_exp2 = [4325541, 1933656, 3219006, 4039208, 3039042, 98030927, 90498271, 71497641, 73507544, 152163171]

refseq_exp2 = ["NM_002121","NR_072994","NR_031601","NM_001290043","NM_002441","NM_018842","NM_178828","NM_001007","NR_030258","NM_001184924"]
~~~


~~~{.python}
df_peaks_exp2 = pd.DataFrame()
df_peaks_exp2['chr'] = chr_exp2
df_peaks_exp2['start'] = start_exp2
df_peaks_exp2['end'] = end_exp2
df_peaks_exp2['refseq'] = refseq_exp2
~~~

Look at both DataFrames.

They have the same column layout (chr, start, end, refseq).

They both represent peaks of a ChIP-seq experiment.

For both DataFrames we can add an additional column indicating from which experiment the data comes ('exp1' or 'exp2')

~~~{.python}
df_peaks_exp1['exp'] = 'exp1'
df_peaks_exp2['exp'] = 'exp2'
~~~

Take a look at the two DataFrames, and note how we did this:

* We used `[ ]` to define a new colum, with the column name between `[ ]`
* We only used only one string ('exp1'), but this was automatically expanded to the whole column


## Creating a pandas DataFrame using a `dictionary`

We can also create a pandas DataFrame from a dictionary. The dictionary should contain **column names as keys** and **lists as values** for the columns. To show this in action, we will use the three following lists:

~~~{.python}
refseq = ["NM_001007","NM_001098638","NM_001110221","NM_001126112","NM_001142599","NM_001164283","NM_001164386","NM_001184924","NM_001290043","NM_002121","NM_002441","NM_003251","NM_003352","NM_004263","NM_005165","NM_006017","NM_007145","NM_016522","NM_018842","NM_024097","NM_147181","NM_173856","NM_178828"]

ensembl = ["ENSG00000198034","ENSG00000166439","ENSG00000121742","ENSG00000141510","ENSG00000072682","ENSG00000056972","ENSG00000158987","ENSG00000198883","ENSG00000225967","ENSG00000237710","ENSG00000235569","ENSG00000151365","ENSG00000116030","ENSG00000135622","ENSG00000109107","ENSG00000007062","ENSG00000167635","ENSG00000182667","ENSG00000006453","ENSG00000164008","ENSG00000185774","ENSG00000196131","ENSG00000177992"]

genename = ["RPS4X","RNF169","GJB6","TP53","P4HA2","TRAF3IP2","RAPGEF6","PNMA5","TAP2","HLA-DPB1","MSH5","THRSP","SUMO1","SEMA4F","ALDOC","PROM1","ZNF146","NTM","BAIAP2L1","C1orf50","KCNIP4","VN1R2","SPATA31E1"]
~~~

First, we use these lists to create a dictionary

~~~{.python}
dict_genes = {
    'refseq': refseq,
    'ensembl': ensembl,
    'genename': genename}
~~~

Now that we have the dictionary, we can create the DataFrame.

~~~{.python}
df_genes = pd.DataFrame(dict_genes)
~~~

Look at the resulting DataFrame called `df_genes`. This DataFrame will help us to map RefSeq to ENSEMBL gene identifiers.

Next, we create another DataFrame with gene expression values

Again, we will use a dictionary for this.

~~~{.python}
ensembl_1 = ["ENSG00000237710", "ENSG00000182667", "ENSG00000121742", "ENSG00000158987", "ENSG00000196131", "ENSG00000151365", "ENSG00000056972", "ENSG00000235569", "ENSG00000006453", "ENSG00000198883", "ENSG00000225967", "ENSG00000166439", "ENSG00000135622", "ENSG00000007062", "ENSG00000109107", "ENSG00000167635", "ENSG00000164008", "ENSG00000141510", "ENSG00000177992"]

sample_1 = [None, 1.686, 0.063, 4.222, 0.021, 0.026, 8.169, None, 16.159, 0.08, None, 3.972, 0.844, 26.208, 18.218, 91.049, 3.828, 58.697, 0.024]

sample_2 = [None, 1.159, 0.011, 4.291, 0.004, 0.013, 0.306, None, 12.671, 0, None, 6.983, 5.352, 14.336, 5.333, 60.28, 5.137, 47.569, 0]

sample_3 = [None, 6.865, 0, 4.727, 0.039, 0.023, 10.459, None, 0.225, 0.011, None, 7.007, 3.402, 6.076, 18.445, 28.716, 4.128, 34.299, 0]

sample_4 = [None, 15.691, 2.16, 5.284, 0, 0.181, 1.428, None, 0.184, 0.395, None, 11.842, 4.186, 6.01, 131.013, 25.126, 9.933, 28.18, 0]
~~~

~~~{.python}
dict_expr = {'ensembl': ensembl_1, 'sample1': sample_1, 'sample2': sample_2, 'sample3': sample_3, 'sample4': sample_4}
df_expr = pd.DataFrame(dict_expr)
~~~

## Merging pandas DataFrames

Look at the DataFrames using print (or by just typing the DataFrame name followed by Enter)

In case we would have very long DataFrames, can use `df.head()` (where `df` is your DataFrame name)


## Merging DataFrames on column values

We now have 3 types of data in our 3 DataFrames:

* ChIP-seq peaks (`df_peaks`)
* A RefSeq to ENSEMBL gene identifier mapping (`df_genes`)
* Expression values of genes in 4 samples, with ENSEMBL identifiers (`df_expr`)

This is a typical example for any (bioinformatical) analysis in which you want to incorporate different data types, often from different sources. You need to merge them if you want to do anything useful. Pandas DataFrames can be of big help here.

So let us try to merge these pieces of data

## Merging DataFrames: `concat`

Using the command `concat` we can concatenate multiple DataFrames.
Let us do this with the 2 ChIP-seq peaks DataFrames:

~~~{.python}
df_peaks = pd.concat([df_peaks_exp1, df_peaks_exp2])
~~~

Check out the result, and note how the `exp` column still allows us to distinghuish the two experiments.

Next, we merge the ChIP-seq peaks with the gene identifier mappings, using the `merge` function

## Merging DataFrames: `merge`

Here we will merge the three different DataFrames (`df_peaks`, `df_genes`, `df_expr`) using the gene identifiers. Note that we have two different gene identifiers (RefSeq and ENSEMBL).

First, we merge `df_peaks` with `df_genes`.

~~~{.python}
df_genes_peaks = pd.merge(df_genes, df_peaks, on='refseq')
~~~

Check the resulting DataFrame.
Our DataFrame now has the ENSEMBL gene identifiers, so we can also merge the `df_expr`.

~~~{.python}
df_genes_peaks_expr = pd.merge(df_genes_peaks, df_expr, on='ensembl')
~~~

For convenience, we will use a shorter name for our merged DataFrame

~~~{.python}
df_all  = df_genes_peaks_expr
~~~

## Tab completion and help

Now that we have merged our data, we can do several neat things.

Because `df_all` is a pandas DataFrame, it automatically inherits DataFrame methods.

You can get an idea if you type:

~~~{.python}
df_all.
~~~

Now hit the left \<Tab\> key

Notice that you get a lot of options....

Suppose you want to know how the `sort_values()` method for a DataFrame works. You can type:

~~~{.python}
df_all.sort_values?
~~~

This HELP menu gives you information about the function. type 'q' or \<ESC\> to leave the HELP

## Sorting a DataFrame

Let us try it out:

~~~{.python}
df_all.sort_values(by='chr')
~~~

or

~~~{.python}
df_all.sort_values(by='start')
~~~

Note that these commands did not replace the original DataFrame `df_all`!


## Using `[ ]` to obtain DataFrame columns or rows

Using `[ ]` we can obtain specific parts of the DataFrame.

~~~{.python}
df_all['genename']
## or, for multiple columns:
df_all[['genename', 'exp']]
~~~

Using `iloc` we can do the same using indexes

One index by default points to **rows** of a DataFrame

To get the 2nd row, type:

~~~{.python}
df_all.iloc[2]
~~~

Or, for multiple rows:

~~~{.python}
df_all.iloc[2:5]
~~~

This works very much like indexes of a `list`

If you use two indices, you can obtain specific rows **and** columns.

The first index is always **rows**, the second **columns**.

~~~{.python}
df_all.iloc[2:5, 4:11]
~~~

## Removing missing values or `NaN`s.

In many cases DataFrames contains `NaN`, missing values, and you often have to remove them.

We have several such `NaN`s in `df_all`.

We can obtain a `boolean` (True/False) for the column called 'sample1' like this:

~~~{.python}
df_all['sample1'].isnull()
~~~

By using the `~` symbol we can invert this `boolean` column. The `~` acts as `not` for a pandas Series.

 ~~~{.python}
~df_all['sample1'].isnull()
~~~

We can use this strategy to remove all rows that have `NaN` in the column `sample1`:

~~~{.python}
df_all[~df_all['sample1'].isnull()]
~~~

Make sure you understand how this worked!

We can do the same for getting all rows corresponding to `exp1` (experiment 1):

~~~{.python}
df_all[df_all['exp'] == 'exp1']
~~~

## Basic calculations

There are a lot of methods that allows you to do basic calculation on DataFrames.

As an illustration we will calculate the pairwise correlations between the columns `sample1`, `sample2`, `sample3`, and `sample4`.

We take the appropriate columns first

~~~{.python}
df_1 = df_all[['sample1', 'sample2', 'sample3', 'sample4']]
~~~

Now we can use the pandas `corr` method to calculate all pairwise correlations:

~~~{.python}
df_1.corr()
~~~

Ooops, this did not work. It turns out that the numbers inside our DataFrame are not actually of the appropriate 'type'. We need to convert them to `float`:

~~~{.python}
df_2 = df_1.astype('float')
~~~

Now we can calculate the correlations:

~~~{.python}
df_2.corr()
~~~

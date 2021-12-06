# GNU Datamash 1.3

## Table of Contents

-   [1 Overview](https://www.gnu.org/software/datamash/manual/datamash.html#Overview)
-   [2 Invoking `datamash`](https://www.gnu.org/software/datamash/manual/datamash.html#Invoking-datamash)
-   [3 Available operations in `datamash`](https://www.gnu.org/software/datamash/manual/datamash.html#Available-Operations)
-   [4 Statistical Operations](https://www.gnu.org/software/datamash/manual/datamash.html#Statistical-Operations)
    -   [Equivalent R functions](https://www.gnu.org/software/datamash/manual/datamash.html#Equivalent-R-functions)
-   [5 Usage Examples](https://www.gnu.org/software/datamash/manual/datamash.html#Usage-Examples)
    -   [5.1 Summary Statistics](https://www.gnu.org/software/datamash/manual/datamash.html#Summary-Statistics)
    -   [5.2 Header Lines and Column Names](https://www.gnu.org/software/datamash/manual/datamash.html#Header-Lines-and-Column-Names)
        -   [Output Header Lines](https://www.gnu.org/software/datamash/manual/datamash.html#Output-Header-Lines)
        -   [Skipping Input Header Lines](https://www.gnu.org/software/datamash/manual/datamash.html#Skipping-Input-Header-Lines)
        -   [Using Header Lines](https://www.gnu.org/software/datamash/manual/datamash.html#Using-Header-Lines)
        -   [Column Names](https://www.gnu.org/software/datamash/manual/datamash.html#Column-Names)
    -   [5.3 Field Delimiteres](https://www.gnu.org/software/datamash/manual/datamash.html#Field-Delimiters)
    -   [5.4 Column Ranges](https://www.gnu.org/software/datamash/manual/datamash.html#Column-Ranges)
    -   [5.5 Reverse and Transpose](https://www.gnu.org/software/datamash/manual/datamash.html#Reverse-and-Transpose)
        -   [Transpose](https://www.gnu.org/software/datamash/manual/datamash.html#Transpose)
        -   [Reverse](https://www.gnu.org/software/datamash/manual/datamash.html#Reverse)
        -   [Combining Reverse and Transpose](https://www.gnu.org/software/datamash/manual/datamash.html#Combining-Reverse-and-Transpose)
    -   [5.6 Groupby on /etc/passwd](https://www.gnu.org/software/datamash/manual/datamash.html#Groupby-on-_002fetc_002fpasswd)
    -   [5.7 Check - checking tabular structure](https://www.gnu.org/software/datamash/manual/datamash.html#Check)
        -   [5.7.1 Expected number of lines/fields](https://www.gnu.org/software/datamash/manual/datamash.html#Expected-number-of-lines_002ffields)
        -   [5.7.2 checks in automation scripts](https://www.gnu.org/software/datamash/manual/datamash.html#checks-in-automation-scripts)
    -   [5.8 Crosstab - Cross-Tabulation (pivot-tables)](https://www.gnu.org/software/datamash/manual/datamash.html#Crosstab)
    -   [5.9 Rounding numbers](https://www.gnu.org/software/datamash/manual/datamash.html#Rounding-numbers)
    -   [5.10 Binning numbers](https://www.gnu.org/software/datamash/manual/datamash.html#Binning-numbers)
    -   [5.11 Binning strings](https://www.gnu.org/software/datamash/manual/datamash.html#Binning-strings)
-   [6 Reporting bugs](https://www.gnu.org/software/datamash/manual/datamash.html#Reporting-bugs)
-   [Appendix A GNU Free Documentation License](https://www.gnu.org/software/datamash/manual/datamash.html#GNU-Free-Documentation-License)
-   [Concept index](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index)

Next: [Overview](https://www.gnu.org/software/datamash/manual/datamash.html#Overview), Up: [(dir)](https://www.gnu.org/software/datamash/manual/dir.html#Top)   [[Contents](https://www.gnu.org/software/datamash/manual/datamash.html#SEC_Contents "Table of contents")][[Index](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index "Index")]

# Datamash

This manual is for GNU Datamash (version 1.3, 25 January 2018), which provides command-line computations on input files.

• [Overview](https://www.gnu.org/software/datamash/manual/datamash.html#Overview):

  

General purpose and information.

• [Invoking datamash](https://www.gnu.org/software/datamash/manual/datamash.html#Invoking-datamash):

  

How to run `datamash`.

• [Available Operations](https://www.gnu.org/software/datamash/manual/datamash.html#Available-Operations):

  

Available operations in `datamash`.

• [Statistical Operations](https://www.gnu.org/software/datamash/manual/datamash.html#Statistical-Operations):

  

Statistical operations in `datamash`.

• [Usage Examples](https://www.gnu.org/software/datamash/manual/datamash.html#Usage-Examples):

  

Usage Examples.

• [Reporting bugs](https://www.gnu.org/software/datamash/manual/datamash.html#Reporting-bugs):

  

Sending bug reports and feature suggestions.

• [GNU Free Documentation License](https://www.gnu.org/software/datamash/manual/datamash.html#GNU-Free-Documentation-License):

  

Copying and sharing this documentation.

• [Concept index](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index):

  

Index of concepts.

---

Next: [Invoking datamash](https://www.gnu.org/software/datamash/manual/datamash.html#Invoking-datamash), Previous: [Top](https://www.gnu.org/software/datamash/manual/datamash.html#Top), Up: [Top](https://www.gnu.org/software/datamash/manual/datamash.html#Top)   [[Contents](https://www.gnu.org/software/datamash/manual/datamash.html#SEC_Contents "Table of contents")][[Index](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index "Index")]

## 1 Overview

The `datamash` program ([https://www.gnu.org/software/datamash](https://www.gnu.org/software/datamash)) performs calculation (e.g. _sum,_, _count_, _min_, _max_, _skewness_, _standard deviation_) on input files.

Example: sum up the values in the first column of the input:

$ seq 10 | datamash sum 1
55

`datamash` can group input data and perform operations on each group. It can sort the file, and read header lines.

Example: Given a file with three fields (name, subject, score), find the average score in each subject:

$ cat scores.txt
Name        Subject          Score
Bryan       Arts             68
Isaiah      Arts             80
Gabriel     Health-Medicine  100
Tysza       Business         92
Zackery     Engineering      54
...

$ datamash --sort --headers --group 2 mean 3 sstdev 3 < scores.txt
GroupBy(Subject)   mean(Score)   sstdev(Score)
Arts               68.9474       10.4215
Business           87.3636       5.18214
Engineering        66.5385       19.8814
Health-Medicine    90.6154       9.22441
Life-Sciences      55.3333       20.606
Social-Sciences    60.2667       17.2273

`datamash` is designed for interactive exploration of textual data and for automating tasks in shell scripts.

`datamash` has a rich set of statistical functions to quickly assess information in textual input files. An example of calculating basic statistic (mean, 1st quartile, median, 3rd quarile, IQR, sample-standard-deviation, and p-value of Jarque-Bera test for normal distribution:

$ datamash -H mean 1 q1 1 median 1 q3 1 iqr 1 sstdev 1 jarque 1 < FILE
mean(x)   q1(x)  median(x)  q3(x)   iqr(x)  sstdev(x)  jarque(x)
45.32     23     37         61.5    38.5    30.4487    8.0113-09

---

Next: [Available Operations](https://www.gnu.org/software/datamash/manual/datamash.html#Available-Operations), Previous: [Overview](https://www.gnu.org/software/datamash/manual/datamash.html#Overview), Up: [Top](https://www.gnu.org/software/datamash/manual/datamash.html#Top)   [[Contents](https://www.gnu.org/software/datamash/manual/datamash.html#SEC_Contents "Table of contents")][[Index](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index "Index")]

## 2 Invoking `datamash`

The format for running the `datamash` program is:

datamash [option]… op1 column1  [op2 column2 …]

Where op1 is the operation to perform on the values in column1. `datamash` reads input from stdin and performs one or more operations on the input data. If --group is used, each operation is performed on every group. If --group is not used, each operation is performed on all the values in the input file.

`datamash` supports the following operations:

Primary operations:

`groupby`, `crosstab`, `transpose`, `reverse`, `check`

Line-Filtering operations:

`rmdup`

Per-Line operations:

`base64`, `debase64`, `md5`, `sha1`, `sha256`, `sha512`, `bin`, `strbin`, `round`, `floor`, `ceil`, `trunc`, `frac`

Group-by Numeric operations:

`sum`, `min`, `max`, `absmin`, `absmax`, `range`

Group-by Textual/Numeric operations:

`count`, `first`, `last`, `rand`, `unique`, `collapse`, `countunique`

Group-by Statistical operations:

`mean`, `mode`, `median`, `q1`, `q3`, `iqr`, `perc`, `antimode`, `pstdev`, `sstdev`, `pvar`, `svar`, `mad`, `madraw`, `sskew`, `pskew`, `skurt`, `pkurt`, `jarque`, `dpo`, `scov`, `pcov`, `spearson`, `ppearson`

Grouping options:

--full

-f

Print entire input line before op results (default: print only the grouped keys).

--group=X[,Y,X]

-g X[,Y,X]

Group input via fields X[,Y,Z]. By default, fields are separated by TABs. Use --field-separator to change the delimiter character. Input file must be sorted by the same fields X[,Y,Z]. Use --sort to automatically sort the input. If --group is not specified, each operation is performed in the entire input file.

--header-in

Indicates the first input line is column headers, and should not be used for any calculations.

--header-out

Print column headers as first line. If the column header names are known (i.e. the input file had a header line, and the `command` was invoked with --header-in, -H or --headers), prints the operation and the name of the field (e.g. ‘mean(X)’). Otherwise, prints the number operation and the field number (e.g. ‘mean(field-3)’).

--headers

-H

Same as ‘--header-in --header-out’. A short option indicating the input file has a header line, and the output should contain a header line as well.

--ignore-case

-i

Ignore upper/lower case when comparing text for grouping, sorting, and comparing unique values in the ‘countunique’ and ‘unique’ operations.

--sort

-s

Sort the input before grouping. `datamash` requires sorted input. If the input is not sorted, using --sort will automatically sort the input before processing it further. Sorting will be performed based on the specified --group parameter, and respecting case --ignore-case option (if used). The following commands are equivalent:

$ cat FILE | sort -k1,1 | datamash --group 1 sum 1
$ cat FILE | datamash --sort --group 1 sum 1

File Operation options:

--no-strict

Allow lines with varying number of fields. By default, transpose and reverse will fail with an error message unless all input lines have the same number of fields.

--filler=x

When use --no-strict option, missing fields will be filled with this value.

General options:

--format=FORMAT

print numeric values with printf style floating-point FORMAT.

--field-separator=x

-t x

Use character X instead of TAB as input and output field delimiter. If --output-delimiter is also used, it will override the output field delimiter.

--narm

Skip NA or NaN values.

--output-delimiter=x

Use character X instead as output field delimiter. This option overrides --field-separator/-t/ --whitespace/-W.

--round=N

-R N

Round numeric output to N decimal places.

--whitespace

-W

Use whitespace (one or more spaces and/or tabs) for field delimiters. TAB character will be used as output field separator. If --output-delimiter is also used, it will override the output field delimiter.

--zero-terminated

-z

End lines with a 0 byte, not newline.

--help

Print an informative help message on standard output and exit successfully.

--version

Print the version number and licensing information of Datamash on standard output and then exit successfully.

---

Next: [Statistical Operations](https://www.gnu.org/software/datamash/manual/datamash.html#Statistical-Operations), Previous: [Invoking datamash](https://www.gnu.org/software/datamash/manual/datamash.html#Invoking-datamash), Up: [Top](https://www.gnu.org/software/datamash/manual/datamash.html#Top)   [[Contents](https://www.gnu.org/software/datamash/manual/datamash.html#SEC_Contents "Table of contents")][[Index](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index "Index")]

## 3 Available operations in `datamash`

Primary operations:

groupby

alternative syntax for --group

crosstab

cross-tabulate two fields (also known as ’pivot-tables’)

transpose

transpose rows, columns of a text file

reverse

reverse fields in each line of a text file

check

verify tabular structure of input (ensure same number of fields in all lines)

Line-Filtering operation:

rmdup

remove lines with duplicated key value

Per-Line operations:

base64

encode the field as base64

debase64

decode the field as base64. Exit with an error if the field is invalid base64 value which cannot be decoded.

md5

calculates md5 hash of the field

sha1

calculates sha1 hash of the field

sha256

calculates sha256 hash of the field

sha512

calculates sha512 hash of the field

Group-by Numeric operations:

sum

sum the of values

min

minimum value

max

maximum value

absmin

minimum of the absolute values

absmax

maximum of the absolute values

range

range of values (maximum - minimum)

Group-By Textual/Numeric operations:

count

count number of elements in the group

first

the first value of the group

last

the last value of the group

rand

one random value from the group

unique

comma-separated sorted list of unique values

collapse

comma-separated list of all input values

countunique

number of unique/distinct values

Group-By Statistical operations:

mean

mean of the values

trimmean

trimmed mean of the values

median

median value

q1

1st quartile value

q3

3rd quartile value

iqr

inter-quartile range

perc

percentile value

mode

mode value (most common value)

antimode

anti-mode value (least common value)

pstdev

population standard deviation

sstdev

sample standard deviation

pvar

population variance

svar

sample variance

mad

Median Absolute Deviation, scaled by a constant 1.4826 for normal distributions

madraw

Median Absolute Deviation, unscaled

sskew

skewness of the (sample) group

pskew

skewness of the (population) group

skurt

Excess Kurtosis of the (sample) group

pkurt

Excess Kurtosis of the (population) group

jarque

p-value of the Jarque-Beta test for normality

dpo

p-value of the D’Agostino-Pearson Omnibus test for normality.

---

Next: [Usage Examples](https://www.gnu.org/software/datamash/manual/datamash.html#Usage-Examples), Previous: [Available Operations](https://www.gnu.org/software/datamash/manual/datamash.html#Available-Operations), Up: [Top](https://www.gnu.org/software/datamash/manual/datamash.html#Top)   [[Contents](https://www.gnu.org/software/datamash/manual/datamash.html#SEC_Contents "Table of contents")][[Index](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index "Index")]

## 4 Statistical Operations

### Equivalent R functions

GNU Datamash is designed to closely follow R project’s ([https://www.r-project.org/](https://www.r-project.org/)) statistical functions. See the files/operators.R file for the R equivalent code for each of datamash’s operators. When building `datamash` from source code on your local computer, operators are compared to known results of the equivalent R functions.

---

Next: [Reporting bugs](https://www.gnu.org/software/datamash/manual/datamash.html#Reporting-bugs), Previous: [Statistical Operations](https://www.gnu.org/software/datamash/manual/datamash.html#Statistical-Operations), Up: [Top](https://www.gnu.org/software/datamash/manual/datamash.html#Top)   [[Contents](https://www.gnu.org/software/datamash/manual/datamash.html#SEC_Contents "Table of contents")][[Index](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index "Index")]

## 5 Usage Examples

• [Summary Statistics](https://www.gnu.org/software/datamash/manual/datamash.html#Summary-Statistics):

  

count,min,max,mean,stdev,median,quartiles

• [Header Lines and Column Names](https://www.gnu.org/software/datamash/manual/datamash.html#Header-Lines-and-Column-Names):

  

Using files with header lines

• [Field Delimiters](https://www.gnu.org/software/datamash/manual/datamash.html#Field-Delimiters):

  

Tabs, Whitespace, other delimiteres

• [Column Ranges](https://www.gnu.org/software/datamash/manual/datamash.html#Column-Ranges):

  

Operating on multiple columns

• [Reverse and Transpose](https://www.gnu.org/software/datamash/manual/datamash.html#Reverse-and-Transpose):

  

swapping and transposing rows, columns

• [Groupby on /etc/passwd](https://www.gnu.org/software/datamash/manual/datamash.html#Groupby-on-_002fetc_002fpasswd):

  

Groupby, count, collapse

• [Check](https://www.gnu.org/software/datamash/manual/datamash.html#Check):

  

Validate tabular structure

• [Crosstab](https://www.gnu.org/software/datamash/manual/datamash.html#Crosstab):

  

Cross-tabulation (pivot-tables)

• [Rounding numbers](https://www.gnu.org/software/datamash/manual/datamash.html#Rounding-numbers):

  

round, ceil, floor, trunc, frac

• [Binning numbers](https://www.gnu.org/software/datamash/manual/datamash.html#Binning-numbers):

  

assigning numbers into fixed number of buckets

• [Binning strings](https://www.gnu.org/software/datamash/manual/datamash.html#Binning-strings):

  

assigning strings into fixed number of buckets

---

Next: [Header Lines and Column Names](https://www.gnu.org/software/datamash/manual/datamash.html#Header-Lines-and-Column-Names), Up: [Usage Examples](https://www.gnu.org/software/datamash/manual/datamash.html#Usage-Examples)   [[Contents](https://www.gnu.org/software/datamash/manual/datamash.html#SEC_Contents "Table of contents")][[Index](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index "Index")]

### 5.1 Summary Statistics

The following are examples of using `datamash` to quickly calculate summary statistics. The examples will use a file with three fields (name, subject, score) representing grades of students:

$ cat scores.txt
Shawn     Arts  65
Marques   Arts  58
Fernando  Arts  78
Paul      Arts  63
Walter    Arts  75
...

Counting how many students study each subject (_subject_ is the second field in the input file, thus groupby 2):

$ datamash --sort groupby 2 count 2 < scores.txt
Arts            19
Business        11
Engineering     13
Health-Medicine 13
Life-Sciences   12
Social-Sciences 15

Similary, find the minimum and maximum score in each subject:

$ datamash --sort groupby 2 min 3 max 3 < scores.txt
Arts             46      88
Business         79      94
Engineering      39      99
Health-Medicine  72     100
Life-Sciences    14      91
Social-Sciences  27      90

find the mean and (population) standard deviation in each subject:

$ datamash --sort groupby 2 mean 3 pstdev 3 < scores.txt
Arts              68.947  10.143
Business          87.363   4.940
Engineering       66.538  19.101
Health-Medicine   90.615   8.862
Life-Sciences     55.333  19.728
Social-Sciences   60.266  16.643

Find the median, first, third quariles and the inter-quartile range in each subject:

$ datamash --sort groupby 2 median 3 q1 3 q3  3 iqr 3  < scores.txt
Arts              71      61.5      75.5     14
Business          87      83        92        9
Engineering       56      51        83       32
Health-Medicine   91      84       100       16
Life-Sciences     58.5    44.25     67.75    23.5
Social-Sciences   62      55        70.5     15.5

See [Header Lines and Column Names](https://www.gnu.org/software/datamash/manual/datamash.html#Header-Lines-and-Column-Names) for examples of dealing with header lines.

---

Next: [Field Delimiters](https://www.gnu.org/software/datamash/manual/datamash.html#Field-Delimiters), Previous: [Summary Statistics](https://www.gnu.org/software/datamash/manual/datamash.html#Summary-Statistics), Up: [Usage Examples](https://www.gnu.org/software/datamash/manual/datamash.html#Usage-Examples)   [[Contents](https://www.gnu.org/software/datamash/manual/datamash.html#SEC_Contents "Table of contents")][[Index](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index "Index")]

### 5.2 Header Lines and Column Names

#### Output Header Lines

If the input does _not_ have a header line, use --header-out to add a header in the first line of the output, indicating which operation was performed:

$ datamash --sort --header-out groupby 2 min  3 max 3 < scores.txt
GroupBy(field-2)  min(field-3)  max(field-3)
Arts              46            88
Business          79            94
Engineering       39            99
Health-Medicine   72           100
Life-Sciences     14            91
Social-Sciences   27            90

#### Skipping Input Header Lines

If the input has a header line (first line containing column names), use --header-in to skip the line:

$ cat scores_h.txt
Name      Major   Score
Shawn     Arts    65
Marques   Arts    58
Fernando  Arts    78
Paul      Arts    63
...


$ datamash --sort --header-in groupby 2 mean 3 < scores_h.txt
Arts             68.947
Business         87.363
Engineering      66.538
Health-Medicine  90.615
Life-Sciences    55.333
Social-Sciences  60.266

If the header line is not skipped, `datamash` will show an error (due to strict input validation):

$ datamash groupby 2 mean 3 < scores_h.txt
datamash: invalid numeric value in line 1 field 3: 'Score'

#### Using Header Lines

Column names in the input header lines can be printed in the output header lines by using --headers (or -H, both are equivalent to --header-in --header-out):

$ datamash --sort --headers groupby 2 mean 3 < scores_h.txt
GroupBy(Major)    mean(Score)
Arts              68.947
Business          87.363
Engineering       66.538
Health-Medicine   90.615
Life-Sciences     55.333
Social-Sciences   60.266

Or in short form (-sH instead of --sort --headers), equivalent to the above command:

$ datamash -sH groupby 2 mean 3

#### Column Names

When the input file has a header line, column names can be used instead of column numbers. In the example below, Major is used instead of the value 2, and Score is used instead of the value 3:

$ datamash --sort --headers groupby Major mean Score < scores_h.txt
GroupBy(Major)    mean(Score)
Arts              68.947
Business          87.363
Engineering       66.538
Health-Medicine   90.615
Life-Sciences     55.333
Social-Sciences   60.266

`datamash` will read the first line of the input, and deduce the correct column number based on the given name. If the column name is not found, an error will be printed:

$ datamash --sort --headers groupby 2 mean Foo  < scores_h.txt
datamash: column name 'Foo' not found in input file

---

Next: [Column Ranges](https://www.gnu.org/software/datamash/manual/datamash.html#Column-Ranges), Previous: [Header Lines and Column Names](https://www.gnu.org/software/datamash/manual/datamash.html#Header-Lines-and-Column-Names), Up: [Usage Examples](https://www.gnu.org/software/datamash/manual/datamash.html#Usage-Examples)   [[Contents](https://www.gnu.org/software/datamash/manual/datamash.html#SEC_Contents "Table of contents")][[Index](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index "Index")]

### 5.3 Field Delimiteres

`datamash` uses tabs (ascii character 0x09) as default field delimiters. Use -W to treat one or more consecutive whitespace characters as field delimiters. Use -t, --field-separator to set a custom field delimiter.

The following examples illustrate the various options.

By default, fields are deparated by a single tab tab. Multiple tabs denotes multiple fields (this is consistent with GNU coreutil’s `cut`):

$ printf '1\t\t2\n' | datamash sum 3
2
$ printf '1\t\t2\n' | cut -f3
2

Using -W, one or more consecutive whitespace characters are treated as a single field delimiter:

$ printf '1  \t  2\n' | datamash -W sum 2
2
$ printf '1  \t  2\n' | datamash -W sum 3
datamash: invalid input: field 3 requested, line 1 has only 2 fields

Using -t, a custom field delimiter character can be specified. Multiple consecutive delimiters are treated as multiple fields:

$ printf '1,10,,100\n' | datamash -t, sum 4
100

---

Next: [Reverse and Transpose](https://www.gnu.org/software/datamash/manual/datamash.html#Reverse-and-Transpose), Previous: [Field Delimiters](https://www.gnu.org/software/datamash/manual/datamash.html#Field-Delimiters), Up: [Usage Examples](https://www.gnu.org/software/datamash/manual/datamash.html#Usage-Examples)   [[Contents](https://www.gnu.org/software/datamash/manual/datamash.html#SEC_Contents "Table of contents")][[Index](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index "Index")]

### 5.4 Column Ranges

`datamash` accepts column ranges such as 1,2,3 and 1-3.

Simulating input with multiple columns:

$ seq 100 | paste - - - -
1    2    3    4
5    6    7    8
9   10   11   12
13  14   15   16
17  18   19   20
...

The following are equivalent:

$ seq 100 | paste - - - - | datamash sum 1 sum 2 sum 3 sum 4
1225  1250   1275   1300

$ seq 100 | paste - - - - | datamash sum 1,2,3,4
1225  1250   1275   1300

$ seq 100 | paste - - - - | datamash sum 1-4
1225  1250   1275   1300

$ seq 100 | paste - - - - | datamash sum 1-3,4
1225  1250   1275   1300

Ranges can be used with multiple operations:

$ seq 100 | paste - - - - | datamash sum 1-4 mean 1-4
1225  1250   1275   1300   49   50   51   52

---

Next: [Groupby on /etc/passwd](https://www.gnu.org/software/datamash/manual/datamash.html#Groupby-on-_002fetc_002fpasswd), Previous: [Column Ranges](https://www.gnu.org/software/datamash/manual/datamash.html#Column-Ranges), Up: [Usage Examples](https://www.gnu.org/software/datamash/manual/datamash.html#Usage-Examples)   [[Contents](https://www.gnu.org/software/datamash/manual/datamash.html#SEC_Contents "Table of contents")][[Index](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index "Index")]

### 5.5 Reverse and Transpose

#### Transpose

Use transpose to swap rows and columns in a file:

$ cat input.txt
Sample   Year   Count
A        2014   1002
B        2013    990
C        2014   2030
D        2014    599

$ datamash transpose < input.txt
Sample  A       B       C       D
Year    2014    2013    2014    2014
Count   1002    990     2030    599

By default, transpose verifies the input has the same number of fields in each line, and fails with an error otherwise:

$ cat input.txt
Sample   Year   Count
A        2014   1002
B        2013
C        2014   2030
D        2014    599


$ datamash transpose < input1.txt
datamash: transpose input error: line 3 has 2 fields (previous lines had 3);
see --help to disable strict mode

Use --no-strict to allow missing values:

$ datamash --no-strict transpose < input1.txt
Sample  A       B        C        D
Year    2014    2013     2014     2014
Count   1002    N/A      2030     599

Use --filler to set the missing-field filler value:

$ datamash --no-strict --filler XYZ transpose < input1.txt
Sample  A       B        C        D
Year    2014    2013     2014     2014
Count   1002    XYZ      2030     599

#### Reverse

Use reverse to reverse the fields order in a file:

$ cat input.txt
Sample   Year   Count
A        2014   1002
B        2013    990
C        2014   2030
D        2014    599

$ datamash reverse < input.txt
Count   Year    Sample
1002    2014    A
990     2013    B
2030    2014    C
599     2014    D

By default, reverse verifies the input has the same number of fields in each line, and fails with an error otherwise. Use --no-strict to disable this behaviour (see section above for an example).

#### Combining Reverse and Transpose

Reverse and Transpose can be combined to achieve various manipulations. (reminder: [tac](https://www.gnu.org/software/coreutils/tac) can be used to reverse lines in a file):

$ cat input.txt
A       1       xx
B       2       yy
C       3       zz


$ tac input.txt
C       3       zz
B       2       yy
A       1       xx


$ tac input.txt | datamash reverse
zz      3       C
yy      2       B
xx      1       A


$ cat input.txt | datamash reverse | datamash transpose
xx      yy      zz
1       2       3
A       B       C

$ tac input.txt | datamash reverse | datamash transpose
zz      yy      xx
3       2       1
C       B       A

---

Next: [Check](https://www.gnu.org/software/datamash/manual/datamash.html#Check), Previous: [Reverse and Transpose](https://www.gnu.org/software/datamash/manual/datamash.html#Reverse-and-Transpose), Up: [Usage Examples](https://www.gnu.org/software/datamash/manual/datamash.html#Usage-Examples)   [[Contents](https://www.gnu.org/software/datamash/manual/datamash.html#SEC_Contents "Table of contents")][[Index](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index "Index")]

### 5.6 Groupby on /etc/passwd

`datamash` with the groupby operation mode can be used to aggregate information.

Using this simuated /etc/passwd file as input:

$ cat passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
mysql:x:115:124:MySQL Server,,,:/var/lib/mysql:/bin/false
sshd:x:116:65534::/var/run/sshd:/usr/sbin/nologin
guest:x:118:125:Guest,,,:/tmp/guest-home.phc17z:/bin/bash
gordon:x:1004:1000:Assaf Gordon,,,,:/home/gordon:/bin/bash
charles:x:1005:1000:Charles,,,,:/home/charles:/bin/bash
alice:x:1006:1000:Alice,,,,:/home/alice:/bin/bash
bob:x:1007:1000:Bob,,,,:/home/bob:/bin/bash
postgres:x:119:126:PostgreSQL administrator,,,:/var/lib/postgresql:/bin/bash
rabbitmq:x:125:138:RabbitMQ messaging server,,,:/var/lib/rabbitmq:/bin/false
redis:x:126:140:redis server,,,:/var/lib/redis:/bin/false
postfix:x:127:141::/var/spool/postfix:/bin/false

Parameter -t is used to indicate the field separator : (instead of the default tab).

Aggregate (groupby) login shells (column 7) and count how many users use each:

$ datamash -t: --sort groupby 7 count 7 < passwd
/bin/bash:7
/bin/false:4
/bin/sync:1
/usr/sbin/nologin:14

Aggregate (groupby) login shells (column 7) and print comma-separated list of users (column 1) for each shell (collapse):

$ cat passwd | datamash -t: --sort groupby 7 collapse 1
/bin/bash:root,guest,gordon,charles,alice,bob,postgres
/bin/false:mysql,rabbitmq,redis,postfix
/bin/sync:sync
/usr/sbin/nologin:daemon,bin,sys,games,man,lp,mail,news,uucp,proxy ,www-data,backup,list,sshd

Aggregate unix-groups (column 4) and print comma-separated list of users (column 1) for in each group:

$ datamash -t: --sort groupby 4 collapse 1 < /etc/passwd
0:root
1:daemon
10:uucp
1000:gordon,charles,alice,bob
12:man
124:mysql
125:guest
126:postgres
13:proxy
138:rabbitmq
140:redis
141:postfix
2:bin
3:sys
33:www-data
34:backup
38:list
60:games
65534:sync,sshd
7:lp
8:mail
9:news

---

Next: [Crosstab](https://www.gnu.org/software/datamash/manual/datamash.html#Crosstab), Previous: [Groupby on /etc/passwd](https://www.gnu.org/software/datamash/manual/datamash.html#Groupby-on-_002fetc_002fpasswd), Up: [Usage Examples](https://www.gnu.org/software/datamash/manual/datamash.html#Usage-Examples)   [[Contents](https://www.gnu.org/software/datamash/manual/datamash.html#SEC_Contents "Table of contents")][[Index](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index "Index")]

### 5.7 Check - checking tabular structure

`datamash` check validates the tabular structure of a file, ensuring all lines have the same number of fields. check is meant to be used in scripting and automation pipelines, as it will terminate with non-zero exit code if the file is not well structured, while also printing detailed context information about the offending lines:

$ cat good.txt
A    1    ww
B    2    xx
C    3    yy
D    4    zz


$ cat bad.txt
A    1    ww
B    2    xx
C    3
D    4    zz


$ datamash check < good.txt && echo ok || echo fail
4 lines, 3 fields
ok


$ datamash check < bad.txt && echo ok || echo fail
line 2 (3 fields):
  B  2 xx
line 3 (2 fields):
  C  3
datamash: check failed: line 3 has 2 fields (previous line had 3)
fail

#### 5.7.1 Expected number of lines/fields

check accepts optional lines and fields and will return failure if the input does not have the requested number of lines/fields.

The syntax is:

datamash check [N lines] [N fields]

Usage examples:

$ cat file.txt
A    1    ww
B    2    xx
C    3    yy
D    4    zz

$ datamash check 4 lines < file.txt && echo ok
4 lines, 3 fields
ok

$ datamash check 3 fields < file.txt && echo ok
4 lines, 3 fields
ok

$ datamash check 4 lines 3 fields < file.txt && echo ok
4 lines, 3 fields
ok

$ datamash check 7 fields < file.txt && echo ok
line 1 (3 fields):
  A    1    ww
datamash: check failed: line 1 has 3 fields (expecting 22)

$ datamash check 10 lines < file.txt && echo ok
datamash: check failed: input had 4 lines (expecting 10)

For convenience, line,row,rows can be used instead of lines; field,columns,column,col can be used instead of fields. The following are all equivalent:

datamash check 4 lines 10 fields < file.txt
datamash check 4 rows  10 columns < file.txt
datamash check 10 col 4 row < file.txt

#### 5.7.2 checks in automation scripts

In pipeline/automation context, it is often beneficial to validate files as early as possible (immediately after file is created, as in [fail-fast methodology](https://en.wikipedia.org/wiki/Fail-fast)). A typical usage in a shell script would be:

#!/bin/sh

die()
{
    base=$(basename "$0")
    echo "$base: error: $@" >&2
    exit 1
}

custom pipeline-or-program > output.txt \
    || die "program failed"

datamash check < output.txt \
    || die "'output.txt' has invalid structure (missing fields)"

If the generated output.txt file has invalid structure (i.e. missing fields), `datamash` will print the stderr enough details to help in troubleshooting (line numbers and offending line’s content).

---

Next: [Rounding numbers](https://www.gnu.org/software/datamash/manual/datamash.html#Rounding-numbers), Previous: [Check](https://www.gnu.org/software/datamash/manual/datamash.html#Check), Up: [Usage Examples](https://www.gnu.org/software/datamash/manual/datamash.html#Usage-Examples)   [[Contents](https://www.gnu.org/software/datamash/manual/datamash.html#SEC_Contents "Table of contents")][[Index](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index "Index")]

### 5.8 Crosstab - Cross-Tabulation (pivot-tables)

Cross-tabulation compares the relationship between two fields. Given the following input file:

$ cat input.txt
a    x    3
a    y    7
b    x    21
a    x    40

Show cross-tabulation between the first field (a/b) and the second field (x/y) - counting how many times each pair appears (note: sorting is required):

$ datamash -s crosstab 1,2 < input.txt
     x    y
a    2    1
b    1    N/A

The default operation is count - in the above example, a and x appear twice in the input file, while b and y never appear together.

An optional grouping operation can be used instead of counting.

For each pair, sum the values in the third column:

$ datamash -s crosstab 1,2 sum 3 < input.txt
     x    y
a    43   7
b    21   N/A

For each pair, list all unique values in the third column:

$ datamash -s crosstab 1,2 unique 3 < input.txt
     x    y
a    3,40 7
b    21   N/A

---

Next: [Binning numbers](https://www.gnu.org/software/datamash/manual/datamash.html#Binning-numbers), Previous: [Crosstab](https://www.gnu.org/software/datamash/manual/datamash.html#Crosstab), Up: [Usage Examples](https://www.gnu.org/software/datamash/manual/datamash.html#Usage-Examples)   [[Contents](https://www.gnu.org/software/datamash/manual/datamash.html#SEC_Contents "Table of contents")][[Index](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index "Index")]

### 5.9 Rounding numbers

The following demonstrate the different rounding operations:

$ ( echo X ; seq -1.25 0.25 1.25 ) \
      | datamash --full -H round 1 ceil 1 floor 1 trunc 1 frac 1

  X     round(X)  ceil(X)  floor(X)  trunc(X)   frac(X)
-1.25   -1        -1       -2        -1         -0.25
-1.00   -1        -1       -1        -1          0
-0.75   -1         0       -1         0         -0.75
-0.50   -1         0       -1         0         -0.5
-0.25    0         0       -1         0         -0.25
 0.00    0         0        0         0          0
 0.25    0         1        0         0          0.25
 0.50    1         1        0         0          0.5
 0.75    1         1        0         0          0.75
 1.00    1         1        1         1          0
 1.25    1         2        1         1          0.25

---

Next: [Binning strings](https://www.gnu.org/software/datamash/manual/datamash.html#Binning-strings), Previous: [Rounding numbers](https://www.gnu.org/software/datamash/manual/datamash.html#Rounding-numbers), Up: [Usage Examples](https://www.gnu.org/software/datamash/manual/datamash.html#Usage-Examples)   [[Contents](https://www.gnu.org/software/datamash/manual/datamash.html#SEC_Contents "Table of contents")][[Index](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index "Index")]

### 5.10 Binning numbers

Bin input values into buckets of size 5:

$ ( echo X ; seq -10 2.5 10 ) \
      | datamash -H --full bin:5 1
    X  bin(X)
-10.0    -15
 -7.5    -10
 -5.0    -10
 -2.5     -5
  0.0      0
  2.5      0
  5.0      5
  7.5      5
 10.0     10

---

Previous: [Binning numbers](https://www.gnu.org/software/datamash/manual/datamash.html#Binning-numbers), Up: [Usage Examples](https://www.gnu.org/software/datamash/manual/datamash.html#Usage-Examples)   [[Contents](https://www.gnu.org/software/datamash/manual/datamash.html#SEC_Contents "Table of contents")][[Index](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index "Index")]

### 5.11 Binning strings

Hash any string input value into a numeric integer. A typical usage would be to split an input file into N chunks, ensuring that all values of a certain key will be stored in the same chunk:

$ cat input.txt
PatientA   10
PatientB   11
PatientC   12
PatientA   14
PatientC   15

Each patient ID is hashed into a bin between 0 and 9 and printed in the last field:

$ datamash --full strbin 1 < input.txt
PatientA   10    5
PatientB   11    6
PatientC   12    7
PatientA   14    5
PatientC   15    7

Splitting the input into chunks can be done with awk:

$ cat input.txt | datamash --full strbin 1 \
    | awk '{print > $NF ".txt"}'

---

Next: [GNU Free Documentation License](https://www.gnu.org/software/datamash/manual/datamash.html#GNU-Free-Documentation-License), Previous: [Usage Examples](https://www.gnu.org/software/datamash/manual/datamash.html#Usage-Examples), Up: [Top](https://www.gnu.org/software/datamash/manual/datamash.html#Top)   [[Contents](https://www.gnu.org/software/datamash/manual/datamash.html#SEC_Contents "Table of contents")][[Index](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index "Index")]

## 6 Reporting bugs

To report bugs, suggest enhancements or otherwise discuss GNU Datamash, please send electronic mail to [bug-datamash@gnu.org](mailto:bug-datamash@gnu.org).

For bug reports, please include enough information for the maintainers to reproduce the problem. Generally speaking, that means:

-   The version numbers of Datamash (which you can find by running ‘datamash --version’) and any other program(s) or manual(s) involved.
-   Hardware and operating system names and versions.
-   The contents of any input files necessary to reproduce the bug.
-   The expected behavior and/or output.
-   A description of the problem and samples of any erroneous output.
-   Options you gave to `configure` other than specifying installation directories.
-   Anything else that you think would be helpful.

When in doubt whether something is needed or not, include it. It’s better to include too much than to leave out something important.

Patches are welcome; if possible, please make them with ‘diff -c’ (see [Overview](https://www.gnu.org/software/datamash/manual/diff.html#Top) in Comparing and Merging Files) and include ChangeLog entries (see [Change Log](http://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Change-Log) in The GNU Emacs Manual). Please follow the existing coding style.

---

Next: [Concept index](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index), Previous: [Reporting bugs](https://www.gnu.org/software/datamash/manual/datamash.html#Reporting-bugs), Up: [Top](https://www.gnu.org/software/datamash/manual/datamash.html#Top)   [[Contents](https://www.gnu.org/software/datamash/manual/datamash.html#SEC_Contents "Table of contents")][[Index](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index "Index")]

## Appendix A GNU Free Documentation License

Version 1.3, 3 November 2008

Copyright © 2000, 2001, 2002, 2007, 2008 Free Software Foundation, Inc.
[https://fsf.org/](https://fsf.org/)

Everyone is permitted to copy and distribute verbatim copies
of this license document, but changing it is not allowed.

1.  PREAMBLE
    
    The purpose of this License is to make a manual, textbook, or other functional and useful document _free_ in the sense of freedom: to assure everyone the effective freedom to copy and redistribute it, with or without modifying it, either commercially or noncommercially. Secondarily, this License preserves for the author and publisher a way to get credit for their work, while not being considered responsible for modifications made by others.
    
    This License is a kind of “copyleft”, which means that derivative works of the document must themselves be free in the same sense. It complements the GNU General Public License, which is a copyleft license designed for free software.
    
    We have designed this License in order to use it for manuals for free software, because free software needs free documentation: a free program should come with manuals providing the same freedoms that the software does. But this License is not limited to software manuals; it can be used for any textual work, regardless of subject matter or whether it is published as a printed book. We recommend this License principally for works whose purpose is instruction or reference.
    
2.  APPLICABILITY AND DEFINITIONS
    
    This License applies to any manual or other work, in any medium, that contains a notice placed by the copyright holder saying it can be distributed under the terms of this License. Such a notice grants a world-wide, royalty-free license, unlimited in duration, to use that work under the conditions stated herein. The “Document”, below, refers to any such manual or work. Any member of the public is a licensee, and is addressed as “you”. You accept the license if you copy, modify or distribute the work in a way requiring permission under copyright law.
    
    A “Modified Version” of the Document means any work containing the Document or a portion of it, either copied verbatim, or with modifications and/or translated into another language.
    
    A “Secondary Section” is a named appendix or a front-matter section of the Document that deals exclusively with the relationship of the publishers or authors of the Document to the Document’s overall subject (or to related matters) and contains nothing that could fall directly within that overall subject. (Thus, if the Document is in part a textbook of mathematics, a Secondary Section may not explain any mathematics.) The relationship could be a matter of historical connection with the subject or with related matters, or of legal, commercial, philosophical, ethical or political position regarding them.
    
    The “Invariant Sections” are certain Secondary Sections whose titles are designated, as being those of Invariant Sections, in the notice that says that the Document is released under this License. If a section does not fit the above definition of Secondary then it is not allowed to be designated as Invariant. The Document may contain zero Invariant Sections. If the Document does not identify any Invariant Sections then there are none.
    
    The “Cover Texts” are certain short passages of text that are listed, as Front-Cover Texts or Back-Cover Texts, in the notice that says that the Document is released under this License. A Front-Cover Text may be at most 5 words, and a Back-Cover Text may be at most 25 words.
    
    A “Transparent” copy of the Document means a machine-readable copy, represented in a format whose specification is available to the general public, that is suitable for revising the document straightforwardly with generic text editors or (for images composed of pixels) generic paint programs or (for drawings) some widely available drawing editor, and that is suitable for input to text formatters or for automatic translation to a variety of formats suitable for input to text formatters. A copy made in an otherwise Transparent file format whose markup, or absence of markup, has been arranged to thwart or discourage subsequent modification by readers is not Transparent. An image format is not Transparent if used for any substantial amount of text. A copy that is not “Transparent” is called “Opaque”.
    
    Examples of suitable formats for Transparent copies include plain ASCII without markup, Texinfo input format, LaTeX input format, SGML or XML using a publicly available DTD, and standard-conforming simple HTML, PostScript or PDF designed for human modification. Examples of transparent image formats include PNG, XCF and JPG. Opaque formats include proprietary formats that can be read and edited only by proprietary word processors, SGML or XML for which the DTD and/or processing tools are not generally available, and the machine-generated HTML, PostScript or PDF produced by some word processors for output purposes only.
    
    The “Title Page” means, for a printed book, the title page itself, plus such following pages as are needed to hold, legibly, the material this License requires to appear in the title page. For works in formats which do not have any title page as such, “Title Page” means the text near the most prominent appearance of the work’s title, preceding the beginning of the body of the text.
    
    The “publisher” means any person or entity that distributes copies of the Document to the public.
    
    A section “Entitled XYZ” means a named subunit of the Document whose title either is precisely XYZ or contains XYZ in parentheses following text that translates XYZ in another language. (Here XYZ stands for a specific section name mentioned below, such as “Acknowledgements”, “Dedications”, “Endorsements”, or “History”.) To “Preserve the Title” of such a section when you modify the Document means that it remains a section “Entitled XYZ” according to this definition.
    
    The Document may include Warranty Disclaimers next to the notice which states that this License applies to the Document. These Warranty Disclaimers are considered to be included by reference in this License, but only as regards disclaiming warranties: any other implication that these Warranty Disclaimers may have is void and has no effect on the meaning of this License.
    
3.  VERBATIM COPYING
    
    You may copy and distribute the Document in any medium, either commercially or noncommercially, provided that this License, the copyright notices, and the license notice saying this License applies to the Document are reproduced in all copies, and that you add no other conditions whatsoever to those of this License. You may not use technical measures to obstruct or control the reading or further copying of the copies you make or distribute. However, you may accept compensation in exchange for copies. If you distribute a large enough number of copies you must also follow the conditions in section 3.
    
    You may also lend copies, under the same conditions stated above, and you may publicly display copies.
    
4.  COPYING IN QUANTITY
    
    If you publish printed copies (or copies in media that commonly have printed covers) of the Document, numbering more than 100, and the Document’s license notice requires Cover Texts, you must enclose the copies in covers that carry, clearly and legibly, all these Cover Texts: Front-Cover Texts on the front cover, and Back-Cover Texts on the back cover. Both covers must also clearly and legibly identify you as the publisher of these copies. The front cover must present the full title with all words of the title equally prominent and visible. You may add other material on the covers in addition. Copying with changes limited to the covers, as long as they preserve the title of the Document and satisfy these conditions, can be treated as verbatim copying in other respects.
    
    If the required texts for either cover are too voluminous to fit legibly, you should put the first ones listed (as many as fit reasonably) on the actual cover, and continue the rest onto adjacent pages.
    
    If you publish or distribute Opaque copies of the Document numbering more than 100, you must either include a machine-readable Transparent copy along with each Opaque copy, or state in or with each Opaque copy a computer-network location from which the general network-using public has access to download using public-standard network protocols a complete Transparent copy of the Document, free of added material. If you use the latter option, you must take reasonably prudent steps, when you begin distribution of Opaque copies in quantity, to ensure that this Transparent copy will remain thus accessible at the stated location until at least one year after the last time you distribute an Opaque copy (directly or through your agents or retailers) of that edition to the public.
    
    It is requested, but not required, that you contact the authors of the Document well before redistributing any large number of copies, to give them a chance to provide you with an updated version of the Document.
    
5.  MODIFICATIONS
    
    You may copy and distribute a Modified Version of the Document under the conditions of sections 2 and 3 above, provided that you release the Modified Version under precisely this License, with the Modified Version filling the role of the Document, thus licensing distribution and modification of the Modified Version to whoever possesses a copy of it. In addition, you must do these things in the Modified Version:
    
    1.  Use in the Title Page (and on the covers, if any) a title distinct from that of the Document, and from those of previous versions (which should, if there were any, be listed in the History section of the Document). You may use the same title as a previous version if the original publisher of that version gives permission.
    2.  List on the Title Page, as authors, one or more persons or entities responsible for authorship of the modifications in the Modified Version, together with at least five of the principal authors of the Document (all of its principal authors, if it has fewer than five), unless they release you from this requirement.
    3.  State on the Title page the name of the publisher of the Modified Version, as the publisher.
    4.  Preserve all the copyright notices of the Document.
    5.  Add an appropriate copyright notice for your modifications adjacent to the other copyright notices.
    6.  Include, immediately after the copyright notices, a license notice giving the public permission to use the Modified Version under the terms of this License, in the form shown in the Addendum below.
    7.  Preserve in that license notice the full lists of Invariant Sections and required Cover Texts given in the Document’s license notice.
    8.  Include an unaltered copy of this License.
    9.  Preserve the section Entitled “History”, Preserve its Title, and add to it an item stating at least the title, year, new authors, and publisher of the Modified Version as given on the Title Page. If there is no section Entitled “History” in the Document, create one stating the title, year, authors, and publisher of the Document as given on its Title Page, then add an item describing the Modified Version as stated in the previous sentence.
    10.  Preserve the network location, if any, given in the Document for public access to a Transparent copy of the Document, and likewise the network locations given in the Document for previous versions it was based on. These may be placed in the “History” section. You may omit a network location for a work that was published at least four years before the Document itself, or if the original publisher of the version it refers to gives permission.
    11.  For any section Entitled “Acknowledgements” or “Dedications”, Preserve the Title of the section, and preserve in the section all the substance and tone of each of the contributor acknowledgements and/or dedications given therein.
    12.  Preserve all the Invariant Sections of the Document, unaltered in their text and in their titles. Section numbers or the equivalent are not considered part of the section titles.
    13.  Delete any section Entitled “Endorsements”. Such a section may not be included in the Modified Version.
    14.  Do not retitle any existing section to be Entitled “Endorsements” or to conflict in title with any Invariant Section.
    15.  Preserve any Warranty Disclaimers.
    
    If the Modified Version includes new front-matter sections or appendices that qualify as Secondary Sections and contain no material copied from the Document, you may at your option designate some or all of these sections as invariant. To do this, add their titles to the list of Invariant Sections in the Modified Version’s license notice. These titles must be distinct from any other section titles.
    
    You may add a section Entitled “Endorsements”, provided it contains nothing but endorsements of your Modified Version by various parties—for example, statements of peer review or that the text has been approved by an organization as the authoritative definition of a standard.
    
    You may add a passage of up to five words as a Front-Cover Text, and a passage of up to 25 words as a Back-Cover Text, to the end of the list of Cover Texts in the Modified Version. Only one passage of Front-Cover Text and one of Back-Cover Text may be added by (or through arrangements made by) any one entity. If the Document already includes a cover text for the same cover, previously added by you or by arrangement made by the same entity you are acting on behalf of, you may not add another; but you may replace the old one, on explicit permission from the previous publisher that added the old one.
    
    The author(s) and publisher(s) of the Document do not by this License give permission to use their names for publicity for or to assert or imply endorsement of any Modified Version.
    
6.  COMBINING DOCUMENTS
    
    You may combine the Document with other documents released under this License, under the terms defined in section 4 above for modified versions, provided that you include in the combination all of the Invariant Sections of all of the original documents, unmodified, and list them all as Invariant Sections of your combined work in its license notice, and that you preserve all their Warranty Disclaimers.
    
    The combined work need only contain one copy of this License, and multiple identical Invariant Sections may be replaced with a single copy. If there are multiple Invariant Sections with the same name but different contents, make the title of each such section unique by adding at the end of it, in parentheses, the name of the original author or publisher of that section if known, or else a unique number. Make the same adjustment to the section titles in the list of Invariant Sections in the license notice of the combined work.
    
    In the combination, you must combine any sections Entitled “History” in the various original documents, forming one section Entitled “History”; likewise combine any sections Entitled “Acknowledgements”, and any sections Entitled “Dedications”. You must delete all sections Entitled “Endorsements.”
    
7.  COLLECTIONS OF DOCUMENTS
    
    You may make a collection consisting of the Document and other documents released under this License, and replace the individual copies of this License in the various documents with a single copy that is included in the collection, provided that you follow the rules of this License for verbatim copying of each of the documents in all other respects.
    
    You may extract a single document from such a collection, and distribute it individually under this License, provided you insert a copy of this License into the extracted document, and follow this License in all other respects regarding verbatim copying of that document.
    
8.  AGGREGATION WITH INDEPENDENT WORKS
    
    A compilation of the Document or its derivatives with other separate and independent documents or works, in or on a volume of a storage or distribution medium, is called an “aggregate” if the copyright resulting from the compilation is not used to limit the legal rights of the compilation’s users beyond what the individual works permit. When the Document is included in an aggregate, this License does not apply to the other works in the aggregate which are not themselves derivative works of the Document.
    
    If the Cover Text requirement of section 3 is applicable to these copies of the Document, then if the Document is less than one half of the entire aggregate, the Document’s Cover Texts may be placed on covers that bracket the Document within the aggregate, or the electronic equivalent of covers if the Document is in electronic form. Otherwise they must appear on printed covers that bracket the whole aggregate.
    
9.  TRANSLATION
    
    Translation is considered a kind of modification, so you may distribute translations of the Document under the terms of section 4. Replacing Invariant Sections with translations requires special permission from their copyright holders, but you may include translations of some or all Invariant Sections in addition to the original versions of these Invariant Sections. You may include a translation of this License, and all the license notices in the Document, and any Warranty Disclaimers, provided that you also include the original English version of this License and the original versions of those notices and disclaimers. In case of a disagreement between the translation and the original version of this License or a notice or disclaimer, the original version will prevail.
    
    If a section in the Document is Entitled “Acknowledgements”, “Dedications”, or “History”, the requirement (section 4) to Preserve its Title (section 1) will typically require changing the actual title.
    
10.  TERMINATION
    
    You may not copy, modify, sublicense, or distribute the Document except as expressly provided under this License. Any attempt otherwise to copy, modify, sublicense, or distribute it is void, and will automatically terminate your rights under this License.
    
    However, if you cease all violation of this License, then your license from a particular copyright holder is reinstated (a) provisionally, unless and until the copyright holder explicitly and finally terminates your license, and (b) permanently, if the copyright holder fails to notify you of the violation by some reasonable means prior to 60 days after the cessation.
    
    Moreover, your license from a particular copyright holder is reinstated permanently if the copyright holder notifies you of the violation by some reasonable means, this is the first time you have received notice of violation of this License (for any work) from that copyright holder, and you cure the violation prior to 30 days after your receipt of the notice.
    
    Termination of your rights under this section does not terminate the licenses of parties who have received copies or rights from you under this License. If your rights have been terminated and not permanently reinstated, receipt of a copy of some or all of the same material does not give you any rights to use it.
    
11.  FUTURE REVISIONS OF THIS LICENSE
    
    The Free Software Foundation may publish new, revised versions of the GNU Free Documentation License from time to time. Such new versions will be similar in spirit to the present version, but may differ in detail to address new problems or concerns. See [https://www.gnu.org/copyleft/](https://www.gnu.org/copyleft/).
    
    Each version of the License is given a distinguishing version number. If the Document specifies that a particular numbered version of this License “or any later version” applies to it, you have the option of following the terms and conditions either of that specified version or of any later version that has been published (not as a draft) by the Free Software Foundation. If the Document does not specify a version number of this License, you may choose any version ever published (not as a draft) by the Free Software Foundation. If the Document specifies that a proxy can decide which future versions of this License can be used, that proxy’s public statement of acceptance of a version permanently authorizes you to choose that version for the Document.
    
12.  RELICENSING
    
    “Massive Multiauthor Collaboration Site” (or “MMC Site”) means any World Wide Web server that publishes copyrightable works and also provides prominent facilities for anybody to edit those works. A public wiki that anybody can edit is an example of such a server. A “Massive Multiauthor Collaboration” (or “MMC”) contained in the site means any set of copyrightable works thus published on the MMC site.
    
    “CC-BY-SA” means the Creative Commons Attribution-Share Alike 3.0 license published by Creative Commons Corporation, a not-for-profit corporation with a principal place of business in San Francisco, California, as well as future copyleft versions of that license published by that same organization.
    
    “Incorporate” means to publish or republish a Document, in whole or in part, as part of another Document.
    
    An MMC is “eligible for relicensing” if it is licensed under this License, and if all works that were first published under this License somewhere other than this MMC, and subsequently incorporated in whole or in part into the MMC, (1) had no cover texts or invariant sections, and (2) were thus incorporated prior to November 1, 2008.
    
    The operator of an MMC Site may republish an MMC contained in the site under CC-BY-SA on the same site at any time before August 1, 2009, provided the MMC is eligible for relicensing.
    

### ADDENDUM: How to use this License for your documents

To use this License in a document you have written, include a copy of the License in the document and put the following copyright and license notices just after the title page:

  Copyright (C)  year  your name.
  Permission is granted to copy, distribute and/or modify this document
  under the terms of the GNU Free Documentation License, Version 1.3
  or any later version published by the Free Software Foundation;
  with no Invariant Sections, no Front-Cover Texts, and no Back-Cover
  Texts.  A copy of the license is included in the section entitled ``GNU
  Free Documentation License''.

If you have Invariant Sections, Front-Cover Texts and Back-Cover Texts, replace the “with…Texts.” line with this:

    with the Invariant Sections being list their titles, with
    the Front-Cover Texts being list, and with the Back-Cover Texts
    being list.

If you have Invariant Sections without Cover Texts, or some other combination of the three, merge those two alternatives to suit the situation.

If your document contains nontrivial examples of program code, we recommend releasing these examples in parallel under your choice of free software license, such as the GNU General Public License, to permit their use in free software.

---

Previous: [GNU Free Documentation License](https://www.gnu.org/software/datamash/manual/datamash.html#GNU-Free-Documentation-License), Up: [Top](https://www.gnu.org/software/datamash/manual/datamash.html#Top)   [[Contents](https://www.gnu.org/software/datamash/manual/datamash.html#SEC_Contents "Table of contents")][[Index](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index "Index")]

## Concept index

Jump to:  

[**-**](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index_cp_symbol-1)   [**/**](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index_cp_symbol-2)    
[**B**](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index_cp_letter-B)   [**C**](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index_cp_letter-C)   [**D**](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index_cp_letter-D)   [**E**](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index_cp_letter-E)   [**F**](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index_cp_letter-F)   [**G**](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index_cp_letter-G)   [**H**](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index_cp_letter-H)   [**I**](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index_cp_letter-I)   [**L**](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index_cp_letter-L)   [**M**](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index_cp_letter-M)   [**N**](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index_cp_letter-N)   [**O**](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index_cp_letter-O)   [**P**](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index_cp_letter-P)   [**Q**](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index_cp_letter-Q)   [**R**](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index_cp_letter-R)   [**S**](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index_cp_letter-S)   [**T**](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index_cp_letter-T)   [**U**](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index_cp_letter-U)   [**W**](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index_cp_letter-W)  

Index Entry

 

Section

---

-

[`--field-separator`](https://www.gnu.org/software/datamash/manual/datamash.html#index-_002d_002dfield_002dseparator):

 

[Invoking datamash](https://www.gnu.org/software/datamash/manual/datamash.html#Invoking-datamash)

[`--field-separator`](https://www.gnu.org/software/datamash/manual/datamash.html#index-_002d_002dfield_002dseparator-1):

 

[Groupby on /etc/passwd](https://www.gnu.org/software/datamash/manual/datamash.html#Groupby-on-_002fetc_002fpasswd)

[`--filler`](https://www.gnu.org/software/datamash/manual/datamash.html#index-_002d_002dfiller):

 

[Invoking datamash](https://www.gnu.org/software/datamash/manual/datamash.html#Invoking-datamash)

[`--filler`](https://www.gnu.org/software/datamash/manual/datamash.html#index-_002d_002dfiller-1):

 

[Reverse and Transpose](https://www.gnu.org/software/datamash/manual/datamash.html#Reverse-and-Transpose)

[`--format`](https://www.gnu.org/software/datamash/manual/datamash.html#index-_002d_002dformat):

 

[Invoking datamash](https://www.gnu.org/software/datamash/manual/datamash.html#Invoking-datamash)

[`--full`](https://www.gnu.org/software/datamash/manual/datamash.html#index-_002d_002dfull):

 

[Invoking datamash](https://www.gnu.org/software/datamash/manual/datamash.html#Invoking-datamash)

[`--group`](https://www.gnu.org/software/datamash/manual/datamash.html#index-_002d_002dgroup):

 

[Invoking datamash](https://www.gnu.org/software/datamash/manual/datamash.html#Invoking-datamash)

[`--header-in`](https://www.gnu.org/software/datamash/manual/datamash.html#index-_002d_002dheader_002din):

 

[Invoking datamash](https://www.gnu.org/software/datamash/manual/datamash.html#Invoking-datamash)

[`--header-in`](https://www.gnu.org/software/datamash/manual/datamash.html#index-_002d_002dheader_002din-1):

 

[Header Lines and Column Names](https://www.gnu.org/software/datamash/manual/datamash.html#Header-Lines-and-Column-Names)

[`--header-out`](https://www.gnu.org/software/datamash/manual/datamash.html#index-_002d_002dheader_002dout):

 

[Invoking datamash](https://www.gnu.org/software/datamash/manual/datamash.html#Invoking-datamash)

[`--header-out`](https://www.gnu.org/software/datamash/manual/datamash.html#index-_002d_002dheader_002dout-1):

 

[Header Lines and Column Names](https://www.gnu.org/software/datamash/manual/datamash.html#Header-Lines-and-Column-Names)

[`--headers`](https://www.gnu.org/software/datamash/manual/datamash.html#index-_002d_002dheaders):

 

[Invoking datamash](https://www.gnu.org/software/datamash/manual/datamash.html#Invoking-datamash)

[`--headers`](https://www.gnu.org/software/datamash/manual/datamash.html#index-_002d_002dheaders-1):

 

[Header Lines and Column Names](https://www.gnu.org/software/datamash/manual/datamash.html#Header-Lines-and-Column-Names)

[`--help`](https://www.gnu.org/software/datamash/manual/datamash.html#index-_002d_002dhelp):

 

[Invoking datamash](https://www.gnu.org/software/datamash/manual/datamash.html#Invoking-datamash)

[`--ignore-case`](https://www.gnu.org/software/datamash/manual/datamash.html#index-_002d_002dignore_002dcase):

 

[Invoking datamash](https://www.gnu.org/software/datamash/manual/datamash.html#Invoking-datamash)

[`--narm`](https://www.gnu.org/software/datamash/manual/datamash.html#index-_002d_002dnarm):

 

[Invoking datamash](https://www.gnu.org/software/datamash/manual/datamash.html#Invoking-datamash)

[`--no-strict`](https://www.gnu.org/software/datamash/manual/datamash.html#index-_002d_002dno_002dstrict):

 

[Invoking datamash](https://www.gnu.org/software/datamash/manual/datamash.html#Invoking-datamash)

[`--no-strict`](https://www.gnu.org/software/datamash/manual/datamash.html#index-_002d_002dno_002dstrict-1):

 

[Reverse and Transpose](https://www.gnu.org/software/datamash/manual/datamash.html#Reverse-and-Transpose)

[`--output-delimiter`](https://www.gnu.org/software/datamash/manual/datamash.html#index-_002d_002doutput_002ddelimiter):

 

[Invoking datamash](https://www.gnu.org/software/datamash/manual/datamash.html#Invoking-datamash)

[`--round`](https://www.gnu.org/software/datamash/manual/datamash.html#index-_002d_002dround):

 

[Invoking datamash](https://www.gnu.org/software/datamash/manual/datamash.html#Invoking-datamash)

[`--sort`](https://www.gnu.org/software/datamash/manual/datamash.html#index-_002d_002dsort):

 

[Invoking datamash](https://www.gnu.org/software/datamash/manual/datamash.html#Invoking-datamash)

[`--version`](https://www.gnu.org/software/datamash/manual/datamash.html#index-_002d_002dversion):

 

[Invoking datamash](https://www.gnu.org/software/datamash/manual/datamash.html#Invoking-datamash)

[`--whitespace`](https://www.gnu.org/software/datamash/manual/datamash.html#index-_002d_002dwhitespace):

 

[Invoking datamash](https://www.gnu.org/software/datamash/manual/datamash.html#Invoking-datamash)

[`--zero-terminated`](https://www.gnu.org/software/datamash/manual/datamash.html#index-_002d_002dzero_002dterminated):

 

[Invoking datamash](https://www.gnu.org/software/datamash/manual/datamash.html#Invoking-datamash)

[`-f`](https://www.gnu.org/software/datamash/manual/datamash.html#index-_002df):

 

[Invoking datamash](https://www.gnu.org/software/datamash/manual/datamash.html#Invoking-datamash)

[`-g`](https://www.gnu.org/software/datamash/manual/datamash.html#index-_002dg):

 

[Invoking datamash](https://www.gnu.org/software/datamash/manual/datamash.html#Invoking-datamash)

[`-H`](https://www.gnu.org/software/datamash/manual/datamash.html#index-_002dH):

 

[Invoking datamash](https://www.gnu.org/software/datamash/manual/datamash.html#Invoking-datamash)

[`-H`](https://www.gnu.org/software/datamash/manual/datamash.html#index-_002dH-1):

 

[Header Lines and Column Names](https://www.gnu.org/software/datamash/manual/datamash.html#Header-Lines-and-Column-Names)

[`-i`](https://www.gnu.org/software/datamash/manual/datamash.html#index-_002di):

 

[Invoking datamash](https://www.gnu.org/software/datamash/manual/datamash.html#Invoking-datamash)

[`-R`](https://www.gnu.org/software/datamash/manual/datamash.html#index-_002dR):

 

[Invoking datamash](https://www.gnu.org/software/datamash/manual/datamash.html#Invoking-datamash)

[`-s`](https://www.gnu.org/software/datamash/manual/datamash.html#index-_002ds):

 

[Invoking datamash](https://www.gnu.org/software/datamash/manual/datamash.html#Invoking-datamash)

[`-t`](https://www.gnu.org/software/datamash/manual/datamash.html#index-_002dt):

 

[Invoking datamash](https://www.gnu.org/software/datamash/manual/datamash.html#Invoking-datamash)

[`-t`](https://www.gnu.org/software/datamash/manual/datamash.html#index-_002dt-1):

 

[Invoking datamash](https://www.gnu.org/software/datamash/manual/datamash.html#Invoking-datamash)

[`-t`](https://www.gnu.org/software/datamash/manual/datamash.html#index-_002dt-2):

 

[Groupby on /etc/passwd](https://www.gnu.org/software/datamash/manual/datamash.html#Groupby-on-_002fetc_002fpasswd)

[`-W`](https://www.gnu.org/software/datamash/manual/datamash.html#index-_002dW):

 

[Invoking datamash](https://www.gnu.org/software/datamash/manual/datamash.html#Invoking-datamash)

[`-z`](https://www.gnu.org/software/datamash/manual/datamash.html#index-_002dz):

 

[Invoking datamash](https://www.gnu.org/software/datamash/manual/datamash.html#Invoking-datamash)

---

/

[/etc/passwd, examples](https://www.gnu.org/software/datamash/manual/datamash.html#index-_002fetc_002fpasswd_002c-examples):

 

[Groupby on /etc/passwd](https://www.gnu.org/software/datamash/manual/datamash.html#Groupby-on-_002fetc_002fpasswd)

---

B

[`bin`](https://www.gnu.org/software/datamash/manual/datamash.html#index-bin):

 

[Binning numbers](https://www.gnu.org/software/datamash/manual/datamash.html#Binning-numbers)

[binning numbers](https://www.gnu.org/software/datamash/manual/datamash.html#index-binning-numbers):

 

[Binning numbers](https://www.gnu.org/software/datamash/manual/datamash.html#Binning-numbers)

[binning strings](https://www.gnu.org/software/datamash/manual/datamash.html#index-binning-strings):

 

[Binning strings](https://www.gnu.org/software/datamash/manual/datamash.html#Binning-strings)

[buckets, binning numbers](https://www.gnu.org/software/datamash/manual/datamash.html#index-buckets_002c-binning-numbers):

 

[Binning numbers](https://www.gnu.org/software/datamash/manual/datamash.html#Binning-numbers)

[buckets, binning strings](https://www.gnu.org/software/datamash/manual/datamash.html#index-buckets_002c-binning-strings):

 

[Binning strings](https://www.gnu.org/software/datamash/manual/datamash.html#Binning-strings)

[bug reporting](https://www.gnu.org/software/datamash/manual/datamash.html#index-bug-reporting):

 

[Reporting bugs](https://www.gnu.org/software/datamash/manual/datamash.html#Reporting-bugs)

---

C

[`ceil`](https://www.gnu.org/software/datamash/manual/datamash.html#index-ceil):

 

[Rounding numbers](https://www.gnu.org/software/datamash/manual/datamash.html#Rounding-numbers)

[check](https://www.gnu.org/software/datamash/manual/datamash.html#index-check):

 

[Check](https://www.gnu.org/software/datamash/manual/datamash.html#Check)

[check, in automation and shell scripts](https://www.gnu.org/software/datamash/manual/datamash.html#index-check_002c-in-automation-and-shell-scripts):

 

[Check](https://www.gnu.org/software/datamash/manual/datamash.html#Check)

[checking tabular structure](https://www.gnu.org/software/datamash/manual/datamash.html#index-checking-tabular-structure):

 

[Check](https://www.gnu.org/software/datamash/manual/datamash.html#Check)

[checklist for bug reports](https://www.gnu.org/software/datamash/manual/datamash.html#index-checklist-for-bug-reports):

 

[Reporting bugs](https://www.gnu.org/software/datamash/manual/datamash.html#Reporting-bugs)

[collapse](https://www.gnu.org/software/datamash/manual/datamash.html#index-collapse):

 

[Groupby on /etc/passwd](https://www.gnu.org/software/datamash/manual/datamash.html#Groupby-on-_002fetc_002fpasswd)

[column names](https://www.gnu.org/software/datamash/manual/datamash.html#index-column-names):

 

[Header Lines and Column Names](https://www.gnu.org/software/datamash/manual/datamash.html#Header-Lines-and-Column-Names)

[column ranges](https://www.gnu.org/software/datamash/manual/datamash.html#index-column-ranges):

 

[Column Ranges](https://www.gnu.org/software/datamash/manual/datamash.html#Column-Ranges)

[columns, reverse](https://www.gnu.org/software/datamash/manual/datamash.html#index-columns_002c-reverse):

 

[Reverse and Transpose](https://www.gnu.org/software/datamash/manual/datamash.html#Reverse-and-Transpose)

[`count`](https://www.gnu.org/software/datamash/manual/datamash.html#index-count-1):

 

[Crosstab](https://www.gnu.org/software/datamash/manual/datamash.html#Crosstab)

[count](https://www.gnu.org/software/datamash/manual/datamash.html#index-count):

 

[Groupby on /etc/passwd](https://www.gnu.org/software/datamash/manual/datamash.html#Groupby-on-_002fetc_002fpasswd)

[count, crosstab and](https://www.gnu.org/software/datamash/manual/datamash.html#index-count_002c-crosstab-and):

 

[Crosstab](https://www.gnu.org/software/datamash/manual/datamash.html#Crosstab)

[cross tabulation](https://www.gnu.org/software/datamash/manual/datamash.html#index-cross-tabulation):

 

[Crosstab](https://www.gnu.org/software/datamash/manual/datamash.html#Crosstab)

[crosstab](https://www.gnu.org/software/datamash/manual/datamash.html#index-crosstab):

 

[Crosstab](https://www.gnu.org/software/datamash/manual/datamash.html#Crosstab)

[crosstab and sum](https://www.gnu.org/software/datamash/manual/datamash.html#index-crosstab-and-sum):

 

[Crosstab](https://www.gnu.org/software/datamash/manual/datamash.html#Crosstab)

[crosstab and unique](https://www.gnu.org/software/datamash/manual/datamash.html#index-crosstab-and-unique):

 

[Crosstab](https://www.gnu.org/software/datamash/manual/datamash.html#Crosstab)

---

D

[delimiters, tabs](https://www.gnu.org/software/datamash/manual/datamash.html#index-delimiters_002c-tabs):

 

[Field Delimiters](https://www.gnu.org/software/datamash/manual/datamash.html#Field-Delimiters)

[delimiters, whitespace](https://www.gnu.org/software/datamash/manual/datamash.html#index-delimiters_002c-whitespace):

 

[Field Delimiters](https://www.gnu.org/software/datamash/manual/datamash.html#Field-Delimiters)

---

E

[example, grouping](https://www.gnu.org/software/datamash/manual/datamash.html#index-example_002c-grouping):

 

[Overview](https://www.gnu.org/software/datamash/manual/datamash.html#Overview)

[example, sorting](https://www.gnu.org/software/datamash/manual/datamash.html#index-example_002c-sorting):

 

[Overview](https://www.gnu.org/software/datamash/manual/datamash.html#Overview)

[example, statistics](https://www.gnu.org/software/datamash/manual/datamash.html#index-example_002c-statistics):

 

[Overview](https://www.gnu.org/software/datamash/manual/datamash.html#Overview)

[example, sum](https://www.gnu.org/software/datamash/manual/datamash.html#index-example_002c-sum):

 

[Overview](https://www.gnu.org/software/datamash/manual/datamash.html#Overview)

[examples, /etc/passwd](https://www.gnu.org/software/datamash/manual/datamash.html#index-examples_002c-_002fetc_002fpasswd):

 

[Groupby on /etc/passwd](https://www.gnu.org/software/datamash/manual/datamash.html#Groupby-on-_002fetc_002fpasswd)

[examples, header](https://www.gnu.org/software/datamash/manual/datamash.html#index-examples_002c-header):

 

[Header Lines and Column Names](https://www.gnu.org/software/datamash/manual/datamash.html#Header-Lines-and-Column-Names)

[examples, header](https://www.gnu.org/software/datamash/manual/datamash.html#index-examples_002c-header-1):

 

[Header Lines and Column Names](https://www.gnu.org/software/datamash/manual/datamash.html#Header-Lines-and-Column-Names)

[examples, header-in](https://www.gnu.org/software/datamash/manual/datamash.html#index-examples_002c-header_002din):

 

[Header Lines and Column Names](https://www.gnu.org/software/datamash/manual/datamash.html#Header-Lines-and-Column-Names)

[examples, header-out](https://www.gnu.org/software/datamash/manual/datamash.html#index-examples_002c-header_002dout):

 

[Header Lines and Column Names](https://www.gnu.org/software/datamash/manual/datamash.html#Header-Lines-and-Column-Names)

[examples, headers](https://www.gnu.org/software/datamash/manual/datamash.html#index-examples_002c-headers):

 

[Header Lines and Column Names](https://www.gnu.org/software/datamash/manual/datamash.html#Header-Lines-and-Column-Names)

[examples, max](https://www.gnu.org/software/datamash/manual/datamash.html#index-examples_002c-max):

 

[Summary Statistics](https://www.gnu.org/software/datamash/manual/datamash.html#Summary-Statistics)

[examples, mean](https://www.gnu.org/software/datamash/manual/datamash.html#index-examples_002c-mean):

 

[Summary Statistics](https://www.gnu.org/software/datamash/manual/datamash.html#Summary-Statistics)

[examples, median](https://www.gnu.org/software/datamash/manual/datamash.html#index-examples_002c-median):

 

[Summary Statistics](https://www.gnu.org/software/datamash/manual/datamash.html#Summary-Statistics)

[examples, min](https://www.gnu.org/software/datamash/manual/datamash.html#index-examples_002c-min):

 

[Summary Statistics](https://www.gnu.org/software/datamash/manual/datamash.html#Summary-Statistics)

[examples, quartiles](https://www.gnu.org/software/datamash/manual/datamash.html#index-examples_002c-quartiles):

 

[Summary Statistics](https://www.gnu.org/software/datamash/manual/datamash.html#Summary-Statistics)

[examples, standard deviation](https://www.gnu.org/software/datamash/manual/datamash.html#index-examples_002c-standard-deviation):

 

[Summary Statistics](https://www.gnu.org/software/datamash/manual/datamash.html#Summary-Statistics)

[examples, summary statistics](https://www.gnu.org/software/datamash/manual/datamash.html#index-examples_002c-summary-statistics):

 

[Summary Statistics](https://www.gnu.org/software/datamash/manual/datamash.html#Summary-Statistics)

[examples, usage](https://www.gnu.org/software/datamash/manual/datamash.html#index-examples_002c-usage):

 

[Usage Examples](https://www.gnu.org/software/datamash/manual/datamash.html#Usage-Examples)

---

F

[fail fast](https://www.gnu.org/software/datamash/manual/datamash.html#index-fail-fast):

 

[Check](https://www.gnu.org/software/datamash/manual/datamash.html#Check)

[field delimiters](https://www.gnu.org/software/datamash/manual/datamash.html#index-field-delimiters):

 

[Field Delimiters](https://www.gnu.org/software/datamash/manual/datamash.html#Field-Delimiters)

[field names](https://www.gnu.org/software/datamash/manual/datamash.html#index-field-names):

 

[Header Lines and Column Names](https://www.gnu.org/software/datamash/manual/datamash.html#Header-Lines-and-Column-Names)

[`floor`](https://www.gnu.org/software/datamash/manual/datamash.html#index-floor):

 

[Rounding numbers](https://www.gnu.org/software/datamash/manual/datamash.html#Rounding-numbers)

[`frac`](https://www.gnu.org/software/datamash/manual/datamash.html#index-frac):

 

[Rounding numbers](https://www.gnu.org/software/datamash/manual/datamash.html#Rounding-numbers)

---

G

[groupby](https://www.gnu.org/software/datamash/manual/datamash.html#index-groupby):

 

[Groupby on /etc/passwd](https://www.gnu.org/software/datamash/manual/datamash.html#Groupby-on-_002fetc_002fpasswd)

[groupby, and collapse](https://www.gnu.org/software/datamash/manual/datamash.html#index-groupby_002c-and-collapse):

 

[Groupby on /etc/passwd](https://www.gnu.org/software/datamash/manual/datamash.html#Groupby-on-_002fetc_002fpasswd)

[groupby, and count](https://www.gnu.org/software/datamash/manual/datamash.html#index-groupby_002c-and-count):

 

[Groupby on /etc/passwd](https://www.gnu.org/software/datamash/manual/datamash.html#Groupby-on-_002fetc_002fpasswd)

[grouping](https://www.gnu.org/software/datamash/manual/datamash.html#index-grouping):

 

[Overview](https://www.gnu.org/software/datamash/manual/datamash.html#Overview)

[grouping](https://www.gnu.org/software/datamash/manual/datamash.html#index-grouping-1):

 

[Invoking datamash](https://www.gnu.org/software/datamash/manual/datamash.html#Invoking-datamash)

---

H

[header, examples](https://www.gnu.org/software/datamash/manual/datamash.html#index-header_002c-examples):

 

[Header Lines and Column Names](https://www.gnu.org/software/datamash/manual/datamash.html#Header-Lines-and-Column-Names)

[header-in, examples](https://www.gnu.org/software/datamash/manual/datamash.html#index-header_002din_002c-examples):

 

[Header Lines and Column Names](https://www.gnu.org/software/datamash/manual/datamash.html#Header-Lines-and-Column-Names)

[header-out, examples](https://www.gnu.org/software/datamash/manual/datamash.html#index-header_002dout_002c-examples):

 

[Header Lines and Column Names](https://www.gnu.org/software/datamash/manual/datamash.html#Header-Lines-and-Column-Names)

[headers, examples](https://www.gnu.org/software/datamash/manual/datamash.html#index-headers_002c-examples):

 

[Header Lines and Column Names](https://www.gnu.org/software/datamash/manual/datamash.html#Header-Lines-and-Column-Names)

[help](https://www.gnu.org/software/datamash/manual/datamash.html#index-help):

 

[Invoking datamash](https://www.gnu.org/software/datamash/manual/datamash.html#Invoking-datamash)

---

I

[input validation, transpose](https://www.gnu.org/software/datamash/manual/datamash.html#index-input-validation_002c-transpose):

 

[Reverse and Transpose](https://www.gnu.org/software/datamash/manual/datamash.html#Reverse-and-Transpose)

[invoking](https://www.gnu.org/software/datamash/manual/datamash.html#index-invoking):

 

[Invoking datamash](https://www.gnu.org/software/datamash/manual/datamash.html#Invoking-datamash)

---

L

[line filtering operation](https://www.gnu.org/software/datamash/manual/datamash.html#index-line-filtering-operation):

 

[Available Operations](https://www.gnu.org/software/datamash/manual/datamash.html#Available-Operations)

[login shell, examples](https://www.gnu.org/software/datamash/manual/datamash.html#index-login-shell_002c-examples):

 

[Groupby on /etc/passwd](https://www.gnu.org/software/datamash/manual/datamash.html#Groupby-on-_002fetc_002fpasswd)

---

M

[max, examples](https://www.gnu.org/software/datamash/manual/datamash.html#index-max_002c-examples):

 

[Summary Statistics](https://www.gnu.org/software/datamash/manual/datamash.html#Summary-Statistics)

[mean, examples](https://www.gnu.org/software/datamash/manual/datamash.html#index-mean_002c-examples):

 

[Summary Statistics](https://www.gnu.org/software/datamash/manual/datamash.html#Summary-Statistics)

[median, examples](https://www.gnu.org/software/datamash/manual/datamash.html#index-median_002c-examples):

 

[Summary Statistics](https://www.gnu.org/software/datamash/manual/datamash.html#Summary-Statistics)

[min, examples](https://www.gnu.org/software/datamash/manual/datamash.html#index-min_002c-examples):

 

[Summary Statistics](https://www.gnu.org/software/datamash/manual/datamash.html#Summary-Statistics)

[missing values, transpose](https://www.gnu.org/software/datamash/manual/datamash.html#index-missing-values_002c-transpose):

 

[Reverse and Transpose](https://www.gnu.org/software/datamash/manual/datamash.html#Reverse-and-Transpose)

[multiple columns](https://www.gnu.org/software/datamash/manual/datamash.html#index-multiple-columns):

 

[Column Ranges](https://www.gnu.org/software/datamash/manual/datamash.html#Column-Ranges)

---

N

[numeric operations](https://www.gnu.org/software/datamash/manual/datamash.html#index-numeric-operations):

 

[Available Operations](https://www.gnu.org/software/datamash/manual/datamash.html#Available-Operations)

---

O

[operations, line filtering](https://www.gnu.org/software/datamash/manual/datamash.html#index-operations_002c-line-filtering):

 

[Available Operations](https://www.gnu.org/software/datamash/manual/datamash.html#Available-Operations)

[operations, numeric](https://www.gnu.org/software/datamash/manual/datamash.html#index-operations_002c-numeric):

 

[Available Operations](https://www.gnu.org/software/datamash/manual/datamash.html#Available-Operations)

[operations, per-line](https://www.gnu.org/software/datamash/manual/datamash.html#index-operations_002c-per_002dline):

 

[Available Operations](https://www.gnu.org/software/datamash/manual/datamash.html#Available-Operations)

[operations, primary](https://www.gnu.org/software/datamash/manual/datamash.html#index-operations_002c-primary):

 

[Available Operations](https://www.gnu.org/software/datamash/manual/datamash.html#Available-Operations)

[operations, statistical](https://www.gnu.org/software/datamash/manual/datamash.html#index-operations_002c-statistical):

 

[Available Operations](https://www.gnu.org/software/datamash/manual/datamash.html#Available-Operations)

[operations, statistical](https://www.gnu.org/software/datamash/manual/datamash.html#index-operations_002c-statistical-1):

 

[Statistical Operations](https://www.gnu.org/software/datamash/manual/datamash.html#Statistical-Operations)

[operations, textual](https://www.gnu.org/software/datamash/manual/datamash.html#index-operations_002c-textual):

 

[Available Operations](https://www.gnu.org/software/datamash/manual/datamash.html#Available-Operations)

[options](https://www.gnu.org/software/datamash/manual/datamash.html#index-options):

 

[Invoking datamash](https://www.gnu.org/software/datamash/manual/datamash.html#Invoking-datamash)

[overview](https://www.gnu.org/software/datamash/manual/datamash.html#index-overview):

 

[Overview](https://www.gnu.org/software/datamash/manual/datamash.html#Overview)

---

P

[patches, contributing](https://www.gnu.org/software/datamash/manual/datamash.html#index-patches_002c-contributing):

 

[Reporting bugs](https://www.gnu.org/software/datamash/manual/datamash.html#Reporting-bugs)

[Per-Line operations](https://www.gnu.org/software/datamash/manual/datamash.html#index-Per_002dLine-operations):

 

[Available Operations](https://www.gnu.org/software/datamash/manual/datamash.html#Available-Operations)

[pivot tables](https://www.gnu.org/software/datamash/manual/datamash.html#index-pivot-tables):

 

[Crosstab](https://www.gnu.org/software/datamash/manual/datamash.html#Crosstab)

[primary operations](https://www.gnu.org/software/datamash/manual/datamash.html#index-primary-operations):

 

[Available Operations](https://www.gnu.org/software/datamash/manual/datamash.html#Available-Operations)

[problems](https://www.gnu.org/software/datamash/manual/datamash.html#index-problems):

 

[Reporting bugs](https://www.gnu.org/software/datamash/manual/datamash.html#Reporting-bugs)

---

Q

[quartiles, examples](https://www.gnu.org/software/datamash/manual/datamash.html#index-quartiles_002c-examples):

 

[Summary Statistics](https://www.gnu.org/software/datamash/manual/datamash.html#Summary-Statistics)

---

R

[ranges, columns](https://www.gnu.org/software/datamash/manual/datamash.html#index-ranges_002c-columns):

 

[Column Ranges](https://www.gnu.org/software/datamash/manual/datamash.html#Column-Ranges)

[reporting bugs](https://www.gnu.org/software/datamash/manual/datamash.html#index-reporting-bugs):

 

[Reporting bugs](https://www.gnu.org/software/datamash/manual/datamash.html#Reporting-bugs)

[reverse columns](https://www.gnu.org/software/datamash/manual/datamash.html#index-reverse-columns):

 

[Reverse and Transpose](https://www.gnu.org/software/datamash/manual/datamash.html#Reverse-and-Transpose)

[reverse, and transpose](https://www.gnu.org/software/datamash/manual/datamash.html#index-reverse_002c-and-transpose):

 

[Reverse and Transpose](https://www.gnu.org/software/datamash/manual/datamash.html#Reverse-and-Transpose)

[reverse, strict](https://www.gnu.org/software/datamash/manual/datamash.html#index-reverse_002c-strict):

 

[Reverse and Transpose](https://www.gnu.org/software/datamash/manual/datamash.html#Reverse-and-Transpose)

[reversing lines](https://www.gnu.org/software/datamash/manual/datamash.html#index-reversing-lines):

 

[Reverse and Transpose](https://www.gnu.org/software/datamash/manual/datamash.html#Reverse-and-Transpose)

[`round`](https://www.gnu.org/software/datamash/manual/datamash.html#index-round):

 

[Rounding numbers](https://www.gnu.org/software/datamash/manual/datamash.html#Rounding-numbers)

[rounding numbers](https://www.gnu.org/software/datamash/manual/datamash.html#index-rounding-numbers):

 

[Rounding numbers](https://www.gnu.org/software/datamash/manual/datamash.html#Rounding-numbers)

---

S

[shell scripts, check](https://www.gnu.org/software/datamash/manual/datamash.html#index-shell-scripts_002c-check):

 

[Check](https://www.gnu.org/software/datamash/manual/datamash.html#Check)

[sorting](https://www.gnu.org/software/datamash/manual/datamash.html#index-sorting):

 

[Overview](https://www.gnu.org/software/datamash/manual/datamash.html#Overview)

[sorting](https://www.gnu.org/software/datamash/manual/datamash.html#index-sorting-1):

 

[Invoking datamash](https://www.gnu.org/software/datamash/manual/datamash.html#Invoking-datamash)

[standard devian, examples](https://www.gnu.org/software/datamash/manual/datamash.html#index-standard-devian_002c-examples):

 

[Summary Statistics](https://www.gnu.org/software/datamash/manual/datamash.html#Summary-Statistics)

[Statistical operations](https://www.gnu.org/software/datamash/manual/datamash.html#index-Statistical-operations):

 

[Available Operations](https://www.gnu.org/software/datamash/manual/datamash.html#Available-Operations)

[statistical operations](https://www.gnu.org/software/datamash/manual/datamash.html#index-statistical-operations):

 

[Statistical Operations](https://www.gnu.org/software/datamash/manual/datamash.html#Statistical-Operations)

[statistics](https://www.gnu.org/software/datamash/manual/datamash.html#index-statistics):

 

[Statistical Operations](https://www.gnu.org/software/datamash/manual/datamash.html#Statistical-Operations)

[`strbin`](https://www.gnu.org/software/datamash/manual/datamash.html#index-strbin):

 

[Binning strings](https://www.gnu.org/software/datamash/manual/datamash.html#Binning-strings)

[strict mode](https://www.gnu.org/software/datamash/manual/datamash.html#index-strict-mode):

 

[Reverse and Transpose](https://www.gnu.org/software/datamash/manual/datamash.html#Reverse-and-Transpose)

[strict, reverse](https://www.gnu.org/software/datamash/manual/datamash.html#index-strict_002c-reverse):

 

[Reverse and Transpose](https://www.gnu.org/software/datamash/manual/datamash.html#Reverse-and-Transpose)

[strict, transpose](https://www.gnu.org/software/datamash/manual/datamash.html#index-strict_002c-transpose):

 

[Reverse and Transpose](https://www.gnu.org/software/datamash/manual/datamash.html#Reverse-and-Transpose)

[`sum`](https://www.gnu.org/software/datamash/manual/datamash.html#index-sum):

 

[Crosstab](https://www.gnu.org/software/datamash/manual/datamash.html#Crosstab)

[sum, crosstab and](https://www.gnu.org/software/datamash/manual/datamash.html#index-sum_002c-crosstab-and):

 

[Crosstab](https://www.gnu.org/software/datamash/manual/datamash.html#Crosstab)

[summary statistics example](https://www.gnu.org/software/datamash/manual/datamash.html#index-summary-statistics-example):

 

[Summary Statistics](https://www.gnu.org/software/datamash/manual/datamash.html#Summary-Statistics)

[swap rows, columns](https://www.gnu.org/software/datamash/manual/datamash.html#index-swap-rows_002c-columns):

 

[Reverse and Transpose](https://www.gnu.org/software/datamash/manual/datamash.html#Reverse-and-Transpose)

---

T

[tab delimiters](https://www.gnu.org/software/datamash/manual/datamash.html#index-tab-delimiters):

 

[Field Delimiters](https://www.gnu.org/software/datamash/manual/datamash.html#Field-Delimiters)

[tac](https://www.gnu.org/software/datamash/manual/datamash.html#index-tac):

 

[Reverse and Transpose](https://www.gnu.org/software/datamash/manual/datamash.html#Reverse-and-Transpose)

[Textual operations](https://www.gnu.org/software/datamash/manual/datamash.html#index-Textual-operations):

 

[Available Operations](https://www.gnu.org/software/datamash/manual/datamash.html#Available-Operations)

[transpose](https://www.gnu.org/software/datamash/manual/datamash.html#index-transpose):

 

[Reverse and Transpose](https://www.gnu.org/software/datamash/manual/datamash.html#Reverse-and-Transpose)

[transpose, and reverse](https://www.gnu.org/software/datamash/manual/datamash.html#index-transpose_002c-and-reverse):

 

[Reverse and Transpose](https://www.gnu.org/software/datamash/manual/datamash.html#Reverse-and-Transpose)

[transpose, filler value](https://www.gnu.org/software/datamash/manual/datamash.html#index-transpose_002c-filler-value):

 

[Reverse and Transpose](https://www.gnu.org/software/datamash/manual/datamash.html#Reverse-and-Transpose)

[transpose, input validation](https://www.gnu.org/software/datamash/manual/datamash.html#index-transpose_002c-input-validation):

 

[Reverse and Transpose](https://www.gnu.org/software/datamash/manual/datamash.html#Reverse-and-Transpose)

[transpose, missing values](https://www.gnu.org/software/datamash/manual/datamash.html#index-transpose_002c-missing-values):

 

[Reverse and Transpose](https://www.gnu.org/software/datamash/manual/datamash.html#Reverse-and-Transpose)

[transpose, strict](https://www.gnu.org/software/datamash/manual/datamash.html#index-transpose_002c-strict):

 

[Reverse and Transpose](https://www.gnu.org/software/datamash/manual/datamash.html#Reverse-and-Transpose)

[`trunc`](https://www.gnu.org/software/datamash/manual/datamash.html#index-trunc):

 

[Rounding numbers](https://www.gnu.org/software/datamash/manual/datamash.html#Rounding-numbers)

---

U

[`unique`](https://www.gnu.org/software/datamash/manual/datamash.html#index-unique):

 

[Crosstab](https://www.gnu.org/software/datamash/manual/datamash.html#Crosstab)

[unique, crosstab and](https://www.gnu.org/software/datamash/manual/datamash.html#index-unique_002c-crosstab-and):

 

[Crosstab](https://www.gnu.org/software/datamash/manual/datamash.html#Crosstab)

[usage](https://www.gnu.org/software/datamash/manual/datamash.html#index-usage):

 

[Invoking datamash](https://www.gnu.org/software/datamash/manual/datamash.html#Invoking-datamash)

[usage examples](https://www.gnu.org/software/datamash/manual/datamash.html#index-usage-examples):

 

[Usage Examples](https://www.gnu.org/software/datamash/manual/datamash.html#Usage-Examples)

---

W

[whitespace delimiters](https://www.gnu.org/software/datamash/manual/datamash.html#index-whitespace-delimiters):

 

[Field Delimiters](https://www.gnu.org/software/datamash/manual/datamash.html#Field-Delimiters)

---

Jump to:  

[**-**](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index_cp_symbol-1)   [**/**](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index_cp_symbol-2)    
[**B**](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index_cp_letter-B)   [**C**](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index_cp_letter-C)   [**D**](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index_cp_letter-D)   [**E**](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index_cp_letter-E)   [**F**](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index_cp_letter-F)   [**G**](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index_cp_letter-G)   [**H**](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index_cp_letter-H)   [**I**](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index_cp_letter-I)   [**L**](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index_cp_letter-L)   [**M**](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index_cp_letter-M)   [**N**](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index_cp_letter-N)   [**O**](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index_cp_letter-O)   [**P**](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index_cp_letter-P)   [**Q**](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index_cp_letter-Q)   [**R**](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index_cp_letter-R)   [**S**](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index_cp_letter-S)   [**T**](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index_cp_letter-T)   [**U**](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index_cp_letter-U)   [**W**](https://www.gnu.org/software/datamash/manual/datamash.html#Concept-index_cp_letter-W)  

---
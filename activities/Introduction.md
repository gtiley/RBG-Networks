---
layout: page
title: Getting started
permalink: /activities/introduction/
---

The examples used throughout the workshop have been designed to run quickly. Phylogenomic investigations in practice are often much larger and may necessitate the use of computing clusters. If this is your first time using a cluster or command line software, some activities are provided below to help you get the most out of the computational exercises. There are four goals this morning:
1. Log on to the UK Crop Diversity HPC
2. Perform basic UNIX operations
3. Run jobs interactively and with submission scripts
4. Understand responsible use of the HPC

Some or all of this may be very familiar. These short instructions are about getting everybody on the same page for advancing later in the workshop. Please take time to help each other throughout the workshop too. 

A few necessary downloads for your local computer are covered on the [pre-workshop page]({{site.baseurl}}/pre-workshop), but the cluster has all of the necessary software for the analyses ready for you.


## Log on to and navigate the UK Crop Diversity HPC
Now you will log into the cluster. Then you will copy materials from a shared folder to your user directory (on the cluster). All analyses will happen in your *scratch* space. The below code block assume you have followed the [key authentication instructions](https://help.cropdiversity.ac.uk/ssh.html#key-authentication-with-2fa) for the HPC.

**Note:** `YOUR_USER_NAME` is *your* user name for the HPC

```
ssh gruffalo
cd /mnt/shared/scratch/YOUR_USER_NAME
pwd
ls
mkdir network_workshop
cd network_workshop
pwd
ls
cp -r /mnt/shared/scratch/gtiley/introduction .
cd introduction
pwd
ls
```
Several things happened here. We *change directory* with `cd`  and check the location of the *present working directory* with `pwd`. We then *list* files with `ls`. These are UNIX commands that will help us navigate the cluster. We then *make directory* named network_workshop with `mkdir`. It is important to avoid special characters and spaces when naming directories and files on the cluster.


Notice that a file exists in the `introduction` directory. Let's have a look and edit it with `nano hello.txt`. You can only use your arrow keys to move around but you can edit the text directly on the cluster without any graphical software. This is a handy skill to have. These text editors, one of which is `nano`, have various capabilities, but the important things to remember are `control + o` to save and `control + x` to exit. Try to open, edit, and save changes to `hello.txt` before closing. You can admire your changes to the file with the command `less`. Try `less hello.txt` to view the file contents but you will exit `less` with `control + z`.

Try making a new file called `my_dreams.txt`. Do do this, go `nano my_dreams.txt`. You could type something like "I am an expert on phylogenetic networks!" and save and exit.

Let's combine two files with the *concatenate* function `cat`. Go `cat hello.txt my_dreams.txt > the_truth.txt`. Now look at the new file you create!

Now you can access the cluster, move files around, and create or edit them.

 
## Moving files between your computer and the cluster
At some point, you will want to move files from your user directory on the cluster to your computer or the other way around. There are some graphical programs that might be helpful, but we will work directly from the command line using the *secure copy protocol* function `scp`. First, log off of the cluster and go back to your own computer with `exit`.

Let's start by downloading materials for today. Open your command-line prompt (using git-bash or similar for Windows users). I suggest making a folder to organize all of the class materials in one place.

**Note:** These steps are happening on your computer and not the cluster
```bash
cd ~
ls
mkdir network_workshop
cd network_workshop
pwd
scp -r gruffalo:/mnt/shared/scratch/YOUR_USER_NAME/introduction .
ls
```
The entire `introduction` directory should now be on *your* computer. The entire directory was download because we used the `-r` *flag* which means *recursively*. Feel free to create new files or change existing ones with your graphical text editor (BBedit, NotePad++, or otherwise) and save them. You can put files from your computer to the cluster with `scp` too.
```bash
cd introduction
scp *.txt gruffalo:/mnt/shared/scratch/YOUR_USER_NAME/introduction
```

## Running submission scripts

On a cluster, there are *login* nodes and *compute* nodes. You **never** run an actual analysis that will require notable memory and disk space on a login node (where we have been this whole time). Instead, a *scheduler* is used to manage requests from users, and queue and execute them in an orderly manner.

Log back on and go to your `introduction` directory and go `ls`. We want to look at `test_submission.sh`, you can do this with `nano` or `less` or even `cat`. I will not define all of the SLURM directives here but we will discuss them in real time.

```sh
#!/bin/bash
#SBATCH --job-name=test_submission
#SBATCH --output=test_submission.log
#SBATCH --mail-user=g.tiley@kew.org
#SBATCH --mail-type=FAIL,END
#SBATCH --time=6:00:00
#SBATCH --mem-per-cpu=2G
#SBATCH --nodes=1
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=1
#SBATCH --partition=short
[[ -d $SLURM_SUBMIT_DIR ]] && cd $SLURM_SUBMIT_DIR

echo "Date              = $(date)"
echo "Hostname          = $(hostname -s)"
echo "Working Directory = $(pwd)"
echo ""
echo "Number of Nodes Allocated      = $SLURM_JOB_NUM_NODES"
echo "Number of Tasks Allocated      = $SLURM_NTASKS"
echo "Number of Cores/Task Allocated = $SLURM_CPUS_PER_TASK"

#To specify the above directives for interactive job go:
#srun --nodes=1 --ntasks=1 --cpus-per-task=1 --mem-per-cpu=2G --partition=short --pty bash

for i in {1..10}
do
  echo "begin step $i"
done
```

Do not forget to change your email! If you did, I will find out. After having a look and editing, submit the script with `sbatch test_submission.sh`.

The job will complete quickly, but if you are fast enough you might find it in the queue with `squeue -u YOUR_USER_NAME`. Check the log file to see that it ran correctly. There should be some information about resource allocation and a list of numbers 1 to 10.

Most of our activities today will happen interactively. This is largely for educational purposes, but it can be helpful for checking analyses are working as intended before submitting a job for days or weeks. Try allocating some resources on an interactive node like this:
```bash
srun --nodes=1 --ntasks=1 --cpus-per-task=1 --mem-per-cpu=2G --partition=short --pty bash
```
You are no longer on the *login* node and you should now be on a *compute* node. Now, you can safely execute the simple counting bash script by `./count.sh`

If the script did not run for you, you might need to change the permissions to make it executable. Check the permissions of the files in the directory, make `count.sh` executable with `chmod`, then check the permissions again and execute:
```
ls -l
chmod u+x count.sh
ls -l
./count.sh
```

You should now see the numbers 1 to 10 printed to your screen on the cluster. Make sure that you are always running analyses on a compute node by allocating resources with `srun` or submitting a job to the scheduler with `sbatch`. You should now be able to log on to the cluster, find your way through directories and edit files, and submit jobs responsibly. A topic that will come up throughout the workshop is allocating the number of cpus correctly so that you get the benefits of working on a compute cluster.


## Some additional notes about scripts

The details will not come up to much over the workshop, but at various points, you might be asked to run *scripts*. This is often a catch-all term for human-readable programming languages that are interpreted at run time, often with decent functionality for handling text data. The most popular scripting language is Python, but Perl still glues together many bioinformatic applications, and bash scripting becomes a necessity working on clusters. The below notes are not necessary but may be interesting or provide context to some.

### Interpreted Code - Scripting
Most genomics applications happen, at least in part, with scripting. Scripting languages are great for performing operations on strings or text, like ACGT. They are also not bad for a human to understand and hide the messiness of turning human-readable code to machine-readable code (compiling). That is the work of the interpreter, which happens in real-time. Scripting languages include Python, Perl, and Ruby. You can also accomplish a bit with bash and awk, but my heart-felt recommendation would be to dedicate some time into learning Python if you are new to this.

Let's see some simple scripts in action and hint at how they might be useful. In `data/`, you will find three files `*.params` with some results from a model.

In all omics data, we are often iterating over many things (e.g. loci, individuals, populations, bootstrap replicates) to do something repetitive. Let's execute three different scripts that will allow us to loop over the `*.params` files.

### loopFiles.sh - a simple bash script
The first line is a shebang. This is letting your computer know which interpreter program to use. Our first example is using bash, which will be available on any UNIX system. First, all of the `*.params` files are collected into a single array or list. We then iterate over the number of elements in that array, print the element to the screen, and quit the script.
```sh
#!/bin/bash                                                                                                                   

fileList="../data/*.params"
for i in $fileList
do
  echo "$i"
done
```

Try executing `loopFiles.sh` on the cluster with you acquired skills. We can execute programs that are not in our *path* by specifying the location. If we are in the same folder that we want to execute a program from we would go `./loopFiles.sh` such that `.` means *here*. The file names should print to your screen.

### loopFiles.pl - give me Perl
Perl is a popular scripting language that is the glue of the internet and played a large role in early genomics applications. It still is, but has waned in popularity as various R packages and Python. It will give you more flexibility than bash in the long-term and can be quick to learn. Here, we use the *glob* function to get an array of the file names. We then loop over array elements from their starting position (0) to the end (2) by getting the number of elements in the array with *scalar* and subtracting 1.
```perl
#!/usr/bin/perl -w                                                                                                            

@fileList = glob("../data/*.params");
for $i (0..(scalar(@fileList)-1))
{
    print "$fileList[$i]\n";
}
exit;
```
We could make this script executable as we did with the bash script, or we could go
```
perl loopFiles.pl
```

### loopFiles.py - Python and its libraries
Python is a relatively recent language but it the bedrock of most new bioinformatic applications. There has been a lot of development on improving abstraction and this is supported by many libraries (or modules). These are groups of functions that you let python know you want to use with the `import <module>` syntax. Here we load two very basic modules `sys` and `os`, but a third one we actually use, `glob`! We can use functions from modules by going `<module>.<function>()`, so we see `glob.glob()` here.
```python
#!/usr/bin/env python3                                                                                                        
import os
import sys
import glob

fileList = glob.glob("../data/*.params")
for i in range(0,len(fileList)):
    print(fileList[i])
exit;
```

Let's run it
```
python loopFiles.py
```

If that did not work, `python` on your system likely points to Python 2 and this is written for Python 3. Systems may differentiate the two by requiring Python 3 be specified as
```
python3 loopFiles.pl
```

## Using scripts to retrieve information from files
Scripting is a helpful way to get results from our inevitable thousands of output files. Here are a couple of examples in Perl and Python that build upon looping over the file list. Now, each file is opened and we process them line-by-line to extract the relevant information. Our goal is to make one table with the parameter values for each params file.

### getResults.pl - a regex approach
Scripting languages can use regular expressions (regex) to find patterns in strings. Good text editors can find and replace with text editors too. You can use them to save pieces of the string you care about and work with those further.
```perl
#!/usr/bin/perl -w                                                                                                          

%data = ();

@fileList = glob("../data/*.params");
for $i (0..(scalar(@fileList)-1))
{
#    print "$fileList[$i]\n";                                                                                               
    open FH1,'<',"$fileList[$i]";
    while(<FH1>)
    {
        if (/(\S+)\s+(\S+)/)
        {
            $parameter = $1;
            $value = $2;
            if ($parameter ne "Parameter")
            {
                push @{$data{$parameter}}, $value;
            }
        }
    }
    close FH1;
}

print "File";
foreach $parameter (sort(keys(%data)))
{
    print "\t$parameter";
}
print "\n";

for $i (0..(scalar(@fileList)-1))
{
    print "$fileList[$i]";
    foreach $parameter (sort(keys(%data)))
    {
        print "\t$data{$parameter}[$i]";
    }
    print "\n";
}
exit;
```

Scripting languages give you access to helpful *data structures*. Here, I make a *hash* called `data`, which is denoted by the `%`. Hashes have two parts, the *key* and the *value*. This is different from an *array* where you only need to know the element number to access the value - it can be a string too. And here, I actually make a hash of arrays, where each key (a,b,c) gives us the values from the three different files. I then loop back over the data structure to print a matrix that we might work with in R.
```
perl getReults.pl
```

### getResults.py - splitting lines and tuples for keys
Python can use regex too, but here I simply apply some prior knowledge about the params files to extract what I want. Python also uses hashes, but here the data structure is called a *dictionary* or *dict*. Here, we actually have a two-dimensional dictionary where each key is a *tuple*.
```python
#!/usr/bin/env python3                                                                                                      
import os
import sys
import glob

data = {}

fileList = glob.glob("../data/*.params")
for i in range(0,len(fileList)):
    InFile = open (fileList[i], 'r')
    for Line in InFile:
        Line = Line.strip('\n')
        ElementList = Line.split('\t')
        if (len(ElementList) > 1) and (ElementList[0] != "Parameter"):
            data[(i,ElementList[0])] = ElementList[1]

print('File\tParameter\tValue',end='\n')
for j,k in sorted(data.keys()):
    print(fileList[j],'\t',k,'\t',data[(j,k)],end='\n')
exit;
```

 You might notice that when printing here, we access the keys a bit more efficiently than the Perl case and print out a file that would be more appropriate for tidyverse R packages, so you can start to see how things fit together here.
```
python3 getReults.py
```

## Compiled Code
Examples of compiled languages are C, C++, and Rust. They implement low-level functions compared to interpreted/scripting languages (e.g. it would take some creativity to re-implement the glob function), but they are more efficient with memory allocation and potentially faster. C and C++ underly many of the workhorses of the genomics field, such as BWA, BLAST, RAxML, and BPP.

There is a silly example available for us to practice with. We will do this on the cluster since getting a good C compiler set up for Windows users can be time-consuming and frustrating. This should work for the mac and linux users though on your own systems.

So, go into the `compiledExample` folder. Since you might still be in `scripts`, all you need to do is go
```
cd ../compiledExample
```

To generate an executable program from the C code, we will run the gcc compiler as follows:
```
gcc -Wall -o betaSolver betaSolver.c solveBeta.c
```
`-Wall` is telling gcc to print warning flags which you will often see when building popular software applications. Our source code is in two different files `betaSolver.c` and `solveBeta.c`, and we use `-o` to indicate the output program `betaSolver`
You should now have the compiled C program `betaSolver`. Give it a try! Pull up the help menu and then see if you can get the program to do what it should.
```
./betaSolver -h
```

You almost never compile programs by invoking gcc yourself though. This would leave a lot of wiggle room for user errors. Thus, programs sometimes come with a Makefile. There is one to compile the `betaSolver` program for you to save some typing. Let's see it in action.
We will first delete the application, then run the makefile to re-compile it.
```
rm betaSolver
make
```

There will often be frustrating moments when compiling a program that you want to use, because you will run make and it will stop compiling with errors and give you very cryptic messages. As programs become very complex, there can be many external libraries that a program depends on. A step that happens with compiling is *linking* all of the bits of code scattered across a computer into a single computer-readable program. Let's break our program.
```
make clean
mv betaSolver.h wrong.h
make
```
Makefiles sometimes come with a *clean* option, so that all of the compiled code is removed. We then change the name of our helper file to `wrong.h` so the compiler no longer fins the correct one. You will now get a error that stops the compiler. This is an easy issue to diagnose from the error message, but sometimes it is not. Often it is a missing header file or library when using a large shared cluster with piecemeal installs.

### Wrap up
You have now logged onto the cluster edited some files and moved them around. You have transferred files between your computer and the cluster. You have successfully run some some scripts and compiled a program.

### Pro Tip
For easy cluster access, be sure you have made the suggested edits to your config file on your local computer. Instructions are provided by the Crop Diversity Cluster [here](https://help.cropdiversity.ac.uk/ssh-terminal-keys.html#additional-notes).
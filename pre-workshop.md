---
layout: page
title: Pre-workshop
permalink: /pre-workshop/
---

Prior to the workshop, participants are expected to install the software below. Prior experience using command line programs and something like R can be helpful but is not necessary for participation. A primer will be delivered at the beginning of the workshop and material will be delivered at an introductory level to keep material accessible.

George will hold office hours on 2 and 3 Nov from 15:00-17:00 to help troubleshoot pre-workshop problems in person at Kew and via Teams. A link has been sent to registered participants via email.

Keep in mind that if you are using an institution-owned machine, you may need to reach out to your IT team in advance for software installs.


## HPC Access for the Workshop

The workshop is supported by the [UK Crop Diversity Cluster](https://www.cropdiversity.ac.uk/). In order to access the cluster, you will need to create an account [here](https://help.cropdiversity.ac.uk/user-accounts.html).

The form will ask you institutional affiliation. If you are attending the workshop but not associated with one of the listed institutions, please clarify that you are attending the workshop on phylogenetic networks.

After successful registration, you will need to follow instructions on generating an ssh key and sharing the public key with the cluster admin [here](https://help.cropdiversity.ac.uk/ssh-terminal-keys.html).

It is best to register and have your ssh key in place as early as possible so as to not overwhelm the admin, but George is available during the office hours if any of the registration and key instructions are unclear.


## Some common prerequisites

If you do not already, please install the following:

- install [Julia](https://www.julialang.org/downloads/)
	- it can be helpful to add Julia to your [PATH](https://julialang.org/downloads/platform/#optional_add_julia_to_path)
- install [R](https://cran.r-project.org)
- install a text editor for editing or writing code if you do not already have one. Some free suggestions include:
    - [NotePad++](https://notepad-plus-plus.org/downloads/) for Windows
    - [BBEdit](https://www.barebones.com/products/bbedit/) for Mac
    - [VSCode](https://code.visualstudio.com/download) for all systems but with some extra features for coding
- if you are on a Windows computer, you will need a way to log into the institutional cluster.
	- I highly recommend [gitbash](https://gitforwindows.org/)
	- A tried and true alternative is [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)

## BPP

We will run the latest version of BPP on the cluster, which can be accessed with the [release page of the BPP GitHub](https://github.com/bpp/bpp/releases/tag/v4.6.2).

However, we will also use R and some R packages to analyze results from BPP that will require installation on your own computer. After R is installed we will install the [bppr R package](https://github.com/dosreislab/bppr). The package can be installed after launching R or Rstudio by
```R
install.packages("devtools")
devtools::install_github("dosreislab/bppr")
```

You should also install [Tracer](https://github.com/beast-dev/tracer/releases/tag/v1.7.2), which is an interactive program used for evaluating the quality of Bayesian posterior samples.


## PhyloNetworks
The bulk of analysis will be carried out on the cluster, but the visualization steps are best carried out on your own computer. This will require installing Julia and the associated packages for PhyloNetworks:
- [Download](https://julialang.org) julia and
  follow instructions to install julia
- Install the necessary packages: open julia then type
    ```julia
    using Pkg # to use functions that manage packages
    Pkg.add("PhyloNetworks") # to download & install package PhyloNetworks
    Pkg.add("PhyloPlots")
    Pkg.add("RCall")      # packaage to call R from within julia
    Pkg.add("CSV")        # to read from / write to text files, e.g. csv files
    Pkg.add("DataFrames") # to create & manipulate data frames
    Pkg.add("StatsModels")# for regression formulas
    using PhyloNetworks   # may take some time: pre-compiles functions in that package
    using PhyloPlots
    ```
and close julia with `exit()`.

# Setup instructions for RNA-Seq Demystified

This document guides you through the installation of software necessary to participate in the RNA-Seq Demystified workshop.

# Table of Contents

- [What is Conda?](#what-is-conda)
- [Installing Miniconda3](#installing-miniconda3)
- [Testing the Installation](#testing-the-installation)
- [Creating a Workshop Environment](#creating-a-workshop-environment)
- [Entering and Exiting an Environment](#entering-and-exiting-an-environment)
- [Extras](#extras)

# What is Conda?

Software is complex and installing it can be tricky. Conda (aka Miniconda aka Anaconda) is a software package manager that enables users to quickly and easily install many programs for bioinformatics analysis, among other things. 

A conda environment can be thought of as a workspace that has specific tools loaded in it. Perhaps a good analogy are different rooms in a research lab. Different rooms can be stocked with different tools to accomplish different tasks. We'll pick this analogy up after we install and test `conda`.

# Installing Miniconda

Go to the Miniconda3 installer [download page](https://docs.conda.io/en/latest/miniconda.html) and download the appropriate installer:

- For Windows that means the [Python 3.8 Miniconda3 Windows 64-bit installer](https://repo.anaconda.com/miniconda/Miniconda3-latest-Windows-x86_64.exe).
- For macOS that means the [Python 3.8 Miniconda MacOSX 64-bit pkg installer](https://repo.anaconda.com/miniconda/Miniconda3-latest-MacOSX-x86_64.pkg).

Once the installer downloads, open it and follow the instructions to install.

> **To testers:** When I first tried using the pkg installer in macOS I got to "How do you want to install this software?" with a big red stop sign that said "You cannot install Miniconda3 in this location" even though "Install for me only" was pre-selected. Clicking "Install for me only" made the message go away and install seemed to go smoothly. Did this occur for other Mac users? What about Windows users?

# Testing the Installation

Let's test the installation by opening a shell.

> - **For Windows that means ... I don't know right now?**
- For macOS that means opening the Terminal.

Once we're in a shell, type:

```
conda --version
```

You should see:

```
conda 4.8.3
```

(Your version number may be a bit higher - that's ok.)

# Creating a Workshop Environment

Picking up the analogy of rooms in a research lab to think about `conda` environments, we need to stock the room with the correct tools. We do this with the `conda install` command. After the full command, we'll explain what each part means.

```
conda create --name workshop \
    --yes \
    --channel bioconda \ 
    --channel conda-forge \
    fastqc=0.11.9 multiqc=1.9 STAR=2.7.6a rsem=1.3.3
```

For clarity, the command has been broken into several lines, but you can copy and paste the whole command above as single block.

> **To us:** If we had our own conda channel, I would use it here.

## Breaking Down the Command

We have used the "long flags" which give a better indication of all the options we've chosen:

- `conda create`: The command to create an environment.
- `--name`: The unique name of the environment. You can see what other environments you have with `conda env list`.
- `--yes` : conda will attempt to create the environment automatically and will not prompt you for confirmation
- `--channel`: `conda` has different channels depending on the type of software. The two most common channels for bioinformatics software are `bioconda` and `conda-forge`.
- `fastqc=0.11.9 ...`: The name of the software packages we want to install and the specific version. By default conda will install the latest version, but it's good to get in the habit of specifying a version for your reference. Any number of packages can be installed at once by separating them with spaces.

# Entering and Exiting an Environment

Again, picking up the analogy of rooms in a research lab, in order to use the microscope you have to enter the room with it. The idea is the same with `conda`. To enter the `conda` environment we just installed, we do:

```
conda activate workshop
```

Note that activating the workshop environemnt added `(workshop)` to the front of your shell prompt (your shell prompt be somewhat different - that's ok):

```
(workshop) rcavalca@x86_64-apple-darwin13 rnaseq_demystified_workshop %
```

The command `which` let's us see that the software tools we put in the environment are available; specifically is shows us the path to where that program is installed. Execute this command to see where `fastqc` is installed:

```
which fastqc
```

You should see a path that ends with "/fastqc". (It's ok if your location is a bit different.):

```
/Users/rcavalca/opt/miniconda3/envs/workshop/bin/fastqc
```

When we're done with the environment (like when we're done with a sample prep at the bench), we can put the tools away and clear the bench:

```
conda deactivate
```

To drive the point home that the tools in that environment are no longer available to us, try:

```
which fastqc
```

Instead of a path to the program, you should see either a blank result or the following (indicating that we've left the workshop environment):

```
fastqc not found
```

Also note that the `(workshop)` prefix on the shell prompt has gone away.

# Extras

## Searching for Packages

The above command begs the question "How did you know what the packages were called and what versions were available?" There are two ways to find tools that conda can install:

1. You can go the website `https://anaconda.org/` and type a tool in the search bar (e.g. `fastq`). The results will list channels and versions that match your tool name (listed by download popularity). You can click on a specific result to see details - including the command to install the tool.

2. From the command line, you can use the `conda search` command. For example, the following command will search the `bioconda` channel for all versions of `fastqc`:

```
conda search --channel bioconda fastqc
```

The output should look like:

```
Loading channels: done
# Name                       Version           Build  Channel
fastqc                        0.10.1               0  bioconda
fastqc                        0.10.1               1  bioconda
fastqc                        0.11.2               1  bioconda
fastqc                        0.11.2      pl5.22.0_0  bioconda
fastqc                        0.11.3               0  bioconda
fastqc                        0.11.3               1  bioconda
fastqc                        0.11.4               1  bioconda
fastqc                        0.11.4               2  bioconda
fastqc                        0.11.5               1  bioconda
fastqc                        0.11.5               4  bioconda
fastqc                        0.11.5      pl5.22.0_2  bioconda
fastqc                        0.11.5      pl5.22.0_3  bioconda
fastqc                        0.11.6               2  bioconda
fastqc                        0.11.6      pl5.22.0_0  bioconda
fastqc                        0.11.6      pl5.22.0_1  bioconda
fastqc                        0.11.7               4  bioconda
fastqc                        0.11.7               5  bioconda
fastqc                        0.11.7               6  bioconda
fastqc                        0.11.7      pl5.22.0_0  bioconda
fastqc                        0.11.7      pl5.22.0_2  bioconda
fastqc                        0.11.8               0  bioconda
fastqc                        0.11.8               1  bioconda
fastqc                        0.11.8               2  bioconda
fastqc                        0.11.9               0  bioconda
```

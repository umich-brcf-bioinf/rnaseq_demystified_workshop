# Table of Contents

- [What is Conda?](#what-is-conda)
- [Installing Miniconda3](#installing-miniconda3)
- [Testing the Installation](#testing-the-installation)
- [Creating a Workshop Environment](#creating-a-workshop-environment)
- [Entering and Exiting an Environment](#entering-and-exiting-an-environment)
- [Extras](#extras)

# What is Conda?

Conda (aka Miniconda3 aka Anaconda) is a software package manager that enables users to quickly and easily install many programs for bioinformatics analysis, among other things. A conda environment can be thought of as a workspace that has specific tools loaded in it. Perhaps a good analogy are different rooms in a research lab. Different rooms can be stocked with different tools to accomplish different tasks. We'll pick this analogy up after we install and test `conda`.

# Installing Miniconda3

Go to the Miniconda3 installer [download page](https://docs.conda.io/en/latest/miniconda.html) and download the appropriate installer:

- For Windows that means the [Python 3.8 Miniconda3 Windows 64-bit installer](https://repo.anaconda.com/miniconda/Miniconda3-latest-Windows-x86_64.exe).
- For macOS that means the [Python 3.8 Miniconda MacOSX 64-bit pkg installer](https://repo.anaconda.com/miniconda/Miniconda3-latest-MacOSX-x86_64.pkg).

Once the installer downloads, open it and follow the instructions to install.

**To testers:** When I first tried using the pkg installer in macOS I got to "How do you want to install this software?" with a big red stop sign that said "You cannot install Miniconda3 in this location" even though "Install for me only" was pre-selected. Clicking "Install for me only" made the message go away and install seemed to go smoothly. Did this occur for other Mac users? What about Windows users?

# Testing the Installation

Let's test the installation by opening a shell.

- For Windows that means ... I don't know right now?
- For macOS that means opening the Terminal.

Once we're in a shell, type:

```
conda --version
```

You should see:

```
conda 4.8.3
```

# Creating a Workshop Environment

Picking up the analogy of rooms in a research lab to think about `conda` environments, we need to stock the room with the correct tools. We do this with the `conda install` command. After the full command, we'll explain what each part means.

```
conda create --name workshop --channel bioconda --channel conda-forge fastqc=0.11.9 multiqc=1.9 STAR=2.7.6a rsem=1.3.3
```

When `conda` prompts you as to whether or not to proceed, say Yes.

## Breaking Down the Command

We have used the "long flags" which give a better indication of all the options we've chosen:

- `conda create`: The command to create an environment.
- `--name`: The name of the environment. This should be unique, and you can see what other environments you have with `conda env list`.
- `--channel`: `conda` has different channels depending on the type of software. For bioinformatics software two of the most common channels are `bioconda` and `conda-forge`.
- `fastqc=0.11.9 ...`: The name of the software packages we want to install and the specific version. By default conda will install the latest version, but it's good to get in the habit of specifying a version for your reference. Any number of packages can be installed at once by separating them with spaces.

# Entering and Exiting an Environment

Again, picking up the analogy of rooms in a research lab, in order to use the microscope you have to enter the room with it. The idea is the same with `conda`.

To enter the `conda` environment we just installed, we do:

```
conda activate workshop
```

Note that activating the workshop environemnt added `(workshop)` to the front of your shell prompt, like:

```
(workshop) rcavalca@x86_64-apple-darwin13 rnaseq_demystified_workshop %
```

Now we can look around and see that the software tools we put in the environment are there:

```
which fastqc
```

Should show you where `fastqc` is installed:

```
/Users/rcavalca/opt/miniconda3/envs/workshop/bin/fastqc
```

Now when we're done using the software in the environment--or when we're done using the microscope--we leave the room and turn off the lights.

```
conda deactivate
```

Just to drive the point home that the tools in that environment are no longer available to us, try:

```
which fastqc
```

You should see the following, which means that we've left the workshop environment.

```
fastqc not found
```

# Extras

## Searching for Packages

The above command begs the question "How did you know what the packages were called and what versions were available?" The answer is the `conda search` command. For example, the following command will search the `bioconda` channel for all versions of `fastqc`:

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

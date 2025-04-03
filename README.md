# Getting Started with the EasyBuild Local Directory

This guide will help you get started with using the EasyBuild repository for local software installations on the Michigan State University (MSU) High Performace Computing Center (HPCC). Follow the steps below to set up your environment, build software, and manage your installations.

### Table of Contents
- [First Time Setup Guide](#first-time-setup-guide)
  - [1. Clone the Repository](#1-clone-the-repository)
  - [2. Set Up Your Environment](#2-set-up-your-environment)
  - [3. Install Software Locally](#3-install-software-locally)
  - [4. Use Locally Installed Software](#4-use-locally-installed-software)
-  [Downloading From the System Repository](#Downloading-From-the-System-Repository)
-  [Installing Software from a GitHub Commit using EasyBuild](#Installing-Software-from-a-GitHub-Commit-using-EasyBuild)
-  [Installing Software from a GitHub Pull Request using EasyBuild](#Installing-Software-from-a-GitHub-Pull-Request-using-EasyBuild)
-  [Steps for R intel16/intel18 install](#Steps-for-R-intel16/intel18-install)  
- [Hello World Example](#hello-world-example)
- [Optional Steps](#optional-steps)
  - [Reset the Environment (Optional)](#reset-the-environment-optional)
  - [Manage Conda Environments (Optional)](#manage-conda-environments-optional)
  - [Troubleshooting](#troubleshooting)
  - [WARNING](#WARNING)
  - [Updating Config Files](#Updating-Config-Files)
- [Additional Resources](#additional-resources)

If this is your first time using EasyBuild or havn't used it in a while, I suggest you take a look at the [EasyBuild and Module System Terminology and Commands](terminology.md) to get a better understanding of the terms used in this guide and what is happening behind the scenes when running these commands. If you feel confident in your understanding of EasyBuild, you can skip to the [Hello World Example](#hello-world-example) to make sure everything is working correctly.

## First Time Setup Guide

## 1. Clone the Repository

First, clone the repository to your home directory on the HPCC:

```
git clone https://github.com/colbrydi/easybuild_local.git
```
![image](https://github.com/user-attachments/assets/c5fd7481-f3d0-43e1-b804-7475bf664c41)

Navigate to the repository directory:

```
cd easybuild_local
```
![image](https://github.com/user-attachments/assets/b3c3f304-2f27-4420-84d3-bb618b76638e)

## 2. Set Up Your Environment

To begin using EasyBuild in this repository, you need to set up your environment by sourcing the `activate_easybuild_local.sh` script. This script loads the necessary modules and points EasyBuild to the local configuration file.

Run the following command:

```
source activate_easybuild_local.sh
```
![image](https://github.com/user-attachments/assets/c056fccb-fdf6-4b81-bce6-c70a7ba4805e)

This will load EasyBuild and configure it to use the paths defined in `config.cfg`.

## 3. Install Software Locally

NOTE: This tutorial teaches how to download and install files in the easyconfig directory. Later on there are instructions on how to download and install files from the HPCC system repository. 

Once your environment is set up, you can start installing software locally using EasyBuild. The repository contains EasyBuild configuration files (`.eb` files) for several software packages in the `easyconfigs/` directory.

To install a software package, run:

```
eb --parallel=8 --robot ./easyconfigs/<FILENAME>.eb
```

The parallel command determines how many cores are being used and robot automatically downloads all of the dependencies in an EasyBuild config. Replace `<FILENAME>` with the name of the configuration file you want to build.

For example, you can run the example eb file provided by typing the following:

```
eb --parallel=8 --robot ./easyconfigs/Hello-2.10-GCCcore-11.2.0.eb 
```
![image](https://github.com/user-attachments/assets/5ff6d5aa-ccf2-480e-8f57-e531f2879ede)

![image](https://github.com/user-attachments/assets/3126a68a-d1d5-4697-83c7-5f19818d04e5)

## 4. Use Locally Installed Software

After the installation is complete, you can make the locally installed software available by running the following script:

```
source eb_local_use.sh
```
![image](https://github.com/user-attachments/assets/9ef58728-b1d9-4c6b-906d-426084afb1b7)

This adds the locally installed modules to the module search path, allowing you to load and use them.

For example, to load the provided example, you would run:

```
module load Hello/2.10-GCCcore-11.2.0
```
![image](https://github.com/user-attachments/assets/3ceb9046-46d5-42e6-b352-c29aa66b28a1)

Now that the module is loaded, you can use the software as needed. For this example, you can run:

```
hello
```

And you should see the following output:

```
Hello, world!
```
![image](https://github.com/user-attachments/assets/55311554-5901-488b-939b-62c34767f2aa)

## Hello World Example

This repository includes a simple "Hello, World!" example to demonstrate the process of building and installing software using EasyBuild. These are similar to the steps above.

```
git clone https://github.com/colbrydi/easybuild_local.git

cd easybuild_local

source activate_easybuild_local.sh
 
eb --parallel=8 --robot ./easyconfigs/Hello-2.10-GCCcore-11.2.0.eb

source eb_local_use.sh

module load Hello/2.10-GCCcore-11.2.0

hello
```
This prints Hello, world!

## Downloading From the System Repository

## 1. Finding EB Files

In order to find dependencies that are required to download and find software on the HPCC, there is an EasyBuild command that is helpful

```
eb -S <SOFTWARE>
```
This command searches for EasyBuild files with the name of <SOFTWARE> that are available to download.

![image](https://github.com/user-attachments/assets/947789f9-f63c-434e-af1c-91903225e00a)



## 2. Downloading and Finding Dependencies

Once you have found the EasyBuild file you want to download, run:

```
eb -M <FILENAME>.eb
```
This command shows all the dependencies that are needed to download to run the EasyBuild file. 

![image](https://github.com/user-attachments/assets/94f845f3-2e30-4b96-aca2-a3a0272f52b5)

To download the dependencies, run:

```
eb --parallel=8 <FILENAME>.eb
```
This command downloads the dependency. When downloading the dependencies, make sure to download the dependency that has the same <FILENAME>.eb as the eb -M search last. In this case, I would download the dependency RNA-SeQC-1.1.8-GCCcore-11.2.0-Java-11.eb last.

![image](https://github.com/user-attachments/assets/21c9a5f7-ccb7-4b6d-aa84-78f1ac654f2b)

## 3. The Robot Command

Dowloading all the necessary dependencies can be very tedious. In order to download all the dependencies and the EasyBuild file at once, use

```
eb --parallel=8 --robot <FILENAME>.eb
```

This allows all the dependencies to be downloaded at once rather than having to download them individually.

![image](https://github.com/user-attachments/assets/4d47a467-f488-4b14-992e-33e4b0e06b8d)




![image](https://github.com/user-attachments/assets/a3ed2ab4-d66f-43c8-898c-3577fa215cc6)

## Installing Software from a GitHub Commit using EasyBuild

This guide explains how to install software from a specific commit in the EasyBuild repository. For this example, we will use OpenFOAM v2112.

## 1. Locate the File

- Find the software you want to install in the easyconfigs folder: https://github.com/easybuilders/easybuild-easyconfigs/tree/develop/easybuild/easyconfigs
- Example for OpenFOAM v2112: https://github.com/easybuilders/easybuild-easyconfigs/blob/develop/easybuild/easyconfigs/o/OpenFOAM/OpenFOAM-v2112-foss-2023a.eb

## 2. Find the Most Recent Commit
- Click on **History** to view previous commits to the file.
- Identify the most recent commit that you want to install from.
- Copy the full commit SHA (a long alphanumeric string).

## 3. Install the Software Using the Commit
Use the following command to download the software from the commit:

```bash
eb --from-commit <commit-SHA> <easyconfig-file>
```

For OpenFOAM v2112, the command looks like this:

```bash
eb --from-commit 70e5f79258b1cb9a429525b774e4131486210e18 OpenFOAM-v2112-foss-2023a.eb
```

Note: Installations may take a while so it is recommended to perform this on an Interactive Desktop.


## Installing Software from a GitHub Pull Request using EasyBuild

This guide explains how to install software from a pull request in the EasyBuild repository. For this example, we will use OpenFOAM v2112.


## 1. Locating the Easyconfig file
  Go to https://github.com/easybuilders/easybuild-easyconfigs and click on the easybuild/easyconfigs folder. 
  
  ![image](https://github.com/user-attachments/assets/d5fd2fa3-b48f-4793-a479-6f3f4fd71b5c)

  
  Scroll through and click on the easybuild configuration you want to download. In this example we want OpenFOAM-v2112-foss-2023a.eb ![image](https://github.com/user-attachments/assets/022657d2-1eb8-4aa6-81b6-1867aa94d1e1)



## 2. Find the Most Recent Pull Request
Once you find the file you want to download, click on the history button in the top right corner. 

![image](https://github.com/user-attachments/assets/ee013b77-8929-400f-893b-0cc12f27193d)


Once on the commits screen, click on the commit you want to download. In this case, the “fix dependency” version is being downloaded.

![image](https://github.com/user-attachments/assets/d4b258d0-ad22-4ebe-91de-9b7b2b8aa563)

The pull number will be found in the top left corner next to the blue “develop”. The pull number for this OpenFOAM easyconfig is 22241.

![image](https://github.com/user-attachments/assets/c88a0163-050a-4c71-8cea-e6a178486351)



## 3. Install the Software Using the Pull Request Number
To download OpenFOAM from the pull request number, run eb --from-pr [pull request number] --robot
In this case I would run eb --from-pr 22241 --robot. 

![image](https://github.com/user-attachments/assets/9dc3a108-e60f-454f-b700-e167579d7057)


Note: It is recommended to download the EasyConfig file on the HPCC Interactive Desktop because some downloads may take hours.



## Installing Under Different Architectures


## R Installation on dev-intel16

## 1. Open dev-intel16


```
ssh dev-intel16
```

Navigate to the repository directory:

```
cd easybuild_local
```

## 2. Set Up Your Environment

Run the following command:

```
source activate_easybuild_local.sh
```

## 3. Download R File Locally


To download the R 4.4.1 GitHub file, run:

```
wget https://raw.githubusercontent.com/easybuilders/easybuild-easyconfigs/refs/heads/develop/easybuild/easyconfigs/r/R/R-4.4.1-gfbf-2023b.eb
```
(URL is from https://github.com/easybuilders/easybuild-easyconfigs/blob/develop/easybuild/easyconfigs/r/R/R-4.4.1-gfbf-2023b.eb)
Replace the URL with the appropriate R GitHub file if needed.

Change the directory to the easyconfigs folder and move the R download:

```
cd easyconfigs
mv R-4.4.1-gfbf-2023b.eb easyconfigs
```

Open the file:

```
vi R-4.4.1-gfbf-2023b.eb
```
Make the following edits to include intel-16 (press 'i' to enter Insert mode):

```
version = '4.4.1-intel16'
toolchain = {'name': 'intel', 'version': '16'}
versionsuffix
```
Press Esc and enter :wq to save and close the file.

Save a copy of the file to include intel16:

```
cp R-4.4.1-gfbf-2023b.eb R-4.4.1-gfbf-2023b-intel16.eb
```


## 4. Install R

Run the following command to install R-4.4.1 using the new .eb file on dev-intel16:

```
eb --parallel=8 --robot ./easyconfigs/R-4.4.1-gfbf-2023b-intel16.eb
```
Check that a new module with intel16 exists:

```
ls ./software/modules/all 
```

## 5. Use Locally Installed Software

Navigate to the R folder:

```
cd ./software/modules/all/R
```

After the installation is complete, you can make the locally installed software available by running the following script:

```
module use ~/easybuild_local/software/modules/all
```

To load the new module, run the following script:

```
module load R/4.4.1-gfbf-2023bintel16.lua
```

Now that the module is loaded, you can use the software as needed. For this example, you can run:

```
R
```

And you should be able to use R as needed.


## 6. Verify

Double check that the working version of R points a install path with intel16 in the name:

```
which R
```

You should see a path that points to the intel16 install:

```
/mnt/ufs18/home-067/nguye922/easybuild_local/software/software/R/4.4.1-gfbf-2023bintel16/bin/R
```

## R Installation on dev-intel18

## 1. Open dev-intel18


```
ssh dev-intel18
```

Navigate to the repository directory:

```
cd easybuild_local
```

## 2. Set Up Your Environment

Run the following command:

```
source activate_easybuild_local.sh
```

## 3. Download R File Locally


To download the R 4.4.1 GitHub file, run:

```
wget https://raw.githubusercontent.com/easybuilders/easybuild-easyconfigs/refs/heads/develop/easybuild/easyconfigs/r/R/R-4.4.1-gfbf-2023b.eb
```
(URL is from https://github.com/easybuilders/easybuild-easyconfigs/blob/develop/easybuild/easyconfigs/r/R/R-4.4.1-gfbf-2023b.eb)
Replace the URL with the appropriate R GitHub file if needed.

Change the directory to the easyconfigs folder and move the R download:

```
cd easyconfigs
mv R-4.4.1-gfbf-2023b.eb easyconfigs
```

Open the file:

```
vi R-4.4.1-gfbf-2023b.eb
```
Make the following edits to include intel-18 (press 'i' to enter Insert mode):

```
version = '4.4.1-intel18'
toolchain = {'name': 'intel', 'version': '18'}
versionsuffix
```
Press Esc and enter :wq to save and close the file.

Save a copy of the file to include intel18:

```
cp R-4.4.1-gfbf-2023b.eb R-4.4.1-gfbf-2023b-intel18.eb
```


## 4. Install R

Run the following command to install R-4.4.1 using the new .eb file on dev-intel18:

```
eb --parallel=8 --robot ./easyconfigs/R-4.4.1-gfbf-2023b-intel18.eb
```
Check that a new module with intel18 exists:

```
ls ./software/modules/all 
```

## 5. Use Locally Installed Software

Navigate to the R folder:

```
cd ./software/modules/all/R
```

After the installation is complete, you can make the locally installed software available by running the following script:

```
module use ~/easybuild_local/software/modules/all
```

To load the new module, run the following script:

```
module load R/4.4.1-gfbf-2023bintel18.lua
```

Now that the module is loaded, you can use the software as needed. For this example, you can run:

```
R
```

And you should be able to use R as needed.


## 6. Verify

Double check that the working version of R points a install path with intel18 in the name:

```
which R
```

You should see a path that points to the intel18 install:

```
/mnt/ufs18/home-067/nguye922/easybuild_local/software/software/R/4.4.1-gfbf-2023bintel18/bin/R
```


## Optional Steps

## Reset the Environment (Optional)

If you need to clean up your environment and reset the installation directories, you can use the `eb_local_reset.sh` script. This script will delete the `software/` directory and recreate the necessary directories for a fresh start. This will also deactivate your conda environment.

To reset the environment, run:

```
source eb_local_reset.sh
```

## Common Problems

If you run into an error like this:

```
FAILED: Installation ended unsuccessfully
(build directory: /mnt/ufs18/home-251/wunderky/easybuild_local/tempdir/build/Hello/2.10/GCCcore-11.2.0):
build failed (first 300 chars):
Module is already loaded (EBROOTHELLO is set), installation cannot continue. (took 3 secs)
```

Here are some steps to try and resolve the issue:

1. Unload the module before running the installation command:

```
module unload Hello
```

2. See if the environment variable is set:

```
env | grep EBROOTHELLO
```

If the environment variable is set, unset it:

```
unset EBROOTHELLO
```

3. Try resetting the environment and starting fresh:

# WARNING

Older tool chains may cause errors while downloading. Some EB files as early as 2021b have been successfully downloaded, but earlier toolchains could cause traceback errors. 

![image](https://github.com/user-attachments/assets/72664976-448e-41be-8c51-3e0c297f0011)

# Updating Config Files

Toolchains that are old or cause errors can be updated. There are two commands that can update EasyBuild files in order to be downloaded.

```
--experimental --try-update-deps
```

This command updates the dependencies in order to successfully download the file if it previously failed. 

![image](https://github.com/user-attachments/assets/e1681f04-9cb6-43c5-b382-9fcd7f3741bb)

```
--try-toolchain-version <VERSION>
```

This command specifically updates the toolchain to see if download will be successful on a different version. In the example below, the earlier toolchain version GCCcore-12.3.0 was used after the download on GCCcore-11.3.0 failed.

![image](https://github.com/user-attachments/assets/2c024630-3549-4f8c-a74c-7ad0d5ce963d)
![image](https://github.com/user-attachments/assets/1af460d3-e935-448c-93cf-6fb8ef2cedc1)



## Additional Resources

- [EasyBuild Documentation](https://easybuild.readthedocs.io/)
- [EasyBuild EasyConfigs](https://github.com/easybuilders/easybuild-easyconfigs)

#### Editors

Nick Panchy <br/>
Connor Neiheisel 
Anna Nguyen

# Getting Started with the EasyBuild Local Directory

This guide will help you get started with using the EasyBuild repository for local software installations on the Michigan State University (MSU) High Performace Computing Center (HPCC). Follow the steps below to set up your environment, build software, and manage your installations.

# WARNING

Older tool chains may cause errors while downloading. Some EB files as early as 2021b have been successfully downloaded, but earlier toolchains could cause traceback errors. 

![image](https://github.com/user-attachments/assets/72664976-448e-41be-8c51-3e0c297f0011)


### Table of Contents
- [First Time Setup Guide](#first-time-setup-guide)
  - [1. Clone the Repository](#1-clone-the-repository)
  - [2. Set Up Your Environment](#2-set-up-your-environment)
  - [3. Install Software Locally](#3-install-software-locally)
  - [4. Use Locally Installed Software](#4-use-locally-installed-software)
- [Hello World Example](#hello-world-example)
- [Optional Steps](#optional-steps)
  - [Reset the Environment (Optional)](#reset-the-environment-optional)
  - [Manage Conda Environments (Optional)](#manage-conda-environments-optional)
  - [Troubleshooting](#troubleshooting)
- [Additional Resources](#additional-resources)

If this is your first time using EasyBuild or havn't used it in a while, I suggest you take a look at the [EasyBuild and Module System Terminology and Commands](terminology.md) to get a better understanding of the terms used in this guide and what is happening behind the scenes when running these commands. If you feel confident in your understanding of EasyBuild, you can skip to the [Hello World Example](#hello-world-example) to make sure everything is working correctly.

## First Time Setup Guide

## 1. Clone the Repository

First, clone the repository to your home directory on the HPCC:

```
git clone https://github.com/colbrydi/easybuild_local.git
```

Navigate to the repository directory:

```
cd easybuild_local
```

## 2. Set Up Your Environment

To begin using EasyBuild in this repository, you need to set up your environment by sourcing the `activate_easybuild_local.sh` script. This script loads the necessary modules and points EasyBuild to the local configuration file.

Run the following command:

```
source activate_easybuild_local.sh
```

This will load EasyBuild and configure it to use the paths defined in `config.cfg`.

## 3. Install Software on the HPCC

Once your environment is set up, you can start installing software on the HPCC using EasyBuild. The repository contains EasyBuild configuration files (`.eb` files) for several software packages in the `easyconfigs/` directory.

To install a software package, run:

```
eb --parallel=8 --robot <FILENAME>.eb
```

Replace `<FILENAME>` with the name of the configuration file you want to build. The parallel command determines the amount of cores used while downloading and the robot command tells the program to download the software and all of its dependencies.

For example, you can run the example eb file provided by typing the following:

```
eb --parallel=8 --robot BWA-0.7.17-GCCcore-11.2.0.eb
```

## 4. Use Installed Software

After the installation is complete, you can make the locally installed software available by running the following script:

```
source eb_local_use.sh
```

This adds the locally installed modules to the module search path, allowing you to load and use them.

For example, to load the provided example, you would run:

```
module load BWA-0.7.17-GCCcore-11.2.0.eb
```



## 5 Installing Dependencies and Finding EB Files

In order to find dependencies that are required to download and find software on the HPCC, there are a few EasyBuild commands that are helpful.

```
eb -S <SOFTWARE>
```
This command searches for EasyBuild files with the name of <SOFTWARE> that are available to download.
![image](https://github.com/user-attachments/assets/947789f9-f63c-434e-af1c-91903225e00a)

Once you have found the EasyBuild file you want to download, run:

```
eb -M <FILENAME>.eb
```
This command shows all the dependencies that are needed to download to run the EasyBuild file. 

![image](https://github.com/user-attachments/assets/94f845f3-2e30-4b96-aca2-a3a0272f52b5)

When downloading the dependencies, make sure to download the dependency that has the same <FILENAME>.eb as the eb -M search last. In this case, I would download the dependency RNA-SeQC-1.1.8-GCCcore-11.2.0-Java-11.eb last. To download the dependencies, run:

```
eb --parallel=8 <FILENAME>.eb
```
This command downloads the dependency. 
![image](https://github.com/user-attachments/assets/21c9a5f7-ccb7-4b6d-aa84-78f1ac654f2b)




## Optional Steps

## Reset the Environment (Optional)

If you need to clean up your environment and reset the installation directories, you can use the `eb_local_reset.sh` script. This script will delete the `software/` directory and recreate the necessary directories for a fresh start. This will also deactivate your conda environment.

To reset the environment, run:

```
source eb_local_reset.sh
```

## Troubleshooting

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


## Additional Resources

- [EasyBuild Documentation](https://easybuild.readthedocs.io/)
- [EasyBuild EasyConfigs](https://github.com/easybuilders/easybuild-easyconfigs)

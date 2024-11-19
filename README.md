# Getting Started with the EasyBuild Local Directory

This guide will help you get started with using the EasyBuild repository for local software installations on the Michigan State University (MSU) High Performace Computing Center (HPCC). Follow the steps below to set up your environment, build software, and manage your installations.

# WARNING

Older tool chains may cause errors while downloading. Some EB files as early as 2021b have been successfully downloaded, but earlier toolchains could cause traceback errors. 

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

## 3. Install Software Locally

Once your environment is set up, you can start installing software locally using EasyBuild. The repository contains EasyBuild configuration files (`.eb` files) for several software packages in the `easyconfigs/` directory.

To install a software package, run:

```
eb --parallel=8 --robot ./easyconfigs/<FILENAME>.eb
```

Replace `<FILENAME>` with the name of the configuration file you want to build.

For example, you can run the example eb file provided by typing the following:

```
eb --parallel=8 --robot ./easyconfigs/Hello-2.10-GCCcore-11.2.0.eb 
```

## 4. Use Locally Installed Software

After the installation is complete, you can make the locally installed software available by running the following script:

```
source eb_local_use.sh
```

This adds the locally installed modules to the module search path, allowing you to load and use them.

For example, to load the provided example, you would run:

```
module load Hello/2.10-GCCcore-11.2.0
```

Now that the module is loaded, you can use the software as needed. For this example, you can run:

```
hello
```

And you should see the following output:

```
Hello, world!
```

## Hello World Example

This repository includes a simple "Hello, World!" example to demonstrate the process of building and installing software using EasyBuild. These are similar to the steps above.

1. Clone the repository and navigate to the directory:

```
git clone https://github.com/colbrydi/easybuild_local.git
cd easybuild_local
```

2. Set up your environment:

```
source activate_easybuild_local.sh
```

3. Install the "Hello, World!" software:

```
eb --parallel=8 --robot ./easyconfigs/Hello-2.10-GCCcore-11.2.0.eb
```

4. Use the locally installed software:

```
source eb_local_use.sh
module load Hello/2.10-GCCcore-11.2.0
```

5. Run the software:

```
hello
```

You should see the output:

```
Hello, world!
```

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

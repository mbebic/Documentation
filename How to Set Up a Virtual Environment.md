# How to Set Up a Virtual Environment

Taken from https://uoa-eresearch.github.io/eresearch-cookbook/recipe/2014/11/20/conda/, condensed here for simplicity and easy access for myself and others.

## Check conda is installed and in your path
1. Open a terminal client.
2. Enter <code>conda -v</code> into the terminal command line and press enter. 
3. If conda is installed, you should see this:

```shell
conda -v
OUTPUT: conda VERSION#
```

## Check conda is up to date

This step isnt entirely necessary, but good if you want to stay up to date with your conda installation.

1. In the terminal, enter:

```shell
conda update conda
```

2. Update any packages if necessary by typing <code>y</code>to proceed.

## Create a virtual environment for your project

1. In the terminal client, enter the following where yourenvname is the name you want to call your environment, and replace x.x with the Python version you wish to use. (To see a list of available python versions first, type <code>conda search "^python$"</code> and press enter).

```shell
conda create -n yourenvname python=x.x anaconda
```

2. Press <code>y</code> to proceed. This will install the Python version and all the associated anaconda packaged libraries at “path_to_your_anaconda_location/anaconda/envs/yourenvname”

## Activate your virtual environment
1. To activate or switch into your virtual environment, simply type the following where yourenvname is the name you gave to your environment at creation.

```shell
conda activate yourenvname
```
2. Activating a conda environment modifies the PATH and shell variables to point to the specific isolated Python set-up you created. The command prompt will change to indicate which conda environemnt you are currently in by pre-pending <code>(yourenvname)</code>. To see a list of all your environments, use the command <code>conda info --envs</code> or <code>conda env list</code>.

## Install additional Python packages to a virtual environment
1. To install additional packages only to your virtual environment, enter the following command where yourenvname is the name of your environment, and [package] is the name of the package you wish to install. Failure to specify “-n yourenvname” will install the package to the root Python installation.

```shell
conda install -n yourenvname [package]
```

## Deactivate your virtual environment
1. To end a session in the current environment, enter the following. There is no need to specify the envname - whichever is currently active will be deactivated, and the PATH and shell variables will be returned to normal.

```shell
conda deactivate
```

## Delete a no longer needed virtual environment
1. To delete a conda environment, enter the following, where yourenvname is the name of the environment you wish to delete.

```shell
conda remove -n yourenvname -all
```

# Jupyter Notebook Git Repo Template

`git` is great for Jupyter Notebooks\*! 

\**With some help...*

## Common complaints with git and Jupyter

* Let's say you're working on a notebook: 
    * You write/run some code. Everything looks good.
    * You add your notebook(s) to the staging area, you commit them, and you push to the remote repo.
    * Turns out you forgot to clear your notebook output. Maybe you wanted this, but if you're developing these for interactive use and collaboration, you probably didn't.
        * If you want your notebooks to exist as static reports, consider saving them as HTML in addition to `.ipynb`
    * What now? You clean the notebook, maybe make a new commit, maybe `git reset` first if you want to get fancy
    * You commit again.
    * And if you're like me, pretty soon you'll make the same mistake again....

## What this repo does

### Pre-commit hook
* This file is located in `.git/hooks/pre-commit`
* Whenever you `git commit`, this script will be run before the commit actually takes place. It contains just a few lines:
    ```
    #!/bin/sh
    jupyter nbconvert --ClearOutputPreprocessor.enabled=True --inplace *.ipynb
    git add .
    ```
* The second line, `jupyter nbconvert...` will iterate through all notebooks in the top-level directory (this will require a slight modification for nested directories) and will remove all output.
* The next line runs `git add .` again. As a result, any changes due removing notebook output will be reflected in the actual commit.

### .gitignore
* This file tells git which directories, files, file extensions, etc to exclude from its version tracking.
* This is great for excluding data and caches, among other things.


## To run locally:
* Download or clone the repository
* Ensure that you have Python and conda installed. 
* If you're using this as a template, you might want to modify `environment.yml` (See below)
* Create a conda environment from `environment.yml` by:
    * Running `conda env create -f environment.yml` from within this directory 
    * Using the Anaconda Navigator app
* Activate the environment and open Jupyter:
    * `conda activate jupyter-workflow; jupyter notebook`
    * Or use Anaconda Navigator

##  Modify `environment.yml` as appropriate!
* Change the name in the first line to a descriptive name for your project. I'd recommend you use the same name as the repository. 
* You'll probably want to keep the `conda-forge` channel in there, but it depends on the libraries your environment uses. 
* Change the dependencies to whatever is suitable for your project. Mine is more of an example than a template.
    * Add the packages you need, and I would recommend deleting the lines referring to packages that you *don't* need.
    * You don't necessarily \**have*\* to specify versions for packages, but:
        * It typically makes it faster to install the environment, which can be nice for spinning up Binder notebooks (more on that below). 
        * It's good practice, anyway, as it ensures maximum reproducibility when installing somewhere else.
* This is certainly not the only way to make a YML file for a conda environment. In simple project repositories like this one, I like this approach because:
    * It's easy to read!
    * It (hopefully) encourages you to include only packages you need. 
* The downside to creating your environment this way:
    * You can't really use `conda install` and keep such a clean environment specification. Instead, you might do something like the following:
        * Update your `environment.yml` file.
        * Run `conda env update -f environment.yml`
* Since you changed the name, that means you activate the environment with `conda activate <your new name here>`

## Sharing with Binder:
* If your repo is publicly hosted, you can very easily share an interactive, temporary version of it through [mybinder.org](mybinder.org)
* To do this, you need that `environment.yml` file. That's how Binder knows which packages to install.
* Tip: If you you go to mybinder.org and type in the link to your repo, you can create a badge that links straight to it! Here's the one for this repo:


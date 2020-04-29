# Workflow-on-Julia-with-Jupyter-Atom-Github
Notes on a workflow approach to Julia using Jupyter, Atom and Github to develop package-ready code

## The basic setup

I like JupyterLab as a place to mess about with snippets of code, and also to control larger packages. There are alternatives. I can't get on with the IDEs lik Juno (too slow and rigid), or Sublime Text 3 (setting it up to work as an IDE is too terrifying for me). 

But to write functions and modules, I need an editor. For me Atom strikes the right balance. I yearn for a bit more snappiness, so maybe one day I will Sublime, but removing all the Juno rubbish makes Atom fast enough for my purposes.

## Setup instructions - Julia

First, let's get the latest version of Julia. From here: https://julialang.org/

Once you've installed the binaries for your platform, open the REPL (i.e., click on "Julia") and install IJulia. To do this, enter the following:
```
] #Puts the REPL into package mode
add iJulia
```
If you already have an installation of Anaconda on your machine, then this is enough to get a new Julia kernel as an option when you start up JupyterLab. If you do not, then follow the instructions here: https://github.com/JuliaLang/IJulia.jl to get it set up in a minimal installation.

If at this point you have a previous kernel you want to get rid of (e.g., an old Julia version) then go to the Anaconda prompt and check all the kernels you have installed:

```
jupyter kernelspec list
```
And then get rid of thee ones no longer needed.
```
jupyter kernelspec uninstall <unwanted-kernel>
```
  
## Setup instructions - Atom

Go here: https://atom.io/

It just works. 

Add whatever bumpf you want, but it's a good idea to get Julia highlighting (Ctrl + , then Install, then "language-julia"). I myself cannot get on with the other packages and in particular the Juno IDE package. You may want to try it.

## Workflow 1 - Jupyter + Atom + include

This is already good enough for a good amount of experimentation, up to and including developing your own code for your own projects. 

Mess around using Jupyter to experiment with code, and run it in the browser. When you want to wrap it up in functions, do so and paste these into Atom to write some "commonly_called_functions.jl" files in the same directory as the Jupyter notebook.  

All you then need to do is to to include a line:
```
include("commonly_called_functions.jl")
```
In your notebook, and then run this line every time you make a change to the code in your functions with Atom. The changes will show up in the notebook and you can experiment to see what happens.

And this is fine, for a personal project. Properly annotated, it can even be robust and repeatable. You can also make your functions and notebooks into a .git directory and push it to Github if that way inclined.

However, at some point you will be interested in developing code that others can use and work with. And this brings us to Workflow 2.

## Workflow 2 - Jupyter + Atom + Pkg + GitHub

The big difference here is that we are going to use Julia's package manager to organise your code. You've probably already used Pkg to import code from others.

I am not a coder, and find Pkg's documentation https://julialang.github.io/Pkg.jl/v1/index.html utterly incomprehensible. Project.toml and Manifest.toml and LOAD_PATH and JULIA_LOAD_PATH kept me going around in circles for ages. Here's the quickstart version that snapped everything into place for me.

Step 1: Create a skeleton project directory structure with a few required files in it. All the documentation raves about PkgTemplates to do this. I found that PkgSkeleton https://github.com/tpapp/PkgSkeleton.jl also did precisely what I needed without a murmer.

Whichever you use, the PackageName.jl in the /src directory contains the main module, and -- if you are developing from Workflow 1 (as I usually am) then you can again put:

```
include("commonly_called_functions.jl")
```
... in that main module, along with any import and export commands you want to be able to interact with your package.

Step 2: Now open a notebook, and install a new package which is pretty much essential: https://github.com/timholy/Revise.jl. Make sure this is included in the "include" lines of your notebook.

Step 3: Ignore all the stuff about LOAD_PATHs in the workflow tips of Julia, or in the Pkg manual. Instead, just type this:
```
] dev <Yourpackagename>
```
And now you refer to your development package in just the same way as any complete one. Most obviously, you can just type into the notebook:
```
include <Yourpackagename>
```
And all exported functions etc from your project's namespace.
  
What's more, by using Revise.jl, then you can edit everything in your new package's code, and - so long as you remember to save your changes - then they will push directly into your notebook. This allows you to work with the package just as if you were a third-party user.

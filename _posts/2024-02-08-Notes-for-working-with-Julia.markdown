---
layout: post
title:  "Getting Started With Julia"
date:   2024-02-08 15:08:46 -0400
categories: jekyll update
---

## Getting Started With Julia, notes for bioinformaticians

I recently started working with Julia, Below are some initial notes that I have been taking while installing and working with the programming language as well as the advantages that it can provide.

I tried to install originally on macintosh but found that it failed immediately. I was able to run the command to install juliaup however.

Finally the most important part is that you want to be able to use the following command

```

curl -fsSL https://install.julialang.org | sh
```

Then you can install a new julia version with the following command. Where 1.10 is the software version of the Julia language.


```{bash}

juliaup add +1.10

```


This will add a "channel" where the channel you will pick from the latest versions that are available for julia.
Currently as of this writing the versions are 1.10 as the stable release version.

https://github.com/jupyter-lsp/jupyterlab-lsp
install jupyterlab LSP

## Which Development Environment to use
can use pluto 
VS code is the only one that has julia insider for viewing dataframes
The vs code plugin can get setup exactly almost the same as Rstudio
with a few complaints and not as indepth for analysis
Julia packages

### Useful functional packages
transducers
folds

But most of the main things are similar
still hard to beat R studio for all of its integration that it has.

---
## Using Package compiler!


For compilation need to use
Pkg.instantiate()
and Pkg.resolve()

first need to activate a julia project
start the package manager and do 

activate .

Go up a directory then activate
then rerun package compiler and get it to work
Then you will have the dependencies correctly listed

https://bkamins.github.io/julialang/2020/05/10/julia-project-environments.html

Julia has made a breaking change in the types
instead of using void the type is now "nothing"

make sure to include this when you are running the code.

## Transducers and automa for quickly parsing files are also available
https://docs.juliahub.com/General/FiniteStateTransducers/stable/
if you care about weighted finite state transducers


## How do I rbind in julia?
want to make a dataframe from a list of vectors

Using vcat fpr rbinding matrices, or hcat for cbinding matrices. Once you have the matrix worked out then you can easily turn this into a dataframe.

## VIM Plugin

I use chad nvim and found there were a few things to be aware of because of the lazy package installation.

to get vim stuff working for inserting hte unicode symbols you need to do a ton of stuff that will take a lot of time
https://github.com/folke/lazy.nvim/blob/c734d941b47312baafe3e0429a5fecd25da95f5f/README.md#L104

you need to only load it when ft is true
```lua
  {
    "JuliaEditorSupport/julia-vim",
    ft=".jl",
    config=true
  },
```

in your package spec change lazy to a different boolean
it won't work and install when you use lazy loading and it will automatically detect the filetype for you anyway
lazy 	boolean?

https://docs.julialang.org/en/v1/manual/unicode-input/

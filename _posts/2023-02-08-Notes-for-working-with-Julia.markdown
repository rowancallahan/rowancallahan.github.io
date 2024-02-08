


if it crahes insstalling on macintosh that is fine
If it doesn't work don't worry about that
Main thing is that you want to be able to use the following command

```

curl -fsSL https://install.julialang.org | sh
```

```{bash}

juliaup add +1.10

```


This will add a "channel" where the channel you will pick from the latest versions that are available for julia.
Currently as of this writing the versions are 1.10 as the stable release version.

https://github.com/jupyter-lsp/jupyterlab-lsp
install jupyterlab LSP

can use pluto 
VS code is the only one that has julia insider for viewing dataframes
The vs code plugin can get setup exactly almost the same as Rstudio
with a few complaints and not as indepth for analysis
Julia packages

transducers
folds
gadfly

But most of the main things are similar
still hard to beat R studio for all of its integration that it has.

---
Using Package compiler!


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
https://docs.juliahub.com/General/FiniteStateTransducers/stable/
if you care about weighted finite state transducers


How do I rbind in julia?
want to make a dataframe from a list of vectors

Using hcat potentially 

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

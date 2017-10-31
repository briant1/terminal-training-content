## Brew Explained

First a few terms:

Term        | Description           
------------- | ------------- 
Formula     | The package definition 
Keg      | The installation prefix of a Formula      
Cellar | All Kegs are installed here      
Tap | A Git repository of Formulae and/or commands      
Bottle | Pre-built Keg used instead of building from source      

[view more](https://docs.brew.sh/Formula-Cookbook.html#homebrew-terminology)

Homebrew installs packages into the /usr/local directory. Why?

Why does Homebrew prefer I install to /usr/local?
* It’s easier
* /usr/local/bin is already in your PATH.
* Apple has left this directory for us. Which means there is no /usr/local directory by default, so there is no need to worry about messing up existing tools.\[[1](https://docs.brew.sh/FAQ.html)\]

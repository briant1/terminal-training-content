## Introduction

A **package manager** automates the process of installing, updating and uninstalling software and libraries on your operating system. Software packages contain metadata, such as the software's name, version, vendor and dependencies necessary for the software to run. A **package manager** maintains it's own database of this metadata to monitor version information and to prevent software mismatches.

The most popular package manager for macOS is (brew)[https://brew.sh/]. To install brew run the following in your terminal: 

<pre>
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
</pre>

Homebrew installs packages to their own directory and then symlinks their files into /usr/local.

These next lessons will walk you though using brew to install a few packages.

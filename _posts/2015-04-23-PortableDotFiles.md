---
layout: post
title:  "Portable Dotfiles Using Git & Github"
date:   2015-04-23
author: Tara Furstenau
categories: Programming
tags:	dot files
cover:  "assets/instacode.png"
---
This set of notes covers how to effectively manage custom dotfiles so they may easily be transferred between different machines. The notes include a script from [Using Git and Github to Manage your Dotfiles](http://blog.smalleycreative.com/tutorials/using-git-and-github-to-manage-your-dotfiles/) by MICHAEL SMALLEY. Once the dotfiles repository is cloned into the home directory, this script will create symbolic links to the custom configuration files.  It will also move any existing dotfiles that match those in the cloned directory into a dotfiles_old directory.  

### Organize dotfiles in new directory
In the home directory create a new directory for the dotfiles  


	$ mkdir ~/dotfiles


Move the dot files into this directory.  


	$mv ~./.bashrc ~/dotfiles


For portability add this makesymlinks.sh install script to the dotfiles directory  



	#!/bin/bash  
	############################
	# .make.sh  
	# This script creates symlinks from the home directory to any desired  dotfiles in ~/dotfiles  
	############################  

	########## Variables  

	dir=~/dotfiles                    # dotfiles directory  
	olddir=~/dotfiles_old             # old dotfiles backup directory  
	files="bashrc vimrc vim zshrc oh-my-zsh private scrotwm.conf   Xresources"    # list of files/folders to symlink in homedir  

	##########  

	# create dotfiles_old in homedir
	echo -n "Creating $olddir for backup of any existing dotfiles in ~ ..."
	mkdir -p $olddir
	echo "done"

	# change to the dotfiles directory
	echo -n "Changing to the $dir directory ..."
	cd $dir
	echo "done"

	# move any existing dotfiles in homedir to dotfiles_old directory, then create symlinks from the homedir to any files in the ~/dotfiles directory specified in $files
	for file in $files; do
	    echo "Moving any existing dotfiles from ~ to $olddir"
	    mv ~/.$file ~/dotfiles_old/
	    echo "Creating symlink to $file in home directory."
	    ln -s $dir/$file ~/.$file
	done


### Create Git/Github Repository
Initialize a new Git repo

	$ cd ~/dotfiles
	$ git init
	$ git add makesymlinks.sh
	$ git add .bashrc
	$ git commit -m "first commit of dotfiles"

Create a new repository on Github and copy the information from the new repo

	$ git remote add origin git@github.com:mygithubusername/dotfiles.git
	$ git push origin master


### Cloning to Another Machine
Move to home directory of new machine

	$git clone git://github.com/<mygithubusername>/dotfiles.git
	$cd ~/dotfiles

Make script executable

	chmod +x makesymlinks.sh

Run script

	./makesymlinks.sh

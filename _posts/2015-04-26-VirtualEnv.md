---
layout: post
title:  "Python Virtual Environments"
date:   2015-04-26 08:43:59
author: Tara Furstenau
categories: Tutorials
tags:	python
cover:  "assets/instacode.png"
---
Python projects often require a specific package version that may be different than the global package. Virtual environments are helpful in this scenario because they create an isolated Python environment within a directory with its own set of packages. Different environments can be created for specific projects and the dependent modules can easily be listed in a requirements.txt file for project release.

To install virtualenv:

	$ pip install virtualenv

Move to the project directory that you want to isolate within the environment

	$ cd project_folder
	$ virtualenv env_name

Specify which version of Python

	$ virtualenv -p /usr/bin/python2.7 env_name

Activate the environment

	$ source env_name/bin/activate

The name of the environment should now appear to the left of the shell prompt.
While inside the environment any packages that are installed using pip will be placed in the env_name directory.

	(env_name)$ pip install package_name

To leave the virtual environment

	$ deactivate

To delete a virtual environment

	$ rm -rf env_name

From [http://docs.python-guide.org/en/latest/dev/virtualenvs/](http://docs.python-guide.org/en/latest/dev/virtualenvs/)

### Duplicate a virtual environment
Generate a requirements file that contains all of the packages installed on the virtual environment.  

	pip freeze > requirements.txt

This will generate a requirements.txt file.

	Django==1.3
	Fabric==1.0.1
	etc...

You may edit the version number in the file if you need. Copy this file to the directory of the new enviroment then activate the new virtual environment and run

	pip install -r requirements.txt

pip will automatically download and install all the python modules listed in your requirements.txt file, at whatever version you sepcified.

from [http://stackoverflow.com/questions/7438681/duplicate-virtualenv](http://stackoverflow.com/questions/7438681/duplicate-virtualenv)

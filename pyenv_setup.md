 Pyenv is a python environment management tool that allows you to create and manage python installs and virtual environments easily. It allows you to create custom environments that are tailored to each use case required without fighting multiple dependency chains without touching your system python.

 https://github.com/pyenv/pyenv

## Installation

 
Mac:

 ```
 brew update
 brew install pyenv
 brew install pyenv-virtualenv
 ```

We also need to install Python build dependencies

```
brew install openssl readline sqlite3 xz zlib tcl-tk
```


 The Installer will prompt you to add some lines to the end of your shell initialization file in the case of ZSH here are the lines to add to `~/.zshrc`

 ```
export PYENV_ROOT="$HOME/.pyenv"
command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init -)"
if which pyenv-virtualenv-init > /dev/null; then eval "$(pyenv virtualenv-init -)"; fi
```
Note: If you would like pyenv to work with non-interactive shells, you need to add the above lines to either `~/.zprofile` or `~/.zlogin`.



## Quick Start

Pyenv lets you install any number of versions you wish. For instance to install `3.10.11`: run the following.

```
pyenv install 3.10.11
```

If you wish to see available versions to install

```
pyenv install -l
```

To uninstall a python version

```
pyenv uninstall 3.10.11
```

To see the available pyenv environments run

```
pyenv versions
```

When you first install pyenv you will have one environment listed and activated. This is the `system` environment. We typically want to leave this one alone.

What I like to do is install a vanilla python and then build a virtual environment for each given application or context based on the vanilla.

Let's create a virtualenv for the `default` aka `global` python for our account so that we can leave the system one alone.

```
pyenv virtualenv 3.10.11 3.10.11global
```

The above command creates a Python virtual environment, based off of `3.10.11` named `3.10.11global`. I like this naming convention as it is clear which python version the virtual environment is using and the intended use.

To activate a python environment or virtual environment `globally` so that it will be the default one for your account.

```
pyenv global 3.10.11global
```

Now whenever we login and run python we will be using `3.1.11global`.


Pyenv has a very useful concept called `local` python installs that allow you to automatically activate the intended virtualenv whenever you enter a directory (such as a directory containing ansible code)


Let's create a new virtualenv for ansible, activate it locally and then install ansible into it.

First create a new virtualenv
```
pyenv virtualenv 3.10.11 3.10.11ansible
```

Now let's change directory into a directory that we wish to use ansible and then activate the virtualenv we just created `locally`.

```
cd ~/code/<ansible_code_dir>
pyenv local 3.10.11ansible
```

Now whenever we enter `~/code/<ansible_code_dir>` it will automatically activate the `3.10.11ansible` virtualenv.

Note: The way this works is by creating a file in the directory named `.python-version` that has as its content the name of the environment. So you can just create that file manually in leiu of running `pyenv local`

Now we can install ansible using pip into our new environment.

```
python3 -m pip install --user ansible
```
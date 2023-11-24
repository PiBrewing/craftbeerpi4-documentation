# Development

## Running a development version of the server

At least the server and some of the plugins have development branches in the GIT repo that can be installed to test new features or bug fixes. A development branch is primarily used to updated the software and run tests, before it is rolled out to the main branch which is more stable.

To install for instance a development branch of the server, you need to look into the github repo if other branches than the master branch are available. The example below shows you the branches that are currently available for the craftbeerpi4 code in the craftbeerpi organization repo.


In this example the server has for instance a branch called `development`. To install this particular branch, you need to run the following command:

{% hint style="info" %} 
Installation or update of cbpi4 itself does not require to activate the virtual environment as it needs to be installed with pipx
{% endhint %}

```
pipx install https://github.com/PiBrewing/craftbeerpi4/archive/development.zip
```

The only difference to the installation of the master branch is that master.zip is replaced with development.zip as the branch has this name. If you want to upgrade from an existing installation, you should use the `upgrade` option.

```
pipx upgrade https://github.com/PiBrewing/craftbeerpi4/archive/development.zip
```

If you want to install another existing branch, you need to check the available branches and adapt the link accordingly.

To revert back to the master branch, just run the commands for [updating your server](../server-installation.md#updating-the-server).

## Running a development version of the user interface

For the installation of the development branch of the user interface, you need to activate the virtual environment first, as this is a cbpi plugin which needs to be installed inside the cbpi4 virtual environment. 

```
source ~/.local/pipx/venvs/cbpi4/bin/activate
```

Then install the plugin:

```
python -m pip install https://github.com/PiBrewing/craftbeerpi4-ui/archive/development.zip
```

To leave the virtual environment, run:

```
deactivate
```


## Cloning the server to your local drive and install it from there

For development, it can be also important to have the server code on your local hard drive and install it from there. Therefore, you need to clone the repo in a first step:

```
git clone https://github.com/PiBrewing/craftbeerpi4
```

This will pull a local copy of the server software to your hard drive.

Typically, the master branch will be pulled. You can check this with once you navigated into the craftbeerpi4 directory:

```
cd craftbeerpi4
git branch
```

If you want to use for instance the `development` branch, you need to checkout this branch from within the craftbeerpi4 directory:

```
git checkout development
```

Afterwards you can check again which branch is used as describe above.

To install the server now, you have two possibilities. 

1. You can either install it as package.
2. You can install the server for development. This will have the effect, that every change in the code will take effect as soon as you start the server.


Package installation:

```
 pipx install ./craftbeerpi4
```

Development installation:

```
pipx install -e ./craftbeerpi4
```
{% hint style="info" %}
The `-e` option allows you to change the code in the server and it will have a direct effect without the requirement of a new installation of the server. This is useful if you want to develop the server itself.
{% endhint %}

## Setting up a virtual environment for development&#x20;

# This section becomes obsolete with cbpi4 installation via pipx

Development of plugins and server is recommended to be done in a virtual environment ot on a separate system / sd-card. This allows you to create / modify code and test it without the risk to harm your running system.

{% hint style="info" %} 
How to create virtual env in [Python](https://docs.python.org/3/tutorial/venv.html)
{% endhint %}

If you choose to follow the path of the virtual environment, you need to ensure that all relevant python packages for the venv are installed and then you need to run the following commands to activate your venv.

```
python3 -m venv venv
source venv/bin/activate
```

Now you can either install the server as described in this manual inside you virtual environment or you can clone the server to your local system and install it from there.

{% hint style="info" %} 
Inside your virtual environment you need to  install the server or anything else without sudo!
{% endhint %}


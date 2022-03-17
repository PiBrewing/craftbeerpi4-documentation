# Development

## Running a development version of the server

At least the server and some of the plugins have development branches in the GIT repo that can be installed to test new features or bug fixes. A development branch is primarily used to updated the software and run tests, before it is rolled out to the main branch which is more stable.

To install for instance a development branch of the server, you need to look into the github repo if other branches than the master branch are available. The example below shows you the branches that are currently avaialble for the cratbeerpi4 code in the craftbeerpi organization repo.

![Install other branch](../../.gitbook/assets/cbpi4-de-installbranch.png)



to run the following command:

```
sudo pip3 install https://github.com/avollkopf/craftbeerpi4/archive/development.zip
```

The only difference to the installation of the master branch is that main.zip is replaced with development.zip. If you want to upgrade from an existing installation, you should add the flag `--upgrade`.

```
sudo pip3 install --upgrade https://github.com/avollkopf/craftbeerpi4/archive/development.zip
```

To revert back to the master branch, just run the commands for [updating your server](server-installation.md#updating-the-server).

## Cloning the server to your local drive and install it from there

For development, it can be also important to have the server code on your local harddrive and install it from there. Therefore, you need to clone the repo in a first step:

```
git clone https://github.com/avollkopf/craftbeerpi4
```

This will pull a local copy of the server software to your harddrive.

Typically, the master branch will be pulled. You can check this with once you navigated into the craftbeerpi4 directory:

```
cd craftbeerpi4
git branch
```

If you want to use the development branch, you need to checkout this branch from within the craftbeerp4 directory:

```
git checkout development
```

Afterwards you can check again which branch is used as describe above.

To install the server now, you have two possibilities. 

1. You can either install it as package.
2. You can install the server for development. This will have the effect, that every change in the code will take effect as soon as you start the server.

Package installation:

```
sudo pip3 install ./craftbeerpi4
```

Development installation:

```
sudo pip3 install -e ./craftbeerpi4
```


## Setting up a virtual environment for development&#x20;

Development of plugins and server is recommende to be done in a virtual environemnt ot on a separate system / sd-card. This allows you to create / modify code and test it without the risk to harm your running system.

{% hint style="info" %} 
How to create virtual env in [Python](https://docs.python.org/3/tutorial/venv.html)
{% endhint %}

If you choose to follow the path of the virtual enviroment, you need to ensure that all relevant python packages for the venv are installen and then you need to run the following commands to activate your venv.

```
python3 -m venv venv
source venv/bin/activate
```

Now you can either install the server as described in this manual inside you virtual environment or you can clone the server to your local system and install it from there.

{% hint style="info" %} 
Inside your virtual environment you need to  install the server or anything else without sudo!
{% endhint %}


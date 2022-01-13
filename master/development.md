# Development

## Running a development version of the server

At least the server and some of the plugins have currently development branches in the GIT repo that can be installed to test new features or bug fixes. The development branch is primarily used to updated the software and run tests, before it is rolled out to the main branch which is more stable.

To install for instance the development branch of the server, you need to run the following command:

```
sudo pip3 install https://github.com/avollkopf/craftbeerpi4/archive/development.zip
```

The only difference to the installation of the master branch is that main.zip is replaced with development.zip. If you want to upgrade from an existing installation, you should add the flag `--upgrade`.

```
sudo pip3 install --upgrade https://github.com/avollkopf/craftbeerpi4/archive/development.zip
```

To revert back to the master branch, just run the commands for [updating your server](server-installation.md#updating-the-server).

## Setting up a development environment&#x20;

Under Construction

## Creating new Plugins

&#x20;Under Construction

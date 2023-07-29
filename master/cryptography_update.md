With cbpi4 4.1.7, the dependency to cryptography has changed to version 40.0.1. due to a security issue with older versions of this package.

In case you are experiencing problems with pip after the upgrade and see this error (`AttributeError: module 'lib' has no attribute 'X509_V_FLAG_CB_ISSUER_CHECK'`) when using pip, you should be able to fix the issues with the following commands:

```
sudo apt remove python3-pip 
wget https://bootstrap.pypa.io/get-pip.py
sudo python3 get-pip.py
sudo pip3 install pyopenssl==23.1.0 --upgrade
```

Some more information is posted [here](https://stackoverflow.com/questions/73830524/attributeerror-module-lib-has-no-attribute-x509-v-flag-cb-issuer-check)

### Arm6 based systems (e.g. Pi Zero W) my end up with `Illegal instruction`

Newer versions of Cryptography may cause issues with Arm6 based devices. This is not an issue with cbpi itself, but the cryptography dependency. The latest version of cbpi4 that is using older versions of cryptography is 4.1.6. 

Users can try to change the dependency manually, but in future, other packages may also depend on newer versions of cryptography. Hence, it is strongly recommended to migrate to newer devices such as the Pi Zero 2 which should not have this issue as it will be able to run also 64 bit based OS.

- To use the older version of cryptography, you can ry the following:

`git clone https://github.com/PiBrewing/craftbeerpi4`

then navigate into the directory

`cd craftbeerpi4`

Afterward, you can checkout the branch you want:

`git checkout development` to checkout the development branch

then you can edit the setup.py file and change

"cryptography==40.0.1" to "cryptography==36.0.1"

navigate on directory up:

`cd ..`

and install cbpi directly from the path:

`sudo pip install ./craftbeerpi4`

The other option is to revert back to 4.1.6.

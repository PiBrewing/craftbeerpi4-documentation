With cbpi4 4.1.7, the dependency to cryptography has changed to version 40.0.0. due to a security issue with older versions of this package.

In case you are experiencing problems with pip after the upgrade and see this error (`AttributeError: module 'lib' has no attribute 'X509_V_FLAG_CB_ISSUER_CHECK'`) when using pip, you should be able to fix the issues with the following commands:

```
sudo apt remove python3-pip 
wget https://bootstrap.pypa.io/get-pip.py
sudo python3 get-pip.py
sudo pip3 install pyopenssl --upgrade
```

Some more information is posted [here](https://stackoverflow.com/questions/73830524/attributeerror-module-lib-has-no-attribute-x509-v-flag-cb-issuer-check)

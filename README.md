# Build bitcoind on Ubuntu 14.10 on Azure

Motivation: To create comprehensive instructions for anyone to follow to allow users to deploy a full bitcoin node using bitcoind, permanlty on using Microsoft's Azure.

##Requirements
1. Secondary drive large enough for the block chain growth
2. No wallet
3. No UI
4. Allow to be discovered by opening firewall
5. Good uptime

##Create an Ubuntu VM on Azure

From the Microsoft Azure portal https://manage.windowsazure.com.

1. Select Virtual Machines -> New -> Create Quick.
2. Give the node a DNS name
3. Select Ubuntu 14.10 from the images drop down
4. Set a password
5. Select your region
6. Attach a second 120GB empty disk (to be used for the block chain)
7. The size should be set to around A2 while the code is built, then scaled back to A0 after.

![Step1](https://github.com/evapeak/bitcoind/blob/master/azure1.png)

User is: azureuser
Password is: xxx

Add port 8333 to the endpoint list.

SSH into the VM, using ssh azureuser@yournode.cloudapp.net

###Pre Reqs
```
sudo apt-get update
sudo apt-get install python-software-properties
sudo apt-get install build-essential libboost-all-dev automake libtool autoconf
sudo apt-get install libdb++-dev
sudo apt-get install libboost-all-dev
sudo apt-get install pkg-config
sudo apt-get install libssl-dev
sudo apt-get install libqt5gui5 libqt5core5a libqt5dbus5 qttools5-dev qttools5-dev-tools libprotobuf-dev protobuf-compiler
sudo apt-get install git
```

###Mount data drive
Format the data drive using ext3 and mount
```
sudo mkdir /media/data
```

###Build bitcoind
Now we will get bitcoin source from github (bitcoin brisbane fork) and compile.  Feel free to use the bitcoin/bitcoin repository if you like.
```
sudo git clone https://github.com/bitcoin/bitcoin
cd bitcoin
sudo git checkout 0.10
sudo ./autogen.sh
sudo ./configure --with-cli=no --with-gui=no --disable-wallet
sudo make 
sudo make install
```

Note, if there is an error on autogen.sh, re install the pre reqs.

###Move blocks
```
sudo mv ~/.bitcoin/blocks /media/data/
sudo ln -s /media/data/blocks blocks
```

###Configure
Last step we need to add the bitcoin config file.
```
cd ~/.bitcoin
sudo nano bitcoin.conf
```

Add the following:
```
server=0
daemon=1
rpcuser=username
rpcpassword=xx
```

###Done
The daemon should now be ready to start.  Simply type bitcoind

###References
https://help.ubuntu.com/community/InstallingANewHardDrive

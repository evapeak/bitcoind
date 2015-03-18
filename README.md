# Build bitcoind on Ubuntu 14.10 on Azure
=========

##Create an Ubuntu VM on Azure

User is: azureuser
Password is: xxx

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
Find the usb flash drive by using sudo fdisk sdc -l
```
sudo mkdir /media/external
pmount /dev/sda external
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
sudo mv ~/.bitcoin/blocks /media/external/
sudo mv ~/.bitcoin/chainstate /media/external/
sudo ln -s /media/external/blocks blocks
sudo ln -s /media/external/chainstate chainstate
```

###Configure
Last step we need to add the bitcoin config file.
```
cd ~/.bitcoin
sudo nano bitcoin.conf
```

Add the following:
```
server=1
daemon=1
rpcuser=evapeak
rpcpassword=xx
```

**Notes**
```
chmod u+rwx backup
./backup
```

###Done
The daemon should now be ready to start.  Simply type bitcoind

###Todo
Startup scripts

**Refrences**
http://elinux.org/Beagleboard:Ubuntu_On_BeagleBone_Black
https://bitcointalk.org/index.php?topic=304389.0

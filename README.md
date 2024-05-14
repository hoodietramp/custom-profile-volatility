# custom-profile-volatility

### Setup volatility 2

#### install pip for python2
```
sudo apt install -y python2 python2.7-dev libpython2-dev
curl https://bootstrap.pypa.io/pip/2.7/get-pip.py --output get-pip.py
sudo python2 get-pip.py
sudo python2 -m pip install -U setuptools wheel
```

#### met vol2 requirements
```
python2 -m pip install -U distorm3 yara pycrypto pillow openpyxl ujson pytz ipython capstone
sudo python2 -m pip install yara
sudo ln -s /usr/local/lib/python2.7/dist-packages/usr/lib/libyara.so /usr/lib/libyara.so
```

#### clone volatility github repo
```
git clone https://github.com/volatilityfoundation/volatility.git
cd volatility
sudo python2 setup.up build
sudo python3 setup.py install
```

- setup an alias for vol2
```
alias vol2='python2 /opt/volatility/vol.py'
```

### create custom linux profile for vol2

#### docker > 
```
sudo docker run -it --rm -v $PWD:/volatility ubuntu:20.04 /bin/bash
```

-it : run the container and drop us in interactive session of `/bin/bash` shell <br>
--rm : clean up the container and remove the file system when the container exits <br>
-v : verbose <br>
\$PWD:/volatility : mount the `/volatility` directory present in current working directory of user in the docker container <br>

#### install dependencies
```
apt update && apt install -y linux-image-5.4.0-166-generic linux-headers-5.4.0-166-generic dwardump make gcc zip
```

#### create custom profile
```
cd volatility/tools/linux
sed -i 's/$(shell uname -r)/5.4.0-166-generic/g' Makefile
make
zip ubuntu20.04.zip module.dwarf /boot/System.map-5.4.0-166-generic
```

- this will make a custom linux profile for ubuntu 20.04 and kernel version 5.4.0-166-generic

-------
### without docker, ubuntu vm >

> download ubuntu 20.04 iso from here - [ubuntu20.04](https://releases.ubuntu.com/focal/)

```
sudo apt update && sudo apt install linux-image-5.4.0-166-generic linux-headers-5.4.0-166-generic dwarfdump gcc zip
```

-- follow above steps to create custom profile
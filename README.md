### CentOS 9 Stream Setup

Setup new linux computer with CentOS 9 stream


#### install nvidia-driver
Follow [this link](https://linuxhint.com/install-nvidia-drivers-centos/) to install nvidia driver on CentOS 9 stream


#### install cuda toolkit 11.7
Follow [this link](https://www.server-world.info/en/note?os=CentOS_Stream_9&p=nvidia&f=4) to install cuda toolkit 11.7 on CentOS 9 stream
```
https://www.server-world.info/en/note?os=CentOS_Stream_9&p=nvidia&f=4
```

#### install `mpich` from source
The `mpich` installed from CentOS 9 Stream does not include mpicxx, mpicc, as such, we install `mpich` from source.
- Download `mpich3.4.3` source from [this link](https://www.mpich.org/static/downloads/3.4.3/).
```
https://www.mpich.org/static/downloads/3.4.3/
```

1. Create a folder called `build` inside the source file folder of `mpich`.
2. Go into the `build` folder, and use the following command to configure the installation:
```
./../configure --with-device=ch4:ofi FFLAGS=-fallow-argument-mismatch |& tee c.txt
```
If the above command does not work, then use the following command:
```
../configure --with-device=ch4:ofi FFLAGS=-fallow-argument-mismatch |& tee c.txt
```
Options `--with-device` and `FFLAGS` above are used to specify communication device and gfortran flags.
- If the `configure` command is not executed successfully, then stop, do not proceed, and fix the issue with the execution of the above `configure` command.
- If gfortran is missing, then it can be installed below before invoking the above command line:
```
sudo dnf install libgfortran
sudo dnf install gfortran
```
3. After the `configure` command has been executted successfully, then compile `mpich` with the following commands:
```
make |& tee m.txt
```
4. After successful `make` command, install with (using sudo since install to default /usr/local/bin directory):

```
sudo make install |& tee mi.txt
```

#### Install Cmake 3.25.3
To install `Cmake` from source:
- Download `cmake 3.25.3` source from [this link](https://cmake.org/files/v3.25/cmake-3.25.3.tar.gz).
```
   https: https://cmake.org/files/v3.25/cmake-3.25.3.tar.gz
```
- Inside the untared `cmake` folder, execute the following command as suggested in `README.rst` file:
```
  ./bootstrap && make && sudo make install
```



#### Change default core dump file pattern
With CentOS 9 stream, the default core dump file location is not the current working directory. This can be
 verified by the following command:
```
cat /proc/sys/kernel/core_pattern
```
After that, its location can be changed to current working directory with:
```
sudo sysctl -w kernel.core_pattern="core_%e.%p"
```

#### Mount new hard drives
Refer to the following posts on how to do this properly:
- https://akrabat.com/setting-up-a-new-hard-drive-in-linux/
- https://www.adamsdesk.com/posts/add-disk-drive-linux/

#### Mount OneDrive
- Look for [this link for steps](https://kb.uconn.edu/space/IKB/26050527301/Setting+up+OneDrive+on+Linux) to mount OneDrive on CentOS 9


- If after computer restart, the hard drive is not visible, then the following command can be used:
```
rclone --vfs-cache-mode writes mount OneDrive: ~/OneDrive &
```

#### Caching your GitHub credentials in Git
Look for below posts on how to cache your github creditials for `pull` and `push` operations:
- https://stackoverflow.com/questions/5343068/is-there-a-way-to-cache-https-credentials-for-pushing-commits
- https://docs.github.com/en/get-started/getting-started-with-git/caching-your-github-credentials-in-git

### CentOS 9 Stream Setup

Setup new linux computer with CentOS 9 stream


#### install nvidia-driver
Follow [this link](https://linuxhint.com/install-nvidia-drivers-centos/) to install nvidia driver on CentOS 9 stream


#### install cuda toolkit 11.7
Follow [this link](https://www.server-world.info/en/note?os=CentOS_Stream_9&p=nvidia&f=4) to install cuda toolkit 11.7 on CentOS 9 stream


#### install `mpich` from CentOS 9 repository (not recommended)

To install mpich `3.4.2` on CentOS 9 Stream, do the following
```
sudo dnf install mpich
```
If we want to use a specific mpich version, we can check available version using:
```
sudo dnf list mpich --showduplicates
```
The results look like below:
```
CentOS Stream 9 - BaseOS                                                                                                                                                            16 kB/s | 6.4 kB     00:00    
CentOS Stream 9 - AppStream                                                                                                                                                         29 kB/s | 6.5 kB     00:00    
CentOS Stream 9 - CRB                                                                                                                                                               35 kB/s | 6.3 kB     00:00    
CentOS Stream 9 - Extras packages                                                                                                                                                   33 kB/s | 7.3 kB     00:00    
Installed Packages
mpich.x86_64                                                                                         3.4.2-1.el9                                                                                         @appstream
Available Packages
mpich.i686                                                                                           3.4.1-4.el9                                                                                         crb       
mpich.x86_64                                                                                         3.4.1-4.el9                                                                                         appstream 
mpich.i686                                                                                           3.4.2-1.el9                                                                                         crb       
mpich.x86_64                                                                                         3.4.2-1.el9                                                                                         appstream 
mpich.i686                                                                                           4.1.1-1.el9                                                                                         crb       
mpich.x86_64                                                                                         4.1.1-1.el9    
```
After that, if we want to install a specific version of mpich, for example 3.4.2 version, we can do the following:
```
sudo dnf downgrade mpich-3.4.2

```
or, install older version directly with
```
sudo dnf install mpich-3.4.2
```

#### install `mpich` from source
The `mpich` installed from CentOS 9 Stream does not include mpicxx, mpicc, as such, we install `mpich` from source.
- Download `mpich` source from [this link](https://www.mpich.org/static/downloads/3.4.3/).

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

#### Mount OneDrive
- Look for [this link for steps](https://kb.uconn.edu/space/IKB/26050527301/Setting+up+OneDrive+on+Linux) to mount OneDrive on CentOS 9


- If after computer restart, the hard drive is not visible, then the following command can be used:
```
rclone --vfs-cache-mode writes mount OneDrive: ~/OneDrive &
```

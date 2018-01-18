# test-tld-resolver

Simple top level domain (NSSwitch hosts) resolver for linux based development environment.

# Supported Linux Distributions

This tool is developed and tested in an Ubuntu 17.04 system. Even though not tested, it should also work in other Linux distributions that supports/uses [nsswitch.conf](http://man7.org/linux/man-pages/man5/nsswitch.conf.5.html) file based configuration. In general, if your Linux installation has a file named **nsswitch.conf** in **/etc/** folder, then you should be ready to go with using this tool.

# Installation

## Ubuntu

- Make sure you have "build-essential" & "git" packages installed. You can install these using following command
```bash
sudo apt-get install build-essentials git
```
- Clone this repository somewhere in your hard drive
```bash
git clone https://github.com/geofflane/dev-tld-resolver.git
```
-  Run following command to build & install the tool
```
cd dev-tld-resolver/src && make
sudo make install
```

> It's very important that you run the ```make install``` command as **root** or using **sudo**, otherwise installation will fail.

- As root (or in super user mode with sudo) also edit **/etc/environment** using a text editor of your choice and export a global environment variable named **TEST_TLD_DOMAINS** with comma separated list of Top Level Domains (**tld**) that you want to resolve to ```127.0.0.1``` automatically. For example, if you want ```.test```, ```.wp```, ```.dpl``` top level domains to be resolved by **test-tld-resolver**, your **/etc/environment** should have following line within it somewhere.
```
TEST_TLD_DOMAINS=test,wp,dpl
```

> Above step is optional if you don't need **test-tld-resolver** to resolve top level domains other than ```.test```, which is the default

- Lastly as root (or in super user mode with sudo) edit ```/etc/nsswitch.conf``` file and append ```test_tld``` to the line starting with ```hosts:```. 

> If you have following line starting with ```hosts:``` in ```/etc/nsswitch.conf``` file
> ```
> hosts: files mdns4_minimal [NOTFOUND=return] dns mdns4
> ```
> then you should change it to look like
> ```
> hosts: files mdns4_minimal [NOTFOUND=return] dns mdns4 test_tld
> ```
> If you experience some sort of delay while resolving a host name, you should try to move ```test_tld``` before dns, as follows
> ```
> hosts: files mdns4_minimal [NOTFOUND=return] test_tld dns mdns4
> ```

- Now logout or reboot your system and login again. After logging in into the system, open a command line and type following command
```
ping test.test
```
If ping is successful, then **test-tld-resolver** is installed & configured correctly.

# Credits

Code is borrowed & modified from prax by ysbaddaden at https://github.com/ysbaddaden/prax

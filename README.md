# distroot

A Distributed Root for C and Assembly development on Linux

The distroot project is my attempt to learn about chroot jails in Linux. It is also a way to distribute small development environments.
I will explain more later about the motivation for this project.

The first step is to create a folder which will be the base of the new minimal operating system. In this case, it will be a folder called ".distroot" short for distribution root.


```
mkdir .distroot
cd .distroot
```

Once in this directory/folder, I can try to enter is as a chroot environment.

`sudo chroot .`

But when I do, it gives me the error: "chroot: failed to run command ‘/bin/bash’: No such file or directory". This helpful information tells me that I need to copy the bash program into the new .distroot folder.

mkdir ./bin
cp /bin/bash ./bin/bash

However, when I do, it still gives me the error message.


Alternative Jail

```
PATH=
echo $PATH
```

# distroot

A Distributed Root for C and Assembly development on Linux

The distroot project is my attempt to learn about chroot jails in Linux. It is also a way to distribute small development environments.
I will explain more later about the motivation for this project.

The first step is to create a folder which will be the base of the new minimal operating system. In this case, it will be a folder called ".distroot" short for distribution root.

# Step 1: Getting a Minimal Bash Shell


```
mkdir .distroot
cd .distroot
```

Once in this directory/folder, I can try to enter is as a chroot environment.

`sudo chroot .`

But when I do, it gives me the error: "chroot: failed to run command ‘/bin/bash’: No such file or directory". This helpful information tells me that I need to copy the bash program into the new .distroot folder.

```
mkdir ./bin
cp /bin/bash ./bin/bash
```

However, when I do, it still gives me the same error message. This indicated that there are dependency libraries that bash needs but can't tell me what they are. The following command can reveal them.

`ldd ./bin/bash`

The output of this program is:

```
	linux-vdso.so.1 (0x00007ffd5352d000)
	libtinfo.so.6 => /lib/x86_64-linux-gnu/libtinfo.so.6 (0x00007fa5871af000)
	libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007fa586fcd000)
	/lib64/ld-linux-x86-64.so.2 (0x00007fa587348000)
```

Which means that I need to copy these to a new directory by the same name and copy these files to it. However, I read that "linux-vdso.so.1" doesn't actually exist as a file and is just something that the kernel loads.

```
mkdir -p ./lib/x86_64-linux-gnu

cp /lib/x86_64-linux-gnu/libtinfo.so.6 ./lib/x86_64-linux-gnu/libtinfo.so.6

cp /lib/x86_64-linux-gnu/libc.so.6 ./lib/x86_64-linux-gnu/libc.so.6

cp /lib/x86_64-linux-gnu/ld-linux-x86-64.so.2 ./lib/x86_64-linux-gnu/ld-linux-x86-64.so.2
```

It is also necessary to create a soft link to the "ld-linux-x86-64.so.2" file inside a folder called lib64.

```
mkdir ./lib64

ln  -rs ./lib/x86_64-linux-gnu/ld-linux-x86-64.so.2 ./lib64/ld-linux-x86-64.so.2
```

The "-rs" means make a soft link inside lib64 directory that is inside the .distroot directory that points to the one in ".distroot/lib/x86_64-linux-gnu/"

Note that the command doesn't actually include ".distroot" because it was merely the location I ran the command from. Therefore "." refers to ".distroot" or wherever I run the commands from.

Functionally, it is the same as if I had just made an extra copy of the file as the command below:

```
cp /lib/x86_64-linux-gnu/ld-linux-x86-64.so.2 ./lib64/ld-linux-x86-64.so.2
```

But making extra copies would be wasteful compared to making a link saves space.

Now that all that is done, I ran the chroot command again and it worked!

`sudo chroot .`

This put me inside a bash shell. This is not much but it is a working shell from which to try other commands.



# Alternative Jail

```
PATH=
echo $PATH
```

I want to introduce some flow which can help to create and use external packages without install them in your target image.

This is very useful in case you don't want to change you image, or to increase its size.
The system build I work with is Yocto - Zeus distribution.
Ubuntu 20.04.6

The demonstration is with "valgrind" package(can change with any other package).
*Note: install the next package in your host by exectue the next command:
$ sudo apt-get install rpm2cpio

After the environment is ready follow the next steps:

1. $ bitbake valgrind
2. $ find . -iname "valgrind*rpm", in my case I found it in the next path: 
	"./build/tmp/deploy/rpm/aarch64/valgrind-3.15.0-r0.aarch64.rpm"
3. $ rpm2cpio "./build/tmp/deploy/rpm/aarch64/valgrind-3.15.0-r0.aarch64.rpm" > valgrind.cpio
4. Create directory which hold the extracted files from the valgrind.cpio, by execute the next command:
	$ mkdir valgrind-package && cd valgrind-package
5. Extract the cpio files into the directory by execute the next command:
	$ cpio -idmv < ../valgrind.cpio
6. Create tar file from the extracted files by execute the next command:
	$ tar czf valgrind.tar ./*
7. Copy the file into your root directory in your target by execute the next command:
	$ scp valgrind.tar username@w.x.y.z:/
8. Inside your target, go the root directory and untar it by exectue the next command:
	$ tar xvf valgrind.tar
	
Congrats, now you can successfully use "valgrind"

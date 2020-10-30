To build qorc-sdk-user run the build from the dockerfiles subdirectory:
```
cd dockerfiles
docker build . -t qorc-sdk-user
```
The above assumes Docker can find an image called qorc-sdk:latest.  

You can base the build on a particular image using the --build-arg PARENT_IMAGE param:
```
docker build --build-arg PARENT_IMAGE="docker.pkg.github.com/thirsty2/qorc-sdk/qorc-sdk:latest" . -t qorc-sdk-user
```
When you run this container, the entrypoint script activates conda and runs 
an optional command inside the container.

The easiest way to test this is to use the samples in the qorc-sdk repo, 
which you can check out from here:

https://github.com/QuickLogic-Corp/qorc-sdk

Be sure to get the submodules.  An easy way to get everything is:
git clone --recursive https://github.com/QuickLogic-Corp/qorc-sdk.git

Try this in the qorc-sdk directory:
```
~/qorc-sdk$ docker run -it --rm -v $(pwd):/home/ic/work qorc-sdk-user bash
```
You will be at a bash prompt inside the qorc-sdk-user container.
Change directories into work/qf_apps and run make as a test:
```
ic@822f51f37df1:~$ ls
work
ic@822f51f37df1:~$ cd work
ic@822f51f37df1:~/work$ ls
BSP  FreeRTOS  HAL  LICENSE  Libraries  README.md  Tasks  Tools  ci  dockerfiles  docs  freertos_gateware  gateware  qf_apps  qf_vr_apps  qorc-example-apps  qorc-testapps  s3-gateware  tensorflow
ic@822f51f37df1:~/work$ cd qf_apps
ic@822f51f37df1:~/work/qf_apps$ ls
CreateNewProject.txt  makefile         qf_bootloader       qf_fpgauart_app   qf_helloworldhw  qf_loadflash       qf_mqttsn_ai_app
create_newapp.py      qf_advancedfpga  qf_bootloader_uart  qf_gwtestharness  qf_helloworldsw  qf_loadflash_uart  quickfeather-initial-binaries
ic@822f51f37df1:~/work/qf_apps$ make

```
The build may take a while.  Each sample project contains an output/bin directory with the results of the build:
```
ic@822f51f37df1:~/work/qf_apps/qf_helloworldhw/GCC_Project/output/bin$ ls
qf_helloworldhw.bin  qf_helloworldhw.elf  qf_helloworldhw.map

```


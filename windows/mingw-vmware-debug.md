#download mingw

https://osdn.net/projects/mingw/releases/

mingw-get-setup.exe

是个minggw installation manager,下载组件的
#install mingw

安装完打开，
all package
--mingw
----mingw base system

左侧列表选 mingw32-gcc-bin,mingw32-gcc-g++-bin,mingw32-gdb-bin,右键，mark for installation

选择 菜单installation--apply changes

C:\MinGW\bin 添加到环境变量path

命令行测试

    gcc -v
    g++ -v
    gdb -v
#gdb debug vmware
*.vmx

    debugStub.listen.guest32 = "TRUE"
    debugStub.listen.guest64 = "TRUE"
    monitor.debugOnStartGuest32 = "TRUE"
开启vmware虚拟机，会中断，等待gdb连接调试

命令行启动gdb

    gdb -q
    target remote :8832
    b *0x7c00
    c
    ...其他gdb命令
    
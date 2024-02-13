# COMpipe
Links a serial COM port to a named pipe.  This command-line utility was developed to allow Hyper-V virtual machines to use the host machine's COM ports. Hyper-V provides a named pipe that maps into the VM as a COM port. It does not connect that named pipe to the host's COM port, which is what you would need to pass the data through to your VM.  This utility currently runs in the console on Windows only, and has been tested on Windows 8 Pro, 10 Pro, and 11 Pro.  If you need to run this as a Windows service, you can use something like [NSSM](https://nssm.cc/), which has been used with COMpipe successfully.

A compiled executable can be found in x64\Release.

Feel free to modify the source code and use as desired.  It was compiled using Visual Studio 2022.  No additional libraries are needed.

*Update*
Version 0.2 (20-July-2023)
Added automatic retries and exponential backoff for when a connection to a named pipe or serial port is temporarily lost.  This can happen when rebooting the virtual machine, for example.  Also, other miscellaneous updates.

Version 0.3 (13.02.2024)
added cygwin g++ compilation, exe=pipe.exe, added -z, -s, -y options

to compile in cygwin, add g++, gcc, libncurses devel in cygwin, run in the directory g++ -c main.cpp and thne g++ -o pipe main.o
the binary in x64\release doesn't include these options. To run as a service : cygrunsrv

```
Usage:
  COMpipe [-b <baud rate>]  [-z <byte size>] [-s <stop bit>] [-y <parity>] -c <COM port name> -p <pipe name>
  Examples:
    COMpipe -c \\.\COM8 -p \\.\pipe\MyLittlePipe
    COMpipe -b 19200 -c \\.\COM8 -p \\.\pipe\MyLittlePipe
  Notes:
  1. COMpipe does not create a named pipe, it only uses an existing named pipe.
  2. COMpipe must be run as administrator.
  3. The default baud rate is 9600.
  Options include: 4800, 9600, 14400, 19200, 38400, 57600, and 115200.
  4. byte size = 8,7. default 8.
  5. stop bit : 1 or 2, default 1.
  6. parity : NONE,ODD,EVEN default NONE
  To run in Cygwin (as admin):
  pipe.exe -b 4800 -p ODD -c COM7 //./pipe/mypipe
```

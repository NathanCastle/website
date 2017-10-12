# Building the Windows Research Kernel (NT 5.2) on Windows 10

> Originally published on 10/21/2016

Having recently taken an interest in operating systems, I decided I wanted to take a look at the Windows NT Kernel. Fortunately, Microsoft made the Windows Research Kernel available with support for Windows Server 2003 about ten years ago. A lot can change in 10 years, and some of the assumptions made by the documentation included with the project are no longer valid.

I'm writing this blog post to hopefully make the process easier for anyone who wants to get started with building the Windows Research Kernel on modern devices.
Background

The Windows Research Kernel was made available to qualified academic institutions and consisted of:

* Build tools for the Windows NT 5.2 Kernel
* HAL binaries for various hardware configurations (no source available)
* Much of the Windows NT kernel source - does not include NTFS, plug-and-play or power management
* Original NT documentation
* A preconfigured Virtual PC environment

The Windows Research Kernel is compatible with Windows Server 2003 with SP1 and SP2.

## Building the WRK

The WRK (v1.2) has the following structure on disk:

* BASE - contains the NT Kernel
   * NTOS - root for the kernel build process
* PUBLIC - various headers needed for building the kernel - the things that we don't have the source for are supplied as compiled binaries
* TOOLS - build tools for x86 and amd64 systems
* WS03SP1HALS - Hardware Abstraction Layer binaries that are binary compatible with the Server 2003 kernel and support various hardware configurations.

The first part of the build process is to have the WRK-V1.2 directory in a convenient, writable place on disk. The documentation has it in C:\ and I'll be maintaining that convention.

The WRK documentation has instructions for building the kernel, which I've modified slightly for consistency and reproduced:

```
set arch=x86
path=C:\WRK-V1.2\TOOLS\%arch%;%path%
cd C:\WRK-V1.2\BASE\NTOS
nmake -nologo %arch%=
```

All required build tools ship with the WRK, so if you get errors like:

```
'nmake' is not recognized as an internal or external command, operable program or batch file.
```

Then you have probably made an error in setting your path.

Important: Changing the path & arch as specified above is a temporary operation - you need to set it each time you build from a new command prompt window.

If you get an error like:

```
NMAKE : fatal error U1077: 'checktoolarch' : return code '0x1'
```

Then you have a mismatch between the tools in your path and in the nmake command. This happens when following the original instructions which fail to use the defined %arch% environment variable.

At this point you will likely get other errors on Windows 10. See below for the appropriate solution.

## Building on Windows 10 - 32 Bit

Ensure that arch is set properly to x86 or amd64 as above. You can verify by typing %arch% and hitting enter. You will see:

```
'x86' is not recognized as an internal or external command, operable program or batch file.
```

Within the C:\WRK-V1.2\BASE\NTOS directory, run:

```
nmake -nologo %arch%=
```

as specified above. You will get an error about missing MSVCR71.dll. This is because Windows no longer ships with this component - applications that rely on it were supposed to include it, but the WRK build tools don't. There is also a dependency on MSVCP71.dll that nmake won't tell you about - it will silently fail if MSVCR71 but not MSCVP71 is present.

The solution is to download or acquire the DLLs from a trusted source - possibly another Windows installation. Dll-files.com is referenced quite frequently online, but use at your own risk.

Once the files are acquired, copy them into:

```
C:\Windows\System32
```

## Building on Windows 10 - 64 Bit

On 64-Bit editions of Windows 10, you can choose whether to build for x86 or amd64.

When building for amd64, nmake needs *only* MSVCR71.dll added. Placing this in C:\Windows\System32 and running:

```
set arch = amd64
```

will allow you to build the kernel with:

```
nmake -nologo %arch%=
```

Building the 32-bit version requires a trick: instead of placing MSVCR71.dll and MSVCP71.dll in C:\SWindows\System32, place the files in:

```
C:\Windows\SysWow64
```

WOW64 is the compatibility layer for running x86 applications on the amd64 architecture. When compiling the x86 version of WRK, the build tools thus look in SysWow64 for the relevant dlls.

## Notes & Usage

The compiled binaries are placed in C:\WRK-V1.2\BASE\NTOS\BUILD\EXE. To use this kernel in production, copy the files to C:\Windows\System32 on a machine running Windows Server 2003 SP1 or SP2.

Don't forget to update C:\boot.ini to enable booting the kernel. Add the following lines, being sure to leave the existing entry intact:

```
multi(0)disk(0)rdisk(0)partition(1)\WINDOWS="Windows Server 2003, WRK" /noexecute=optout /fastdetect /debug /debugport=com1 /kernel=wrkx86.exe /hal=halacpim.dll

multi(0)disk(0)rdisk(0)partition(1)\WINDOWS="Windows Server 2003, WRK - no debug" /noexecute=optout /fastdetect /kernel=wrkx86.exe /hal=halacpim.dll
```

These instructions work for both x86 and amd64 build devices and targets.

If your device bluescreens during boot, check that you are in fact running Windows Server 2003 SP1. WRK is not compatible with versions prior to SP1.

Do note that the WRK kernel is only compatible with a small set of Windows operating systems. You'll notice that boot.ini is a bit of a throwback - it was used by NTLDR. NTLDR was replaced by the more robust Windows Boot Manager in Windows Vista. As such, even if the WRK were to be made compatible with a later version of Windows, configuring the Windows Boot Manager to recognize it would be a project.

## Finding Windows Server 2003

Interestingly enough, it has become quite difficult to acquire a copy of Windows Server 2003 SP1 since support has discontinued. Having checked MSDNAA & my school's web store, I couldn't find anything before 2008. My school's IT department was able to provide an iso, but not a license.

Having found the installation media, finding a valid license is the next challenge. Without a valid license, things like installing SP1 won't work. I was not able to put my hands on a valid key, but I was fortunately able to use another resource.

The Curriculum Research Kit, which shipped with the Windows Research Kernel, includes a Virtual PC 2007 image set up for use with the WRK. I was able to plug the .vhd into VirtualBox and use that. There are some hardware incompatibilities/changes, so Windows will demand a reactivation. I was able to reactivate by phone, but once again, YMMV.
Going Forward

The experience of compiling and running the NT kernel was interesting but ultimately anticlimactic. Soon, I'd like to start implementing new features or experimenting. A first step will be setting up a more robust environment, including configuring VirtualBox to work with debugging (and hopefully getting it to talk to a vm in parallels). Another step might include setting up a Visual Studio project to handle the build & project management more reliably.

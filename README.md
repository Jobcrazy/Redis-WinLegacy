## Redis on Legacy Windows 

- This is a modified version of [Redis](https://github.com/microsoftarchive/redis) for Windows XP and above.
- Redis can be installed as a Windows Service.

## Windows-specific changes
- There is a replacement for the UNIX fork() API that simulates the copy-on-write behavior using a memory mapped file on 2.8. Version 3.0 is using a similar behavior but dropped the memory mapped file in favor of the system paging file.
- In 3.0 we switch the default memory allocator from dlmalloc to jemalloc that is supposed to do a better job at managing the heap fragmentation.
- Because Redis makes some assumptions about the values of file descriptors, we have built a virtual file descriptor mapping layer. 

## Redis release notes
- Redis on UNIX [3.0 release notes](https://raw.githubusercontent.com/antirez/redis/3.0/00-RELEASENOTES)
- Redis on Windows [3.0 release notes](https://raw.githubusercontent.com/MSOpenTech/redis/3.0/Redis%20on%20Windows%20Release%20Notes.md)

## How to configure and deploy Redis on Windows
- [Memory Configuration for 3.0](https://github.com/MSOpenTech/redis/wiki/Memory-Configuration-For-Redis-3.0 "Memory Configuration")
- [Windows Service Documentation](https://raw.githubusercontent.com/MSOpenTech/redis/3.0/Windows%20Service%20Documentation.md "Windows Service Documentation")
- [Redis on Windows](https://raw.githubusercontent.com/MSOpenTech/redis/2.8/Redis%20on%20Windows.md "Redis on Windows")
- [Windows Service Documentation](https://raw.githubusercontent.com/MSOpenTech/redis/2.8/Windows%20Service%20Documentation.md "Windows Service Documentation")

## How to build Redis using Visual Studio

You can use the free [Visual Studio 2013 Community Edition](http://www.visualstudio.com/products/visual-studio-community-vs). Regardless which Visual Studio edition you use, make sure you have updated to Update 5, otherwise you will get a "illegal use of this type as an expression" error.

- Open the solution file msvs\redisserver.sln in Visual Studio, select a build configuration (Debug or Release) and target (x64) then build.

    This should create the following executables in the msvs\$(Target)\$(Configuration) folder:

    - redis-server.exe
    - redis-benchmark.exe
    - redis-cli.exe
    - redis-check-dump.exe
    - redis-check-aof.exe

## Testing

To run the Redis test suite some manual work is required:

- The tests assume that the binaries are in the src folder. Use mklink to create a symbolic link to the files in the msvs\x64\Debug|Release folders. You will
  need symbolic links for src\redis-server, src\redis-benchmark, src\redis-check-aof, src\redis-check-dump, src\redis-cli, and src\redis-sentinel.
- The tests make use of TCL. This must be installed separately.
- To run the cluster tests against 3.0, Ruby On Windows is required.
- To run the tests you need to have a Unix shell on your machine, or MinGW tools in your path. To execute the tests, run the following command: 
  "tclsh8.5.exe tests/test_helper.tcl --clients N", where N is the number of parallel clients . If a Unix shell is not installed you may see the 
  following error message: "couldn't execute "cat": no such file or directory".
- By default the test suite launches 16 parallel tests, but 2 is the suggested number. 
  
## Code of Conduct

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/). For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.

# 운영체제 2장

- 학습목표
  - 운영체제의 서비스를 이해한다
  - 시스템콜을 이해한다
  - 운영체제의 구조를 이해한다
  - 컴퓨터시스템의 부팅과정을 이해한다

## Operating System Structure

### Operating System Services

#### operating system provides the following services to users

- user interface
- program execution
- I/O operations
- File system manipulation
- Communications
- Error detection

#### Os functions for efficient operations of system itself

- Resource allocation
- Accounting
- Protection and security
  - protection : control for access to system resources
  - security: user authentication, defense from invalid access attempts

#### OS User Interface

- CLI
- GUI

### System call

-  os가 제공해주는 interface

- 프로그램이  os에 서비스를 요청하는 방법

  - 하드디스크에 access하는 방법
  - 프로세스를 생성하거나 실행하는 방법

- 프로세스와 os사이의 중요한 인터페이스 생성

- c나 c++로 만들어져 있다.

  ![image-20190318141738542](/Users/kyu/Library/Application Support/typora-user-images/image-20190318141738542.png)

- 어떤 파일의 내용을 다른 파일로 복사하는 경우

  ![image-20190318141814473](/Users/kyu/Library/Application Support/typora-user-images/image-20190318141814473.png)

### API

- 개발자에게 이용가능한 함수들을 제공
- common apis
  - Win32 api
  - POSIX API for POSIX-based system(unix/linux)
  - Java API for JVM
- 왜 system call 대신 api를 사용하는가
  - Portability
  - Easy to use

- 프로그래머가 시스템콜을 실행하는 것에 대해 몰라도 상관없다.
  - API와 os가 무엇을 하는지 이해해도 된다.
  - API에 의해서 os의 디테일이 프로그래머에게 숨겨진다

### system call vs API

![image-20190318143325641](/Users/kyu/Library/Application Support/typora-user-images/image-20190318143325641.png)

- Kernel level - system call

![image-20190318143610014](/Users/kyu/Library/Application Support/typora-user-images/image-20190318143610014.png)

- system call의 종류는 그렇게 많지 않음

### system call handling

![image-20190318144141756](/Users/kyu/Library/Application Support/typora-user-images/image-20190318144141756.png)

[ 리눅스 fork](http://blog.naver.com/PostView.nhn?blogId=lee_seha&logNo=220337575979&parentCategoryNo&categoryNo=33&viewDate&isShowPopularPosts=true&from=search&fbclid=IwAR0e33pquA88t8n0IGle_Eckog2pE_wc6WKOJXpAgoYL1y32dI1idLKHi0Q)

### System call parameter passing

- More information is often required than system call identification
- Three methods for passing parameters to the OS
  - pass the parameters in <b>registers</b>
  - store parameters in a table on memory, and then pass <b>the address of table</b> in a register
    - Linux, Solaris
  - push parameters onto the <b>stack</b> by program, and pop off the stack by OS.
  - ![image-20190318150015815](/Users/kyu/Library/Application Support/typora-user-images/image-20190318150015815.png)

### types pf System calls

- Process control
  - create/terminate, load/execute, wait/signal event
  - Fork, execve(), getpid(), signal()
- File Management
  - create/delete, open/cloase, read/write
  - Open(), read(), write(), close()
- Memory management
  - allocate memory
  - brk()
- Information maintenance
  - get/set timer or date, get/set process, file, or device attributes
  - time()
- Communications
  - create/delete connection, send/receive message
  - socket(), bind(), connect()

### Operating System Design and Implementaion

- Design goals
  - Choice of hardware and system type
    - batch, time sharing, single/multi user, distributed, real-time
  - requirementes
    - userview
      - Convenient to use, easy, reliable, safe, and fast
    - System's view
      - easy to design, implement, and maintain
      - flexible, reliable, error-free, efficient
- Seperation of policy from mechanism
  - Mechanism for giving priority to certain types of program. 
  - Policy that I/O intensive process have higher priority than CPU intensive process. 

- Implementation

  - 이전에 어셈블리였던 것들이 지금은 c나 c++로 작성됨

  -  <b>Data structures and algorithms</b> are important rather than assembly language 

    implementation.

    - Memory management, CPU scheduling, etc. 

### Operating system structures

- Simple structure

  - Interfaces and levels of functionality are not well seperated

  - Applications can acess I/O routines directly

  - It may cause entire system crash when an application fails.

    ![image-20190318152718184](/Users/kyu/Library/Application Support/typora-user-images/image-20190318152718184.png)

- Layered structure

  - Os is divided into a number of layers (levels)
  - The bottom layer ( layer 0 ) is the hardware.
  - The highest layer(layer N-1) is the user interface 
  - Easy to debug, but difficult to define the layers and not efficient
  - ![image-20190318152826355](/Users/kyu/Library/Application Support/typora-user-images/image-20190318152826355.png)

- Microkernel structure

  - Moves as much from the kernel into "user" space

  - Communication between user modules with message passing

    - file access through file server

  - easy to extend, easy to port

  - reliable and secure ( less code is running in kernel mode)

  - performance degradation

    - because of frequent communication between user modules and kernel
      - Mach, QNX, windows NT

    ![image-20190318153009155](/Users/kyu/Library/Application Support/typora-user-images/image-20190318153009155.png)

- Module structure

  - Most modern operating systems implement kernel as modules

  - it uses object-oriented approach

  - Each core component is seperated

  - Each talks to the other over known interfaces

  - Each is loaded into the kernel as needed

  - solalis, linux, mac os x

  - ![image-20190318153205164](/Users/kyu/Library/Application Support/typora-user-images/image-20190318153205164.png)

    

    #### System boot

  - How to load Kernel
    - Bootloader
      - run diagnostics, initialize system
      - locates the kernel, loads it into memory, and starts it
    - small system
      - store bootloader and OS in ROM
    - Large system( PC)
      - Store bootloader in ROM and OS in disk, respectively
      - simple boot loader in boot block -> complicated bootloader -> kernel

### System boot in linux

![image-20190318153551385](/Users/kyu/Library/Application Support/typora-user-images/image-20190318153551385.png)

- 메인보드에서 BIOS 실행
- ROM BIOS가 수정되고 boot loader 까지는 os가 아님
- 커널 적재후 - 메모리에 커널 올린 후 os 시작
- 메모리에 마운트

### Summary

![image-20190318154003138](/Users/kyu/Library/Application Support/typora-user-images/image-20190318154003138.png)


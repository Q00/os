# File System

- 파일 시스템의 개념을 설명할 수 있다.
- 파일 시스템의 다양한 인터페이스를 설명할 수 있다.

![image-20190610112104975](/Users/kyu/Library/Application Support/typora-user-images/image-20190610112104975.png)

- File System of the user's viewpoint
  - File system interface
  - How to show the file system to user
  - tree structure
- File system of storage management viewpoint
  - File system implementation
  - How to map the logical file system to the storage device
  - It is required to understand the storage internals

### File

- A named collection of related information (이름을 가지는 디스크에 저장되는 바이트의 집합)
- It is just a sequence of bytes
- It is stored on secondary storage
- It is divided into data files and program files



- magic number : a number embedded at or near the beginning of a file that indicates its file format

### Acces methods

- sequential access
  - File is accessed in order, one record after the other
  - Read/writes the next portion of the file and automatically advances a file pointer (editor, compiler)
  - 방대한 코드를 컴파일 할때
- Random access
  - File is accessed in randaom order
  - There is no restriction on the other of reading/writing files ( DBMS)
  - 데이터 베이스에서 특정 데이터를 읽을 때, demanding에서 특정 page를 읽을 때
- 두가지 access를 효율적으로 지원해야함
  - 작은 file, 큰 file 모두 효율적으로 지원해야함

### Directory

- A virtual container in which groups of files and possibly other directories

![image-20190610113713971](/Users/kyu/Library/Application Support/typora-user-images/image-20190610113713971.png)

![image-20190610115816489](/Users/kyu/Library/Application Support/typora-user-images/image-20190610115816489.png)

![image-20190610115852652](/Users/kyu/Library/Application Support/typora-user-images/image-20190610115852652.png)



#### hard link

- copy the file's metadata into Lee's directory
- if delete file : Use reference count

#### Symbolic link

- create the pathname for file in directory
- if delete file : dangling reference

### Unix directory structure

![image-20190610113757617](/Users/kyu/Library/Application Support/typora-user-images/image-20190610113757617.png)

### Big picture

- File System 
  - Logical File name ( local resource name )을 physical address 로 변환하기 위해 naming 기능을 제공
  - 사용자의 데이터를 저장장치로부터 read/write하는 기능을 제공
- Views on Files
  - User
    - File name + byte offset
    - File pointer로 offset 을 움직이며 파일 읽기
      - sequential : file pointer를 한 칸 씩(bytes) 움직이면서 파일의 내용을 읽기
      - Random : file pointer 를 임의로 이동하면서 파일의 내용을 읽기
  - Kernel
    - Inode + logical block number
      - file 안에 존재하는 sequence of block
      - bytes 단위가 아닌 block 단위로 내용을 읽기
      - kernel 의 file system은 device driver와 interface해야함
  - Device Driver
    - Physical block number
      - disk volume 단위로 관리, sector들의 linear sequence
      - Volume: 단일 file system 으로 관리되는 저장 장치상의 영역
      - sector들의 number


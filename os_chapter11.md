# File System implementation

- 파일 시스템 구현을 위한 기본 개념을 설명할 수 있다.



![image-20190610121438850](/Users/kyu/Library/Application Support/typora-user-images/image-20190610121438850.png)

- Fil System regards a storage device as a sequence of blocks
  - Disk is transferred between disk and memory in units of blocks
  - Each block has one or more sectors, which is typically 512 bytes

### Unix File system

- Inode ( file descriptors)
  - Attribute + indexes for file data
    - 파일이 크기, 소유자 소유그룹, 액세스타임, 접근권한 등...
  - Inode regionb and file data region are separated 

![image-20190610121950138](/Users/kyu/Library/Application Support/typora-user-images/image-20190610121950138.png)

- Inode의 사이즈는 각각 똑같다 ( inumber를 통해 접근 가능)
- file data allocation
  - contiguous
  - linked 
  - indexed allocation

#### Contigous Allocation

![image-20190610122202335](/Users/kyu/Library/Application Support/typora-user-images/image-20190610122202335.png)

Only starting location and length are required

Each file occpies a set of contiguous blocks on the disk

- It is simple to sequential/random access the file
- It is difficult to find space for a new file

#### Linked Allocation

![image-20190610122419824](/Users/kyu/Library/Application Support/typora-user-images/image-20190610122419824.png)

- 해당 파일 디스크립터에 기록된 파일의 첫번째 블록의 주소를 통해 파일 접근
- Blocks may be sacttered anywhere on the disk - No waste of space
  - Fragmentation 문제 해결
- Effective only for sequential access
  - random access의 경우에는 seek time이 굉장히 오래걸리게 됨



#### indexed allocation

![image-20190610122812848](/Users/kyu/Library/Application Support/typora-user-images/image-20190610122812848.png)

- It brings all pointers into the index block
  - file descriptor에 파일이 가지고 있는 Block 들에 대한 pointer를 index array로 저장
- It doesn't need to find a contigous blocks.
- sequential, random access 둘다 용이
- It requires one index block per file
- If the size of index block is still small to hold pointers for a large file?
  - 해결법 : Multilevel index ( but 이중으로 처리하는 이러한 구조는 기존에 비해 데이터를 더 액세스 해야하므로 seek time이 증가)

![image-20190610123548153](/Users/kyu/Library/Application Support/typora-user-images/image-20190610123548153.png)

### Free space Management

#### Bitmap ( also called bit vector)

- if block[i] is allocated, bit is 1; else, bit is 0

![image-20190610124348770](/Users/kyu/Library/Application Support/typora-user-images/image-20190610124348770.png)

- Easy to get contiguous free blocks
- Bit map requires extra space
- disk size / block size
- seek time 을 줄이기 위해 같은 실린더 안에 있는 block의 인접한 위치에 할당하는 방법

#### Linked List

- free-space list head
- No space overhead
- cannot get contiguous free blocks easily

![image-20190610124654824](/Users/kyu/Library/Application Support/typora-user-images/image-20190610124654824.png)

#### Grouping

- Modification of linked list approach
- The first n-1 of free blocks are actually free
- The last block contains the addresses of another n free blocks



### Directory Implementation

- Linear list
  - (file name, pointer to the file)
  - simple to implement
  - Time-consuming to search a file
  - B-tree can be used
- Hash Table
  - Linear list with hash data structure
  - Reduces directory search time
  - Collisions should be solved

### Performance and Recovery

- Efficiency
  - keeps a file's data blocks near that file's inode block to reduce seek time
- performance
  - Buffer cache
    - Seperate section of main memory for frequently used blocks
  - read-ahead
    - Techniques to optimize sequential access

### Buffer cache

- whenever files are accessed, file data should be fetched from disks.
- However, the disks accesses incurs large I/O overhead.
- On file access patterns, temporal locality ( 사용된 프로그램은 또 사용됨)
- Buffer cache keeps the blocks that will be used again - > reduce disk accesses

![image-20190610130241466](/Users/kyu/Library/Application Support/typora-user-images/image-20190610130241466.png)

- Inconsistency can occurs
  - Solutions for consistency
    - consistency checker
    - synchronous write
    - journaling
      - 변경되지 않은 사항 추적


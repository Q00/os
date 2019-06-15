# Secondary storage

- 하드디스크의 내부구조를 설명할 수 있다.
- 디스크 스케쥴링 알고리즘을 설명할 수 있다.

![image-20190610130704346](/Users/kyu/Library/Application Support/typora-user-images/image-20190610130704346.png)

- Moving head disk mechanism

### Hard Disk internals

- Disk I/O service time
  - seek time + Rotational deley(Rotational latency) + Data transfer time
  - seek time
    - Time to move the disk head to the desired track ( 실린더)
    - seek distance
  - Rotatinal delay
    - Time for the desired sector to rotate to the disk head
    - Best case = 0
    - Worst case = time for on rotation
  - Data transfer time
    - Time for transferring data from disk media to the disk buffer, and vice versa
  - <span style="color:red">Seek time or rotational dely >> data transfer time</span>

### Disk Scheduling Algorithms

- Disk I/O scheduler

  - Request : a scheduling unit in the disk I/O scheduler

    - type ( R/W),  disk address, the number of sectors

    ![image-20190610131630929](/Users/kyu/Library/Application Support/typora-user-images/image-20190610131630929.png)

  - when a request is completed, disk I/O scheduler chooses the next one

  - Average disk I/O service time depends on disk I/O scheduling chooses the next one

- disk algorithm의 목적

  - Minimizes the seek time and rotational delay
  - roatational delay에 대해서 roatational speed 와 track 마다 sector의 개수가 다르므로 os가 estimate할 수 없다.
  - 따라서 minimizing the seek time에 focusing

- Evaluation of disk scheduling algorithms

  - total head movements

#### FCFS

- 도착한 순서대로 찾음

![image-20190610132007473](/Users/kyu/Library/Application Support/typora-user-images/image-20190610132007473.png)

It has too long seek distance.

#### SSTF

- Shortest seek time first
- current head position에서 minimum seek time first

![image-20190610132145628](/Users/kyu/Library/Application Support/typora-user-images/image-20190610132145628.png)

- It is a greedy algorithm
- 몇몇의 리퀘스트에서 starvation이 일어날 수 있음

#### SCAN

- Elevator algorithm

- disk scans down towards at one end of the disk and then when it hist the bottome, it scans up other end

![image-20190610132827453](/Users/kyu/Library/Application Support/typora-user-images/image-20190610132827453.png)


#### C-SCAN

- It is variant of SCAN
- Circular scan ( SCAN 처럼 reverse servicing 이 아니라 huge jump로 다른쪽 헤드로 감)

![image-20190610133041198](/Users/kyu/Library/Application Support/typora-user-images/image-20190610133041198.png)

#### C-LOOK

- It is a practical implementation of C-SCAN

- in each direction, disk head는 final request까지 처리한 후 disk의 end까지 가지 않고 reverse direction immediatley

![image-20190610133314039](/Users/kyu/Library/Application Support/typora-user-images/image-20190610133314039.png)

### Disk Formatting

#### Physical formatting ( low level formatting)

- disk는 쓰기전에 sector로 변경하여 disk controller가 read하고 write할 수 있게 해야된다.
- 제조 과정에서 보통 physically formatted

![image-20190610133918907](/Users/kyu/Library/Application Support/typora-user-images/image-20190610133918907.png)

- header와 trailer에는 disk controller에서 사용되는 정보를 포함하고 있음
  - Sector number + error correcting code ( ecc )

#### partition

- A disk can be divided into multiple logical disks

![image-20190610134035759](/Users/kyu/Library/Application Support/typora-user-images/image-20190610134035759.png)

- OS can treat each partition as though it were seperate disk

#### logical formatting

- OS store the initial file system data structures
  - Superblock, free space bitmap, etc.

![image-20190610134140018](/Users/kyu/Library/Application Support/typora-user-images/image-20190610134140018.png)



### Swap Space Management

- Virtual memory 는 main memory의 확장 디스크로서 사용한다.
- 하드디스크에 있는 가상메모리의 일부
- 사용되지 않거나 사용량이 적은 응용프로그램은 스왑 파일에 보관할 수 있다.
- It can be used as a single contiguous memory which reduces i/o operations to read or wirte a file
- A normal file in file system : windows
- A seperate disk partition : UNIX

![image-20190610134950235](/Users/kyu/Library/Application Support/typora-user-images/image-20190610134950235.png)
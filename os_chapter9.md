# virtual memory

- 요구시 페이징을 설명할 수 있다.
- 페이지 교체 정책을 설명할 수 있다.

### Virtual memory concepts

- restricted physical memory size

  - prgorams larger than availiable space of the physical memory cannot be executed

- Virtual memory allows the execution of processs that are not completely in memory.

- parts of the program needs to be in memory for execution

   

### Demand Paging

- Virtual can be implemented via <span style="color:red">demand paging</span>
- Bring a page into memory when it is needed
  - Less I/Os are needed
  - Less memory is needed
  - needs to allow pages to be swapped in and out

![image-20190609210851366](/Users/kyu/Library/Application Support/typora-user-images/image-20190609210851366.png)

- when a page is referenced
  - Valid-invalid bit 
    - valid : reference it
    - invalid : not in memory or not legal(bad address, protection violation- bring it into memory 
  - access to invalid page causes page fault

#### Page fault handling

![image-20190609213655391](/Users/kyu/Library/Application Support/typora-user-images/image-20190609213655391.png)

- invalid reference? 
  - if bad address or protection violation -> abort process
  - if not in. memory -> continue following
- get empty page frame
  - if no empty page frames, replace some page rames
- read the page into the page frame, replace some page frames
- read the page into the page frame
  - 4 번 과정에서 disk I/O 가 수행되는 동안 process blocked
  - when the disk I/O is completed, set the page tables entry to valid
  - insert the process into ready queue again and dispatch later
- restart the instruction that caused the page fault

#### effective access time

- page fault rate 0<=p<=1
  - p = 1 every reference is a fault
  - p = 0 no page fault
- (1-p) * Memory access time + p * page fault service time

- It is import to keep the page fault rate low
  - page rplacement policy is required.



### Page replacement

- Page replacement
  - 사용되지 않는 페이지 교체
  - page fault가 적어야 좋음
- Locality of reference
  - Same pages may be brought into memory several times
  - paging system 이 성능이 좋게 되는 근거

![image-20190609214745643](/Users/kyu/Library/Application Support/typora-user-images/image-20190609214745643.png)

![image-20190609215036187](/Users/kyu/Library/Application Support/typora-user-images/image-20190609215036187.png)



```
7,2,3,1,2,5,3,4,6,7,7,1,0,5,4,6,2,3,0,1
```

#### LRU replacement

- Least Recently Used 알고리즘으로 가장 최근에 사용되지 않은 것을 제거하는 알고리즘이다. 이 알고리즘은 가장 오랫동안 사용하지 않았던 데이터는 앞으로도 사용할 확률이 적다는 것이라는 것을 전제로 한다.
- Reference time should be recorded for each page
- Too large space and time overhead
  - many approximation algorithms are proposed
    - Reference bit
      - 참조되면 1로바꾸고 초기화는 0으로 함
      - 0인 비트를 replace 
    - second change page replacement algorithm(clock alogrithm)
      - leave page in memory
      - set reference bit 0 ( second chance)
      - advance pointer for next victim until it finds a page with reference bit 0
      - 메모리 오버커밋( 모두 reference bit 1) 이 되면 FIFO 알고리즘 형태가 됨
    - LFU ( least Frequently Used)
      - keep the number of references that have been made
      - replace with smallest count

- Counter implementation
  - Every page entry has a counter
  - whenever page is referenced, the clock is icremented
  - when page A is referenced, copy the clock into A's counter
  - Replace the page with the smallest counter value
- Stack implementation
  - Keep a duplex stack of pages
  - when a page is referenced, move it to the top
  - No search for replacement

![image-20190527153329783](/Users/kyu/Library/Application Support/typora-user-images/image-20190527153329783.png)

#### FIFO replacement

- 가장 간단한 페이지교체 알고리즘으로 가장 오래된 페이지를 제거하는 알고리즘이다.
- Belady's anomaly로 프로세스에게 프레임을 더 많이 할당하였는데 페이지 부재율이 증가하는 현상이 있다.

![image-20190609215055995](/Users/kyu/Library/Application Support/typora-user-images/image-20190609215055995.png)

![image-20190527152653195](/Users/kyu/Library/Application Support/typora-user-images/image-20190527152653195.png)

#### Optimal replacement

- 장래에 가장 오랫동안 사용되지 않는 페이지가 교체되는 알고리즘으로 현실에서는 앞으로 사용되지 않을 페이지에 대해 알 수 없으므로 비현실적인 방법이다.

![image-20190527154439911](/Users/kyu/Library/Application Support/typora-user-images/image-20190527154439911.png)



### Allocation

- 위에 있는 알고리즘들은 fixed allocation을 가정
- page fault가 일어날 시에 페이지 프레임을 하나 더 할당한다면?
  - We must also allocate at least a minimum number of frame - > physical memory limitation

#### Allocation problem

- Decide allocation size at page fault
  - fixed allocation
    - Equal allpocation ( 100 frames, 5 processes, 20frames per process)
    - proportional allocation : 프로세스 수에 비례
  - Priority Allocation
    - using priorities rather than size
    - give more memory to highter process

#### Global vs Local Allocation

- Global replacement
  - process selects replacement frame from the set of all frames
  - one process can take a frame from another
- Local replacement 
  - Each process selects from only its own set of allocated frames

### Thrashing

- If a process does not have enough pages, page fault rate is very high
  - Low CPU utilization	-새로운 프로세스를 시스템에 추가시킴으로써 멀티프로그래밍의 차수를 증가시킨다.
- 프로세스가 Swapping pages in and out을 하느라 매우 busy 한 상태

#### Thrashing의 원인

![image-20190609222555870](/Users/kyu/Library/Application%20Support/typora-user-images/image-20190609222555870.png)

- 시스템에 충분히 많은 프로세스가 동작 중
- 동작 중인 프로세스가 요구하는 메모리가 가용한 물리 메모리보다 많아서 page fault 발생
- page fault를 처리하는 동안 프로세스는 block ( disk I/O ) -> Low Cpu utilization
- CPU Scheduler는 시스템이 한가한 것으로 판단하고 더 많은 프로세스를 실행
- Memory 가 점점 부족해짐

따라서 Global allocation을 사용해야함

local allocation을 사용하면 특정 프로세스가 과도하게 메모리를 사용하여 다른 프로세스가 영향을 받는 현상을 막을 수 없음.

시스템의 메모리가 부족한 상황이면 충분한 메모리가 global allocation으로 할당되었더라도 부족한 다른 프로세스에 의해 page fault가 지연되어 성능이 나빠질 수 있음



#### 근본적인 원인

- 현재 active 하게 사용되는 메모리 > physical memory ( page table )
- 불필요한 프로세스를 swapping 이나 terminate ( multiprogramming의 degree 낮추기)



### working set model

- It is based on the assumption of locality

![image-20190609223245416](/Users/kyu/Library/Application Support/typora-user-images/image-20190609223245416.png)

주소 주변 ( 위아래) : Spatial locality

똑같은 주소 : temporal locality


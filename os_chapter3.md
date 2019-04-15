# Processes

- 프로세스의 개념을 이해한다
- 프로세스 관리 기법을 이해한다
- 프로세스간 통신 기법을 이해한다

## process concept

### Operating system executes a variety of programs

- Batch system - jobs
- Time-shared systems - user programs or tasks
  - JOB과 process는 유사하다

### Process

- A program in execution ( 수행중인 프로그램)

### A process includes

- The program code, also called text section
- Data section containing global variable, static variable
- Stack containing temporary data
  - Function parameters, return addresses, local variables
- Heap containing memory dynamically allocated during run time ( 수행중에 메모리가 달라짐)
- Current activity including program counter, processor registers

![image-20190318154818260](/Users/kyu/Library/Application Support/typora-user-images/image-20190318154818260.png)

## Process State

### As a process executes, it changes state

- New
  - The process is being created
- Running
  - Instructions are being executed
- wating ( block, sleep)
  - The process is waiting for some event to occur
- Ready
  - The process is waiting to be assigned to a processor
- Terminated
  - The process has finished execution

![image-20190318162153983](/Users/kyu/Library/Application Support/typora-user-images/image-20190318162153983.png)

## process control Block

- Metadata to manage data

  - process Control Block for process, Task Control Block for Task

  - task_struct 라는 구조체안에 정보가 들어잇음

  - File Control Block for file

    - vnode in unix file system

     ![image-20190318164131064](/Users/kyu/Library/Application Support/typora-user-images/image-20190318164131064.png)

- Information associated with each process
- ![image-20190318164200871](/Users/kyu/Library/Application Support/typora-user-images/image-20190318164200871.png)

##  process Scheduling queues

- Ready queue
- Device queues
- Processs migrate among the various queues
- Ready queue and various I/O device queues
- ![image-20190318164245894](/Users/kyu/Library/Application Support/typora-user-images/image-20190318164245894.png)

- Representation of process scheduling
- ![image-20190318164303336](/Users/kyu/Library/Application Support/typora-user-images/image-20190318164303336.png)

## Schedulers

- CPU scheduler
  - selects which process should be executed next and allocates CPU( 스케쥴러는 cpu에 할당할 프로세스를 고른다.)
- Processes can be classified into 
  - I/O-bound process
    - spends more time doing I/O than computations
    - many short CPU bursts ( CPU를 조금조금씩 끊어서 사용)
  - CPU-bound process
    - spends more time doing computations
    - a few very long CPU bursts(한번에 많이 사용)
  - Scheduling algorithms are studied later.



## Context switch

- 어떠한 프로세스를 수행하는 일정시간이 지나면 다른 프로세스를 진행하게 되고 이후에 해당 프로세스를 다시 불러서 사용한다.
- when cpu switches to another process, the system must
- save the state of the old process, and load the saved state for the new process
- context switch time is pure overhead.
  - system does not any useful work while switching
- context switch time depends on hardware
  - the register set is different
- Cpu switch from process to process
- 너무 자주 빈번하게 하다보면 오버헤드 발생후 컴퓨터가 더 느려질 수 있음(수행시간보다 바꾸는 시간이 길면)

![image-20190318165017039](/Users/kyu/Library/Application Support/typora-user-images/image-20190318165017039.png)

## Process creation

- Process creation
  - parent process creates child processes,
  - which, in turn creates other processes.
  - Finally it forms a tree of processes


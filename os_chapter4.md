# Threads

- 쓰레드의 개념을 설명할 수 있다
- 쓰레드와 프로세스의 차이를 설명할 수 있다.

## Thread concept

#### Motivation

- 하나의 프로세스에는 하나의 control의 존재
- 프로세스에서 할 작업을 여러개로 나눈 후 thread 화를 통해 병렬적으로 작업하는 것이 필요
- process 안에서 parallellism 

### What is thread?

#### Thread

- a basic unit of CPU utilization ( CPU 이용의 기본단위)
- Each process can cinclude many thread.

#### All thread of process share

- code, data, heap
- os resources ( open files, signal handlers, working environment)
- PCB

#### Thread has it's own

- Stack, registers, thread ID, program counter

#### RPC

- remove procedure call system
- IPC 를 일반적인 함수나 프로시저 호출과 비슷하게 할 수 있게 해준다. 

#### Benefit

- Responsive : 프로그램 수행이 계속되어 사용자에 대한 응답성을 증가시킴
- Resource sharing : 프로세스의 자원을 쓰레드들이 공유하여 같은 주소공간 내에 다른 작업을 하는 쓰레드를 가질 수 있음
- Economy :  프로세스 생성보다 쓰레드 생성하고 context switch 하는것이 더 경제적임
- Utilization of multi processor architecture

### Thread vs Process

#### process

- Unit of execution
- 소유하고 있는 자원에 대한 보호, protect domain
- 실행된 program, executing program with a single thread of control
- context switch : heavy

#### thread

- 스케쥴링의 단위 : Execution unit , process 보다 작은 단위
- code, data, heap 영역 공유
- Execution flow in process ( 프로세스 내의 하나의 실행흐름)
- 같은 메모리 영역을 사용하여 context switch 비용이 상대적으로 적음

#### multiple thread vs cooperative process

- process
  -  IPC가 필요하며 (shared memory, message passing ) 비용이 큼
  - Context switching 도 비용이 듬

- thread를 cooperative 하게 만든다면 process 보다 적은 비용으로 같은 효과 

### Thread utilization

- Thread 수가 증가할 수록 utilization증가
- but context switch로 인한 비용으로 임계값을 넘으면 utilization 감소
- Multiprocessor 환경에서 parallel 장점이 더욱 커짐



### User thread vs kernel thread

#### user thread

- supported by user level threads library
- kernel thread를 통해서 processor 사용
  - 동일한 메모리 영역에서 thread가 생성 및 관리 속도 빠름

#### kernel thread

- managed directly by the os
  - Multi core이면 thread수가 여러개 있어야 parallel하게 수행됨
- kernel thread 가 cpu scheduling의 단위가 됨
# 운영체제 7장 Deadlocks

- 데드락의 개념을 설명할 수 있다
- 데드락의 발생조건을 설명할 수 있다
- 데드락의 처리방안을 설명할 수 있다

## what is deadlock

- holding a resource and waiting to acquire a resource held
- Deadlock can arise if four conditions hold simultaneously
  - Mutual exclusion
  - hold and wait
  - no preemption
  - circular wait

#### Methods for handling deadlocks

- prevention
  - at least one of four conditions cannot hold
- avoidance
  - a priori information about how each process utilize resources
- detection and recovery
  - it allows the system to enter a deadlock state and then recover

####  Deadlock prevention

- Ensures that at least one of four conditions cannot hold
  - Mutual Exclusion
    - is impossible to deny the mutual exclusion
  - Hold and wait
    - requires process to request and be allocated all its resources before it begins execution
  - No preemption
  - Circular wait

#### Deadlock Avoidance

- Deadlock avoidance alogrithm
  - when a process requesets an availiable resource, system must decide if the immediate allocation leaves the system in a safe state
- if a system is in **safe state**
  - No deadlock
- if a system is in **unsafe state**
  - possibility of deadlock

#### Deadlock Detection & Recovery

- Allow system to enter deadlock state
- Detection alogrithm
  - periodically invoke an algorithm that searches for a cycle in the graph
    - if there is a cycle, there exists a deadlock
- Recovery scheme
  - process termination
    - abort all deadlock processes
    - abort one process at a time until the deadlock cycle is eliminated
  - Resource preemption
    - preempt some resources from processes and give theses resources to other processes until the deadlock cycle is broken
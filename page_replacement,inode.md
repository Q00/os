# 운영체제 report

### 1. 연습문제 9.8

Consider the following page reference string:

```
7,2,3,1,2,5,3,4,6,7,7,1,0,5,4,6,2,3,0,1
```

Assuming demand paging with three frames, how many page faults would occur for the following replacement algorithms?

- LRU replacement
  - Least Recently Used 알고리즘으로 가장 최근에 사용되지 않은 것을 제거하는 알고리즘이다. 이 알고리즘은 가장 오랫동안 사용하지 않았던 데이터는 앞으로도 사용할 확률이 적다는 것이라는 것을 전제로 한다.

![image-20190527153329783](/Users/kyu/Library/Application Support/typora-user-images/image-20190527153329783.png)

- FIFO replacement
  - 가장 간단한 페이지교체 알고리즘으로 가장 오래된 페이지를 제거하는 알고리즘이다.
  - Belady's anomaly로 프로세스에게 프레임을 더 많이 할당하였는데 페이지 부재율이 증가하는 현상이 있다.

![image-20190527152653195](/Users/kyu/Library/Application Support/typora-user-images/image-20190527152653195.png)

- Optimal replacement
  - 장래에 가장 오랫동안 사용되지 않는 페이지가 교체되는 알고리즘으로 현실에서는 앞으로 사용되지 않을 페이지에 대해 알 수 없으므로 비현실적인 방법이다.

![image-20190527154439911](/Users/kyu/Library/Application Support/typora-user-images/image-20190527154439911.png)

### 2. inclue/linux/fs.h

파일 시스템은 하드디스크에서 부트블록, 슈퍼블록, 데이터블록, inode 블록으로 이루어져있다.

부트블록 : 시스템이 부팅될때 부트스트랩 정보가 저장되는 블록

슈퍼블록 : 전체 파일 시스템에 관한 정보가 저장되는 블록

데이터블록 : 실제 데이터를 저장하는 블록

inode 블록 : 파일 구조체를 가리키는 포인터들의 집합

- inode
  - unix에서 사용하는 자료구조로 파일 시스템 내부에 파일을 유지하는 중요한 정보를 담고 있다. 유닉스에서 파일 시스템을 생성할때 수많은 inode집합을 생성하게 되며 일반적으로 전체 파일 시스템 디스크 용량의 1%정도가 inode테이블에 할당된다.
  - attribute
    - Inode 번호
    - Stat C 함수에서 사용되는 파일 유형을 이해하기 위한 모드 정보
    - 파일 링크 숫자
    - 소유주 UID
    - 소유주 GID
    - 파일 크기
    - 파일이 사용하는 실제 블록 개수
    - 마지막으로 수정된 시각
    - 마지막으로 접근한 시각
    - 마지막으로 변경된 시각
- file
  - stream 파일을 가리키는 파일 포인터로서 파일의 정보를 담을 수 있는 구조체이다. 
  - 프로세스가 소유하는 열린파일과 관련된 구조체
  - 각각의 task는 file handle에 의해 열린 파일을 추적하게 된다.


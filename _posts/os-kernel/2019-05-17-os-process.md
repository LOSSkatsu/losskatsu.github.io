---
title: "[운영체제] 프로세스(process)란(1)" 
categories:
  - os-kernel
tags:
  - os
use_math: true
toc: true
toc_label: "My Table of Contents"
toc_icon: "cog"
sidebar:
  title: "AI Machine Learning"
  nav: sidebar-contents
---

# 프로세스(process)란?


**참고링크**

* [프로세스란?](https://losskatsu.github.io/os-kernel/os-process/)
* [리다이렉션, 파이프 개념](https://losskatsu.github.io/os-kernel/linux-redirection/)
* [네임스페이스 개념](https://losskatsu.github.io/os-kernel/linux-namespace/)
* [분산시스템 개념(1)](https://losskatsu.github.io/os-kernel/dist-sys-concept01/)
* [분산시스템 개념(2)](https://losskatsu.github.io/os-kernel/dist-sys-concept02/)
* [프로그램, 바이너리, 프로세스, 스레드의 개념](https://losskatsu.github.io/os-kernel/process-thread/)



예전 컴퓨터는 한번에 하나의 프로그램만 실행가능 했었다. 
그리고 실행중인 프로그램은 모든 시스템 자원을 사용했었다. 
시간이 흘러 현대의 컴퓨터는 여러 프로그램을 메모리에 올리고 동시에 실행이 가능하다. 
여기서 프로세스(process)라는 개념이 탄생하는데, 여러 프로그램 중 실행중인 프로그램을 프로세스라고 한다. 
현대 개발자는 프로세스는 작업 단위로 생각한다. 
그러므로 시스템은 프로세스의 집합이라고 생각할 수 있다. 
OS는 시스템 코드 뿐만 아니라 유저가 실행한 코드도 실행한다. 

## 프로세스 개념

운영체제에서 논의되는 쟁점들은 모두 CPU activity와 관련이 있다. 그것이 배치 잡(batch job)이던, 
유저 프로그램(user program)이나 태스크(task)던 말이다. 
심지어 유저프로그램 하나만 놓고봐도 유저는 여러가지 프로그램을 동시에 실행시킨다. 
예를들어 웹브라우저를 켜고, 워드프로세스 실행하고, 노래를 듣는 것 처럼 말이다. 
이때 운영체제는 메모리 관리 같은 것을 도와준다. 
job이라는 용어와 process라는 용어는 함께 사용될 수 있다. 개인적으로는 process라는 용어를 선호하지만...

### 프로세스

서두에서 말했듯, 프로세스는 실행중인 프로그램이다. 
프로세스는 아래와 같은 공간을 포함한다. 

* 프로세스 스택(process stack): 임시 데이터를 담는 곳이다. 
* 데이터 섹션(data section): 전역 변수를 담는 곳.
* 힙(heap): 프로세스 실행 중 동적 할당되는 메모리 영역. 

프로그램 자체를 프로세스라고 혼동하지마라. 둘은 다르다. 
프로그램은 비활성 객체(passive entity)이다. 즉, 디스크안에서 명령문(instruction)을 포함하고 있다. 
마치 나 좀 실행해 주세요라고 말하는것처럼. 
반대로 프로세스는 활성 객체(active entity)이다. 프로그램은 실행가능한 파일이 메모리에 올라갈때 프로세스가 된다. 
두가지 프로세스는 하나의 같은 프로그램과 연관되있을수 있지만, 그럼에도 불구하고 그 두 프로세느는 분리된 실행순서로 고려된다. 
예를들어 한 유저가 똑같은 크롬 브라우저를 여러개 실행할 수 있다. 
각각의 프로세스는 각자의 데이터, 힙, 스택 부분을 따로 가진다. 

### 프로세스 상태(Process State)

* New: 프로세스 생성
* Running: 명령문(Instruction) 실행됨
* Waiting: 프로세스는 이벤트(I/O이나 시그널) 발생을 기다리는 중...
* Ready: 프로세스는 프로세서에 할당되기를 기다림
* Terminated: 프로세스 실행됨.

### 프로세스 컨트롤 블락(Process Control Block)

각 프로세스는 운영체제에 의해 PCB(process control block)로 표현된다. 
PCB는 각 프로세스에 관해 아래와 같은 정보를 포함한다. 
쉽게말해 PCB는 프로세스마다 다양한 정보를 저장하는 저장소라고 할 수 있다. 

* Process state: new, readyk, running, waiting 중 어떤 상태인지 나타냄
* Program counter: 해당 프로세스에 대해 다음 실행 예정인 명령문 주소를 담고 있는 레지스터
* CPU registers: 우리가 잘 알고 있는 그 레지스터.
* CPU-scheduling information: 프로세스 우선순위, 스케쥴링 큐, 파라미터에 대한 포인터
* 메모리 관리 정보: 페이지테이블, 세그먼터 테이블 과 같은 정보
* Accounting information: CPU 사용량, 시간 제한, account number, job 또는 프로세스 넘버.
* I/O status information: 해당 프로세스와 관련된 I/O 디바이스 정보

### 쓰레드(Threads)

프로세스는 싱글 쓰레드를 실행하는 프로그램이다. thread of execution을 줄여서 쓰레드(thread)라고 부른다.
예를들어, 프로세스가 워드프로그램을 실행할때, 명령문의 싱글쓰레드가 실행된다. 
컨트롤의 싱글 쓰레드는 프로세스가 한번에 하나의 테스크를 수행하게 한다. 
현대에는 프로세스의 개념을 확장시켜, 
하나의 프로세스가 다중 쓰레드(multiple threads)를 가져, 한가지 이상의 테스크를 동시에 수행하게 도와준다. 


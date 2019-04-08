---
title: "[리눅스] 프로세스 확인(ps), 파이프, 그렙(grep), 리다이렉션(>)" 
categories:
  - it-infra
tags:
  - it-infra
use_math: true
toc: true
toc_label: "My Table of Contents"
toc_icon: "cog"
sidebar:
  title: "AI Machine Learning"
  nav: sidebar-contents
---

# [리눅스]프로세스 확인(ps), 파이프(|), 그렙(grep), 리다이렉션(>)

## ps - 현재 실행 중인 프로세스 확인 

```bash
$ ps -ef | grep apache
```

ps 옵션 | 설명 
--------|------
-e | 모든 프로세스 출력
-f | Full 포맷  

## 파이프(|) - 출력 결과를 필터링

## grep - 파일 패턴을 스캔

## 리다이렉션(>) - 출력 결과를 파일로 저장 


기호 | 의미
-----|-----
\>  | 결과를 파일로 저장
\>> | 결과를 기존 파일에 데이터 추가
\<  | 파일 데이터를 입력  

예를 들어,

```bash
$ ls -al > abc.txt
```
라고 하면 ls -al을 입력 후 출력되는 결과물을 abc.txt라는 파일에 저장하라는 뜻. 

기호 | 의미
-----|-----
0  | 표준입력
1 | 표준출력
2  | 표준에러 
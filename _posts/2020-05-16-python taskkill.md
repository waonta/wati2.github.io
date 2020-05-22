---
layout: post
title: python + taskkill
headTitle: Automation Anywhere AA 익스플로러 응답없음
date: 2020-05-16
dateEdit: 2020-05-22
---

#### python으로 윈도우에서 응답없음 프로그램 종료하기  
[https://github.com/wati2/python-taskkill](https://github.com/wati2/python-taskkill)

#### 1. 현상

Automation Anywhere 익스플로러 응답없음 상태인경우  
익스플로러(응답없음 상태)와 관련된 커맨드가 진행되지 않음  
AA Player가 멈춰 있음 (Hang 상태)

#### 2. 환경

IE 11  
AA Client 11.3.1

#### 3. 확인된 진행되지 않는 command

응답없음 상태의 익스플로러 창에 커맨드 적용하면 AA가 Hang 상태가 됨 ( 다른 명령도 안될 수 있음 )  
+ object cloning  
+ mouse click

#### 4. Hang은 AA에서 치명적인 문제

차라리 에러가 발생하면 상관없는데  
Hang 발생하면  
이후 발생하는 모든 업무에 지장  
(주말이면 모든 태스크 봇이 수행 안되어 있음)

#### 5. 해결방안 Flow

python으로 10초마다 체크하는 코드 작성  
python코드 내에서 tasklist, taskkill 수행 및 결과 확인 가능

tasklist 로 프로그램 상태 확인  
iexplore.exe 상태가 not responding 인지 확인하여  
taskkill 로 종료함

현재는 매개변수 없이 만들었지만  
상황에 따라 응용 가능함

#### 6. AA 에서 적용 순서

1. AA Bot 시작시 open program commmand 로 python프로그램 실행  
**OR** python코드를 exe로 만들어서 실행  
2. Bot 업무 진행중 iexplore.exe 상태가 not responding이면 python 프로그램이 감지하여 종료시킴
3. ie가 종료되면 ie창이 존재하지 않으므로, AA에서 Error발생 > Error Handling으로 처리
4. 작업 진행
5. 작업 완료 후, Bot 종료시 close window command로 python프로그램 종료

#### 7. Runner 에 파이썬 설치가 귀찮은 환경설정이라면
+ exe프로그램으로 만들어서 시작시 exe 프로그램 실행
+ exe파일은 My Docs에 넣으면 Dependency 설정 가능

#### 8. 코드응용 가능부분 생각해봄

- 특정 프로그램이 응답없음 상태여도 작업이 진행중일 수 있음. 10분정도까지는 종료하지않고 기다려야 할 상황이라면 ?  
  => 응답없는 경우 기다릴 시간을 매개변수로 전달하여 기다린다음 taskkill로 iexplore.exe 종료

- ieplore.exe가아닌 다른 프로그램을 관리하고 싶으면?  
  => 프로그램명을 매개변수로 전달

- 절대적인 시간 스케줄을 관리하기 위해 AA 를 스케줄 시간만큼만 수행하게 하고 죽이기? (AAplayer.exe)

#### 9. **기타**

python 실행시 따로 커맨드 프롬프트 실행 되지 않음  
코드내에서 tasklist 수행결과를 변수로 받아서 출력하여 확인 가능  
tasklist option을 통해 not responding인 경우만 필터하여 확인 가능함

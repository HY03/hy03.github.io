---
title: 정보보안 - 유닉스 프로세스 식별자 및 정보 확인
date: '2021-01-13 15:00:00'
tags:
- 정보보안기사
- UNIX
related: true
categories:
- IS_Certification
toc: true
---

# 프로세스 식별자

- 프로세스 : 실행중인 프로그램
- 프로세스 식별자 : PID
- 프로세스 생성 과정
    + OS 부팅 되면서 초기 시스템 프로세스를 생성
        * 0 : swapper (sched) - 커널 프로세스 (커널은 메모리에 상주)
            - exec(), fork() 를 수행하여 아래 프로세스 생성
        * 1 : init - 시스템 내의 모든 프로세스의 조상 프로세스
            - 부모프로세스가 죽고 자식 프로세스가 남은 경우 init이 부모 역할을 함
        * 2 : pagedaemon - 커널 프로세스 (커널은 메모리에 상주)
        * exec() : 자신의 현재 코드, 자료, 실행 가능한 다른 코드, 스택 등으로 치환
        * fork() : 자식 프로세스에게 부모의 코드와 자료, 스택영역 할당
- CPU 1개 = 실행 가능한 프로세스 1개
    + 커널은 CPU(자원)을 골고루 나눠 쓸 수 있도록 스케줄링을 함
    + 프로세스들은 필연적으로 상태 전이가 일어남
        * CPU 사용권을 할당 받아 실행중인 상태
        * CPU 사용권을 반납하고 대기 중인 상태
    + 실행중인 프로세스는 누구나 정보를 확인할 수 있음
    + 프로세스 종료는 수퍼유저(root) 또는 프로세스를 실행한 사용자로 제한

# 프로세스 자료구조

- `ps [option]` : 프로세스에 대한 정보 출력
    + `-f` : 프로세스 정보를 한 줄씩 출력
    + `-l` : f 옵션보다 더 많은 정보를 출력
    + `-e` : 현재 시스템에서 실행중인 모든 프로세스의 정보를 출력

- PCB (프로세스 제어블록) (시스템)
    + 프로세스 상태정보
        * ready
        * running
        * blocked
    + 프로세스 번호 (PID)
    + 프로그램 카운터 : 컨텍스트 스위칭이 일어날 때 다음에 실행할 명령어 위치값 저장
    + 레지스터 : 컨텍스트 스위칭이 일어날 경우 현재 프로세스의 실행상태 정보
    + 메모리 정보
        * page, segment table

- FDT (File Descripter Table) (시스템)
    + 프로세스들이 오픈한 파일들을 관리하기 위한 테이블
    + 개별 프로세스별로 생성
    + stdin : 표준 입력
    + stdout : 표준 출력
    + stderr : 표준 에러
    + 사용 파일들 (File Descripter)
        * 숫자값 : 오픈한 파일을 구별하기 위한 식별자

- System Open File Table (커널)
    + 오픈된 파일을 관리하기 위한 테이블
    + 커널/운영체제에서 관리
    + open mode
        * 읽기 (read)
        * 쓰기 (write / append)
    + offset
        * 입력, 출력을 수행하기 위한 위치값
    + reference count
        * 해당 파일의 참조 개수

- Active vnode table (커널)
    + i-node 를 관리하기 위한 캐시 테이블

# 프로세스의 종류

- 0 (swapper) : boot process, 시스템 부팅 담당
- 1 (init)
- orphan process : 부모 프로세스가 먼저 죽은 경우
    + init 프로세스가 부모 역할을 함
- 예: test 실행
    + init 프로세스 fork() -> exec()
    + 부모프로세스(pid:20) 생성 -> 자식 프로세스(pid:21) 생성
        * `kill -9 [pid]` : 강제종료
        * `kill -9 20` : 부모프로세스 강제종료
        * 자식프로세스의 ppid 는 1 (init) 이 됨
        * 프로세스 강제종료시에는 자신의 종료상태정보를 부모프로세스에게 모두 반환해야 정상종료됨.
            - pid, exit code, cpu time
        * 부모프로세스가 종료상태정보를 확인 -> 자식프로세스 소멸
        * 부모프로세스가 종료상태정보를 미확인 -> 자식프로세스가 남아있게 됨 (좀비 프로세스)
            - 모든 프로세스는 종료 시 일시적으로 좀비프로세스 상태를 거침
            - 자식 프로세스의 종료 사실을 커널이 알리고 부모 프로세스가 확인하는 동안은 좀비 상태가 됨
        * 좀비프로세스가 많아지면 더이상 프로세스 생성 X
            - kill 명령어로도 제거 불가
            - 부모프로세스가 확인 혹은 재부팅해야 함
            - `ps -l` 로 프로세스 확인할 경우
                + 필드 : F S PRI NI ADDR SZ WCHAN
                + F 필드 : 프로세스 플래그
                    * 1 : fork() 로 생성, exec() 미실행
                    * 4 : 슈퍼유저로 실행
                + S 필드 : 프로세스의 현재 상태
                    * S : Inturrupt 가능 Sleep 상태
                    * R : Run / Ready / Runnable 상태
                    * Z : Zombie 상태
                    * D : Inturrupt 불가능 Sleep 상태 (I/O 대기)
                    * T : Stop 상태 (프로세스 정지)
                + PRI 필드 : 프로세스의 우선순위
                    * 수치가 낮을수록 우선순위 높음
                + NI 필드 : 프로세스 우선순위 근거
                + ADDR 필드 : 메모리 주소
                + SZ 필드 : 프로세스가 차지하는 메모리 크기
                + WCHAN 필드 : Sleep 상태 프로세스가 대기하는 커널의 함수명 

# System Call

- 메모리
    + Stack
    + Heap
    + Data
    + Code

- 프로세스
    + 실행중인 프로그램
    + 프로그램의 instance (복제물)
    + 프로그램 호출한 프로세스를 fork() -> exec() 로 프로그램 코드 대체

- fork() : 복제
    + 메모리의 Stack, Heap, Data, Code 영역을 복제. pid 값은 다름.
    + 원본 : 부모, 복제물 : 자식

- exec() : 대체
    + fork() 에 의해 복제된 자식 프로세스의 코드영역을 다른 내용으로 덮어씀.

- 프로세스 그룹
    + 프로세스 생성 -> 프로세스 그룹 생성 (실행프로세스 - 자식프로세스들)
    + 커널이 터미널 제어권을 관리하기 위함
        * 터미널 제어권 : 입력되는 시그널 등을 관리
        * 중앙 서버와 A 터미널 연결 (Session) 
        * Session 내 A1 프로세스 그룹, A2 프로세스 그룹이 있을 경우
        * A1 프로세스 그룹이 터미널에 대한 제어권을 가질 경우 : 포그라운드모드 (유일)
            - PGID : A1 프로세스의 대표되는 프로세스 PID, 쉘을 실행시킨 PID
        * A2 프로세스 그룹이 터미널에 대한 제어권이 없을 경우 : 백그라운드모드

- `ps -ef`
    + CMD 컬럼 : 프로세스명
    + C 컬럼 : CPU의 사용량 - 스케줄링에 영향 (잘 사용되지 않음)
    + STIME : 프로세스 시작 시간
    + TTY : 프로세스와 연결된 터미널
    + TIME : CPU 사용시간

- 프로세스간 통신 (InterProcess Communication)
    + 시그널
        * 외부
        * 에러
        * 이벤트
        * 인위적
    + Pipe : `ps -ef | grep tomcat`
    + Message Queue
    + 공유메모리
    + 세마포어
- `kill [-signal number or name] [PID]` : 시그널
    + `kill -9 1000` : PID 1000번 프로세스 종료 시그널 
    + SIGKILL(9) : 프로세스 종료 시그널(무시, 임의처리 불가)
    + SIGSTOP(23) : 프로세스 정지 시그널 (무시, 임의처리 불가)

- 시그널 통신을 사용하는 경우
    + 데몬 프로세스의 정상적인 종료를 위해
    + 데몬 프로세스의 환경 설정 파일을 수정하기 위해
    + 프로그램을 디버깅하기 위해
    + 프로세스 간의 동기화를 위해
    + 커널과 시스템 관리자는 모든 프로세스에게 시그널을 보낼 수 있지만, 일반 프로세스는 동일한 UID 와 GID 를 갖는 프로세스 또는 같은 프로세스 그룹 내의 프로세스에게만 시그널을 보낼 수 있음


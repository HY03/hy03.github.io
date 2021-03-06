---
title: 04.정보보안 일반 - 02.접근 통제
date: '2020-10-28 08:00:00'
tags:
- 정보보안기사
- 정보보안 일반
- 접근 통제
related: true
categories:
- IS_Certification
toc: true
---

# 접근 통제(Access Control) 정책의 이해 및 구성요소
## 1. 개요 (용어)
+ 정당한 사용자에게는 권한을 부여하고 그 외의 다른 사용자는 거부하는 것
+ 식별(Identification) : 사용자 ID를 확인하는 과정
+ 인증(Authentication) : 패스워드가 정확한지 확인
+ 인가(Authorization) : 읽고, 쓰고, 실행시키는 권한을 부여
+ 주체 : 자원의 접근을 요구하는 활동 개체(사람, 프로그램, 프로세스 등)
+ 객체 : 자원을 가진 수동적인 개체(Database, 컴퓨터, 파일 등)
+ 접근 : 주체와 객체의 정보 흐름

## 2. 정보 접근의 단계

|단계|내용|
|:---|:---|
|식별(Identification) 단계|사용자가 시스템에 본인이 누구라는 것을 밝히는 행위(예: ID)|
|인증(Authentication) 단계|사용자가 맞음을 시스템이 인정(예: Password, 스마트카드, 생체인증)|
|인가(Authorization) 단계|접근 권한 유무 판별 후 접근 권한 부여|

## 3. 접근 통제 정의
+ 접근 통제(Access Control)는 주체에 대한 객체의 접근을 통제
+ A 라는 사용자가 b.html 파일객체에 대해 읽기, 쓰기, 실행 권한이 있는지 확인하고 권한이 있으면 권한을 부여
+ 권한이 없으면 접근을 차단
+ 통제 활동 : 비인가된 접근 감시, 접근을 요구하는 이용자를 식별, 정당한 이용인지 확인
+ 통제 목적 : 주체의 접근으로부터 객체의 기밀성, 무결성, 가용성을 보장

## 4. 접근 통제 원칙
+ 최소 권한의 원칙 : 최소한의 권한만을 허용하여 권한의 남용을 방지
+ 직무분리 : 업무의 발생, 승인, 변경, 확인, 배포 등이 한 사람에 의해 처리되지 않도록 직무를 분리 (예: 보안관리자와 감사자, 개발자와 운영자)

## 5. 참조 모니터(Reference Monitor)
+ 개요
    * 주체가 객체를 참조할 때 직접참조를 수행하지 않고 보안 커널을 통해 참조
    * 보안 커널은 주체의 권한을 확인하고 접근한 객체에 대한 정보를 로그에 기록
    * 취약점은 보안 커널을 통해 차단
    * 주체의 객체에 대한 접근 통제 결정을 중재하는 OS의 보안 커널로서 일련의 소프트웨어임
+ 참조 모니터의 3가지 요소

    |요소|내용|
    |:---|:---|
    |완전성(Completeness)|우회가 불가능해야 함|
    |격리(Isolation)|Tamper proof(부정 조작이 불가능해야 함)|
    |검증성(Verifiability)|분석하고 테스트할 정도로 충분히 작아야 함(Simple, Small, Understandable)|

## 6. 식별과 인증
+ 식별 : 자신을 시스템에 밝히는 수단, Unique, 책임 추적성 분석의 기초
+ 인증 : 시스템이 사용자가 맞음을 검증하고 인정하는 것

## 7. 인증 방식에 따른 분류

|인증 구분|설명|기반|종류|
|:---|:---|:---|:---|
|Type I 인증|Something you know|**지식**|Password, Pin, Passphrase|
|Type II 인증|Something you have|**소유**|Smart Card, Tokens|
|Type III 인증|Something you are|**존재**|홍채, 지문, 정맥|
|Type IV 인증|Something you do|**행동**|음성, 서명, Keystroke Dynamics|

- **2-Factor 인증**
    + **지식, 소유, 존재, 행동기반 인증에서 2개를 같이 인증**하는 방법


# 접근 통제 기술 정책의 특징 및 적용 범위
## 1. 접근 통제 기술 개요
+ 미국 국방성에서 기밀 분류 방법으로부터 유래하는 접근 통제 정책
    * **MAC(Mandatory Access Control)**
        - 자동으로 시행되는 어떤 규칙에 기반
        - 사용자와 타깃에 광범위한 그룹 형성이 요구됨
    * **DAC(Discretionary Access Control)**
        - 특별한 사용자별로 정보에 대한 접근을 제공
        - 추가적 접근 통제를 사용자에게 일임
+ OSI 보안 구조
    * 신분기반(Identity-based) : DAC 와 유사
        - 개인기반(Individual-Based Policy:IBP)
        - 그룹기반(Group-Based Policy:GBP)
    * 규칙기반(Rule-based) : MAC 와 유사
        - 다중단계(Multi-Level Policy:MLP)
        - 부서기반(Compartment-Based Policy:CBP)
    * 직무기반(Role-Based)
        - 신분기반과 규칙기반 정책의 양쪽 특성
+ 접근 통제 메커니즘
    * ACL(Access Control List)
    * CL(Capability List)
    * SL(Security Label)
    * 통합정보 메커니즘 (3가지 정보를 종합적 고려)
    * PB(Protection Bit) : 각 파일에 접근 통제를 위한 Bit 부여
+ 접근 통제 보안모델
    * HRU 접근행렬 모델 : 접근행렬 이용
    * BLP 보안모델 : 엄격한 기밀성 통제
    * Biba 보안모델 : 무결성 정책을 지원
    * Clark-Wilson 모델 : 실행할 수 있는 프로그램에 의하여 통제

## 2. MAC(Mandatory Access Control)
* **MAC(Mandatory Access Control) - 강제적 접근 통제**
    - 접근 권한이 **주체의 비밀 취급 인가 레이블** 및 **객체의 민감도 레이블**에 따라 지정되는 방식
    - **관리자에 의해서 권한이 할당되고 해제**됨
* MAC의 주요 특징
    - 데이터에대한 접근을 시스템이 결정(Rule에 의해)
    - 관리자만이 자원의 카테고리 변경 가능
    - 비밀성을 포함하고 있는 객체에 대해 주체가 가지고 있는 권한에 근거하여 객체의 접근을 제한하는 정책
* MAC의 종류

    |종류|설명|
    |:---|:---|
    |Rule-based MAC|주체와 객체의 특성에 관계된 특정 규칙에 따른 접근 통제 방화벽|
    |Administratively-directed MAC|객체에 접근할 수 있는 시스템 관리자에 의한 통제|
    |CBP(Compartment-Based Policy)|⦁ 일련의 객체 집합을 다른 객체들과 분리<br/>⦁ 동일 수준의 접근허가를 갖는 부서라도 다른 보안등급을 가질 수 있음<br/>⦁ 예)팀장은 자기 팀원의 급여정보를 볼 수 있으나 다른 팀원 급여정보는 볼 수 없음|
    |**MLP(Multi-Level Policy)**|⦁ Top Secret, Secret, Confidentiality, Unclassified와 같이 각 객체별로 지정된 허용 등급을 할당하여 운영<br/>⦁ 미국 국방성 컴퓨터 보안 평가지표에 사용, BLP 수학적 모델로 표현 가능<br/>⦁ 일종의 다단계 접근 통제방법<br/>⦁ 주체와 객체에 대하여 접근 권한에 대한 보안등급이나 역할에 따른 보호번주 등의 보안 레이블을 부여하여 접근을 통제하는 방법|

## 3. DAC(Discretionary Access Control)
+ **DAC(Discretionary Access Control) - 자율적 접근 통제**
    * **객체의 소유자가 권한 부여** : 접근하려는 사용자에 대하여 권한을 추가 및 삭제할 수 있다.
    * User-based, Identity : **사용자의 신분**에 따라 임의로 접근을 제어하는 방식
    * 융통성이 좋아 UNIX, DBMS 등의 상용 OS에서 구현이 가능
    * 접근 통제 목록(ACL, Access Control List) 사용 : Read, Write, Execute
    * MAC의 단점을 극복하기 위해 나온 것이 아님
+ DAC 종류
    * Identity-based DAC : 주체와 객체의 ID에 따른 접근 통제, 주로 유닉스에서 사용
    * User-directed : 객체 소유자가 접근 권한을 설정 및 변경할 수 있는 방식
+ Non-DAC(Non Discretionary Access Control) - 비임의적 접근 통제
    * 주체의 역할에 따라 접근할 수 있는 객체를 지정하는 방식
    * 기업 내 개인의 작은 이동(예:직무순환) 및 조직 특성에 밀접하게 적용하기 위한 통제 방식
    * Role-based 또는 Task-based 라고도 함
    * Central authority(중앙 인증) : 중앙 관리자에 의해 접근 규칙을 지정
    * 사용자별 접근 통제 규칙을 설정할 필요가 없다.
+ **Non-DAC의 종류**

    |종류|설명|
    |:---|:---|
    |**Role-based Access Control(RBAC)**|⦁ 사용자의 역할(임무)에 의해 권한이 부여(예:PM, 개발자, 디자이너)<br/>⦁ 사용자가 적절한 역할에 할당되고 역할에 적합한 권한이 할당된 경우만 사용자가 특정한 모드로 정보에 대한 접근을 통제할 수 있는 방법|
    |**Lattice-based Non-DAC**|⦁ 역할에 할당된 민감도 레벨에 의해 결정<br/>⦁ 관련된 정보로만 접근 가능(예: 핵무기 임수 수행자는 관련된 상/하위 정보로만 접근 가능)<br/>⦁ 주체와 객체의 관계에 의거하여 접근을 통제할 수 있는 Upper bound와 Low bound를 설정하여 제어하는 방식, 정보의 흐름을 통제|
    |**Task-based Non-DAC**|⦁ 조직 내 개인의 임무에 의한 접근 통제(알 필요성의 원칙)<br/>⦁ 핵무기와 관련된 임무를 수행하고 있는 경우 다른 관련 업무는 볼 수 없음|

## 4. RBAC(Role Base Access Control)
+ **권한들의 묶음으로 Role을 만들어서 사용자에게 Role 단위로 권한을 할당하고 관리**
+ RBAC의 장점
    * 관리 수월 : 관리자에게 편리한 관리 능력을 제공, 비용이 줄어듦
    * 보안관리 단순화 : 권한 지정을 논리적, 독립적으로 할당하거나 회수 가능
    * 최소 권한 : 최소한의 권한만을 허용하여 권한의 남용을 방지
    * 직무분리 : 시스템상에서 오용을 일으킬 정도로 충분한 특권을 가진 사용자를 없게 한다는 것이 가장 큰 특징임

## 5. 접근 통제 기술 간의 차이점

|항목|MAC|DAC|RBAC|
|:---|:---|:---|:---|
|권한 부여자|System|Data Owner|Central Authority|
|접근 여부 결정 기준|Security Label|Identity|Role|
|오렌지북|B|C|C|
|장점|안전/중앙 집중관리|유연, 구현 용이|관리 용이|
|단점|구현/운영 어려움, 높은 비용|트로이목마 공격에 취약, ID 도용문제|사용자-역할 중심이기 때문에 접근 요청이 발생하는 상황 정보 등이 접근 제어 정책에 제대로 반영되기 어려움|
|적용 사례|방화벽|Linux 파일시스템|HIPAA(보건 보험)|

# 접근 통제 기법과 각 모델의 특징
## 1. 접근 통제 방법

![ACL CL](/assets/images/posts/ACL_CL.png)

[이미지 출처](https://srl.cs.jhu.edu/pubs/SRL2003-02.pdf)

+ **Capability List**
    * **주체별로 객체를 링크드리스트로 연결하고 권한을 할당한 구조**
    * A 에 대해 모든 파일을 리스트하고 파일별 접근권한을 나열한 구조
    * 권한을 알기 위한 탐색시간이 오래 걸리는 문제점
+ **Access Control List**
    * 주체와 객체간의 접근 권한을 테이블로 구성한 것
    * **행에는 주체, 열에는 객체를 두고 행과 열의 교차점에는 주체가 객체에 대한 접근 권한(W, R, D, E)을 기술하여 이름 기반으로 제어**
    * 사용자가 비교적 소수, 분포도가 안정적일 때 적합 (지속적 변경환경에서는 부적합)

## 2. 접근 통제 매트릭스 종류
+ ACL(Access Control List) : 객체 기반 접근 제어
    * 객체 관점에서 접근 권한을 테이블 형태로 기술하여 접근 제어
    * 사용자가 비교적 소수, 분포도가 안정적일 때 적합 (지속적 변경환경에서는 부적합)
+ 내용 의존 접근 통제(Content Dependent Access Control)
    * 데이터베이스에서 가장 많이 사용, 접근제어가 내용에 의해 이루어짐
    * 예) DB File에서 직원의 경력, 인사 등의 내용이 있을 때 일반 직원은 자신의 것만 볼 수 있지만 팀장의 경우 팀의 모든 직원을 볼 수 있게 하는 방식
+ 제한적 인터페이스(Restricted Interfaces, Constricted User Interface)
    * 특정 기능이나 자원에 대한 접근 권한이 없을 경우 아예 접근을 요청하지 못하도록 하는 것
    * 예) DB View

## 3. 접근 통제 모델(Access Control Model)
### 1. Bell-Lapadula 모델
* 개념
    - **기밀성 모델로서 높은 등급의 정보가 낮은 레벨로 유출되는 것을 통제하는 모델**
    - 정보구분 : Top Secret, Secret, Unclassified
    - **최초의 수학적 모델**로서 보안 등급과 범주를 이용한 강제적 정책에 의한 접근 통제 모델
    - 미 국방성(DOD)의 지원을 받아 설계된 모델로서 **오렌지북인 TCSEC의 근간**이 됨
    - 시스템의 비밀성을 보호하기 위한 보안 정책
* **No Read-Up(NRU or ss-property) : 단순 보안 규칙**
    - 주체는 자신보다 높은 등급의 객체를 읽을 수 없다
    - 주체의 취급인가가 객체의 비밀 등급보다 같거나 높아야 그 객체를 읽을 수 있다.
* **No Write-Down(NWD or *-property) = Confinement property : *(스타-보안규칙)**
    - 주체는 자신보다 낮은 등급의 객체에 정보를 쓸 수 없다.
    - 주체의 취급인가가 객체의 비밀 등급보다 낮거나 같을 경우 그 객체를 주체가 기록할 수 있다.
* **Strong *-property**
    - 더욱 강화한 모델로, 주체는 자신과 등급이 다른 객체에 대해 읽거나 쓸 수 없다.
* 단계 등급별 구분

    |Level|ss-property 읽기 권한(Read Access)|\*-property 쓰기 권한(Write Access)|Strong\*-property 읽기/쓰기(Read/Write Access)|
    |:---:|:---:|:---:|:---:|
    |높은 등급|통제|가능(OK Write Up)|통제|
    |같은 등급|가능|가능|가능|
    |낮은 등급|가능(OK Read Down)|통제|통제|

### 2. Biba 모델
* 개념
    - Bell-Lapadula 모델의 단점인 **무결성을 보장할 수 있는 모델**
    - 주체에 의한 객체 접근의 하목으로 무결성을 다룸
* Biba 모델의 속성
    - No Read Down(NRD or Simple Integrity Axiom)
    - No Write Up(NWU or \*Integrity Axiom)
* 단계 등급별 구분

    |Level|단순 무결성 규칙 (Simple Integrity Property) 읽기 권한(Read Access)|(스타) - 무결성 규칙 (Integrity \*-property) 쓰기 권한(Write Access)|
    |:---:|:---:|:---:|
    |높은 등급|가능(OK Read Up)|통제|
    |같은 등급|가능|가능|
    |낮은 등급|통제|가능(OK Write Down)

### 3. Clark and Wilson(클락윌슨 모델)
* 클락 윌슨(Clark and Wilson)
    - 무결성 중심의 상업용으로 설계한 것으로 Application의 보안 요구사항을 다룸
    - 정보의 특성에 따라 비밀 노출 방지보다 자료의 변조 방지가 더 중요한 경우가 있음을 기초로 함
    - 주체와 객체 사이에 프로그램이 존재, 객체는 항상 프로그램을 통해서만 접근이 가능
    - 2가지 무결성을 정의 : 내부 일관성(시스템 이용), 외부 일관성(감사에 활용)
* 만리장성 모델(Chinese Wall = Brewer-Nash)
    - 서로 상충 관계에 있는 객체 간의 정보 접근을 통제하는 모델(이익의 상충 금지)이다.
    - 상업적으로 기밀성 정책에 따른다.

# 보안 운영체제(Secure OS)
* **보안 운영체제(Secure OS)**
    + 기존의 운영체제에서 발생 가능한 보안 취약성으로부터 시스템 자체를 보호하기 위해 기존 운영체제의 커널 등급에 부가적인 보안 기능을 강화시킨 운영체제

## 1. Secure DBMS 구조

|구조|설명|
|:---|:---|
|신뢰 필터 구조|⦁ 신뢰할 수 없는 전위 사용자 인터페이스와 후위 데이터베이스 사이에 신뢰 필터를 사용하여 데이터에 대한 접근 통제 및 보안 서비스를 제공<br/>⦁ 신뢰 필터는 하부의 보안 운영체제가 제공하는 보안 서비스 및 메커니즘에 의존<br/>⦁ 장점 : 다른 구조에 비해 간단하며 작기 때문에 보안 기능의 검증 및 평가에 용이<br/>⦁ 단점 : 데이터의 보안을 침해하는 일부의 위협에 대해서는 취약점을 지님|
|커널 구조|⦁ 커널 구조는 TCB 분할 개념에 의해서 구현됨. 따라서 데이터베이스 시스템은 보안 커널 외부에 존재하면서 임의적으로 보안만을 관리<br/>⦁ 데이터베이스 객체에 대한 임의적 접근 통제 : DBMS에 의해 수행<br/>⦁ 데이터베이스 파일에 대한 임의적 접근 통제 및 모든 강제적 접근 통제 : 하부의 보안 운영체제에 의하여 제공|
|이중 커널 구조|강제적 보안 기능을 갖는 데이터베이스 시스템을 구현하고, 이를 보안 운영체제와 함께 시스템의 TCB로 간주하여 보안 시스템을 평가|
|중복 구조|낮은 보호 수준의 데이터를 데이터베이스에 중복하여 저장하는 방식|

## 2. 은닉채널(Convert Channel)
+ 기본 통신채널에 기생하는 통신채널로서 은닉메시지를 송신자와 수신자만 확인할 수 있다. 그 이유는 메시지가 스테가노그래피로 숨겨져 있어서 다른 사용자는 확인할 수 없기 때문이다.

# 키 분배 프로토콜
## 1. 대칭키 암호화(Symmetric Key)
+ **대칭키 암호화 기법**
    * 암호화 키와 복호화 키가 동일한 암호화 방식으로, 양방향 암호화 기법이다.
    * 암호문을 송신하거나 수신하는 사용자는 사전에 암호화키를 교환해야 한다.
    * Session Key, Shared Key, Secret Key, 대칭키(Symmetric Key), 관용키 (Conventional Key) 라고도 함.

    ![Symmetric Key](/assets/images/posts/Symmetric_key_encryption.svg.png)

    [이미지 출처](https://commons.wikimedia.org/wiki/File:Symmetric_key_encryption.svg)

+ 대칭키 암호화 기법의 특징
    * 기밀성을 제공하나 무결성, 인증, 부인방지는 보장할 수 없으며, 암호화와 복호화 속도가 빠르다.
    * 같은 키를 사용하므로 안전한 키 전달 및 공유 방법이 필요하며, 대용량 Data 암호화에 적합하다.
+ 세션키(Session Key)
    * IPSEC, SSL 등을 학습하다 보면 세션키라는 말이 나온다. 세션키는 송신자와 수신자가 연결하고 있는 동안만 사용하는 암호화키이며, 송신자와 수신자가 같은 암호화키를 가지고 있는 것이다. 세션키는 다른 말로 임시키라고도 한다. 즉, 연결이 종료되면 세션키는 사라진다.
+ **대칭키 암호화의 종류**

    |구분|스트림 암호(Stream Cipher)|블록 암호(Block Cipher)|
    |:---|:---|:---|
    |개념|하나의 Bit, 또는 바이트 단위로 암호화|여러 개의 Bit를 묶어 블록 단위로 암호화|
    |방법|평문을 XOR로 1Bit 단위로 암호화|블록 단위로 치환.대칭을 반복하여 암호화|
    |장점|실시간 암호, 복호화, 블록 암호화보다 빠름|대용량의 평문 암호화|
    |종류|RC4, SEAL, OTP|DES, 3DES, AES, IDEA, Blowfish, SEED|

## 2. 공개키 기반 키 분배 방식의 원리
+ **공개키 암호화(Public Key)의 주요 특징**
    * 암호화 키와 복호화 키가 다른 암호화 방식, 키 교환은 키 합의 또는 키 전송을 사용한다.
    * 공개키/개인키를 사용하여 인증, 서명, 암호화를 수행한다.
    * **공개키로 암호화하여 메시지를 전송, 개인키로 복호화한다.**
    * 대칭키 암호화 기법의 키 공유 문제를 해결한 방법이지만 암호화키의 길이가 길어서 암호화 및 복호화의 성능이 떨어진다.
+ 공개키 암호화의 필요성

    |필요성|주요 내용|
    |:---|:---|
    |키 관리 문제|비밀키의 배분, 공유 문제, 수많은 키의 저장 및 관리 문제|
    |인증|메시지의 주인을 인증할 필요|
    |부인방지|메시지의 주인이 아니라고 부인하는 것을 방지|

+ 공개키 암호화 방식

    ![Public Key](/assets/images/posts/Public_key_encryption.svg.png)

    [이미지 출처](https://en.wikipedia.org/wiki/Public-key_cryptography)

+ **공개키 암호화 종류**

    |구분|특징|수학적 배경|장점|단점|
    |:---|:---|:---|:---|:---|
    |Diffie Hellman|⦁ 최초의 공개키 알고리즘<br/>⦁ 키 분배 전용 알고리즘|이산대수 문제|⦁ 키 분배에 최적화<br/>⦁ 키는 필요 시에만 생성, 저장 불필요|⦁ 암호 모드로 사용 불가(인증 불가)<br/>⦁ 위조에 취약|
    |RSA|대표적 공개키 알고리즘|소인수분해|여러 Library 존재|컴퓨터 속도의 발전으로 키 길이 증가|
    |DSA|전자서명 알고리즘 표준|이산대수 문제|간단한 구조(Yes or No의 결과만 가짐)|⦁ 전자서명 전용<br/>⦁ 암호화, 키 교환 불가|
    |ECC|⦁ 짧은 키로 높은 암호 강도<br/>⦁ PDA, 스마트폰, 핸드폰|타원 곡선|⦁ 오버헤드 적음<br/>⦁ 160키 = RSA 1024|키 테이블(20 Kbyte) 필요|

## 3. 공개키 암호화와 대칭키 암호화 방식의 차이점

|항목|대칭키 암호화|공개키 암호화|
|:---|:---|:---|
|**키 관계**|암호화 키 = 복호화 키|암호화 키 != 복호화 키|
|**안전한 키 길이**|128Bit 이상|2048Bit 이상|
|**구성**|비밀키|공개키,개인키|
|**키 개수**|N(N-1)/2|2N(키 쌍으로는 N)|
|**대표적인 예**|DES, 3DES, AES|RSA, ECC|
|**제공서비스**|기밀성|기밀성, 부인방지, 인증|
|**목적**|Data 암호화|대칭키 암호화 전달(키 분배용)|
|**단점**|키 분배 어려움, 확장성 떨어짐|중간자 공격(대응 PKI)|
|**암호화 속도**|공개키(비대칭키)보다 빠름|대칭키보다 느림|


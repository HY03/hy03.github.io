---
title: 03.애플리케이션 보안 - 04.기타 애플리케이션 보안
date: '2020-10-28 06:00:00'
tags:
- 정보보안기사
- 애플리케이션 보안
- 기타 애플리케이션 보안
related: true
categories:
- IS_Certification
toc: true
---

# **웹 취약점 및 버그 유형**
## 1. OWASP Top 10
* **OWASP(The Open Web Application Security Project)**
    - 신뢰성 있는 웹 애플리케이션 개발 및 운영을 위한 웹 취약점의 우선순위와 위험도 기준의 정보보안 가이드라인을 제시

### 1. 인젝션
* 개요
    - SQL, OS, XXE, LDAP 인젝션 취약점
    - 적절한 입력 유효성 검사 없이 신뢰할 수 없는 데이터가 명령이나 쿼리문의 일부분으로 인터프리터로 전송될 때 발생
    - 예상치 못한 명령 실행, 접근권한 없이 데이터 접근 등의 문제 발생
* 대응방안
    - 서버 측 입력 검증, 특수문자 필터링 및 유효성 검사
    - 화이트리스트 조합 사용

### 2. 취약한 인증
* 개요
    - 인증 및 세션관리 관련 애플리케이션 기능이 잘못 구현됨
    - 공격자에게 취약한 암호, 세션 토큰 등을 제공
    - 다른 사용자의 권한을 얻어 익스플로잇(Exploit)
* 대응방안
    - 멀티팩터 인증 구현
    - 기본 자격 증명 사용 금지
    - 높은 수준의 암호 정책 구현
    - 지연 로그인, 무작위 세션 ID, 세션 시간 초과 등등으로 제어
    - 실패한 로그인 시도 기록(Log)

### 3. 민감한 데이터 노출
* 개요
    - 중요한 정보를 제대로 보호하지 않음
    - 암호화되어야 하는 정보들을 일반 텍스트로 노출
* 대응방안
    - 데이터를 중요도에 따라 분류
    - 암호화 적용, 적절한 키 관리 및 표준 알고리즘 사용
    - 불필요한 데이터, 민감한 데이터를 포함하는 응답을 회피
    - 캐싱 비활성화

### 4. XML 외부 개체(XXE)
* 개요
    - 잘못된 설정, 오래된 XML프로세서는 XML 문서 내에서 외부 개체를 참조함
    - 내부 시스템 스캔, 포트 스캔, 원격코드 실행, 서비스 거부 공격, 데이터 추출 등의 작업 가능
* 대응방안
    - 서버 입력 유효성 검증, 필터링, 검사
    - 모든 XML파서의 XML 외부 개체와 DTD 처리 비활성화
    - JSON과 같은 덜 복잡한 형식 사용, 중요한 데이터의 연속화 방지, 모든 XML 프로세스 및 라이브러리 패치 (SOAP 1.2이상)

### 5. 취약한 접근 통제
* 개요
    - 인증된 사용자가 수행할 수 있는 작업에 대한 제한이 제대로 적용되지 않을 때 발생
    - 결함을 악용하여 타 사용자 계정에 접근 혹은 중요 파일 노출, 데이터 수정, 접근권한 변경이 가능
* 대응방안
    - 배타적 자원허용
    - 액세스 제어 실패 및 알림 로그 유지
    - 교차 출처 리소스 공유(Cross-Origin Resource Sharing, CORS) 사용 최소화

### 6. 잘못된 보안 구성
* 개요
    - 응용 프로그램의 데이터를 안전하게 유지하는 제어장치의 부적절한 구현
    - 보안 헤더를 잘못 설정
    - 민감한 정보를 노출하는 오류메시지
    - 패치 또는 업그레이드 시스템을 무시, 기본 구성을 사용
* 대응방안
    - 불필요하고 사용되지 않는 기능 또는 프레임워크 제거
    - 안전한 설치 프로세스 구현
    - 제로 트러스트 모델 (내/외부를 막론하고 적절한 인증 절차 없이 누구도 신뢰하지 않음)을 구현
        + 보호가 필요한 자산 및 데이터 
    - 사용자 권한 검토 및 업데이트

### 7. 크로스사이트 스크립팅(XSS)
* 개요
    - 다른 사람의 웹 응용 프로그램의 페이지 출력에 스크립트를 주입
    - 브라우저는 페이지의 일부로 믿고 스크립트를 실행
    - 예) 링크를 클릭하면 피해자의 웹 브라우저에서 스크립트가 실행되어 세션, 쿠키, 사용자 자격 증명을 훔치거나 악성코드를 전달
* 대응방안
    - 상황에 맞는 인코딩을 적용
    - 엔티티 적용

### 8. 안전하지 않은 역직렬화
* 개요
    - 직렬화 개념 : 응용 프로그램 코드에서 객체를 바이트 스트림으로 변환하거나 디스크에 저장하는 등의 다른 용도로 사용할 수 있는 형식으로 변환
    - 역직렬화 과정 중 DoS 공격이나 원격 코드 실행과 같은 심각한 결과를 초래할 수 있도록 변조되는 것을 말함
* 대응방안
    - 악성객체 생성이나 데이터 변조 방지를 위해 직렬화된 객체에 대한 디지털서명과 같은 무결성 검사를 구현
    - 엄격한 형식 제약 조건을 적용
    - 낮은 권한의 환경에서 코드를 분리하여 실행
    - 모니터링하면서 예외와 오류를 기록

### 9. 알려진 취약점이 있는 구성요소 사용
* 개요
    - 라이브러리 등의 구성요소를 사용하면서 검증하지 않거나 해당 요소의 업데이트된 버전을 사용하지 않고 특정 기능을 구현하는 광범위한 문제
* 대응방안
    - 클라이언트 및 서버 측 구성요소의 종속성과 함께 버전 업데이트, 패치

### 10. 불충분한 로깅 및 모니터링
* 대응방안
    - 위심스러운 활동 (로그인 실패, 엑세스 제어 실패, 입력 유효성 검사 실패) 을 기록, 중앙 집중식 로그관리 솔루션 이용
    - 변조 또는 삭제를 방지하기 위한 감사 시스템

## 2. 주요 정보통신기반시설 취약점 분석, 평가기준 고시
+ 주요 정보통신기반시설로 지정된 **첫 회는 지정 후 6개월 이내**에 취약점 분석 및 평가를 실시해야 한다.
    * 6개월 이내 취약점 분석 및 평가를 수행치 못할 경우 관할 중앙행정기관의 장의 승인을 얻어 **3개월 연장** 가능하다.

# 웹 취약점 및 버그 방지 개발 방법
## 1. 소프트웨어 개발 보안(보안 약점)
+ 안전한 소프트웨어 개발을 위해, 소스코드 등에 존재할 수 있는 잠재적인 보안 약점 제거, 보안을 고려한 기능 설계, 구현 등 SDLC(Software Development Life Cycle) 전반에서 수행하는 일련의 보안 활동
+ SDLC 단계별 소프트웨어 개발 보안 활동

    |1.요구사항 분석|2.설계단계|3.구현단계|
    |:---|:---|:---|
    |**보안 항목 요구사항 식별**<br/>⦁ 암호화해야 할 중요 정보<br/>⦁ 중요 기능에 대한 분류(시스템 리부팅)<br/>**보안 요구 항목 20개-세분화 54개**|**위협원 도출**<br/>⦁ 위협 모델링: 보안 전문가, 개발 전문가<br/>⦁ 전문방법론(도구):MS 등에서 제공<br/>⦁ 분석/설계 20개, 구현 54개의 위협원이 반영될 수 있도록 설계|**표준 코딩, SW 개발 보안 가이드 준수**<br/>⦁ 구현단계에서 준수할 수 있도록 설계단계에서 가이드<br/>**개발 가이드 제공**<br/>⦁ 개발 보안 가이드를 래퍼런스로 하여 개발 환경에 적합하도록 구축<br/>⦁ 표준 코딩 가이드는 사이트마다 작성<br/>**소스코드 보안 약점 진단(도구 활용)**|

* Secure SDLC

    |방법론|내용|
    |:---|:---|
    |**CLASP**|⦁ OWASP의 보안 약점을 고려한 개발 보안 방법론<br/>⦁ 개념, 역할, 활동을 정의한 모델|
    |**MS-SDL**|⦁ PreSDL 단계에서 위협분석을 수행하고, '분석, 설계, 구현, 테스트'의 SDLC 단계에서 보안을 고려한 모델링을 수행<br/>⦁ Microsoft의 자동화 진단 도구로 활용|
    |**7-Touch Point**|위협분석, 코드리뷰, 자동화 진단 등의 활동을 정의|

## 2. 분석/설계단계 개발 보안 활동
### 1. 분석단계 활동
* 개요
    - '처리 대상 정보'와 '해당 정보를 처리하는 기능'을 대상으로, 적용해야 하는 보안 항목을 식별
    - 사용자 권한 : 권한을 가진 사용자만이 안전하게 수집, 전송, 처리, 보관, 폐기해야 하는 정보를 식별한다.
    - 법/제도 : 개인정보보호법, 정보통신망법, 금융거래법 등 다양한 법, 제도, 규정에 의하여 보호해야 하는 정보를 정의한다.
    - 보안정책 : 내부정책(개인정보보호 규정, 정보보안 규정)과 외부정책(관련 지침) 자료 등을 기준으로 보안 항목을 식별한다.
* 요구사항1 : 입력 데이터 검증 및 표현 (10개 항목)
    - DBMS 조회 및 결과 검증
        + SQL문 생성 시 사용하는 입력값과 쿼리 결과에 대한 검증 방법을 설계
        + 유효하지 않은 값에 대한 처리 방법을 설계
    - XML 조회 및 결과 검증
        + XPath, XQuery 생성 시 사용하는 입력값과 조회 결과에 대한 검증 방법을 설계
        + 유효하지 않은 값에 대한 처리 방법을 설계
    - 디렉터리 서비스 조회 및 결과 검증
        + LDAP 생성 시 사용하는 입력값과 조회 결과에 대한 검증 방법을 설계
        + 유효하지 않은 값에 대한 처리 방법을 설계
    - 시스템 자원 접근 및 명령어 수행 입력값 검증
        + 시스템 자원 접근 및 명령어 수행을 위해 사용하는 입력값과 조회 결과에 대한 검증 방법을 설계
        + 유효하지 않은 값에 대한 처리 방법을 설계
    - 웹 서비스 요청 및 결과 검증
        + 웹 서비스 요청과 응답 결과에 대한 검증 방법과 적절하지 않은 데이터에 대한 처리 방법을 설계
        + 예) 게시판에 스크립트 게시 후 게시 항목 응답
    - 웹 기반 중요 기능 수행요청 유효성 검증
        + 사용자 권한 확인이 필요한 중요 기능 및 서비스 요청에 대한 유효성 검증 방법을 확인
        + 유효하지 않은 요청에 대한 처리 방법을 설계(예: 결제 기능에 대한 인증)
    - HTTP 프로토콜 유효성 검증
        + 사용자가 원하지 않은 결과를 생성할 수 있는 HTTP 헤더 및 응답 결과에 대한 유효성을 검증하는 방법을 설계
        + 유효하지 않은 요청의 처리 방법을 설계 (예: 비정상적인 HTTP 헤더, 자동연결 URL 링크)
    - 허용된 범위 내 메모리 접근
        + 허용된 범위의 메모리 버퍼에만 접근하여 저장 및 읽기를 수행하고 버퍼오버플로우가 발생하지 않도록 처리 방법을 설계
    - 보안기능 동작에 사용되는 입력값 검증
        + 보안기능 동작을 위해 사용하는 입력값과 함수의 외부 입력값 및 수행결과에 대한 처리 방법을 설계 (예: 인증, 인가, 권한부여 등의 보안 기능)
    - 업로드/다운로드 파일 검증
        + 업로드/다운로드 파일의 무결성, 실행권한 등에 관한 유효성 검사 방법과 검사 대응 방안을 설계
* 요구사항2 : 보안기능(8개 항목)
    - 인증 대상 및 방식
        + 중요 정보 및 기능과 인증방식을 정의하고, 정보접근과 중요 기능 수행 시 인증 기능을 우회할 수 없도록 설계
    - 인증 수행 제한
        + 반복적인 인증 시도를 제한하도록 설계
        + 인증실패 시 인증제한 기능을 설계
    - 비밀번호 관리
        + 안전한 비밀번호 조합규칙을 설정하고 주기적으로 변경하도록 설계 (예: 패스워드 길이, 허용문자 조합 및 하드코딩 금지)
    - 중요 자원 접근 통제
        + 중요 자원을 정의하고, 정의한 중요 자원에 대한 접근을 통제하는 신뢰할 수 있는 방법과 접근 통제 실패 시 대응방안을 설계 (예: 주요 설정, 민감정보에 대한 접근 권한)
    - 암호키 관리
        + 암호키의 생성, 분배, 접근, 파기 등 암호키 생명주기를 안전하게 관리할 수 있는 방법을 설계
    - 암호연산
        + 국제표준 혹은 검증된 프로토콜로 인정된 안전한 암호 알고리즘을 서정하여, '충분한 암호키 길이', '솔트', '충분한 난수값'에 기반한 암호연산의 수행 방법을 설계
    - 중요 정보 저장
        + 중요 정보 저장 시, 안전한 저장 및 관리 방법을 설계 (예: 패스워드, 개인정보, 민감정보)
    - 중요 정보 전송
        + 중요 정보 전송 시, 안전한 전송 방법을 설계 (예: 정책에 의하여 중요 정보로 분류한 정보)
* 요구사항3 : 에러처리(1개 항목)
    - 예외처리
        + 오류 메시지에 중요 정보가 포함되어 출력되거나, 에러 및 오류가 부적절하게 처리되어 의도하지 않은 상황이 발생하는 것을 방지하기 위하여 안전한 처리 방법을 설계 (예: 개인정보, 시스템정보 등의 노출 방지)
* 요구사항4 : 세션통제(1개 항목)
    - 세션통제
        + 세션을 안전하게 관리할 수 있는 방법을 설계 (예: 세션간 데이터 공유금지, 세션 ID 노출 방지, 재로그인시 세션 ID 변경, 세션 유효기간 등)

### 2. 설계단계의 보안 요구 사항 적용
* 요구사항정의서의 보안 기능 요구사항 확인
* 요구사항 추적표를 활용한 SDLC 단계별 산출물 확인
* 적절한 보안대책 반영여부를 진단

## 3. 구현단계 개발 보안 활동 (보안약점)
* **소스코드 보안 취약점 7개**
### 1. 입력데이터 검증 및 표현
- 프로그램 입력 값에 대한 검증 누락 또는 부적절한 검증, 데이터의 잘못된 형식 지정으로 인해 발생할 수 있는 보안 약점
* **SQL 삽입** : 사용자의 입력 값 등 외부 입력 값이 SQL 쿼리에 삽입되어 공격자가 쿼리를 조작해 공격할 수 있는 보안 약점
* **경로조작 및 자원 삽입** : 외부 입력 값에 대한 검증이 없거나 혹은 잘못된 검증을 거쳐서 시스템 자원에 접근하는 경로 등의 정보로 이용될 때 발생하는 보안 약점
* **크로스사이트스크립트** : 검증되지 않은 외부 입력 값에 의해 브라우저에서 악의적인 코드가 실행되는 보안 약점
* **운영체제 명령어 삽입** : 운영체제 명령어를 구성하는 외부 입력 값이 적잘한 필터링을 거치지 않고 쓰여서 공격자가 운영체제 명령어를 조작할 수 있는 보안 약점
* **위험한 형식 파일 업로드** : 파일의 확장자 등 파일 형식에 대한 검증 없이 업로드를 허용하여 발생하는 보안 약점
* **신뢰되지 않은 URL 주소로 자동 접속 연결** : 사용자의 입력 값 등 외부 입력 값이 링크 표현에 사용되고, 이 링크를 이용하여 악의적인 사이트로 리다이렉트(Redirect)되는 보안 약점
* **XQuery 삽입** : 사용자의 입력 값 등 외부 입력 값이 XQuery 표현에 삽입되어 악의적인 쿼리가 실행되는 보안 약점
* **XPath 삽입** : 사용자의 입력 값 등 외부 입력 값이 XPath 표현에 삽입되어 악의적인 쿼리가 실행되는 보안 약점
* **LDAP 삽입** : 검증되지 않은 입력 값을 사용해서 동적으로 생성된 LDAP문에 의해 악의적인 LDAP 명령이 실행되는 보안 약점
* **크로스사이트 요청 위조**
    - 검증되지 않은 외부 입력 값에 의해 브라우저에서 악의적인 코드가 실행되어 공격자가 원하는 요청(Request)이 다른 사용자(관리자 등) 권한의 서버로 전송되는 보안 약점
    - 지정된 경로 밖의 파일 시스템 경로에 접근하게 되는 보안 약점
* **HTTP 응답 분할** : 사용자의 입력 값 등 외부 입력 값이 HTTP 응답 헤더에 삽입되어 악의적인 코드가 실행되는 보안 약점
* **정수 오버플로** : 정수를 사용한 연산의 결과가 정수값의 범위를 넘어서는 경우 프로그램이 예기치 않게 동작할 수 있는 보안 약점
* **보안기능 결정에 사용되는 부적절한 입력값** : 사용자에 의해 변경될 수 있는 값을 사용하여 보안 결정(인증/인가/권한부여 등)을 수행하여 보안 매커니즘이 우회될 수 있는 보안 약점
* **메모리 버퍼 오버플로** : 버퍼를 이용하여 메모리를 사용할 때, 버퍼의 크기보다 큰 데이터를 버퍼에 기록하는 경우 데이터가 버퍼의 경계를 넘어 다른 메모리 영역을 침범하기 때문에 발생하는 보안 약점
* **포맷 스트링 삽입** : printf(), fprintf(), sprintf() 와 같이 포맷 스트링을 사용하는 함수를 사용하는 경우, 외부로부터 입력된 값을 검증하지 않고 입출력 함수의 포맷 문자열 그대로 사용하는 경우 발생할 수 있는 보안약점

### 2. 보안 기능
* 보안 기능(인증, 접근 제어, 기밀성, 암호화, 권한 관리 등)을 적절하지 않게 구현 시 발생할 수 있는 보안 약점
* 적절한 인증 없는 중요 기능 허용 : 적절한 인증 없이 중요 정보(계좌이체 정보, 개인정보 등)를 열람(또는 변경)할 수 있도록 하는 보안 약점
* 부적절한 인가 : 적절한 접근 제어 없이 외부 입력 값을 포함한 문자열로 서버 자원에 접근(혹은 서버 실행 인가)할 수 있게 하는 보안 약점
* 중요한 자원에 대한 잘못된 권한 설정 : 중요 자원(프로그램 설정, 민감한 사용자 데이터 등)에 대한 적절한 접근 권한을 부여하지 않아 의도하지 않은 사용자에 의해 중요 정보가 노출,수정되는 보안 약점
* 취약한 암호화 알고리즘 사용 : 중요 정보(패스워드, 개인정보 등)의 기밀성을 보장할 수 없는 취약한 암호 알고리즘을 사용하여 정보가 노출될 수 있는 보안 약점
* 중요 정보 평문저장 : 중요 정보(패스워드, 개인정보, 사용자 권한 정보 등)를 암호화하여 저장하지 않아 발생할 수 있는 보안 약점
* 중요 정보 평문전송 : 중요 정보(패스워드, 개인정보, 사용자 권한 정보 등) 전송 시 암호화하거나 암호화 채널을 통하지 않는 경우 발생할 수 있는 보안 약점
* 하드코드 된 비밀번호 : 소스코드 내에 비밀번호를 하드코딩함에 따라 관리자 비밀번호가 노출되거나 주기적 변경 등 수정(관리자 변경 등)이 쉽지 않은 보안 약점
* 충분하지 않은 키 길이 사용 : 데이터의 기밀성, 무결성 보장을 위해 사용되는 키의 길이가 충분하지 않아 기밀정보 누출, 무결성이 깨지는 보안 약점
* 적절하지 않은 난수 값 사용 : 예측 가능한 난수 사용으로 공격자로 하여금 다음 숫자 등을 예상하여 시스템 공격이 가능한 보안 약점
* 하드코드 된 암호화 키 : 소스코드 내에 암호화 키를 하드코딩 하는 경우 향후 노출될 수 있으며, 키 변경 등 수정이 쉽지 않은 보안 약점
* 취약한 비밀번호 허용 : 비밀번호 조합규칙(영문, 숫자, 특수문자 등) 및 길이가 충분하지 않아 노출될 수 있는 보안 약점
* 사용자 하드디스크에 저장되는 쿠키를 통한 정보 노출 : 쿠키(세션 ID, 사용자 권한 정보 등 중요 정보)를 사용자 하드디스크에 저장함으로써 개인정보 등 기밀정보가 노출될 수 있는 보안 약점
* 주석문 안에 포함된 패스워드 등 시스템 주요 정보 : 소스코드 내의 주석문에 비밀번호가 하드코딩 되어 비밀번호가 노출될 수 있는 보안 약점
* 비밀번호 일방향 해시 함수 사용 : 공격자가 솔트 없이 생성된 해시값을 얻게 된 경우, 미리 계산된 레인보우 테이블을 이용하여 원문을 찾을 수 있는 보안 약점
* 무결성 검사 없는 코드 다운로드 : 원격으로부터 소스코드 또는 실행파일을 무결성 검사 없이 다운로드 받고 이를 실행하는 경우 공격자가 악의적인 코드를 실행할 수 있는 보안 약점
* 반복된 인증시도 제한 기능 부재 : 프로그램 내에서 로그인과 같은 인증시도를 하는 수를 제한하지 않거나 인증시도에 대한 측정을 구현하지 않아서 발생하는 보안 약점

### 3. 시간 및 상태 
* 동시 또는 거의 동시 수행을 지원하는 병렬 시스템, 하나 이상의 프로세스가 동작하는 환경에서 시간 및 상태를 부적절하게 관리하여 발생할 수 있는 보안 약점
* 경쟁조건-검사시점과 사용시점(TOCTOU) : 멀티프로세스 상에서 자원을 검사하는 시점과 사용하는 시점이 달라서 발생하는 보안 약점
* 제어문을 사용하지 않는 재귀함수 : 적절한 제어문 사용이 없는 재귀함수에서 무한 재귀가 발생하는 보안 약점

### 4. 에러처리
* 에러를 처리하지 않거나, 불충분하게 처리하여 에러정보에 중요 정보(시스템 등)가 포함될 때 발생할 수 있는 보안 약점
* 오류 메시지를 통한 정보 노출 : 개발 시 활용을 위한 오류정보의 출력 메시지를 배포될 버전의 S/W에 포함시킬 때 발생하는 보안 약점
* 오류 상황 대응 부재 : 시스템에서 발생하는 오류 상황을 처리하지 않아 프로그램 다운 등 의도하지 않은 상황이 발생할 수 있는 보안 약점
* 적절하지 않은 예외 처리 : 예외에 대한 부적절한 처리로 인해 의도하지 않은 상황이 발생할 수 있는 보안 약점

### 5. 코드 오류
* 타입변환 오류, 자원(메모리)의 부적절한 반환 등과 같이 개발자가 범할 수 있는 코딩오류로 인해 유발되는 보안 약점
* 널(Null) 포인터 역참조 : Null로 설정된 변수의 주소값을 참조했을 때 발생하는 보안 약점
* 부적절한 자원 해제 : 사용된 자원을 적절히 해제하지 않으면 자원의 누수 등이 발생하고, 자원이 모자라 새로운 입력을 처리하지 못하는 보안 약점
* 해제된 자원 사용 : 메모리를 해제한 자원을 참조할 경우, 예기치 않은 오류가 발생할 수 있는 보안 약점
* 초기화되지 않은 변수 사용 : 변수를 초기화하지 않고 사용하는 경우 예기치 않은 오류가 발생할 수 있는 보안 약점

### 6. 캡슐화
* 중요한 데이터 또는 기능성을 불충분하게 캡슐화하였을 때 인가되지 않은 사용자에게 데이터 누출이 가능해지는 보안 약점
* 잘못된 세션에 의한 데이터 정보 노출 : 잘못된 세션에 의해 권한 없는 사용자에게 데이터 노출이 일어날 수 있는 보안 약점
* 제거되지 않고 남은 디버그 코드 : 디버깅을 위해 작성된 코드를 통해 권한 없는 사용자의 인증우회(또는 중요 정보) 접근이 가능해지는 보안 약점
* 시스템 데이터 정보 노출 : 사용자가 볼 수 있는 오류 메시지나 스택 정보에 시스템 내부 데이터나 디버깅 관련 정보가 공개되는 보안 약점
* Public 메소드로부터 반환된 Private 배열 : private로 선언된 배열을 public으로 선언된 메소드를 통해 반환(return)하면 그 배열의 레퍼런스가 외부로 공개되어 외부에서 배열이 수정될 수 있는 보안 약점
* Private 배열에 Public 데이터 할당 : public으로 선언된 데이터 또는 메소드의 인자가 private 선언된 배열에 저장되면 private 배열을 외부에서 접근할 수 있게 되는 보안 약점

### 7. API 오용
* 의도된 사용에 반하는 방법으로 API를 사용하거나 보안에 취약한 API를 사용할 때 발생할 수 있는 보안 약점
* DNS lookup 에 의존한 보안 결정 : DNS는 공격자에 의해 DNS 스푸핑 공격 등이 가능하므로 보안 결정을 DNS 이름에 의존할 경우, 보안 결정 등이 노출되는 보안 약점
* 취약한 API 사용 : 취약하다고 알려진 함수를 사용할 경우 예기치 않은 오류가 발생할 수 있는 보안 약점

## 4. SQL Injection
### 1. SQL Injection
* 개요
    - `WHERE ID = '1' ...` : 1을 파라미터 입력한 정상 구문
    - `WHERE ID = '' OR 1=1 #` : **' OR 1=1 #** 을 입력하여 공격
* **SQL Injection의 특징**
    - 개념
        + 사용자가 서버에 제출한 데이터가 SQL Query로 사용되어 데이터베이스 및 응용 시스템에 영향을 주는 공격기법
        + SQL문을 조작하거나 오류를 발생시켜 정보를 유출하거나 변조
        + OWASP TOP 10에서 가장 위험한 공격 기법 중 하나
    - 발생 원인
        + 공격자의 입력 값이 데이터베이스의 쿼리 작성에 이용되는 환경에서 입력 값 미검증 또는 부적절한 검증
        + 동적으로 Query 구문이 완성되는 애플리케이션
    - 결과
        + 쿼리 조작을 통한 데이터베이스 노출 및 변조
        + 웹 애플리케이션 인증 우회
        + 데이터베이스 덤프, 파괴
        + 시스템 커맨드의 실행 (MS-SQL)
        + 시스템 주요 파일 노출
        + DDoS 공격
    - 공격 도구
        + Havij
        + Pangolin
        + HDSL
    - 대응 방안
        + 입력 값 필터링
        + 입력 값 크기 제한
        + Dynamic SQL 지양
        + ORM 사용 지향
        + **Prepared Statement 사용**
        + 데이터 타입 패턴 체크
        + 데이터베이스 권한 관리
        + 공통 오류 페이지 사용(오류 반환 설정)
        + WAS/IDS
        + Stored Procedure 사용

### 2. Blind SQL Injection
* 개요
    - 문자열을 하나씩 자르고 ASCII 코드 값을 입력하여 해당 문자열을 완성하는 공격 기법
    - SQL 실행결과가 True / False 로 오기 때문에 입력해본 문자열이 맞는지 틀린지 공격자가 알 수 있음
* **SQL Injection의 유형**
    - 인증 우회
        + 취약한 인증방식(ID/PW를 입력받아 ID와 PW가 일치하는 레코드가 존재하는지 검사하는 방식)에서 SQL문을 조작하여 PW를 무력화
        + WHERE 절 이하의 조건이 항상 참이 되도록 하고, 쿼리 문에 에러가 없어야 함
            * **인증용 SQL문** : `SELECT * FROM USER WHERE ID="입력 값" AND PW="입력 값";`
            * **변조된 SQL문** : `SELECT * FROM USER WHERE ID='' OR '1'='1' AND PW=''--`
            * **변조된 SQL문** : `SELECT * FROM USER WHERE ID='NAME'; DELETE FROM USER--`
    - **Blind SQL Injection**
        + SQL Injection에 대응하기 위해 내부 데이터베이스 오류를 보여주지 않도록 설정한 경우, 참과 거짓을 구분할 수 있는 구문을 만들어 데이터를 알아내는 방법
        + SQL Injection에서 데이터를 삽입 및 수정하려면 DB 스키마를 먼저 파악해야 함 (DB 이름, 테이블 명, 컬럼명, 컬럼타입 순)
            * **변조된 SQL문** : **`' AND SUBSTRING(DB_NAME(),1,1)='W'`**
    - Mass SQL Injection
        + 한 번의 공격으로 대량의 DB값이 변조되어 서비스에 치명적인 악영향을 끼치는 확장된 개념의 SQL Injection 공격 기법
        + **Cookie Injection** : Get/Post가 아닌 **Cookie를 통해 데이터가 전달되는 방식**으로, 대부분의 WAF(Web Application Firewall)에서 조차 Get/Post 방식만을 검사하기 때문에 우회할 수 있는 통로로 활용되어 Mass SQL Injection 공격에 활용될 수 있음

### 3. Union SQL Injection
* 개념
    - SQL 문에 union을 입력해서 원하는 테이블에 접근
    - `' UNION SELECT FIRST_NAME, 2 FROM USERS #`

### 4. JAVA 언어에서 SQL Injection 대응 방안
* preparedStatement 이용
    - `String name = request.getParamenter("name");`
    - `String query = "SELECT * FROM USER WHERE NAME=?";`
    - `PreparedStatement pstmt = con.prepareStatement(query);`
    - `pstmt.setString(1, name);`
    - `rs = stmt.executeQuery();`

## 5. 운영체제 명령어 삽입
+ 운영체제 명령어 삽입 (Command Injection)
    * 입력 값을 검증하지 않아서 운영체제 명령어를 실행할 수 있는 취약점
    * `shell_exec` 함수 사용
    * 코드 : `shell_exec('ping ', $target );`
    * 입력값 : `192.168.0.10 | ls`
+ JAVA 언어에서 운영체제 명령어 삽입 대응방안
    * 취약 코드
        - `String cmd = new String("cmd.exe /K manDB.bat");`
        - `Runtime.getRuntime().exec(cmd + "c:\\data\\" + version);`
    * 입력값 검증 후 대입해야 함

## 6. 위험한 파일형식 업로드
+ 개요
    * **스크립트가 실행될 수 있는 프로그램을 업로드하여 악성 스크립트를 실행하게 함**
    * **이런 스크립트를 웹쉘(Web Shell) 이라고 함**
    * **코드와 같은 형태를 게시판을 통해 업로드하고 url 호출하여 서버에 명령을 실행**
+ 대응 방안
    * 탐지된 파일 삭제
    * 업로드 확장자 체크
    * 업로드 파일 이름 변경, 이동 및 실행권한 제거

## 7. XSS(Cross Site Scripting)
### 1. XSS(크로스 사이트 스크립팅) 개요
* **개요**

    ![XSS](/assets/images/posts/xss.png)

    [이미지 출처](https://www.acunetix.com/websitesecurity/cross-site-scripting/)

    - 공격자가 제공한 실행 가능한 코드를 재전송하도록 하는 공격 기법
    - 서버를 장악하지 않고도 사용자 정보가 유출될 수 있으며, 필터링을 우회할 수 있는 다양한 방법이 존재
    - OWASP TOP 10 에서 가장 위험한 공격 기법 중 하나
* **발생 원인**
    - 사용자로부터 입력된 데이터에 대한 부적절한 검증을 통하여 웹 도큐먼트로 출력
    - 서버를 경유하여 조작된 웹 페이지 및 URL을 열람하는 클라이언트를 공격
* **결과**
    - 개인정보 유출
    - Cookie Access
    - DOM(Document Object Mode) Access 를 통한 페이지 조작
    - Clipboard Access
    - Key logging
    - 악성코드 실행 및 세션 하이제킹
* **대응 방안**
    - 서버 측면
        + 사용자 입력 문자열에서 `<. >, &, "` 등을 replace 와 같은 문자변환 함수나 메소드로 `&lt, &gt, &amp, &quot` 로 치환
        + HTML tag 리스트 선정과 해당 태그만 허용 (White list)
        + 보안성이 검증된 API를 사용해 위험한 문자열 제거(OWASP의 ESAPI 활용)
        + 입력값 검증 - 서버에서 White List 방식 필터링
        + 출력 값 인코딩 - HTML 인코딩 출력
        + HTML 포맷 입력 페이지 최소화
        + 중요 정보 쿠키 저장 지양
        + 인증강화 - 세션과 IP를 통합하여 서버에서 인증
            * TCP 연결은 세션값만 확인
        + 스크립트에 의한 쿠키 접근 제한
    - 클라이언트 측면
        + 주기적 패스워드 변경
        + 브라우저 최신 패치
        + 링크 클릭 대신 URL 직접 입력
        + 브라우저 보안 옵션 등급 상향(쿠키 사용 금지)

### 2. XSS의 종류
* **Stored XSS**
    - **게시판에 악성 스크립트를 올리고 사용자가 클릭하면 악성 스크립트를 실행**
* **Reflected XSS**
    - **메일로 악성 스크립트가 포함된 첨부파일을 사용자에게 전송**
    - 해당 첨부파일을 열면 악성스크립트를 포함한 요청이 취약점이 있는 웹 서버에 전송
    - 악성 스크립트를 포함한 응답을 웹 서버로부터 받음
    - 악성 스크립트는 서버에 저장되지 않음

### 3. XSS의 공격 대상
        
|항목|내용|
|:---|:---|
|XSS에 취약한 웹 페이지|⦁ HTML을 지원하는 게시판<br/>⦁ Search Page<br/>⦁ Personalize Page<br/>⦁ Join Form Page<br/>⦁ Referer를 이용하는 Page<br/>⦁ 기타 사용자로부터 입력받아 화면에 출력하는 모든 페이지에서 발생|
|XSS 공격|이용 HTML Tag (예: `<script>,<object>,<applet>,<embed>,<img>`)<br/>언어/스크립트 : Javascript, VB Script, Active X, HTML, Flash<br/>대상 코드의 위치 : URL parameter, Form elements, Cookie, DB Query 등<br/>|
|사례|`<script>...</script>`<br/>`<img src="javascript : ...">`<br/>`<div style="background-image : url(javascript...)"></div>`<br/>`<embed>...</embed>`<br/>`<iframe>...</iframe>`|

### 4. XSS에 안전한 JAVA 소스코드
* 취약한 소스코드
    - `<p>NAME : <%=name%></p>`
* 안전한 소스코드
    - `name = name.replaceAll("<", "&lt;");`
    - `name = name.replaceAll(">", "&gt;");`
    - `name = name.replaceAll("&", "&amp;");`
    - `name = name.replaceAll(""", "&quot;");`
    - `<p>NAME : <%=name%></p>`

## 8. CSRF(Cross Site Request Forgery, 크로스 사이트 요청 위조)
+ 개념
    * 어플리케이션 취약점 중 하나로 인터넷 사용자(희생자)가 자신의 의지와는 무관하게 공격자가 의도한 행위(수정, 삭제, 등록 등)를 특정 웹사이트에 요청하게 만드는 공격
    * 피해자에 대해 **사용자가 인식하지 못한 상황에서 의도하지 않은 공격 행위를 수행하게 하는 공격**
    * 세션쿠키, SSL 인증서 등 **자동으로 입력된 신뢰정보를 기반으로 사용자의 요청을 변조하여 해당 사용자의 권한으로 악의적 공격 수행**
    * 사용자를 통해 공격이 이루어지기 때문에 공격자 추적이 불가능
    * 세션 라이딩, 원 클릭 공격, 악의적 연결, 자동화된 공격, SEA SURF, IFRAME EXPLOIT 등으로 불림
+ 과정
    * 악성 스크립트가 포함된 스크립트를 웹 서버에 올림
    * 사용자는 악성 스크립트가 포함된 스크립트를 요청
    * 악성 스크립트는 웹 서버에 서비스를 요청
    * 사용자는 이미 인증을 받았기 때문에 정상적인 사용자가 웹 서버를 공격하게 됨
+ 예제
    * 위조 요청을 전송하는 서비스에 사용자가 로그인한 상태
    * 사용자가 해커가 만든 피싱 사이트에 접속 혹은 XSS 공격을 성공한 정상 사이트를 통해 CSRF 공격을 수행
+ 주요 특징

    |구분|내용|
    |:---|:---|
    |발생원인|⦁ 웹 서버에 외부의 입력 값에 대한 인증서 등을 저장하여 해당 내용을 열람하게 할 경우 발생<br/>⦁ 이메일, 특정 웹사이트 접속 사용자 등이 웹사이트 정보를 로딩하는 과정에서 다른 URL을 요청하게 함|
    |결과|관리자 계정인 경우 권한 관리, 게시물 삭제, 사용자 등록 및 삭제|
    |대응 방안|⦁ 입력 폼 작성 시 POST 방식을 사용<br/>⦁ 입력 폼과 해당 입력 처리 프로그램 간에 토큰을 사용<br/>⦁ 중요한 기능에 대해 세션 검증과 재인증 처리 또는 트랜잭션 서명을 수행<br/>⦁ referrer 검증|

## 9. 포맷 스트링(Format String)
+ 개요
    * **데이터에 대한 포맷 스트링을 정확하게 정의하지 않아서 발생되는 보안 취약점**
    * printf 함수에 의해서 해석되는 문자열 "str"은 문자열이 아니라 형식 지시자를 포함한 포맷스트링으로 인식
    * 버퍼 오버플로우처럼 가장 심각한 보안 취약점
    * **포맷 스트링 문자를 사용한 메모리 열람, 메모리 변조, 셀코드(Shell Code) 삽입과 같은 보안 취약점이 발생**
    * 대상 함수 목록
        - Fprintf
        - printf
        - sprintf
        - snprintf
        - vfprintf
        - vprintf
        - vsprintf
        - syslog

# DRM 기술 개념 및 활용
## 1. DRM(Digital Rights Management) 개요
+ **DRM(Digital Rights Management)**
    * 디지털 컨텐츠를 안전하게 보호할 목적으로 암호화 기술을 이용
    * 허가되지 않은 사용자로부터 **컨텐츠 저작권 관련 당사자의 권리 및 이익을 지속적으로 보호 및 관리하는 시스템**
+ DRM 요소 기술
    * 암호화 : 대칭키 및 비대칭키 암호화 기술
    * 인증 : 정당한 사용자 식별을 위한 인증
    * Watermarking : 원저작권 정보 삽입 및 식별 수행
    * 사용자 Repository : 정당한 사용자 및 라이선스 정보 저장
    * 사용자 권한 관리 : 열람 및 배포에 대한 권리, 편집, 복사, 다운로드 등의 권한 관리
    * Temper Proofing : 불법 수정 여부를 검증, Cracking 을 방지

## 2. DRM 처리방식
+ DRM 개념도

    ![DRM](/assets/images/posts/DRM.gif)

    [이미지 출처](http://www.indicare.org/tiki-read_article.php?articleId=125)

# Watermarking
## 1. 워터마킹(Watermarking)
+ 디지털 정보에 사람이 인지할 수 없는 마크를 삽입
+ **디지털 컨텐츠에 대한 소유권을 추적할 수 있는 기술**
+ **정보은닉기술**
+ 오디오, 비디오, 이미지 등의 디지털 데이터에 삽입되는 또 다른 디지털 데이터로 **Steganography 기법 중 하나**

## 2. 워터마킹 특징

|기술적 특징|주요 내용|
|:---|:---|
|비인지성(Fidelity)|사용자의 워터마크 정보 인지 불가, 데이터 품질 저하 없음|
|강인성(Robustness)|변형에 대한 견고성 유지, 삽입 정보 제거불가|
|연약성(effeminacy)|컨텐츠 복제와 관계없이 워터마크 복제 불가능|
|위조방지(Tamper-resistance)|워터마크를 삭제하려는 고의적인 신호에 제거되지 않아야 함|
|키 제한(Key-resistance)|소수의 사용자만 워터마크를 추출할 수 있는 키 제공|

## 3. Wartermarking 공격 기법
+ Filtering Attack : 워터마크를 노이즈로 보고 노이즈 제거 방법을 활용
+ Copy Attack : 임의의 신호를 추가하여 워터마크를 사용하지 못하게 함
+ Mosaic Attack : 워터마크가 검출되지 않도록 작은 조각으로 분해하여 다시 합침
+ Template Attack : 워터마크의 패턴을 파괴하여 검출되지 않도록 함

## 4. 핑거프린트(Fingerprint)
+ 워터마킹 기법 중 하나
+ **디지털 컨텐츠 원저작자 정보(판매자)와 함께 구매자 정보도 삽입**
+ **디지털 컨텐츠가 불법적으로 유통될 때 불법적으로 유통시킨 구매자를 확인할 수 있는 디지털 컨텐츠 추적기술**
+ **Dual Watermark 라고도 함**
+ 구매자가 다르더라도 컨텐츠 내용 자체는 동일함

# 포렌식(Forensic) 개요
## 1. 디지털 포렌식(Digital Forensic)
### 1. 정의
* 디지털 기기를 대상으로 발생하는 특정 행위의 사실관계를 법정에서 증명하기 위한 방법 및 절차
* 과학수사 및 수사과학 분야에서 디지털 기기를 대상으로 하는 기술

### 2. 디지털 증거휘발성

|용어|내용|
|:---|:---|
|**디지털 증거**|컴퓨터 또는 기타 디지털 저장매체에 저장되거나 네트워크를 통해 전송 중인 자료로서 조사 및 수사업무에 필요한 증거자료를 말한다.|
|**디지털 증거분석**|컴퓨터 또는 기타 디지털 저장매체(네트워크를 통해 전송 중인 자료를 포함)에 남아있는 자료에 대한 원본 보존과 사건 관련 증거를 과학적인 절차를 통하여 추출, 검증, 판단하는 조사 및 수사과정|
|**휘발성 증거**|컴퓨터 실행 시 일시적으로 메모리 또는 임시파일에 저장되는 증거로 네트워크 접속상태, 프로세스 구동상태, 사용중인 파일 내역 등 컴퓨터 종료와 함께 삭제되는 디지털 증거이다.|
|**비휘발성 증거**|컴퓨터 종료 시에도 컴퓨터 또는 기타 디지털 저장매체에 삭제되지 않고 남아있는 디지털 증거이다.|

### 3. 디지털 포렌식 원칙

|기본원칙|내용|
|:---|:---|
|**정당성 원칙**|⦁ 증거가 적법절차에 의해 수집되었는가?<br/>⦁ 위법수집 증거 배제법칙: 위법절차를 통해서 수집된 증거는 증거 능력이 없다(즉, 해킹을 통해서 수집된 증거)<br/>⦁ 독수 독과(과실)이론: 위법하게 수집된 증거에서 얻어진 2차 증거도 증거능력이 없다(불법적인 해킹을 통해서 얻은 패스워드로 특정파일을 복호화하여 얻은 증거)|
|**재현원칙**|⦁ 같은 조건과 상황 하에서 항상 같은 결과가 나오는가?<br/>⦁ 불법 해킹 용의자의 해킹 툴이 증거능력을 가지기 위해서는 같은 상황의 피해 시스템에 툴을 적용할 경우 피해결과와 일치하는 결과가 나와야 한다.|
|**신속성 원칙**|⦁ 디지털 포렌식의 전 과정이 신속하게 진행되었는가?<br/>⦁ 휘발성 데이터의 특성 상 수사진행의 신속성에 따라 증거수집 가능여부가 달라진다.|
|**절차 연속성 원칙**|⦁ 증거물 수집, 이동, 보관, 분석, 법정제출의 각 단계에서 담당자 및 책임자가 명확해야 한다.<br/>⦁ 수집된 저장매체가 이동단계에서 물리적 손상이 발생하였다면, 이동 담당자는 이를 확인하고 해당 내용을 정확히 인수인계하여 이후의 단계에서 적절한 조치가 취해지도록 해야 한다.|
|**무결성 원칙**|⦁ 수집된 증거가 위변조되지 않았는가?<br/>⦁ 일반적으로 해시 값을 이용하여 수집 당시 저장매체의 해시 값과 법정 제출 시 저장매체의 해시 값을 비교하여 무결성을 입증해야 한다.|

+ 디지털 포렌식 도구
    * Encase, FTK

## 2. 휘발성 및 비휘발성 데이터
### 1. 휘발성 및 비휘발성 정보

|휘발성|비휘발성|
|:---|:---|
|⦁ 램슬랙 영역<br/>⦁ 램 비할당 영역<br/>⦁ 네트워크 설정 값<br/>⦁ 네트워크 연결정보<br/>⦁ 실행 중인 프로세스<br/>⦁ 열려진 파일<br/>⦁ 로그인 세션<br/>⦁ 운영체제 시간|⦁ 설정 값<br/>⦁ 로그<br/>⦁ 애플리케이션 파일<br/>⦁ 데이터 파일<br/>⦁ 스왑 파일<br/>⦁ 덤프 파일<br/>⦁ 하이버네이션 파일<br/>⦁ 임시 파일|

### 2. 휘발성 데이터
* 종류
    - 현재 컴퓨터 시스템 날짜 및 시간
    - 현재 컴퓨터 시스템에서 실행되는 프로세스 정보
    - 현재 컴퓨터 시스템 접속자
    - 오픈된 포트
    - 현재 실행되고 있는 프로그램
    - 컴퓨터 시스템 최근 접속기록
* 고려사항
    - 휘발성 데이터는 전원이 차단되면 데이터가 손실됨 -> 우선순위 고려
    - 동작 중인 시스템 메모리 내 존재하는 데이터를 수집해야 함
    - `netstat` 을 활용한 네트워크 연결상태 정보
    - 윈도우 클립보드에 저장된 정보
* **휘발성 데이터 수집 우선순위**
    - Register 및 Cache 정보
    - Routing Table, ARP Cache, Process Table, Kernel Statistics
    - Memory
    - Temporary File Systems
    - Disk
    - Remote Logging 과 Monitoring Data
    - Physical Configuration, Network Topology
    - Archival media

### 3. 비휘발성 데이터
* 고려사항
    - 전원이 차단되어도 데이터 손실이 발생되지 않는 데이터로 저장장치를 압수
    - 시스템 시각 및 데이터 변조를 방지하기 위한 조치 필요 -> 디스크 복제를 수행
* 비휘발성 데이터 수집 우선순위
    - Registry, 시간정보, Cache, Cookie, History, E-Mail
    - 암호화된 파일, 윈도우 로그 등

### 4. 로드카르 법칙
* 개요
    - 접촉하는 두 개체는 서로 흔적을 주고 받는다는 원칙
    - 사용자든 조사자든 동작 중인 시스템을 다루게 되면 해당 시스템은 변화가 발생
* 로드카르(교환) 법칙으로 영향을 받는 데이터
    - 프로세스 활동
    - 데이터 저장 및 삭제
    - 네트워크 상의 데이터 흐름
    - 접속 사이트 Cache Data, 최근 접속목록 Update, 클라이언트 IP 및 Port, 방문URL, 시간 등

## 3. 디지털 포렌식 절차
+ **준비**
    * 사전 정보수집, 네트워크 구성현황, 서버 등의 수집/이송에 필요인원
    * 질문서, 영장, 사무실 용도 등을 개괄적으로 파악
    * 운영체제, 데이터베이스, 네트워크, 시스템 등 분야별 전문가
+ **획득**
    * 전원확인, 컴퓨터의 시간 차 확인, 휘발성 정보수집, 주변장치 확보, 관련자료 확보
    * 압수물 라벨링 및 포장
+ **이송**
    * 전자파 차폐용기, X-RAY 통과금지, 차량 이동 시 충격완화 장치 준비
    * 증거물 보관실(클린 룸, 항온항습 장치 유지)에 보관
+ **분석**
    * 원본 쓰기방지 장치 연결, 사본 복제, 복사본으로 분석
    * 전문 분야별 포렌식 프로그램 활용, 분석과정을 명확하게 기록, 재현대비
+ **증거분석서 작성**
    * 쉽고 평이하게 분석과정을 상세하게 기록
    * 발견 증거물(삭제파일)의 경로기재 및 사진을 첨부
+ **보관**
    * 전자파 차폐, 항온 항습장치 유치 
    * 봉인/봉인해제/재봉인 시 보관/분석 담당자 기록유지
    * 사건 관계자 입회 하에 반출입 기록유지
    * CCTV 보관


# 개발보안(보안약점) (IT Compliance)
## 입력 데이터 검증 및 표현

* SQL Injection
* 경로 조작 및 자원 삽입
* XSS
* 운영체제 명령 삽입
* 위험한 형식 파일 업로드
* CSRF
* HTTP 분할 응답
* 메모리 버퍼 오버플로우
* 포맷 스트링 삽입

## 보안기능

* 취약한 암호화 알고리즘 사용
* 중요정보 평문 전송
* 하드코드 된 비밀번호
* 적절하지 않은 난수 값 사용
* 취약한 비밀번호 허용
* 솔트 없이 일방향 해시

## 시간 및 상태

* 경쟁조건
* 제어문을 사용하지 않는 재귀함수

## 에러처리

* 오류 메시지를 통한 정보 노출
* 오류상황 대응 부재
* 적절하지 않은 예외 처리

## 코드오류

* Null 포인터 역참조
* 부적절한 자원 해제
* 해제된 자원 사용
* 초기화 되지 않은 변수 사용

## 캡슐화

* 잘못된 세션에 의한 데이터 정보 노출
* 제거되지 않고 남은 디버그 코드
* 시스템 데이터 정보 노출
* Public 메소드부터 반환된 Private 배열 등

## API 오용

* DNS Lookup에 의존한 보안 결정
* 취약한 API 사용


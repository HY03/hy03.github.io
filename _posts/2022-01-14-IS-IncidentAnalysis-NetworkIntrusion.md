---
title: 정보보안 - 네트워크 침해사고
date: '2022-01-14 09:00:00'
tags:
- 정보보안기사
- 네트워크 침해사고
related: true
categories:
- IS_Certification
toc: true
---

# 네트워크 침해사고
- 사고 유형별 데이터 수집
    + 불법적인 자원 사용
        * 호스트 불법 사용 : 액세스로그, ps 상태, CPU 사용률, 파일 저장공간 상태
        * 네트워크 대역폭 불법사용 : 회선상태, 송수신 패킷 수, IP주소, 프로토콜 사용현황, 스위치 포트 상태
        * 메일 프록시 서버의 불법 Relay : 호스트 애플리케이션 로그, 프로세스 상태정보, 네트워크 IP주소, 프로토콜 사용현황
    + DoS : 서버의 자원소모를 통한 서비스 중단
        * 서비스 자원소모를 통한 서비스 중단 / 불안정
            - 호스트 : 프로세스 상태정보, CPU 사용률, 비정상적인 패킷 로그
            - 네트워크 : 회선상태, 비정상적인 패킷개수, IP주소, 비정상적인 패킷내용
        * 네트워크 대역폭 과도한 점유를 통한 통신 중단 / 불안정
            - 네트워크 : 송수신된 패킷 개수, IP주소, 프로토콜 사용현황, 데이터 내용
    + 데이터 손상 및 변조
        * 변조의 종류
            - 웹페이지 변조
            - 데이터파일 변조
            - 프로그램 파일 변조
        * 호스트 : 액세스 로그, 파일/저장장치 상태, 환경파일 내용
        * 네트워크
            - IP주소, 프로토콜 사용현황, 데이터 내용, 스위치 포트 상태
    + 정보 누설
        * 통신가로채기, 비공개자료 누설
        * 호스트 : 액세스 로그, 파일/저장장치 상태
        * 네트워크 : IP주소, 프로토콜 사용현황, 데이터 내용, 스위치 포트 상태
- 네트워크 트래픽 (패킷) 정보 분석 방법 (공통적으로 수집해야 할 정보)
    + 분석 솔루션 / 툴
        * MRTG
        * NMS (Network Management System)
        * Firewall
        * IDS
        * 라우터 / 스위치 장비 명령어
        * Ethereal
    + 네트워크의 정보
        * 대역폭 사용량
        * 트래픽을 대량으로 발생시키는 IP 주소
        * 내부 사용자의 IP 주소
        * 서버의 IP 주소
        * TCP/UDP 별 사용 현황
        * 라우터, 스위치 트래픽 발생 현황
- 프로토콜 개요
    + IPv4
        * Version(4) / Header Length(4) / Type of Service(8) / Total Length(16)
        * Identification(16) / Flags(3) / Fragment Offset(13)
            - Flags(3bit)
                + 0 Bit : 0으로 설정
                + 1 Bit : 0 (큰 패킷에 대한 조각), 1 (조각을 허용하지 않음)
                + 2 Bit : 0 (해당 패킷이 마지막 조각), 1 (마지막이 아님) 
        * Time to Live(TTL)(8) / Protocol type(8) / Header Checksum(16)
            - Time to Live(TTL)
                + 해당 패킷이 최대로 통과할 수 있는 라우터 홉수 (전송수명)
                + 0이 되면 해당 패킷은 소실 - ICMP 프로토콜을 사용해 송신 시스템에게 에러가 발생함을 통보
            - Protocol type
                + 전송계층의 프로토콜을 표시
                + ICMP : 1
                + TCP : 6
                + UDP : 17
        * Source Address(32)
        * Destination Address(32)
        * Option / Padding
        * Data(32)...
    + IPv6 : 기존 14개 헤더에서 8개 헤더로 줄어듦
        * Version(4) / Traffic Class(8) / Flow Label(20)
        * Payload Length(16) / Next Header(8) / Hop limit(8)
        * Source Address(128)
        * Destination Address(128)
    + TCP : 시스템과 네트워크를 연결해주는 가장 중요한 계층
        * 20 Bytes Header
            - Source Port number(16) / Destination Port number(16)
                + Port : 0 ~ 65535
            - Sequence number(32)
                + 송신자가 전송하는 데이터의 TCP Segment 번호
            - Acknowledgement number(32)
                + 송신자가 해당 패킷을 수신한 호스트로부터 다음에 받아야 하는 패킷의 TCP Segment 번호
            - header length(4) / 예비(6) / TCP Control Flag(6) / Window size(16)
                + TCP Control Flag : 패킷의 용도를 표시
                    * URG : 긴급
                    * ACK : 응답패킷
                    * PSH : 세그먼트를 메모리에서 합성하지 않고 애플리케이션에 바로 전달
                    * RST : 해당 세션을 강제로 종료
                    * SYN : 초기에 접속을 시작하는 경우 사용, Sequence Number, Window Size 명시
                        - SYN (100) -> SYN (1) + ACK (101) -> ACK (2)
                    * FIN : 해당 세션을 정상적으로 종료
                        - FIN (100) -> ACK(100+1) | FIN, ACK(1) -> ACK(2)
                + Window size : 버퍼크기, 운영체제나 응용프로그램에 따라 다름.
                    * 너무 큰 세그먼트의 크기를 줄이라고 통보
                    * 네트워크 웜의 경우 TCP Window의 크기가 일정하게 고정됨
            - TCP Checksum(16) / urgent pointer(16)
        * Options
        * Data

- 패킷 수집 및 디코드
    + 트래픽과 패킷 수집
        * 목적 : 외부 침입과 공격을 확인하기 위함
    + 패킷 측정 구간
        * 목적에 따라 위치가 달라질 수 있음
            - WAN - Router - F/W - LAN/WLAN ...
                + 기본 : 라우터와 가까운 위치에 센서를 설치
                + F/W : NAT (Network Address Transition) : 사설IP <--> 공인IP 일 경우 방화벽 앞 뒤로 센서를 설치
                    * 같은 시간대에 세션을 찾아서 동일한 패킷들로 분석
                + 네트워크 웜 (IP 를 변조) 분석
                    * 내부 네트워크 전 구간이 측정대상이 될 수 있음
                    * 백본, 주요 액세스 스위치의 포트 트래픽, CPU 상태 관찰 필요
                    * 대부분의 네트워크 공격은 내부에 있는 호스트가 서로 공격하도록 하는 DDoS 방법 이용 (22:38)
    + 패킷 수집
        * 캡쳐 기능을 이용해 버퍼나 파일로 저장
            - 일정 개수의 파일을 항상 유지
                + 약 24-32MB 정도의 용량
                + 패킷 수집구간에서 발생하는 트래픽과 패킷저장에 필요한 시간을 계산해서 설정
            - 항상 최근의 패킷들만 포함 (Ring Buffer)
            - 언제 어떤 사고가 발생할지 알 수 없기 때문에 항상 일정시간 동안의 트래픽 정보와 패킷을 보관
        * 실제 사용자들의 Data 가 필요하지 않을 때 패킷의 Header 부분만 캡쳐
            - 모든 패킷의 시작부분에서 일정한 크기만큼만 캡쳐
    + 트래픽 통계 관찰 - 분석기, NMS 나 시스템 내부 명령어를 통해 관찰
        * MAC 주소별 트래픽 통계
            - 사용자들이나 서버들이 연결된 액세스 스위치 구간에서는 호스트별 MAC 주소를 확인, 트래픽 발생 상태를 관찰
        * IP 주소별 트래픽 통계
            - 대부분 침입, 공격은 IP 기반
            - IP 주소별로 모든 트래픽 통계를 분리 관찰 필요
            - 네트워크에서 웜이 활동하거나 DDoS 가 발생한 경우 
                + 내부 IP 주소에서 전송하는 패킷 개수가 수신하는 패킷 개수보다 훨씬 많음
                + 패킷 개수에 비해서 송수신되는 Byte 수가 적음
        * TCP/UDP 세션별 트래픽 통계 
            - 어느 세션이 서버에서 몇 분 동안 작업을 했는지, 어느정도의 패킷이나 데이터가 송수신되었는지 확인
    + 패킷 디코드
        * 패킷 컬럼
            - Time (Relative Time) : 기준 패킷으로부터 상대적인 시간차
            - Delta Time : 시간 간격 - 바로 이전 패킷과 해당 패킷사이의 간격
            - Abs.Time : 패킷이 캡쳐된 실제 시스템 시간
            - Source : 패킷 송신 호스트의 주소 (IP or MAC)
            - Destination : 패킷 수신 호스트의 주소 (IP or MAC)
            - Length : 실제 패킷의 크기
                + CRC (Cycle Redundancy Check) : 순환중복검사, 네트워크 오류 체크 값을 얻어내는 방식
                    * 데이터 전송 전에 데이터 값에 따라 CRC 계산, 값을 Data 에 붙여넣어 전송
                    * H/W 방식 (직렬데이터-CRC:비트단위 출력) 과 S/W (병렬데이터-CRC:Octet-8비트 단위 출력) 방식이 있음
                    * 오류를 검출하는 데 탁월하나, 수정이 쉬워 데이터 무결성 검증은 할 수 없음.
                + CRC 값이 Length 에 포함될수도 안될수도 있음
            - Protocol
            - Info : 송수신포트, 패킷의 용도, 애플리케이션의 설명, 패킷의 Seq 번호, Ack 번호, TCP Window size 등
        * 패킷 헤더
        * 패킷 데이터
            - 16진수 혹은 ASCII 코드
            
- 공격형 트래픽의 특징
    + 특정 서버만을 공격하는 패턴
        * Web 서버, E-mail 서버, DNS 서버 등 (Application 서버) 을 공격
            - 패킷의 크기가 크거나 불규칙
            - 해당 서버에서 인식하지 못하는 코드를 전송
            - 해당 서버에서 처리할 수 없을 정도의 많은 요청을 보냄
            - 서버에서 허용되지 않는 문장을 사용해서 요청 패킷을 전송
        * 애플리케이션 서비스 중단
            - 서비스 중단 코드를 서버에 삽입 : Buffer Overflow 공격 > 네트워크 과부하 발생 공격 (발전된 공격형태)
    + 네트워크에 과부하를 발생시키는 공격 패턴
        * 스위치, 방화벽, 라우터 등에 영향
            - 영향을 주는 트래픽을 찾는 것은 쉬우나, 발생시키는 호스트를 찾는 것이 어려움 (IP 변조)
        * 네트워크 과부하 발생 트래픽의 특징
            - 불특정 IP 주소를 순차적으로 사용하여 특정 한 개의 IP 주소 대상으로 같은 형태의 패킷이 발생
            - 특정 IP 주소에서 불특정 다수의 IP 주소 대상으로 순차적으로 같은 형태의 패킷이 발생
            - 1초에 송신하는 패킷의 개수가 수백에서 수만개 까지 발생
            - 한 개 또는 몇 개의 TCP 포트를 대상으로 SYN 패킷을 전송
            - 발생하는 패킷의 크기가 작음
            - 패킷의 개수가 수신보다 송신이 매우 많음 (100-1000배 이상)
        * 침입형 트래픽의 특징
            - 목적지를 알고있는 상태인 경우 접속이 가능한 포트를 검색하여 접속을 시도 (여러 방법, 도구) 함
            - 목적지를 모르는 경우 네트워크를 통해 IP 와 포트를 검색해야 함
            - 해당 IP 주소에서 송신한 패킷 개수와 수신한 패킷 개수가 비슷함
            - 일반적으로 검색과정에서는 실제 데이터 송수신이 발생하지 않기때문에 송수신한 바이트 수도 비슷함
            - 패킷 가운데 RST 나 login Failure 가 포함된 패킷이 많음
            - 검색하려는 IP 주소 또는 포트가 순차적으로 바뀜
        * 일반적인 침입과정
            - 공격자
                + 침입자 -> Target : IP Scan
                    * Ping 명령어 이용하여 검색
                        - Echo Request - Echo Reply (호스트 있을 경우) 
                + 침입자 -> Target : 호스트 정보검색 (취약점)
                + 침입자 -> Target : 취약점을 이용한 호스트 공격
                    * 관리자 권한 획득, Data 삭제, 백도어 설치, 자원의 불법사용, 보안정보 유출
            - Target
                + IP Scan 검사 : 침입자의 IP 주소, 검사시간 확인
                + Port 검사
                + 침입자의 명령어 검사, 코드 검사
                + 침입자의 데이터 송수신 상태 검사
                + 불법적인 통신 상태 검사
    + 예시
        * MS Blaster 패턴
            - Window 계열 운영체제를 통해 확산된 웜 (Blaster Worm) - 2003년 전파
            - 여러 IP 에서 한 IP 주소에 패킷을 보냄
            - 짧은 Delta time, 많은 SYN 패킷 전송
        * Gabot 패턴
            - 한 IP 주소에서 여러 IP 에 패킷을 보냄
            - 짧은 Delta time, 많은 SYN 패킷 전송
        * IP Scan 패턴
            - Echo Request 사용 (Ping)
        * Port Scan 패턴
            - TCP 포트 검색, 포트를 순차적 검색
            - 대상 Host 에서 연결을 거부할 경우 RST 패킷이 발생
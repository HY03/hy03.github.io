---
title: 정보보안 - 대칭키 암호
date: '2021-07-26 09:00:00'
tags:
- 정보보안기사
- 대칭키 암호
related: true
categories:
- IS_Certification
toc: true
---

# 대칭키 암호의 종류
- DES (Data Encryption Standard)
    + 개요
        * NIST(미국 국립기술표준원) 에서 1973년 국가적으로 사용할 대칭키 암호시스템 제안요청서를 발표
        * IBM의 제안으로 DES가 채택됨
        * FIPS(연방정보처리기준) 의 초안으로 공표
        * DES 발표 후 가장 널리 사용된 대칭키 블록 암호
        * 이후 삼충 DES 사용을 권고한 FIPS 46-3 발표
        * 최신 AES 는 DES 를 대체한 알고리즘
    + 개관
        * 평문의 길이는 64비트, 키의 길이는 56비트, 이보다 길면 64비트 블록으로 나눔
        * DES 구조는 Feistel 네트워크의 변형, 라운드 회수는 16
        * 원래 키로부터 16개의 서브키를 생성하고 서브키를 각 라운드에서 사용
        * DES 복호화 과정은 암호화 과정과 동일
    + DES의 구조
        * 개요
            - 두 개의 전치(P-박스)와 16개의 Feistel 라운드 함수로 구성
            - P-박스는 초기 전치(initial Permutation) 과 최종 전치(Final Permutation) 임
            - 암호화
                + Plain Text | Plain Text (평문을 절반으로 자름)
                + P-Box
                + 1 Round
                    * R2 = L1 XOR F1(R1)<-K1
                    * L2 = R2
                + ...
                + 16 Round (교차 없음)
                    * R16 = R15
                    * L16 = L15 XOR F15(R15)<-K15
                + P-Box
                + 64Bit 암호문(Cipher Text)
            - 복호화도 동일하게 마지막 라운드에 교차 없음
        * 라운드 함수
            - 개요
                + DES 는 16번의 라운드 함수를 사용
                + 각 라운드 함수는 Fiestal 암호로 되어있음
                + 라운드 함수는 이전 라운드 함수의 출력 값 L-1 과 R-1 을 입력으로 받고 다음 라운드에 입력으로 전송될 L 과 R을 생성
                + 각 라운드에는 2개의 암호 요소 혼합기(mixer) 와 교환기 (swapper) 가 있음, 이런 요소들은 역연산이 가능함
            - DES 함수
                + DES 의 핵심은 함수
                + 라운드 함수에서 사용된 함수를 가리킴
                + DES 함수는 32비트 출력값을 산출하기 위해 가장 오른쪽의 32비트에 48비트 키를 적용
                + DES 함수는 확장 P-박스, 키 XOR, 8개의 S-박스 그리고 단순 P-박스의 4개 부분으로 구성
                    * 예시
                        - 32Bit 입력
                        - 확장 P-박스 (48Bit)
                        - 48Bit XOR Key (48Bit)
                        - S-Box (8개)
                        - 32Bit 출력
                        - 단순 P-박스
                        - 32Bit 결과
        * 암호화 알고리즘과 복호화 알고리즘
            - 라운드 키들이 역순으로 적용되어야 한다는 사실이 중요
            - 암호화 과정에서 1라운드는 K1, 복호화 과정에서 1라운드는 K16
    + DES 분석
        * 설계 기준
            - S-박스
                + 각 라운드에서 그 다음 라운드까지 혼돈과 확산 성질을 만족하도록 설계
                + 비선형 함수
                + 입력 값의 한 비트를 바꾸면 출력 값에서 두 비트 이상 바뀜
            - P-박스
                + 32비트에서 32비트로 가는 하나의 단순 P-박스
                + 32비트에서 48비트로 가는 하나의 확장 P-박스
                + 2개의 P-박스는 비트들을 동시에 확산(diffusion) 시킴
        * DES의 취약점
            - 키의 크기(56비트)
            - 키에 대한 전수 조사를 위해 2의 56승 키를 조사
            - 전사공격 가능
                + 다중 DES 사용
    + 다중 DES
        * 삼중 DES
            - 개요
                + 두 개의 키를 갖는 삼중 DES 와 세 개의 키를 갖는 삼중 DES가 있음
            - 두 개의 키를 갖는 삼중 DES (DES-EDE2)
                + 두 개의 키를 갖는 삼중 DES 에서는 k1과 k2 두 개의 키를 사용
                + 첫 번째와 세번째 단계에서는 k1, 두 번째 단계에서는 k2를 사용
                + 하나의 DES로 삼중 DES를 만들기 위하여 암호화 과정의 중간 단게에서는 복호화 알고리즘을 사용하고, 복호화 과정에서는 DES 암호화 알고리즘을 사용
                + 기지평문 공격에 취약
            - 세 개의 키를 갖는 삼중 DES (Triple-DES)
                + 세 개의 키 k1, k2, k3 를 사용
                + P -> 암호화(k1) -> 복호화(k2) -> 암호화(k3) -> 암호문
                + PGP (이메일 보안, Pretty Good Privacy) 에서 사용
                + 기지평문 공격 보완
                + 복호화 : 암호화의 역
                + 트리플 DES에서 모든 키를 동일하게 하면 3DES는 보통 DES와 같아짐
                + 과거에 DES로 암호화된 암호문을 3DES를 사용하여 복호화 가능
- AES
    + 개요
        * 역사
            - 1997년 미국 국립기술표준원(NIST)은 DES를 대체하기 위해 Advanced Encryption Standard(AES) 암호 알고리즘을 공모
            - NIST의 제안 요청서
                + 128비트 블록크기
                + 128, 192, 256 비트의 세 가지 키 크기
                + 공개 알고리즘으로 누구나 이용 가능해야 함
            - 레인달(Rijndael) AES 알고리즘 선택
        * 선정기준
            - 안정성
            - 비용
            - 구현 효율성
        * 라운드(Rounds)
            - AES는 128비트 평문을 128비트 암호문으로 출력하는 알고리즘
            - Non-feistel 알고리즘
            - 10 (128bit key), 12 (192bit key), 14 (256bit key) 라운드 사용
            - AES-128, AES-192, AES-256 으로 불림
    + 암호(Ciphers)
        * 개요
            - DES : Feistel 네트워크를 기본 구조로 사용
            - Rijndael : SPN (Substitution-Permutation Network) 구조 사용
            - 연산은 바이트 단위의 연산을 사용
            - S-Box 를 이용한 치환, 행이동, 행연산 열변환, 키연산 열변환의 4가지 기본연산을 수행
            - 알려진 공격방법들로부터 안전하도록 설계
            - 하드웨어, 소프트웨어 구현 시 속도나 코드 압축성 면에서 효율적
        * SPN 구조
            - 라운드 함수가 역변환이 되어야 한다는 등의 제약이 있음
            - 더 많은 병렬성을 제공
            - 입력을 여러 개의 소블록으로 나누고 각 소블록을 S-box에 입력하여 치환, S-box의 출력을 P-box로 전치하는 과정 반복
        * Feistel 과 SPN 구조 알고리즘
            - Feistel : DES, Blowfish, MISTY, RC5
            - SPN : IDEA, Rijndael (AES), Square
        * DES 와 AES 의 비교

            |구분|DES|AES|
            |:---:|:---:|:---:|
            |년도|1976|1999|
            |블록크기|64bits|128bits|
            |키 길이|56bits|128,192,256bits|
            |암호화 프리미티브|치환,전치|치환,시프트,비트 혼합|
            |암호학적 프리미티브|혼돈,확산|혼돈,확산|
            |설계|공개|공개|
            |설계 원칙|비공개|공개|
            |선택 과정|비밀|비밀,공모|
            |출처|IBM,NSA|벨기에 암호학자|

- 기타 대칭키 암호 알고리즘
    + IDEA(International Data Encryption Algorithm)
        * DES를 대체하기 위해 스위스 연방 기술 기관에서 개발
        * 128비트의 키를 사용해 64비트의 평문을 8라운드를 거쳐 64비트 암호문으로 변경
        * Feistel 구조
        * DES 보다 2배 빠름
        * 무차별 공격에 더욱 효율적으로 대응
        * PGP 암호화 소프트웨어에서 사용
    + SEED
        * KISA 에서 개발한 알고리즘
        * 인터넷, 전자상거래, 무선 통신 등에서 공개될 경우 민감한 영향을 끼칠 수 있는 중요 정보 및 개인 정보를 보호하기 위해 개발된 대칭 알고리즘
        * 1999년 9월 정보통신단체표준(TTA) 로 제정
        * 2005년 국제 표준화 기구인 ISO/IEC 및 IETF에서 국제 블록암호 알고리즘 표준으로 제정
            - IEC(International Electro-technical Commission) : 국제전기기술위원회
            - IETF(Internet Engineering Task Force) : 국제 인터넷 표준화기구
        * 128비트 비밀키에서 생성된 16개의 64비트 라운드키를 사용하여 16라운드
            - 2009년에는 256비트 비밀키 사용 (SEED256)
        * 128비트의 평문 블록을 128비트 암호문 블록으로 출력
        * DES 와 유사한 변형된 Feistel 구조
        * f 함수의 비선형성 안전도에 의존
    + ARIA (Academy Research Institute Agency)
        * 국가보안기술연구소(NSRI) 주도로 학계, 국가정보원 등의 암호기술전문가들이 개발한 국가 암호화 알고리즘
        * ARIA : 개발팀이 학계, 연구소, 정부기관으로 구성됨
        * ISPN(Involutional SPN) 구조
        * 128비트 블록 암호
        * 128비트, 192비트, 256비트의 3종류의 키 사용을 제공
        * 길이에 따라 ARIA-128, ARIA-192, ARIA-256
        * 입출력 크기와 사용가능한 키 크기는 미국 표준 블록 암호인 AES와 동일
    + HIGHT (HIGh security and light weigHT)
        * KISA, ETRI, 고려대에서 2005년도에 개발
        * 2006년에 TTA (정보통신단체) 표준으로 제정됨
        * 2010년 ISO/IES 국제표준 암호로 제정
        * 64bit 블록암호
        * 저전력, 경량화
    + LEA (Lightweight Encryption Algorithm)
        * 2012년 국가보안기술연구소(NSRI) 가 개발한 경량 고속 암호화 알고리즘
        * 128bit 블록암호
        * AES에 비해 1.5~2배 빠름
        * 스마트폰, 사물인터넷 등에 사용
    + RC5
        * 1994년 RSA 연구소에서 개발
        * 입출력, 키, 라운드 수가 가변적임
        * 32bit, 64bit, 128bit 블록암호
        * DES의 10배의 속도

- 대칭키 암호 정리

    |구분|국가|개발년도|특징|블록크기|키의길이|라운드수|
    |:---:|:---:|:---:|:---:|:---:|:---:|:---:|
    |DES|미국|1927년|NIST에서 표준으로 공표(1997년)|64|56|16|
    |IDEA|유럽|1990년|PGP 채택|64|128|8|
    |Rijndael|벨기에||2000년 AES알고리즘으로 선정|128|128,192,256|10,12,14|
    |SEED|한국|1999년|한국표준 블록암호 알고리즘|128|128|16|
    |CRYPTON|한국|1998년||128|0-256|12|
    |RC5|미국|1994년|알고리즘이 간단, 속도가 빠름|64|0-256|16|
    |FEAL|일본|1987년|S/W구현에 적합|64|64|4|
    |MISTY|일본|1996년|차분/선형공격에 안정성증명구조|64|128|8|
    |SKIPJACK|미국|1990년|Fortezza카드에 사용|64|80|32|

- 현대 대칭키 암호를 이용한 암호화 기법
    + 현대 블록 암호의 사용
        * 개요
            - 기본개념
                + 블록 암호를 다양한 응용에 사용하기 위해 NIST에서 5가지 운영모드를 정의
                + DES, AES를 포함한 어떤 대칭 블록 암호에도 적용 가능
                + ECB모드 : Electric CodeBook mode (전자 부호표 모드)
                + CBC모드 : Cipher Block Chaining mode (암호 블록 연쇄모드)
                + CFB 모드 : Cipher-FeedBack mode (암호 피드백 모드)
                + OFB 모드 : Output-FeedBack mode (출력 피드백 모드)
                + CTR 모드 : CounTeR mode (카운터 모드)
                    * CFB, OFB, CTR 모드 : 스트림 암호화 방식
        * Electric CodeBook(ECB) 모드
            - 개요
                + 가장 간단한 모드. 평문은 N개의 n비트 블록으로 분할
                + 평문 크기가 블록 크기의 배수가 아니라면, 평문의 마지막 블록은 다른 블록들과 동일한 크기로 만들기 위하여 padding이 필요
                + 각 블록에 키는 동일
            - 응용
                + 매우 많은 데이터베이스를 암호화할 때 병렬적으로 처리 가능
                + 암호화된 레코드를 저장하거나 복호화 해야하는 분야에서 독립성이 유용
                + 다른 레코드에 영향을 주지 않음
            - 암호화 예시
                + P1 -> Enc(K1) = C1
                + P2 -> Enc(K1) = C2
                + P3 -> Enc(K1) = C3
                + P4 -> Enc(K1) = C4
        * Cipher Block Chaining(CBC) 모드
            - 개요
                + 각각의 평문 블록은 암호화되기 전에 이전 암호화 블록과 XOR 됨
                + 블록이 암호화될 때 암호화블록은 전송되지만, 다음 블록 암호화를 위해 메모리에 저장됨
                + 첫 번째 블록은 이전 암호화 블록이 없으므로 초기벡터라고 불리는 허구의 블록이 사용됨
            - 암호화와 복호화
                + 1단계 전에 수행되어 결과로 출력된 암호문 블록에 평문 블록을 XOR 하고 나서 암호화를 수행함
                + 복호화할 때 암호문 블록 1개가 파손된 경우, 평문 블록에 미치는 영향은 2개 블록에 머문다. 반면 평문 블록 한 비트 오류는 모든 암호문에 영향을 미친다.
            - CBC 모드 활용
                + 인터넷에서 보안을 제공하는 프로토콜 중 하나인 IPSec 에서 통신의 기밀성을 지키기 위해 CBC 모드를 사용
                + 3DES 를 CBC 모드로 사용한 3DES-CBC, AES를 CBC로 사용한 AES-CBC
                + CBC 모드 인증으로 수행하는 대칭키 암호 시스템의 하나인 Kerberos version 5에서도 사용
            - 암호화 예시
                + P1 -> IV Xor -> Enc(K1) = C1
                + P2 -> C1 Xor -> Enc(K2) = C2
                + P3 -> C2 Xor -> Enc(K3) = C3
                + P4 -> C3 Xor -> Enc(K4) = C4
                + IV 가 바뀌면 암호문 블록 모두가 바뀌게 됨
                + P1 가 바뀌면 암호문 블록 모두가 바뀌게 됨
            - 복호화 예시
                + C1 -> Dec(K1) -> IV Xor = P1 
                + C2 -> Dec(K2) -> C1 Xor = P2
                + C3 -> Dec(K3) -> C2 Xor = P3
                + C4 -> Dec(K4) -> C3 Xor = P4
                + C2 가 손상되면 P2, P3가 영향을 받음
                    * 암호문 길이에는 변화가 없어야 함
        * Cipher-FeedBack(CFB) 모드 / 스트림암호 방식 블록암호 모드
            - 개요
                + 어떤 블록 암호도 스트림 암호로 바꿀 수 있다.
                + 스트림 암호의 경우 메시지의 길이가 블록의 정수배가 되도록 패딩 할 필요가 없으며 실시간으로 사용 가능
                + 한 문자를 전송하는 경우 문자 중심 스트림 암호를 이용하여 각 문자가 암호화되는 즉시 전송 가능
            - 암호화와 복호화
                + 복호화 과정은 평문 블록과 암호문 블록의 역할만 바뀌었을 뿐 동일
                    * CBC에서는 복호화 함수를 사용
                    * CFB에서는 암호화 함수를 복호화에 사용
                + DES 나 AES 와 같은 블록 암호를 이용한 운용 모드
                + 그 결과는 스트림암호와 같다.
                    * 키 스트림이 암호문에 의존하는 비동기식 스트림암호와 비슷
            - 암호화 예시
                + IV -> Enc(K1) -> P1 Xor = C1
                + C1 -> Enc(K2) -> P2 Xor = C2
                + C2 -> Enc(K3) -> P3 Xor = C3
                + C3 -> Enc(K4) -> P4 Xor = C4
            - 복호화 예시
                + IV -> Enc(K1) -> C1 Xor = P1
                + C1 -> Enc(K2) -> C2 Xor = P2
                + C2 -> Enc(K3) -> C3 Xor = P3
                + C3 -> Enc(K4) -> C4 Xor = P4
        * Output-FeedBack(OFB) 모드 / 스트림암호 방식 블록암호 모드
            - 개요
                + CFB 모드와 유사하지만 모든 암호문 블록의 각 비트는 이전 암호문 블록의 비트들과 독립적
                    * 오류 파급의 영향을 피할 수 있음
                    * 오류 발생 시 다른 비트에 영향 안줌
                    * CFB와 마찬가지로 초기화 벡터 사용
                + 스트림 암호로서의 OFB 모드
                    * 블록 암호를 기반으로 한 스트림 암호
                    * 키 스트림은 평문이나 암호문과는 독립적이므로 동기식 스트림암호
                    * IV 암호화 -> 평문 블록과 XOR -> 암호화 블록 생성
                    * IV 암호화 결과를 암호화 -> 평문 블록과 XOR -> 암호화 블록 생성
            - 암호화 예시
                + IV -> Enc(K1) = I1 -> P1 Xor = C1
                + I1 -> Enc(K2) = I2 -> P2 Xor = C2
                + I2 -> Enc(K3) = I3 -> P3 Xor = C3
                + I3 -> Enc(K4) = I4 -> P4 Xor = C4
            - 복호화 예시
                + IV -> Enc(K1) = I1 -> C1 Xor = P1
                + I1 -> Enc(K2) = I2 -> C2 Xor = P2
                + I2 -> Enc(K3) = I3 -> C3 Xor = P3
                + I3 -> Enc(K4) = I4 -> C4 Xor = P4
            - 초기치가 바뀌면 모든 암호문이 바뀜
            - 암호화 알고리즘의 결과는 평문과는 무관하다
        * CounTeR(CTR) 모드 / 스트림암호 방식 블록암호 모드
            - 개요
                + ATM(asynchronous transfer mod) 네트워크 보안과 IPSec 에 응용되면서 관심이 늘어남.
                + 암호화 시 피드백이 존재하지 않음. -> 암/복호화 병렬처리가 가능
                + 키 스트림의 의사난수성은 카운터를 사용함으로써 성취될 수 있음
                + CTR 값은 암호화 할 때마다 새로운 비표값으로 생성
                + CTR 모드는 OFB 모드와 마찬가지로 이전 암호문 블록과 독립적인 키 스트림을 생성하지만 (동기식) 피드백을 사용하지 않음
                + ECB 모드처럼 CTR 모드는 서로 독립적인 n비트 암호문 블록을 생성함
                    * ECB와 CTR은 암복호화 병렬처리가 가능
                    * OFB : 병렬처리 불가
                    * CBC/CFB : 복호화만 병렬처리 가능
                + CTR 암호화 -> 평문블록과 XOR -> 암호화 블록 생성
            - 암호화 예시
                + CTR+0 -> Enc(K1) -> P1 Xor = C1
                + CTR+1 -> Enc(K2) -> P2 Xor = C2
                + CTR+2 -> Enc(K3) -> P3 Xor = C3
                + CTR+3 -> Enc(K4) -> P4 Xor = C4
            - 복호화 예시
                + CTR+0 -> Enc(K1) -> C1 Xor = P1
                + CTR+1 -> Enc(K2) -> C2 Xor = P2
                + CTR+2 -> Enc(K3) -> C3 Xor = P3
                + CTR+3 -> Enc(K4) -> C4 Xor = P4

- 블록 암호 공격
    + 차분공격에 대한 기본 개념 (Differential Cryptanalysis)
        * 선택 평문 공격
        * 두 개의 평문 블록들의 비트의 차이에 대응되는 암호문 블록들의 비트의 차이를 이용하여 사용된 암호키를 찾는 방법
    + 선형공격에 대한 기본 개념 (Linear Cryptanalysis)
        * 기지 평문 공격
        * 알고리즘 내부의 비선형 구조를 적당히 선형화시켜 암호키를 찾는 방법
        * 평문과 암호문 비트를 XOR 하여 0이 되는 확률
    + 전수공격법 (Exhaustive key search)
        * 암호화할 때 일어날 수 있는 모든 경우에 대해 조사
        * 경우의 수가 적을 때 좋음
    + 통계적 분석 (Statistical analysis)
        * 암호문에 대한 평문의 각 단어의 빈도에 관한 자료와 더불어 지금까지 알려진 모든 통계적인 자료를 이용하여 해독
    + 수학적 분석 (Mathematical analysis)
        * 통계적인 방법과 수학적 이론을 이용
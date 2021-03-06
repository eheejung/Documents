<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Altibase 버전 별 지원 플랫폼](#altibase-%EB%B2%84%EC%A0%84-%EB%B3%84-%EC%A7%80%EC%9B%90-%ED%94%8C%EB%9E%AB%ED%8F%BC)
  - [Altibase 7.1](#altibase-71)
    - [지원 플랫폼](#%EC%A7%80%EC%9B%90-%ED%94%8C%EB%9E%AB%ED%8F%BC)
    - [Altibase 7.1 Java 호환성](#altibase-71-java-%ED%98%B8%ED%99%98%EC%84%B1)
      - [Oracle JDK / IBM Java](#oracle-jdk--ibm-java)
      - [OpenJDK](#openjdk)
  - [Altibase 6.5.1](#altibase-651)
    - [지원 플랫폼](#%EC%A7%80%EC%9B%90-%ED%94%8C%EB%9E%AB%ED%8F%BC-1)
    - [Altibase 6.5.1 Java 호환성](#altibase-651-java-%ED%98%B8%ED%99%98%EC%84%B1)
      - [Oracle JDK / IBM Java](#oracle-jdk--ibm-java-1)
      - [OpenJDK](#openjdk-1)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

**Table of Contents**  

- [Altibase 버전 별 지원 플랫폼](#altibase-%EB%B2%84%EC%A0%84-%EB%B3%84-%EC%A7%80%EC%9B%90-%ED%94%8C%EB%9E%AB%ED%8F%BC)
  - [Altibase 7.1](#altibase-71)
    - [Altibase 7.1 Java 호환성](#altibase-71-java-%ED%98%B8%ED%99%98%EC%84%B1)
      - [Oracle JDK / IBM Java](#oracle-jdk--ibm-java)
      - [OpenJDK](#openjdk)
  - [Altibase 6.5.1](#altibase-651)
    - [Altibase 6.5.1 Java 호환성](#altibase-651-java-%ED%98%B8%ED%99%98%EC%84%B1)
      - [Oracle JDK / IBM Java](#oracle-jdk--ibm-java-1)
      - [OpenJDK](#openjdk-1)



# Altibase 버전 별 지원 플랫폼



## Altibase 7.1

### 지원 플랫폼

Altibase 7.1 지원 플랫폼은 [Altibase 7.1 Installation 매뉴얼](https://github.com/ALTIBASE/Documents/blob/master/Manuals/Altibase_7.1/kor/Installation.md#지원-플랫폼) 을 참고하세요. 

>  *Altibase 서버/클라이언트 모두 64-bit 만 지원합니다.*
>
> *Microsoft Windows 는 Altibase 클라이언트만 지원합니다.*


|                                                              | Altibase 서버<br /> | Altibase 클라이언트<br /> | 소프트웨어 요구사항                                          |
| ------------------------------------------------------------ | :-----------------: | :-----------------------: | :----------------------------------------------------------- |
| **AIX on IBM Power Systems**                                 |                     |                           |                                                              |
| AIX 6.1 TL3 <br />AIX 6.1 TL9<br />                          |          ●          |             ●             |                                                              |
| AIX 7.1<br />AIX 7.2<sup>[권장 버전](#footnote_1)</sup>      |          ●          |             ●             | *- altiMon : Altibase 7.1.0.1.9 이상*<br />                  |
| **HP-UX Itanium (IA-64)**                                    |                     |                           |                                                              |
| HP-UX 11.31                                                  |          ●          |             ●             |                                                              |
| **Linux x86-64**                                             |                     |                           |                                                              |
| Red Hat Enterprise Linux 6<br/>Red Hat Enterprise Linux 7<br/>Red Hat Enterprise Linux 8 |          ●          |             ●             | *- GNU glibc 2.12 이상*                                      |
| **Linux on Power**                                           |                     |                           |                                                              |
| POWER7 Red Hat Enterprise Linux 6<br/>POWER7 Red Hat Enterprise Linux 7<br />POWER8 Red Hat Enterprise Linux 6<br/>POWER8 Red Hat Enterprise Linux 7 |          ●          |             ●             | *- GNU glibc 2.12 이상*                                      |
| **Linux on Power** **(Little Endian)**                       |                     |                           |                                                              |
| POWER8(LE) Red Hat Enterprise Linux 7<sup>[권고 버전](#footnote_2)</sup> |          ●          |             ●             | *- GNU glibc 2.17 이상<br />- Altibase 7.1.0.0.8 이상<br />- altiMon : Altibase 7.1.0.3.6 이상 <br />- Adapter for JDBC : Altibase 7.1.0.3.6 이상*<br /> |
| **Microsoft Windows (x64)**                                  |                     |                           |                                                              |
| Microsoft Windows 2008<sup>[권고 버전](#footnote_3)</sup>    |        **X**        |             ●             |                                                              |

> <a name="footnote_1">AIX 7.2</a> : *AIX 7.2의 경우 BUG-48201이 반영된 Altibase 7.1.0.4.7 이상 권장한다. BUG-48201은 AIX 7.2 에서 발견된 버그로 비정상 종료가 발생해야하는 상황에서 무한 대기가 발생하는 현상을 조치하였다.*
>
> <a name="footnote_2">POWER8(LE) Red Hat Enterprise Linux 7</a> : *Altibase 서버/클라이언트 7.1.0.0.8 이상에서 호환성을 보장한다.*
>
> <a name="footnote_3">Microsoft Windows 2008</a> : *Altibase 클라이언트 7.1.0.4.5 이상에서 호환성을 보장한다.*
>
> *Red Hat Enterprise Linux 경우  Red Hat Enterprise Linux업데이트 버전에 대해 호환성을 보장한다.*
>
> ​	*예) Red Hat Enterprise Linux 8 은 Red Hat Enterprise Linux 8.x 에 대한 호환성 보장을 의미한다.*
>
> *Altibase 버전을 명시하지 않은 플랫폼은 Altibase 7.1 모든 버전에서 호환성을 보장한다.*



### Altibase 7.1 Java 호환성

#### Oracle JDK / IBM Java

|                      |                                 |
| :------------------- | :------------------------------ |
| **JDBC Driver**      | JDK1.5 이상에서 정상 동작한다.  |
| **DB Link**          | JRE 1.5 이상에서 정상 동작한다. |
| **Adapter for JDBC** | JRE 1.7 이상에서 정상 동작한다. |
| **altiMon**          | JDK 1.5 이상에서 정상 동작한다  |

#### OpenJDK

> *Altibase 7.1.0.2.6 이상에서 호환성 보장*

|                      | JDK 11 | JDK 12 |
| :------------------- | :----: | :----: |
| **JDBC Driver**      |   ●    |   ●    |
| **DB Link**          |   ●    |   ●    |
| **Adapter for JDBC** |   ●    |   ●    |
| **altiMon**          |   ●    |   ●    |









## Altibase 6.5.1

### 지원 플랫폼

> Altibase 서버는 64비트만 지원합니다.


|                           | Altibase 서버<br /> | Altibase 클라이언트<br />32비트 | Altibase 클라이언트<br />64비트 | 소프트웨어 요구사항 |
| ------------------------- | :-----------------: | :-----------------------------: | :-----------------------------: | ------------------- |
| **AIX on IBM Power Systems** |                     |                                 |                                 |                     |
| AIX 6.1 TL3<br />AIX 6.1  TL9<br />AIX 7.1 |          ●          |                ●                |                ●                |                     |
| **HP-UX Itanium (IA-64)** |                     |                                 |                                 |                     |
| HP-UX 11.31               | ● | ● | ● |                     |
|**Linux x86-64**|||||
|Red Hat Enterprise Linux 6<br/>Red Hat Enterprise Linux 7<br/>Red Hat Enterprise Linux 8|●|●|●|*- glibc 2.12 이상*|
|**Linux on Power**|||||
|POWER7 Red Hat Enterprise Linux 6<br/>POWER7 Red Hat Enterprise Linux 7|●|-|●||
|POWER8 Red Hat Enterprise Linux 6<br/>POWER8 Red Hat Enterprise Linux 7|●|-|●||
|**Linux on Power (Little Endian)**|||●||
|POWER8(LE) Red Hat Enterprise Linux 7|●|-||*\- Altibase 6.5.1.4.5 이상*<br />*- glibc 2.17 이상*|
|POWER9(LE) Red Hat Enterprise Linux 7|●|-|●|*\- Altibase 6.5.1.7.1 이상*<br />*- glibc 2.17 이상*|
|**Oracle Solaris (Sparc)**|||||
|Solaris 10|●|●|●||
|Solaris 11|●|●|●|*- Altibase* *6.5.1.4.2 이상*|
|**Microsoft Windows (x64)**|||||
|Microsoft Windows Server 2008<br/>Microsoft Windows Server 2012<br/>Microsoft Windows Server 2016<br/>Microsoft Windows Server 2019|●|●|●|*- Altibase 서버 6.5.1.7.7 이상 권장*|
|Microsoft Windows 7<br/>Microsoft Windows 8|●|●|●||
|Microsoft Windows 10|●|●|●|*- Altibase 클라이언트 6.5.1.6.2 이상*|



### Altibase 6.5.1 Java 호환성

#### Oracle JDK / IBM Java

|                 | JDK 4 | JDK 5 |
| :-------------- | :---: | :---: |
| **JDBC Driver** |   ●   |   ●   |
| **DB Link**     |   ●   |   ●   |

#### OpenJDK

> *Altibase 6.5.1.6.6 이상에서 호환성 보장*

|                 | JDK 11 | JDK 12 |
| :-------------- | :----: | :----: |
| **JDBC Driver** |   ●    |   ●    |
| **DB Link**     |   ●    |   ●    |


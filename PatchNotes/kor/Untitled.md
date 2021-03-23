
<table>
  <tr>
    <th>제목</th>
    <th>제목</th>
    <th>제목</th>
  </tr>
  <tr>
    <td>내용</td>
    <td>내용</td>
    <td>내용</td>
  </tr>
  <tr>
    <td>내용</td>
    <td>내용</td>
    <td>내용</td>
  </tr>
</table>
<table>
  <tr>
    <td>내용</td>
    <td>내용</td>
  </tr>
  <tr>
    <td colspan="2">내용</td>
  </tr>
</table>

<table>
  <tr>
    <td rowspan="3">내용</td>
    <td>내용</td>
  </tr>
  <tr>
    <td>내용</td>
  </tr>
  <tr>
    <td>내용</td>
  </tr>
</table>





|                           | Altibase 서버<br /> | Altibase 클라이언트<br />32비트 | Altibase 클라이언트<br />64비트 | 소프트웨어 요구사항 |
| ------------------------- | :-----------------: | :-----------------------------: | :-----------------------------: | ------------------- |
| **AIX**                   |                     |                                 |                                 |                     |
| AIX 6.1 TL3<br />AIX 6.1  TL9<br />AIX 7.1 |          ●          |                ●                |                ●                |                     |
| **HP-UX Itanium (IA-64)** |                     |                                 |                                 |                     |
| HP-UX 11.31               | ● | ● | ● |                     |
|**Linux x86-64**|||||
|Red Hat Enterprise Linux 6.x<br/>Red Hat Enterprise Linux 7.8<br/>Red Hat Enterprise Linux 8.1|●|●|●||
|CentOS 6.x<br/>CentOS 7.8<br/>CentOS 8.1|●|●|●||
|Oracle Linux 6.5<br/>Oracle Linux 6.6<br/>Oracle Linux 7.1<br/>Oracle Linux 7.2<br/>Oracle Linux 7.4|●|●|●||
|**Linux on Power**|||||
|POWER7 Red Hat Enterprise Linux 6.5<br/>POWER7 Red Hat Enterprise Linux 7.1|●|-|●||
|POWER8 Red Hat Enterprise Linux 6.5<br/>POWER8 Red Hat Enterprise Linux 7.1|●|-|●||


# Altibase 7.1




|                                                              | Altibase 서버<br /> | Altibase 클라이언트<br /> | 소프트웨어 요구사항                                          |
| ------------------------------------------------------------ | :-----------------: | :-----------------------: | :----------------------------------------------------------- |
| **AIX**                                                      |                     |                           |                                                              |
| AIX 6.1 TL3 이상<br />AIX 7.1<br />AIX 7.2                   |          ●          |             ●             |                                                              |
| **HP-UX Itanium (IA-64)**                                    |                     |                           |                                                              |
| HP-UX 11.31 이상                                             |          ●          |             ●             |                                                              |
| **Linux x86-64**                                             |                     |                           |                                                              |
| Red Hat Enterprise Linux 6.0 이상<br/>Red Hat Enterprise Linux 7.0 이상<br/>Red Hat Enterprise Linux 8.0 이상 |          ●          |             ●             | - GNU glibc 2.12 이상                                        |
| **Linux on Power**                                           |                     |                           |                                                              |
| POWER7 Red Hat Enterprise Linux 6.5 이상<br/>POWER7 Red Hat Enterprise Linux 7.0 이상<br />POWER8 Red Hat Enterprise Linux 6.5 이상<br/>POWER8 Red Hat Enterprise Linux 7.0 이상 |          ●          |             ●             | - GNU glibc 2.12 이상                                        |
| **Linux on Power** **(Little Endian)**                       |                     |                           |                                                              |
| POWER8(LE) Red Hat Enterprise Linux 7.2 이상                 |          ●          |             ●             | - GNU glibc 2.12 이상<br />- Altibase 7.1.0.0.8 이상<br />- altimon 은 Altibase 7.1.0.3.6 이상 지원<br />- jdbcAdapter 는 Altibase 7.1.0.3.6 이상 지원 |
| **Microsoft Windows (x64)**                                  |                     |                           |                                                              |
| Microsoft Windows 2008                                       |       미지원        |             ●             | - Altibase 클라이언트 7.1.0.4.5 이상                         |

|



# dvsNMS (DVSwitch Multi-User NMS)

[![Platform](https://img.shields.io/badge/Platform-Debian-A80030.svg?logo=debian&logoColor=white)](#)
[![Language](https://img.shields.io/badge/Language-Bash%20%2F%20PHP-4D5A92.svg)](#)
[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

**dvsNMS**는 DV Switch 멀티유저 버전 관리자를 위한 강력한 웹 기반 네트워크 관리 시스템(NMS)입니다. 시스템 모니터링, 멀티유저 관리, 텔레그램 연동 알림 등 서버 관리에 필요한 핵심 기능들을 통합 제공합니다.

> [!CAUTION]
> ver2.0 부터 PORT규칙이 변경되었습니다.<BR>
> 기존 DVS 서비스에 업데이트 이후에는 NMS에서만 설정해야합니다.<BR>
> 시스템의 마이그레이션을 원치 않으시면 Ver1.0을 사용하시고<BR>
> 사용자 관리는 콘솔에서 해주시기 바랍니다.<BR><BR>
> 설치 과정 중 원치 않는 단계는 건너뛸 수 있으나, <br>
> 필수 패키지가 누락되어 발생하는 시스템 오류는 해결되지 않을 수 있습니다. <BR>
> 가급적 자동 설치나 순차적인 수동 설치를 권장합니다.

<div align="left">
  <img src="https://github.com/DS1UYM/DVSMU-PHP-NMS/blob/main/NMS_cap_20260426.png" alt="dvsNMS Dashboard Screenshot" width="800">
</div>

---

## 📢 최근 업데이트 내역 (4월 22일 13시 기준)

> [!NOTE]
> **기존 사용자 안내:** 전체를 다시 설치할 필요 없이 **[수동 설치 - Step2]**만 재설치하시면 최신 버전이 적용됩니다.

* **통합 및 편의성 강화**
  * 웹페이지에서 모든 설정 가능
  * 관리자 페이지 패스워드 관리 기능 통합
  * '시스템 자동 재부팅' 예약 관리 기능 추가
  * 개발자 '공지사항' 및 '뉴스' 플립창 추가, BM Hoseline 바로가기 제공
  * 유저별 최근 접속일자 기록 및 확인
  * 서버 자동 재부팅 일정 관리
  * 메인 페이지에 LastHeard 모니터링 페이지가 통합되었습니다
* **실시간 모니터링 및 통계**
  * **[통계]** 페이지 추가: 사용자별 이용량 확인 가능
  * Websocket 방식 적용으로 실시간 대시보드 반응 속도 대폭 향상 (⚠️ **TCP 3000 포트 개방 필수**)
  * *사용자 모디오 청취는 추가 업데이트가 필요합니다*
* **스마트 알림 (Telegram)**
  * 즐겨찾기 유저에 대한 텔레그램 실시간 알
  * 일일 리포트 텔레그램 자동 발송 기능 추가
  * 텔레그램 메시지 수신 '방해 금지 시간' 설정 기능 지원
* **멀티유저 무한 버전**
  * `dvsMU` 패키지 설치 없이도 웹상에서 멀티유저 추가/삭제/편집 완벽 지원
  * 멀티유저 생성 수 제한 해제
* **서버 데이터 백업 및 복원**
  * 서버 데이터 전체에 대한 원클릭 백업 및 복원 - 버그 수정 완료
  * 백업 용량 간소화
* 사용자 포트 TOPOLOGY 제공
* 간편 방화벽 관리 기능 추가
* 사용자별 실시간 로그 확인 가능
  
---

<BR><BR>

## ⚙️ 시스템 권장 사양 및 OS 설정

**지원 OS:** Debian 13 (Trixie)

> [!TIP]
> * **OS 설치 시 주의사항:** 패키지 선택 화면에서 가장 아래 **3개 항목(SSH server, standard system utilities 등)**만 선택하세요.
> * **권장 하드웨어:** ROM 8GB 이상, RAM 4GB 이상 (CPU 코어는 많을수록 원활합니다.)
> * **계정 설정:** `root` 패스워드는 별도로 설정하지 않고 기본값을 유지하는 것을 권장합니다.

---
<br><br>

## 🚀 설치 안내

OS(Debian or ubuntu) 설치 완료 후 
```
ip addr
```
위 명령으로 서버 IP주소 확인 명령프롬프트에서 
터미널 원격 접속합니다.
```
SSH [계정]@[서버주소]
```
접속 후 아래 명령들을 복사하여 실행합니다.

<BR><BR>

### 💡 [방법 A] 자동 설치 (권장)
단일 스크립트로 필요한 모든 구성을 자동으로 진행합니다.
<BR>
Ver 1.0 (구버전)
```
cd /tmp
wget -O setup https://raw.githubusercontent.com/ds1uym/DVSMU-PHP-NMS/main/Auto_Install.sh
sudo chmod +x ./setup
sudo ./setup
```
Ver 2.0 (최신버전)
```
cd /tmp
wget -O setup https://raw.githubusercontent.com/ds1uym/DVSMU-PHP-NMS/main/Auto_Install_ver2.sh
sudo chmod +x ./setup
sudo ./setup
```
<br><br>
### 🛠 [방법 B] 수동 설치
필요한 단계만 선택하여 개별적으로 설치할 수 있습니다.

Step 0. GRUB 대기 시간 0초 설정
```
cd /tmp
wget -O setup0 https://raw.githubusercontent.com/ds1uym/DVSMU-PHP-NMS/main/Step0_grubzero.sh
sudo chmod +x ./setup0
sudo ./setup0
```
<BR>

Step 1. DVS-Server 설치
```
cd /tmp
wget -O setup1 https://raw.githubusercontent.com/ds1uym/DVSMU-PHP-NMS/main/Step1_DVS_Setup.sh
sudo chmod +x ./setup1
sudo ./setup1
```
<BR>

Step 2. NMS 패키지 설치 Ver 1.0
```
cd /tmp
wget -O setup2 https://raw.githubusercontent.com/ds1uym/DVSMU-PHP-NMS/main/Step2_NMS_Setup.run
sudo chmod +x ./setup2
sudo ./setup2
```
<BR><BR>

Step 2. NMS 패키지 설치 Ver 2.1
```
cd /tmp
wget -O setup2 https://raw.githubusercontent.com/ds1uym/DVSMU-PHP-NMS/main/Step2_NMS_Setup_ver2.1.run
sudo chmod +x ./setup2
sudo ./setup2
```
<BR><BR>

## 💻 초기 설정 및 사용 방법
### 1. 사용자 구성
Step 2까지 설치를 마친 후, 웹 브라우저를 통해 http://[서버-IP-주소]로 접속합니다.

초기 패스워드: admin (최초 비밀번호 변경 메뉴에서 수정하세요)

로그인 후 '접속자 관리' 페이지에서 메인 유저(00) 설정 부터 완료해 주세요.

무한 유저 적용을 위해 사용자 PORT규칙이 변경되었습니다.

기존 DVS 또는 DVSMU 사용자는 이후 모든 설정을 NMS에서 하셔야 합니다.
<BR>

### 2. Step 4. NMS+TG 알림봇(텔레그램) 연동

> [!NOTE]
> 텔레그램 검색창에서 BotFather를 검색하여 실행합니다.
>
> /newbot 명령어를 입력하여 나만의 챗봇을 생성합니다.
>
> 발급받은 API 토큰과 Chat ID를 NMS 웹페이지의 'TG알리미' 설정 메뉴에 입력합니다.




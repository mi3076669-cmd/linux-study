# Chapter 02. Linux 사용 기초

## 1. 학습 개요

Ubuntu의 데스크톱 환경과 터미널 사용 방법을 학습하고,
Linux 명령어의 기본 구조와 관리자 권한의 개념을 익혔다.

또한 SSH를 이용한 원격 접속 방법과 `systemctl`을 이용한
서비스 관리 방법을 학습하였다.

---

## 2. 핵심 개념

### 2.1 Linux GUI 환경

Linux의 GUI 환경에서는 X Window와 Wayland 같은 윈도 시스템을 사용할 수 있다.

Ubuntu 24.04에서는 GNOME 데스크톱 환경을 사용하며,
Wayland를 기본 윈도 시스템으로 사용할 수 있다.

### 2.2 X Window

X Window는 X 서버와 X 클라이언트 구조로 동작한다.

- **X Server**: 화면 출력과 키보드·마우스 입력 관리
- **X Client**: 화면 표시나 사용자 입력 처리를 X Server에 요청하는 프로그램

네트워크를 통해 X Server와 X Client가 서로 다른 컴퓨터에서 동작할 수도 있다는 점을 학습하였다.

### 2.3 터미널과 프롬프트

Ubuntu에서 터미널을 실행하면 다음과 같은 형태의 프롬프트를 확인할 수 있다.

```text
user@hostname:~$
```

각 부분의 의미는 다음과 같다.

- `user`: 현재 사용자 계정
- `hostname`: 컴퓨터의 호스트 이름
- `~`: 현재 위치가 사용자의 홈 디렉터리임을 의미
- `$`: 일반 사용자의 명령 입력 프롬프트

터미널은 다음 명령으로 종료할 수 있다.

```bash
exit
```

---

## 3. 명령어 기본 구조

Linux 명령어는 기본적으로 다음과 같은 구조로 사용한다.

```text
명령어 [옵션] [인자]
```

예를 들어 다음 명령은 `date` 명령에 출력 형식을 지정한 것이다.

```bash
date +%Y-%m-%d
```

---

## 4. sudo와 관리자 권한

Linux에서는 일반 사용자와 관리자(root)의 권한이 구분된다.

일반 사용자가 관리자 권한이 필요한 작업을 수행할 때
`sudo`를 사용할 수 있다.

```bash
sudo apt update
```

위 명령은 관리자 권한으로 패키지 목록 정보를 업데이트한다.

> `sudo`를 이용한 관리자 권한과 CPU의 커널 모드는 서로 다른 개념이다.

---

## 5. SSH 원격 접속 학습

### 5.1 SSH 구성

SSH 원격 접속에서는 Ubuntu와 Windows가 다음 역할을 담당한다.

| 시스템 | 역할 |
| --- | --- |
| Ubuntu | SSH Server |
| Windows | SSH Client (PuTTY) |

Windows에서 PuTTY를 이용해 Ubuntu에 접속하면,
PuTTY에 입력한 Linux 명령어는 Windows가 아니라 원격 Ubuntu에서 실행된다.

### 5.2 Ubuntu IP 주소 확인

SSH 접속에 사용할 Ubuntu의 IP 주소는 다음과 같은 명령으로 확인할 수 있다.

```bash
ip addr
```

또는

```bash
hostname -I
```

### 5.3 패키지 목록 업데이트

```bash
sudo apt update
```

### 5.4 SSH 서버 설치

```bash
sudo apt install ssh
```

### 5.5 SSH 서비스 상태 확인

```bash
systemctl status ssh
```

서비스 상태에서 `active`, `inactive` 등의 상태를 확인할 수 있다.

### 5.6 SSH 서비스 관리

SSH 서비스를 시작하거나 중지할 때 `systemctl`을 사용할 수 있다.

```bash
sudo systemctl start ssh
sudo systemctl stop ssh
sudo systemctl restart ssh
```

부팅 시 SSH가 자동으로 시작되도록 설정할 수도 있다.

```bash
sudo systemctl enable ssh
```

자동 시작 설정을 해제하려면 다음 명령을 사용한다.

```bash
sudo systemctl disable ssh
```

### 5.7 stop과 disable의 차이

| 명령 | 의미 |
| --- | --- |
| `stop` | 현재 실행 중인 서비스 중지 |
| `restart` | 서비스 재시작 |
| `enable` | 부팅 시 자동 시작 설정 |
| `disable` | 부팅 시 자동 시작 설정 해제 |

`disable`은 자동 시작 설정을 변경하는 것이므로 현재 실행 중인 서비스를 즉시 중지하는 `stop`과 역할이 다르다.

### 5.8 ssh.service와 ssh.socket

SSH 서비스를 중지했을 때 환경에 따라 `ssh.socket`이 계속 활성화되어 있을 수 있다는 점을 학습하였다.

상태는 각각 다음과 같이 확인할 수 있다.

```bash
systemctl status ssh.service
systemctl status ssh.socket
```

SSH 연결을 완전히 중지하는 실습에서는 다음과 같이 두 unit을 각각 중지할 수 있다.

```bash
sudo systemctl stop ssh.service
sudo systemctl stop ssh.socket
```

다시 사용할 때는 다음과 같이 시작할 수 있다.

```bash
sudo systemctl start ssh.socket
sudo systemctl start ssh.service
```

### 5.9 exit와 SSH 서버 중지의 차이

```bash
exit
```

`exit`는 현재 원격 셸과 SSH 접속을 종료하지만 SSH 서버 자체를 중지하지는 않는다.

반면

```bash
sudo systemctl stop ssh
```

는 SSH 서버 서비스를 중지하는 명령이다.

---

## 6. 터미널 사용

### 명령행 편집

| 단축키 | 기능 |
| --- | --- |
| `Ctrl + W` | 앞의 단어 삭제 |
| `Ctrl + U` | 현재 입력 중인 행 삭제 |

### 터미널 화면 정리

```bash
clear
```

`clear`는 터미널 화면을 정리하며 이전에 실행한 명령이나 파일을 삭제하는 명령은 아니다.

---

## 7. 기초 명령어

### 7.1 date

현재 시스템의 날짜와 시간을 확인한다.

```bash
date
```

출력 형식을 지정할 수도 있다.

```bash
date +%Y-%m-%d
date +%H:%M:%S
date +%Y/%m
date '+%H시 %M분'
```

주요 형식 지정자는 다음과 같다.

| 형식 | 의미 |
| --- | --- |
| `%Y` | 연도 |
| `%m` | 월 |
| `%d` | 일 |
| `%H` | 시 |
| `%M` | 분 |
| `%S` | 초 |

### 7.2 man

명령어의 사용법과 옵션을 확인할 수 있다.

```bash
man date
```

매뉴얼 화면에서 사용할 수 있는 기본 조작은 다음과 같다.

| 조작 | 기능 |
| --- | --- |
| 방향키 | 내용 이동 |
| `/검색어` | 문자열 검색 |
| `n` | 다음 검색 결과 |
| `q` | 종료 |

### 7.3 passwd

현재 사용자 계정의 암호를 변경할 수 있다.

```bash
passwd
```

암호 입력 시 화면에 문자나 `*`가 표시되지 않을 수 있다.

---

## 8. 직접 수행한 실습

### date 명령어 사용

터미널에서 `date` 명령어와 여러 형식 지정자를 사용하여
시스템의 날짜와 시간을 다양한 형식으로 출력해 보았다.

```bash
date
date +%Y-%m-%d
date +%H:%M:%S
date +%Y/%m
date '+%H시 %M분'
```

이를 통해 하나의 명령어에서도 옵션과 형식을 지정하여
필요한 결과를 출력할 수 있다는 것을 확인하였다.

---

## 9. 학습 정리

이번 Chapter에서는 Ubuntu의 데스크톱 환경과 터미널의 기본 사용 방법을 학습하였다.

특히 다음 내용을 새롭게 익혔다.

- Linux의 GUI 환경과 X Window, Wayland의 기본 개념
- Linux 터미널과 프롬프트의 의미
- Linux 명령어의 기본 구조
- 일반 사용자와 관리자 권한의 차이
- `sudo`를 이용한 관리자 권한 명령 실행
- SSH의 Server/Client 구조
- `systemctl`을 이용한 서비스 상태 확인 및 관리
- `date`, `clear`, `man`, `passwd` 등의 기초 명령어 사용

단순히 명령어를 암기하기보다 각 명령어가 어떤 상황에서 사용되는지 이해하는 것이 중요하다는 것을 확인하였다.


---
title: 키오스크 모드
---

# 제품용 키오스크 모드 실행

키오스크 모드를 통해 부팅 후 데스크탑 로드 후 자동으로 전체화면으로 실행하는 프로그램을 시작하도록 합니다. 데스크톱 GUI를 사용하지 않기 때문에 메모리의 부담을 줄일 수 있습니다.

> **주의**: 개발용 보드에서 키오스크 모드를 사용하는 경우 키오스크 앱은 데스크톱 화면으로 넘어가기 위한 프로그램 종료 기능을 추가해야 합니다.

## Jetson 설정

Jetson Ubuntu Unity 기준으로 `Settings` 을 실행 한 뒤 `Brightness & Lock` 메뉴 아이콘을 클릭하고, `Turn screen off when inactive:`항목을 `Never`로 변경합니다. 이 설정은 키오스크 작동 중 일정 시간이 지나면 화면을 절전모드로 전환하는 것을 막아줍니다.

## 로그인 설정 변경

여기서는 `sudo`를 사용한 편집이 필요하기 때문에 터미널 소스코드 편집기인 `nano`가 필요합니다. `sudo apt install nano`를 사용하여 설치합니다. `vim`이나 `emacs`등 다른 편한 도구가 있다면 그것을 사용하셔도 괜찮습니다.

`sudo nano /etc/lightdm/lightdm.conf`:

```ini
[SeatDefaults]
autologin-user=nstech
autologin-user-timeout=0
user-session=unity
greeter-session=unity-greeter
session-setup-script=/home/nstech/nstech.sh
```

작성 후 <kbd>Ctrl+S</kbd>, <kbd>Ctrl+X</kbd>로 `nano`를 종료합니다.

## 자동 실행 스크립트 작성

자동 실행 스크립트를 작성합니다.

```bash
code ~/nstech.sh
```

`~/nstech.sh`:

```bash
#! /bin/bash
export DISPLAY=:0

# running some program
python3 ...
```

쉘 스크립트 작성 후 저장 후에는 잊지 말고 실행 권한을 부여합니다.

```bash
chmod +x ~/nstech.sh
```

---
title: 시작하기
---

# 시작하기

Jetson 초기설정을 수행합니다. 현재 Jetson NX에서 수행을 완료하였습니다.

## Step 1. SD 카드 플래싱

PC에 SD카드를 연결한 후 **balenaEtcher**를 실행하여 `Flash from URL`을 선택하고 다음 URL을 사용합니다.

```bash
# Jetson NX:
https://developer.nvidia.com/jetson-nx-developer-kit-sd-card-image

# Jetson Nano:
https://developer.nvidia.com/jetson-nano-sd-card-image

# Jetson Nano 2GB:
https://developer.nvidia.com/jetson-nano-2gb-sd-card-image
```

다음으로 드라이버에서 `SDHC Card`혹은 이외의 SD 카드 드라이버를 선택한 후 `Flash!`버튼을 눌러 플래시를 시작합니다. 플레시가 끝난 후 SD 카드의 마운트를 해제하고 SD 카드를 제거합니다.

## Step 2. Jetson 초기화

Jetson에 SD 카드를 장착한 뒤 모니터와 입력장치를 연결하고 전원을 연결하여 부팅을 시작합니다.

초기 설정에서 언어를 `English`로 하는 것이 쉘에서 다루기 편합니다.

## Step 3. VSCode 세팅

- 확장: [Remote Development](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack)

VSCode 확장을 설치한 뒤 [Advanced Port Scanner](https://www.advanced-port-scanner.com/)을 활용하여 네트워크상의 Jetson 디바이스를 찾아냅니다. 일반적으로 `Port 22 (TCP): OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 Ubuntu Linux; protocol 2.0` 속성을 가지고 있습니다.

사이드바의 원격탐색기 아이콘을 선택한 뒤 원격탐색기 모드를 `SSH Targets`를 선택합니다. `+`를 눌러 새로운 연결을 생성합니다. `ssh {Jetson 계정명}@{Jetson IP} -X`와 같은 방식으로 입력합니다.

> **새 연결을 구성했는데 접속이 안되는 경우**
>
> - Jetson 초기화때 만들었던 계정명과 연결한 IP주소를 확인합니다.
> - PC와 Jetson이 동일한 네트워크에 있는지 확인합니다.
> - `.ssh/known_hosts`를 삭제합니다.
>   - 윈도우의 경우 `C:\Users\{사용자 계정}\.ssh\known_hosts`

파워쉘이나 리눅스 터미널에서 아래 명령을 사용하여 SSH 자동 인증을 활성화합니다.

```bash
# .ssh/id_rsa.pub가 없는 경우.
# 자동 인증을 사용하려면 passphrase 항목을 비움
ssh-keygen
# 인증키 복제
cat ~/.ssh/id_rsa.pub | ssh {Jetson 계정명}@{Jetson IP} 'mkdir ~/.ssh && cat >> ~/.ssh/authorized_keys'
```

처음 SSH 연결을 수행하면 `vscode-server`등의 설치로 시간이 좀 걸립니다.

## Step 4. OS 업데이트

SSH 접속 후 VSCode에서 <kbd>Ctrl + `</kbd>를 눌러 터미널을 열고 다음 명령을 실행합니다.

```bash
sudo apt update && sudo apt upgrade -y
```

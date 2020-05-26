---
title: Nordic nRF Embedded
background: assets/nRF52832_SoC_promo.png
summary: >
  Nordic SoC 펌웨어 개발을 위한 개발자용 문서
toc:
---

# Nordic SoC 개발

Nordic사의 nRF SoC 칩에 사용하는 펌웨어를 제작합니다. NSTech은 nRF52832, nRF52840을 활용하여 각종 센서를 탑재한 스캐너 등을 제작합니다. 장비는 블루투스 저에너지(BLE) 및 블루투스 메쉬 네트워크 통신을 통해 유기적인 통신을 지원합니다.

## 관련 저장소

관련 저장소는 현재 비공개입니다. 관리자에게 저장소 권한을 요청하세요.

- [NSTech-Dev-nRF-Boilerplate](https://github.com/nstech-dev/NSTech-Dev-nRF-Boilerplate): Mesh 통합 보일러플레이트
- [NSTech-Dev-nRF-SDK](https://github.com/nstech-dev/NSTech-Dev-nRF-SDK): OS별 통합 툴체인 및 오프라인 문서 제공

## 필요 장비

개발 대상 보드/기판과 함께 아래의 장비가 필요합니다.

- **개발용 PC**: 리눅스(추천) 혹은 윈도우10 머신
- **JLink 디버거**: 컴퓨터와 칩을 연결하여 디버그, 플래싱 등을 수행
- **소켓 점퍼 케이블**: JLink 디버거와 기판을 연결

## 빠른 시작

- [Git](https://git-scm.com/) 설치 (Git Bash 추가 설치)
- [nRF Command Line Tools](https://www.nordicsemi.com/Products/Development-tools/nRF-Command-Line-Tools/Download#infotabs) 설치 (JLink 드라이버 추가 설치)
- [GNU Arm Embedded Toolchain](https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm/downloads) 설치 (권장 버전: 9-2019-q4-major)
- Python 3.X (> 3.4) 설치

### 설치 과정

```bash
# PIP 업데이트 및 nordic 도구 설치
python3 -m  pip install --upgrade pip
python3 -m  pip install --upgrade nrfutil pynrfjprog
# 저장소 클론
git clone https://github.com/nstech-dev/NSTech-Dev-nRF-Boilerplate \
    {프로젝트 이름}
# Git 리모트 해제, 새로운 로컬 저장소 생성
bash bin/cleanup-git.sh
# SDK 및 종속성 다운로드
cmake -Bcmake-build-download -G "Unix Makefiles" .
cmake --build cmake-build-download/ --target download
```

### 빌드 및 디버그

```bash
# DFU를 위한 비밀 키 생성
nrfutil keys generate keys/dfu_private.key
nrfutil keys display \
    --key pk \
    --format code keys/dfu_private.key \
    --out_file keys/dfu_public_key.c
# 빌드와 디버그
cmake -Bcmake-build-debug -G "Unix Makefiles" -DCMAKE_BUILD_TYPE=Debug .
cmake --build cmake-build-debug/ --target START_JLINK_RTT
```

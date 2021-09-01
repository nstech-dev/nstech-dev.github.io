---
title: 추론 환경 구성
---

# Jetson 추론환경 구성

이 추론환경은 아래 저장소 개발을 위한 초기작업입니다.

- [NSTech-Demo-AI-CCTV-Collision-Detector](https://github.com/nstech-dev/NSTech-Demo-AI-CCTV-Collision-Detector): AI CCTV 성능 시험 및 데모 운용을 위한 프로그램 저장소
- [NSTech-APP-AI-CCTV-Collision-Detection](https://github.com/nstech-dev/NSTech-APP-AI-CCTV-Collision-Detection): AI CCTV 제품 개발 소스코드 저장소

## Python 및 관련 패키지 설치

- Python 3.6.X과 PIP 패키지 관리자
- Python3를 위한 `opencv`, `numpy`, `gst`(gstreamer)
- [gst-python](https://github.com/GStreamer/gst-python)
- [Jetson stats](https://github.com/rbonghi/jetson_stats)
- `pyqt5`, GTK와 비디오소스를 위한 종속성

```bash
function Install_Python_Tools {
    sudo apt install -y \
        python3-dev python3-pip libpython3-dev python3-setuptools \
        python3-opencv python3-numpy python3-gi python3-gst-1.0 \
        python3-pyqt5* \
        libcanberra-gtk-module libcanberra-gtk3-module \
        v4l-utils python-gi-dev \
        libv4l-dev v4l-utils qv4l2 v4l2ucp

    python3 -m pip install mypy
    python3 -m pip install -U black
    sudo -H python3 -m pip install -U jetson-stats

    export GST_LIBS="-lgstreamer-1.0 -lgobject-2.0 -lglib-2.0"
    export GST_CFLAGS="-pthread -I/usr/include/gstreamer-1.0 -I/usr/include/glib-2.0 -I/usr/lib/x86_64-linux-gnu/glib-2.0/include"

    mkdir -p ~/Util && cd ~/Util
    rm -rf ~/Util/gst-python
    git clone https://github.com/GStreamer/gst-python.git
    cd gst-python
    git checkout 1a8f48a
    ./autogen.sh PYTHON=/usr/bin/python3
    ./configure PYTHON=/usr/bin/python3
    make
    sudo make install
}
```

## Jetson.GPIO 설치

- [Jetson.GPIO](https://github.com/NVIDIA/jetson-gpio)

```bash
function Install_Jetson_GPIO {
    sudo pip3 install Jetson.GPIO
    sudo groupadd -f -r gpio
    sudo usermod -a -G gpio $USER
}
```

## Jetson Inference 및 관련 라이브러리 설치

- `nvidia-jetpack` 및 종속성
- [Jetson Inference](https://github.com/dusty-nv/jetson-inference)설치 (`~/Util`)

Jetson Inference는 설치 중 모델과 PyTorch 설치 여부를 묻습니다. 모델의 경우 `<Quit>`로 빠져나가도 이후에 `jetson-inference/tools/download-models.sh`를 실행하여 다시 내려받을 수 있습니다. PyTorch는 사용해야 하니 Python 3.6 버전을 설치하시기 바랍니다.

```bash
function Install_Jetson_Inference {
    sudo apt install -y \
        nvidia-jetpack \
        libpython3-dev python3-numpy \
    mkdir -p ~/Util && cd ~/Util
    rm -rf ~/Util/jetson-inference
    git clone --recursive https://github.com/dusty-nv/jetson-inference
    cd jetson-inference
    mkdir build && cd build
    cmake ../
    make -j$(nproc)
    sudo make install
    sudo ldconfig
}
```

## 전체 스크립트

스크립트 실행 후 정상작동을 위해 재부팅이 필요합니다.

```bash
#! /bin/bash

set -e

function Install_Python_Tools {
    sudo apt install -y \
        python3-dev python3-pip libpython3-dev python3-setuptools \
        python3-opencv python3-numpy python3-gi python3-gst-1.0 \
        python3-pyqt5* \
        libcanberra-gtk-module libcanberra-gtk3-module \
        v4l-utils python-gi-dev \
        libv4l-dev v4l-utils qv4l2 v4l2ucp

    python3 -m pip install mypy
    sudo -H python3 -m pip install -U jetson-stats

    export GST_LIBS="-lgstreamer-1.0 -lgobject-2.0 -lglib-2.0"
    export GST_CFLAGS="-pthread -I/usr/include/gstreamer-1.0 -I/usr/include/glib-2.0 -I/usr/lib/x86_64-linux-gnu/glib-2.0/include"

    mkdir -p ~/Util && cd ~/Util
    rm -rf ~/Util/gst-python
    git clone https://github.com/GStreamer/gst-python.git
    cd gst-python
    git checkout 1a8f48a
    ./autogen.sh PYTHON=/usr/bin/python3
    ./configure PYTHON=/usr/bin/python3
    make
    sudo make install
}

function Install_Jetson_GPIO {
    sudo pip3 install Jetson.GPIO
    sudo groupadd -f -r gpio
    sudo usermod -a -G gpio $USER
}

function Install_Jetson_Inference {
    sudo apt install -y \
        nvidia-jetpack \
        libpython3-dev python3-numpy \
    mkdir -p ~/Util && cd ~/Util
    rm -rf ~/Util/jetson-inference
    git clone --recursive https://github.com/dusty-nv/jetson-inference
    cd jetson-inference
    mkdir build && cd build
    cmake ../
    make -j$(nproc)
    sudo make install
    sudo ldconfig
}

sudo apt-get update && sudo apt upgrade -y
Install_Python_Tools
Install_Jetson_GPIO
Install_Jetson_Inference
```

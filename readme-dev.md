# GitHub Pages 개발 가이드

## Windows 10(1909) 환경 구성

1. Powershell(관리자)에서 리눅스 하위시스템 설치.

```powershell
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
```

2. [리눅스 배포판(MS 스토어)](https://aka.ms/wslstore) 설치 및 실행. 호스트 설정까지 해놓을 것. 추천: 우분투 bionic (Ubuntu 18.04 LTS, 브라이트 안정버전을 위한 우분투 버전)

3. 터미널에서 리눅스 실행

```powershell
bash
```

4. 리눅스 시스템 업데이트

```bash
sudo apt-get update -y && sudo apt-get upgrade -y
```

5. 루비 사용을 위한 [브라이트박스(Brightbox)](https://www.brightbox.com/docs/ruby/ubuntu/) 패키지 저장소 등록 및 루비 2.5 개발도구 설치

```bash
sudo apt-add-repository ppa:brightbox/ruby-ng
sudo apt-get update
sudo apt-get install ruby2.5 ruby2.5-dev build-essential dh-autoreconf
```

6. 루비 젬 업데이트 및 지킬 설치

```bash
gem update
gem install bundler
# gem install jekyll bundler
```

7. 프로젝트 루트에 `Gemfile`을 생성하고 번들로 설치

**`Gemfile` 내용**
```
source 'https://rubygems.org'
gem 'github-pages', group: :jekyll_plugins
```

```bash
bundle install
```

8. 설치 확인
```bash
jekyll -v
```

9. `bundle exec jekyll serve`를 사용하여 로컬에서 테스트 수행

### 문제 해결

**권한 문제**
```
ERROR:  While executing gem ... (Gem::FilePermissionError)
```

`C:\사용자\유저명\AppData\Local\Packages\CanonicalGroupLimited.UbuntuonWindows_{...}\LocalState\rootfs\home\리눅스유저명`의 `.bashrc`파일 끝에 다음 내용을 붙여넣고 WSL을 재시작.

```
# Ruby exports

export GEM_HOME=$HOME/gems
export PATH=$HOME/gems/bin:$PATH
```

**nokogiri 설치 문제**
```
checking if the C compiler accepts ... yes
Building nokogiri using packaged libraries.
Using mini_portile version 2.4.0
checking for gzdopen() in -lz... no
zlib is missing; necessary for building libxml2
*** extconf.rb failed ***
```

```bash
sudo apt-get install zlib1g-dev
```

# 윈도우 설치 후 리눅스세팅

## 1. git 설치

https://git-scm.com/

---

## 2. VSCode 설치

https://code.visualstudio.com/

- 확장팩
  1. Korean Language Pack for Visual Studio code
  2. Material Theme
  3. ESLint
  4. Prettier - Code formatter
  5. (기존 세팅이 있을 경우) Setting Sync

## 3. Chocolatey 설치

[https://chocolatey.org/](https://chocolatey.org/)

- windows power shell 관리자 권한 실행 후 아래의 코드 붙혀넣기
  ```bash
  Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
  ```

## 4. Chocolatey 사이트에서 패키지 검색으로 Python 설치하기

- windows power shell 관리자 권한 실행 후 아래의 코드 붙혀넣기

  `choco install python`

## 5. Microsoft Store 에서 Windows Terminal 설치

- windows power shell 관리자 권한 실행 후 아래의 코드 붙혀넣기

  `choco install microsoft-windows-terminal`

- 이제 windows power shell 대신 Windows Terminal 을 사용한다

## 6. Windows Subsystem for Linux(WSL) 세팅

- wsl1, wsl2 선택 따라 설정법이 다름

* Windows Terminal 관리자 권한 실행 후 아래의 코드 붙혀넣기

  1. WSL 활성화 (재부팅 필수)

     dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart

* wsl1 (가상머신이 아닌 윈도우 상에서 사용)

  2. Microsoft Store 에서 Ubuntu 18.04 설치 (재부팅 필수)
  3. Ubuntu 실행 후 사용자 설정

* wsl2

2. Microsoft Store 에서 Ubuntu 20.04 설치 (재부팅 필수)
3. Ubuntu 실행 후 사용자 설정
4. 가상머신 활성화 (재부팅 필수)
   `dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart`
5. 재부팅 후 메인보드 BIOS 설정에서 가상화 설정할 것
6. [https://docs.microsoft.com/ko-kr/windows/wsl/wsl2-kernel](https://docs.microsoft.com/ko-kr/windows/wsl/wsl2-kernel) 에서 최신 WSL2 Linux 커널 업데이트 패키지를 다운로드 후 설치
7. Windows Terminal 관리자 권한 실행 후 아래의 코드 붙혀넣기 (재부팅 필수)

   `wsl --set-version Ubuntu-20.04 2`

   `wsl --set-default-version 2`

- 설치 확인 (Windows Terminal 실행 후 아래의 코드 붙혀넣기)

  `wsl --list --verbose`

## 7. Terminal 커스터 마이징

1. VSCode 확장팩에서 Remote - WSL 설치

2. zsh 설치

   `https://github.com/ohmyzsh/ohmyzsh/`

   `https://github.com/ohmyzsh/ohmyzsh/wiki/Installing-ZSH`

   `sudo apt install zsh`

3. oh my zsh 설치

   `https://github.com/ohmyzsh/ohmyzsh`

   `sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"`

4. terminal colorSchems

`https://docs.microsoft.com/ko-kr/windows/terminal/customize-settings/color-schemes` 에서

- One Half Dark 설정

  Windows Terminal 설정 settings.json 54번째 줄

  ```json
    {
      "guid": "{c6eaf9f4-32a7-5fdc-b5cf-066e8a4b1e40}",
      "hidden": false,
      "name": "WSL", // "Ubuntu-18.04"
      "source": "Windows.Terminal.Wsl",
      "colorScheme": "One Half Dark" <- 여기 부분 추가(앞으로 테마 이름 설정은 여기에서 한다)
    }
  ```

5. [https://terminalsplash.com/](https://terminalsplash.com/) 에서 테마 선택

- 예시) Monokai Night for Windows Terminal 선택 시 code 를 클릭하여 다음에 나오는 코드를 copy

  Windows Terminal 설정 settings.json 56번째 줄 schemes 배열에 copy한 객체를 넣기

  `"colorScheme": "One Half Dark" -> "colorScheme": "Monokai Night"` 로 변경한다

- wsl 을 포함하여 모든 terminal 테마 변경을 원할 시

  Windows Terminal 설정

  `settings.json 21번째 줄 defaults 객체`에 `"colorScheme": "Monokai Night"` 키값을 넣기

6.  Powerlevel10k - [https://github.com/romkatv/powerlevel10k#oh-my-zsh](https://github.com/romkatv/powerlevel10k#oh-my-zsh)

7.  Windows Terminal 에서 아래 코드 입력

    `sudo git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k`

    - Windows Terminal 환경설정 옵션은 code ~/.zshrc 로 한다

    2. .zshrc 에서 11번째 줄 ZSH_THEME="robbyrussell"을 아래 코드로 변경

       ```
       ZSH_THEME="powerlevel10k/powerlevel10k"
       ```

    3. 폰트 Windows에서 설치 - [https://www.nerdfonts.com/font-downloads](https://www.nerdfonts.com/font-downloads)

       MesloLGS NF, Hack NF 폰트 설치

    4. Windows Terminal 설정 settings.json 21번째 줄

    ```json
    "defaults": {
      // Put settings here that you want to apply to all profiles.
      "colorScheme": "Monokai Night",
      "fontFace": "Hack NF", // "MesloLGS NF" or "Hack NF" <- 폰트추가
      "cursorColor" : "#FFFFFF",
      "cursorShape" : "vintage",
      "useAcrylic" : true,
      "acrylicOpacity" : 0.8
    },
    ```

    5. VSCode settings.json 에 옵션 추가

    ```json
      "terminal.integrated.fontFamily": "Hack NF",
      "terminal.integrated.shell.windows": "C:\\Windows\\System32\\wsl.exe",
      "workbench.colorCustomizations": {
        // "terminal.background": "#212121",
        "terminal.foreground": "#ffffff",
        "terminalCursor.foreground": "#F1C40F",
        "terminal.ansiBlack": "#2C3E50",
        "terminal.ansiRed": "#C0392B",
        "terminal.ansiGreen": "#27AE60",
        "terminal.ansiYellow": "#F39C12",
        "terminal.ansiBlue": "#2980B9",
        "terminal.ansiPurple": "#8E44AD",
        "terminal.ansiCyan": "#16A085",
        "terminal.ansiWhite": "#ffffff",
        "terminal.ansiBrightBlack": "#34495e",
        "terminal.ansiBrightRed": "#E74C3C",
        "terminal.ansiBrightGreen": "#2ECC71",
        "terminal.ansiBrightYellow": "#F1C40F",
        "terminal.ansiBrightBlue": "#3498DB",
        "terminal.ansiBrightPurple": "#9B59B6",
        "terminal.ansiBrightCyan": "#1ABC9C",
        "terminal.ansiBrightWhite": "#ECF0F1"
      }
    ```

    6.  ls color 설정

    - terminal 에서 `code ~/.zshrc` 입력

    - 110번 맨 아래줄에 입력

    `LS_COLORS="ow=01;36;40" && export LS_COLORS`

    - Powerlevel10k 재설정 방법

    `p10k configure`

---

### 기타 플러그인 - [참고 https://boltlessengineer.tistory.com/84](https://boltlessengineer.tistory.com/84)

    1. syntax-highlighting - [https://github.com/zsh-users/zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting)

    	- install : [https://github.com/zsh-users/zsh-syntax-highlighting/blob/master/INSTALL.md](https://github.com/zsh-users/zsh-syntax-highlighting/blob/master/INSTALL.md)

    	- 설치

    		`git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting`

    	- `code ~/.zshrc` 의 `plugins` 부분에 아래코드 추가

    		`plugins=(git zsh-syntax-highlighting)`

    2. zsh-autosuggestions - [https://github.com/zsh-users/zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions)

    	- install : [https://github.com/zsh-users/zsh-autosuggestions/blob/master/INSTALL.md](https://github.com/zsh-users/zsh-autosuggestions/blob/master/INSTALL.md)

    	- 설치
    		`git clone https://github.com/zsh-users/zsh-autosuggestions $ZSH_CUSTOM/plugins/zsh-autosuggestions`

    	- code ~/.zshrc 의 plugins 부분에 아래코드 추가

    		`plugins=(git zsh-syntax-highlighting zsh-autosuggestions) - uninstall`

    		`rm -rf ~/.zsh/zsh-autosuggestions # Or wherever you installed`

    3. lsd 설치

    	- install : [https://boltlessengineer.tistory.com/86](https://boltlessengineer.tistory.com/86)

    	- 설치


    		`sudo dpkg -i {다운로드한*.deb*패키지*이름*확장자\_포함}`

    		`sudo dpkg -i ./lsd_0.18.0_amd64.deb`

---

## 8. node 설치

1. node js는 `sudo apt-get install nodejs` 로는 설치 할 수 없다
2. node js install ubuntu 검색 (일부 패키지의 설치 방식예시)

   `https://nodejs.org/ko/download/package-manager/` 에서3

   데비안과 우분투 기반 리눅스 배포판. 엔터프라이즈 리눅스/페도라와 Snap 패키지 '클릭'

   공식 Node.js 바이너리 배포판 '클릭'
   [https://github.com/nodesource/distributions/blob/master/README.md](https://github.com/nodesource/distributions/blob/master/README.md)

   - node 12.xx 설치
     [apt-get 업데이트 명령어] `curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -`
     [설치] `sudo apt-get install -y nodejs`

## 9. deadsnake 설치 - [https://launchpad.net/~deadsnakes/+archive/ubuntu/ppa](https://launchpad.net/~deadsnakes/+archive/ubuntu/ppa)

- 일부 패키지는 개인 패키지 저장소를 사용하는 경우가 있음

1. [개인 패키지 저장소 추가] `sudo add-apt-repository ppa:deadsnakes/ppa`
2. [패키지 데이터베이스 업데이트] `sudo apt-get update`
3. [설치] `sudo apt-get install python3.8`
4. code ~/.zshrc 실행

   - 맨 아래줄에 해당 명령어 추가 - python 입력시 python3.8 실행되게 함
     `alias python=python3.8\`

5. git, git-cli 설치

- 리눅스에 git 설치가 되어 있지 않은 경우
  `sudo apt-get install git`
- git cli - [https://cli.github.com/](https://cli.github.com/)
  1. [설치방법] [https://cli.github.com/manual/installation](https://cli.github.com/manual/installation)
  2. [설치파일] [https://github.com/cli/cli/releases/tag/v0.11.1](https://github.com/cli/cli/releases/tag/v0.11.1)
  3. `gh_0.11.1_linux_amd64.deb` 다운로드
  4. [설치] `sudo apt install ./gh_0.11.1_linux_amd64.deb`
  5. 유저 설정(github에 가입)
     `git config --global user.name "{이름}"`
     `git config --global user.email "{이메일}"`
  6. `gh repo view`

## 10. nvm - [https://github.com/nvm-sh/nvm](https://github.com/nvm-sh/nvm)

1. 설치 후 터미널 재시작
   `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash`
2. nvm 입력시 오류 출력
   zsh: command not found: nvm
3. code ~/.zshrc 하여 아래 코드 입력

```
 export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
 [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
```

4. 설치 가능한 node.js 버전 목록 확인
   사용 가능한 node.js lts버전
   `nvm ls-remote`
   사용 가능한 node.js lts버전
   `nvm ls-remote --lts`

5. node version 선택 설치
   `nvm install {버전}`
   ex) `nvm install v12.18.3`
   `nvm install --lts={버전이름}`
   ex) `nvm install --lts=erbium`

6. node version 선택
   `nvm use {버전}`
   `nvm use v12.18.3`

7. 설치된 버전 보기
   `nvm ls`

8. 리눅스 명령어
9. `ls` - 디렉토리 목록 list of Directory
   `ls -a` 숨김파일까지 전체 보기 (리눅스에서는 파일이름앞에 .이 붙으면 숨긴파일이 된다)
   `ls -al` 숨김파일 + 위에서 아래로 나열

10. `cd` - 디렉토리 이동 Change Directory
    `cd ..` - 현재 디렉토리를 포함하고 있는 디렉토리로 이동
    `cd 폴더이름` - 해당 폴더로 이동
    `cd mnt` - 윈도우
    `cd home` - 리눅스
    `cd {키보드 tab키}` - 현재 디렉토리 내에서 이동할 폴더 선택

11. `touch {경로/파일명.확장자}` - 파일 생성
    `touch test/index.js`

12. `whoami` - 현재 사용자

13. `sudo` - 슈퍼유저(어드민)
    `sudo apt-get install {패키지명}` - ex) `sudo apt-get install openssl`
    `sudo apt-get upgrade` - 설치된 패키지 업그레이드
    `sudo apt-get update` - 프로그램이나 패키지들의 데이터베이스를 업데이트

14. `mkdir {폴더명}` - 폴더 생성
    mkdir test - 현재 경로에 test 라는 폴더를 생성한다

15. mv {경로/파일명.확장자} {경로/파일명.확장자} - 이름 변경
    mv index.js new_index.js
    mv test/index.js test/new_index.js

16. rm {경로/파일명.확장자} - 파일삭제
    rm test/index.js

17. rm -rf {폴더명} - 폴더 삭제 (remove folder - rf)
    rm -rf test

18. vim {파일명} - vim에디터로 파일 생성 및 내용 입력
    vim 텍스트 에디터에서 .env 생성
    `vim .env - a`나 `i`를 누르면 텍스트 입력 가능 - 입력완료 후 `ESC` 누르면 command 입력 부분 활성화
    command
    `:wq - w`가 저장. `q`는 종료

19. `cat {파일명}` - 파일 내용 확인
    `cat .env`

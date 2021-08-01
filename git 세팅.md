## 1. git 계정 등록

[리눅스 사용할 경우]
`git config --global user.email "you@example.com"`
`git config --global user.name "Your Name"`
`gh repo view`

## 2. git에 등록될 폴더 설정

`git init`

## 3. git 계정 연결 (github 에서 repository 미리 만들어 놓기)

`git remote add origin https://github.com/{사용자이름}/{레포지토리}`
ex) `git remote add origin https://github.com/Steve-Jung/react-nodebird`

## 4. git commit 하기 (추가와 확정)

`git commit -am "{이번 확정본에 대한 설명}"`
ex) `git commit -am "prepare for aws"`

## 5. git push 하기 (변경 내용 발행)

`git push origin {branch명}`
ex) `git push origin master`

## 6. 클라우드 서버 접속시 clone 하기 (처음 한번만 clone 하면됨. 데이터가 변경되면 git pull 한다)

`git clone https://github.com/{사용자이름}/{레포지토리}`
ex) `git clone https://github.com/Steve-Jung/react-nodebird`

## 7. 클라우드 서버 접속시 git에서 새롭게 등록된 데이터 다시 받아오기

`git pull`

- Aborting 이 뜨는 경우
  `git reset --hard`

## 8. 클라우드 서버에 node 설치하기

- build-essential
  소스코드 빌드 시 필요한 기본적인 패키지들을 다운로드 합니다. bycrpt 나 이미지 리사이징하는 샤프 설치할때 에러가 안남
- curl 명령어 추가
  `sudo apt-get update`
  `sudo apt-get install -y build-essential`
  `sudo apt-get install curl`
  `curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -`
  `sudo apt-get install -y nodejs`

## 9. mysql 설치

- [참조 https://www.tecmint.com/install-mysql-8-in-ubuntu/](https://www.tecmint.com/install-mysql-8-in-ubuntu/)
  `wget -c https://repo.mysql.com//mysql-apt-config_0.8.13-1_all.deb`
  `sudo dpkg -i mysql-apt-config_0.8.13-1_all.deb`

  옵션에서 ok인 4를 선택한다

  `sudo apt-get update`
  `sudo apt-get install mysql-server`

- root 계정으로 변경 - 필수
  `sudo su`

- mysql 비밀번호 설정 - root 계정에서 실행 필수
  `mysql_secure_installation`

- mysql 접속
  `mysql -uroot -p`

- npm start 실행시 'ER_ACCESS_DENIED_NO_PASSWORD_ERROR' 인 경우
  - mysql 접속
    `mysql -uroot -p`
  - 비밀번호 재설정
    `ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '비밀번호';`
    ex) `ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'nodejsbook';`
  - DB 생성
    `npx sequelize db:create`
  - node 서버 실행
    `npm start 혹은 node app`
  - 일반user 인 경우
    `sudo npm start 혹은 sudo node app`

## 10. env 생성

    - vim 텍스트 에디터에서 .env 생성
      `vim .env - a`나 `i`를 누르면 텍스트 입력 가능 - 입력완료 후 `ESC` 누르면 command 입력 부분 활성화
    - command
      `:wq - w`가 저장. `q`는 종료

11. 서버 접속 종료시 서버 동작 유지하기
    foreground process : 터미널 종료 시 같이 서버 종료 (node app)
    background process : 터미널 종료해도 서버는 동작 유지

- pm2 설치
  `npm i pm2`
- 설정
  vim package.json - 파일 열기

  ```json
  "scripts": {
    "dev": "nodemon app",
    "start": "cross-env NODE_ENV=production pm2 start app.js", // 여기 부분
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  ```

  으로 변경

- 실행
  `sudo npm start`
  `sudo npx pm2 start app.js`
- 모니터링
  `sudo npx pm2 monit`
- 실행, 모니터링 같이 실행
  `sudo npm start && sudo npx pm2 monit`
- 종료
  `sudo npx pm2 kill`
- 로그보기
  `sudo npx pm2 logs`
- 에러로그보기
  `sudo npx pm2 logs --error`
- 재시작
  `sudo npx pm2 reload all`
- 서버 정보 리스트
  `sudo npx pm2 list`
- 포트 확인
  `sudo lsof -i tcp:80`
  `sudo lsof -i tcp:3065`

- package.json에 next.js로 인해 pm2 옵션이 없는 경우
  `sudo npx pm2 start npm -- start`
  `sudo npx pm2 start npm -- start && sudo npx pm2 monit`

12. Amazon S3

- S3에서 버킷을 만든 다음 버킷정책에 아래 코드 복사
  \_ 버킷생성시 "모든 퍼블릭 액세스 차단" 체크해제

```js
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AddPerm",
      "Effect": "Allow",
      "Principal": "_",
      "Action": [
        "s3:GetObject",
        "s3:PutObject"
      ],
      "Resource": "arn:aws:s3:::react-nodebird-jungjisub/\*"
    }
  ]
}
```

13. lambda

- 파입압축
  `zip -r aws-upload.zip ./\*`
- 압축파일 등록
  `curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"`
- 언집
  `unzip awscliv2`
- aws 설치
  `sudo ./aws/install`
- aws 세팅
  `aws configure`
- aws 업로드
  `aws s3 cp "aws-upload.zip" s3://react-nodebird-jungjisub`
- aws 사이트 세팅 (아마존 aws 페이지에서 -> 서비스/lambda)
  `https://react-nodebird-jungjisub.s3.ap-northeast-2.amazonaws.com/aws-upload.zip`

14. https 도입

- nginx
  프론트서버에 next 서버만 있는데 nginx 서버를 하나더 도입하여 80포트로 들어오는 유저를 443 포트로 바꿔준다

  `sudo apt-get install nginx`

  `sudo su`

  `vim /etc/nginx/nginx.conf` 으로 편집기를 열어 아래 Virtual Host Configs에 server 부분을 넣는다 `## # Virtual Host Configs ##`

  ```nginx
  include /etc/nginx/conf.d/*.conf;
  include /etc/nginx/sites-enabled/*;

  // front 서버 일때
  server {
          server_name jungjisub.com
          listen 80;
          location / {
                  proxy_set_header HOST $host;
                  proxy_pass http://127.0.0.1:3060;
                  proxy_redirect off;
          }
  }

  // backend 서버 일때
  server {
          server_name jungjisub.com
          listen 80;
          location / {
                  proxy_set_header HOST $host;
                  proxy_set_header X-Forwarded-Proto $scheme;
                  proxy_pass http://127.0.0.1:3060;
                  proxy_redirect off;
          }
  }
  ```

  - 80번 포트확인
    `sudo lsof -i tcp:80`

- 인증서 (3개월 무료 - 갱신이 가능하며 자동화 갱신도 가능하다)

  - sudo su 상태인 경우 exit로 나가서 일반 유저 상태로 만들어 주자

    설치 `wget https://dl.eff.org/certbot-auto`

    권한 `chmod a+x certbot-auto` (a+x 는 모든 유저에게 권한 부여)

    오류 발생
    `sudo lsof -i tcp:80` - 80포트에서 사용되는 PID 확인
    `sudo kill -9 {PID}`

    nginx 실행
    `sudo systemctl start nginx`
    `sudo systemctl status nginx`
    `sudo systemctl restart nginx`

    실행
    `./certbot-auto`

    certbot-auto 실행 후

    ```bash
    ***
    Congratulations! You have successfully enabled https://jungjisub.com
    ***
    ```

    상단 문장 확인 후 IMPORTANT NOTES 에 아래 두 줄이 있는지 확인해야함

    `/etc/letsencrypt/live/jungjisub.com/fullchain.pem`
    `/etc/letsencrypt/live/jungjisub.com/privkey.pem`

    확인
    `sudo su`
    ``vim /etc/nginx/nginx.conf`
    `sudo systemctl restart nginx`

    갱신
    `./certbot-auto renew`

    갱신 자동화 - 리눅스 스케쥴러 크론탭
    `crontab`
    분단위 시간단위 몇일 몇월 요일 실행할명령어
    `6 5,10 \* \* \* /home/ubuntu/react-nodebird/front/cerbot-auto renew`

    crontab 스케쥴 목록 확인
    `crontab -l`

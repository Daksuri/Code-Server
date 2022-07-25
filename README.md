# Code-Server

Visual Studio Code를 통해 작업하는 Linux

## Code-Server 설치
Intall Version : v4.5.1

1. Code-Server 패키지 설치
```
$ cd /opt/
$ wget https://github.com/cdr/code-server/releases/download/v4.5.1/code-server-4.5.1-linux-amd64.tar.gz
```

2. Code-Server 압축풀기 및 폴더위치 조정
```
$ tar -xzvf code-server-4.5.1-linux-amd64.tar.gz -C /opt
$ mv /opt/code-server-4.5.1-linux-amd64 code-server
```

3. Code-Server 폴더 구성
```
$ cd code-server
$ mkdir data extensions
$ vim config.yaml

※ "I" 키를 눌러 편집을 활성화 ※
bind-addr: [your_ip]:[your_port]
auth: password
password: [your_password]
cert: false
user-data-dir: /opt/code-server/data/
extensions-dir: /opt/code-server/extensions/
※ 편집 완료후 -> ESC -> ":wq"을 입력해 편집기 나가기 ※
```

4. Code-Server 방화벽 설정
```
$ ufw allow port/tcp
$ ufw reload
```

5. Code-Server Daemon Service 등록
```
$ vi /etc/systemd/system/code-server.service

[Unit]
Description=Visual Studio Code Server

[Service]
Environment="PASSWORD=mxl12#$cod" 
Environment="PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/opt/code-server" 
WorkingDirectory=/opt/code-server
User=root
Group=root
Type=simple
ExecStart=/opt/code-server/code-server --auth password --extensions-dir /opt/code-server/extensions --user-data-dir /opt/code-server/data --locale ko-KR --cert --host [your_ip] --port [your_port]
Restart=always

[Install]
WantedBy=multi-user.target
```

6. Daemon Service 새로고침 및 Code-Server Daemon Service 실행
```
$ systemctl daemon-reload
$ systemctl start code-server.service
```

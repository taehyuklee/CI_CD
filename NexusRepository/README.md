# Nexus Repository 설치 및 설정관련 내용입니다. 

참고 공식문서: <br>
<a href=https://help.sonatype.com/en/download-nexus-2.html#Download-NexusRepositoryManager2OSS> Nexus Repository 공식문서</a>
<a href=https://potato-yong.tistory.com/144> 참고 블로그 </a>

## Nexus Repository

- Maven에서 사용할 수 있는 오픈소스 Repository중 하나.
- Java의 대표적인 WAS중 하나이 Jetty기반으로 되어 있다.
- nexus는 Java8만이 기동이 가능하다 (이상이 아님)
  - 참고사항: <br>
    jre는 openjdk-8-jre-headless인데 일반적으로 서버에서 구동하는 프로그램은 GUI가 불필요하기 때문에 관련 요소를 제거한 버전이 바로 headless이다. (이걸 설치 권장)

Nexus Rpoesitory 요약: <br>
- 사설망(사내망 등)에서 프로젝트에 필요한 라이브러리를 다운받을 수 있도록 해야한다. 즉, Nexus Repository는 외부 Library를 저장해서 사설망에서 사용할 수 있게 해주는 저장이다.
  (Nexus Repository안에 Library를 저장하는 것도 또는 Proxy역할을 해서 외부 Maven Central과 연결시켜주는 역할도 한다)

---
## 설치작업
- 설치는 Ubuntu 개인 서버 기준으로 되어 있습니다. 데비안 계열의 Linux이지만, RedHat계열또한 크게 다르지 않습니다. <br>
```df -hT``` Linux상에서 용량 한 번 확인 체크작업을 했다.

<br>

### OpenJdk8 설치
```shell
#우선 openjdk를 apt 패키지 매니저를 통해서 다운받음 경로 - /usr/lib/jvm 하위에 다운 받아짐
apt install -y openjdk-8-jre-headless
```

<br>

### nexus Repository binary파일 다운
```bash
#Nexus는 apt나 yum 패키지 매니저가 따로 관리하지 않는 것 같음. 
wget https://download.sonatype.com/nexus/3/latest-unix.tar.gz

#tar (taped Archive)파일.gzip을 해제 해준다.
tar -xvzf latest-unix.tar.gz

#왜그런지 모르겠지만, 공식문서에서 압축해제한 파일을 /opt밑으로 옮기는 작업을 한다
mv {압축 해제 파일} /opt/nexus
mv sonatype-work /opt

#Result 이상태까지 오게 된다.
thlee@thlee-HP-t620-Dual-Core-TC:/opt$ ls
containerd  google  nexus-3.66.0-02  sonatype-work

```

<br>

### Nexus관련 파일들에 대한 권한 변경 과정
```bash
#권한 바꿔줘야 한다. (필요시 작업 진행)
chown [소유자]:[그룹] [디렉토리 or 파일]

#nexus사용자를 따로 추가할 경우 
sudo useradd -m -s /bin/bash nexus

#password설정
passwd nexus

#아래 과정을 거치게 된다.
New password:
BAD PASSWORD: The password is shorter than 8 characters
Retype new password:
passwd: password updated successfully

#user 잘 만들어졌는지 확인
cat /etc/passwd | tail -1

#nexus 하위 경로의 모든 파일(-R 옵션)에 대해 소유자를 바꿔주자 change owner
sudo chown -R nexus:nexus /opt/nexus
sudo chown -R nexus:nexus /opt/sontatype-work

#Result
thlee@thlee-HP-t620-Dual-Core-TC:/opt$ ls -ial
total 24
 917505 drwxr-xr-x  6 root  root  4096  3월 20 09:27 .
      2 drwxr-xr-x 25 root  root  4096  3월 20 09:27 ..
 917506 drwx--x--x  4 root  root  4096 12월 25  2022 containerd
1048609 drwxr-xr-x  3 root  root  4096 10월 19 08:00 google
4456451 drwxr-xr-x 10 nexus nexus 4096  3월 20 09:26 nexus-3.66.0-02
4457975 drwxr-xr-x  3 nexus nexus 4096  3월 20 09:26 sonatype-work

#nexus계정에 visudo를 통해 sudo권한 주기
맨 아래에 이렇게 넣어주면 된다.
# User privilege specification
root    ALL=(ALL:ALL) ALL
nexus   ALL=(ALL:ALL) ALL

# Members of the admin group may gain root privileges
%admin ALL=(ALL) ALL

# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL

# See sudoers(5) for more information on "@include" directives:

@includedir /etc/sudoers.d

#혹시나 계정 삭제 필요시
userdel [user]		// 사용자 계정만 삭제
userdel -r [user]	// 계정의 home directory 등을 포함한 완전 삭제
```

<br>

- 다음 작업으로 nexus사용자로 nexus를 실행하기 위해 opt/nexus/bin/nexus.rc 파일에 nexus사용자를 등록해주자. (vim 또는 vi편집기로 들어가보면) - 즉, nexus를 어떤 사용자로 실행할 것인지에 대한 문제이다.

```bash
run_as_user="nexus"
```

<br>

### Nexus 데몬 서비스 등록
- **Linux 서비스로 등록**

이후 Linux 데몬 프로세스로 돌리고 싶을 경우 ([공식 문서](https://help.sonatype.com/en/run-as-a-service.html), [문서2](https://velog.io/@markyang92/systemd-timer))

repository manager를 `systemd`나 `init.d`로 서비스를 등록하고 데몬으로 돌릴 수 있다.

 (즉, 서버가 부팅시 자동으로 실행되게끔하는 서비스 작업이고 systemd로 관리되기 때문에, systemctl명령어를 사용할 수 있다)

 init.d 공식문서상에서는 init.d로 symbolic link를 만들어서 자동 부팅해놓고 systemd에서 사용하기 위해 
service로 등록한다. 사실 이해가 가질 않지만 일단 공식문서대로 해보자.

```bash
sudo ln -s /opt/{설치된 nexus package}/bin/nexus /etc/init.d/nexus

# /etc/systemd/system/ 하위에 nexus.service를 등록한다.
[Unit]
Description=nexus service
After=network.target
  
[Service]
Type=forking
LimitNOFILE=65536
ExecStart=/etc/init.d/nexus start
ExecStop=/etc/init.d/nexus stop 
User=nexus
Restart=on-abort
TimeoutSec=600
  
[Install]
WantedBy=multi-user.target

#systemctl reload
sudo systemctl daemon-reload
sudo systemctl enable nexus.service
sudo systemctl start nexus.service

# Result
nexus@thlee-HP-t620-Dual-Core-TC:/etc/systemd/system$ sudo systemctl daemon-reload
nexus@thlee-HP-t620-Dual-Core-TC:/etc/systemd/system$ sudo systemctl enable nexus.service
Synchronizing state of nexus.service with SysV service script with /lib/systemd/systemd-sysv-install.
Executing: /lib/systemd/systemd-sysv-install enable nexus
Created symlink /etc/systemd/system/multi-user.target.wants/nexus.service → /etc/systemd/system/nexus.service.

```

---
참고로 Nexus Repository에 대한 Docker Img또한 Docker Hub에 존재한다. 

```bash
#Docker 또한 설치 방법이 있다 (공식문서) https://help.sonatype.com/en/installation-methods.html
docker images (다운받고 확인)
https://www.sktenterprise.com/bizInsight/blogDetail/dev/2743
```

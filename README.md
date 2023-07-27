# Linux
Study of Linux

##Ubuntu (Linux)

**[Prompt]**

```bash
**thlee@DESKTOP-TC89OVT**:**/home**$
```

“thlee@DESKTOP-TC890VT” 이 부분을 Prompt라 하며 현재 사용자, Computer 이름, Directory, 권한(`**$**` : 일반 사용자 or `**#**` : 관리자) 에 대한 표기를 해주고 있다. 

관리자 계정으로 접속하기 위한 명령어

```bash
su - root
```

 정확히는 su 명령어 뒤에 추가적으로 - 와 root를 따로 썼다. 이것이 바로 **`-뒤의 [옵션]`**, `**argument**`로 구분된다. (root - argument라 함)

### **기본 문법 구조**

- **`명령어(프로그램) - (옵션) 대상 (Argument)`**
- **`명령어(프로그램) -[옵션] [옵션에 대한 인자] [명령어에 대한 인자]`**

**- single hypen과 -- double hypen의 차이점**

 옵션의 경우 - single hypen과 -- double hypen으로 쓸수 있는데 이에 대한 차이는 약어를 쓰느냐, 단어 단위로 사용하느냐의 차이이다. 예를 든다면 다음과 같이 -a, --all의 차이를 볼수가 있다.

ls -a (약어)

ls --all (단어 단위)

- 리눅스의 기본 구조

/dev (device약자)

/etc (**etcetera 약자)**

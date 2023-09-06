```bash
#List
pwd
cd
ls
ls -al #ls -al /경로, ls -R 등
mkdir
rm -rf
mv test goodDir
cp -r test2 copy
```

<aside>
📎 디렉토리 관련 명령어를 사용할때는 Argument를 입력하는 두 가지 방식이 있다.

 

**`절대 경로`** : **최상위 디렉토리인 /** 에서부터 특정 파일 또는 디렉토리의 경로를 모두 입력
![image](https://github.com/taehyuklee/Linux/assets/89365465/08a9bbc9-9419-4784-abed-a64e2e806e0b)

→ 위 사진이 최상위 root인데 여기서부터 모든 경로를 다 적어줘야 한다. (윈도우의 C, D드라이브와 같이 파티션이 나뉘는거와 다르게 **리눅스는 최상위가 하나**)

**`상대 경로`** : 현재 작업 디렉토리를 기준으로 특정 파일 또는 디렉토리의 경로를 입력.
![gggg JPG](https://github.com/taehyuklee/Linux/assets/89365465/f5f64edf-68a7-445a-adc5-9e58caf1b0e6)

</aside>

---

`**pwd`** : 현재 작업 디렉토리 확인

```bash
pwd
```

---

**`cd`** : (change directory)작업 디렉토리 변경

```bash
cd [direcotry name]

cd .. #[상위 디렉토리로]
```

---

**`ls`** : 디렉토리 내용 확인

```bash
ls #[확인 할 디렉토리]: 디렉토리 내용 확인

ls -a #[-a만 사용하면 숨겨진 파일까지 모두 표시, -l은 좀 더 자세한 결과를 출력한다]

ls -l #[단순히 내용 목록에 대한 나열만이 아니라 세부사항들도 표기해준다]

ls -al #[숨겨진 파일까지 포함하여 세부사항들을 보여준다 - 가장 많이 사용한다]

ls -R #[하위 디렉토리 목록까지 모두 출력해준다]
```

**`ls-al`** 을 쳤을때

![이런느낌.JPG](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/944b6903-7ae2-48f2-8853-0da1255a517c/%EC%9D%B4%EB%9F%B0%EB%8A%90%EB%82%8C.jpg)

- 윈도우에서 그냥 ls친거는 큰 아이콘을 생각하면 되고 ls -l을 친 것은 자세한 내용까지 나오는 작은 아이콘으로 보는 것과 마찬가지라 생각하면 된다.

 따라서 ls-al을 쳤을때 이 파일에 대한 **접근권한**(drwxr-x—- 과 같은)이라던지 **하드 링크**에 대한 갯수 이 **파일의 소유자**, **관리 그룹**, **파일크기**, **날짜**, **파일 이름** 순으로 자세한 정보를 제공해준다.

![[출처: 02.디렉토리 관련 명령어 -이론 따라학IT 강의]](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9fb4d6ee-b10b-45db-9202-c893d7f248a0/%E3%85%87%E3%85%87%E3%85%87.jpg)

[출처: 02.디렉토리 관련 명령어 -이론 따라학IT 강의]

- 참고로 접근권한 앞에 -로 되어 있으면 파일이고 d로 되어 있으면 directory이다.

Tip 권한이 중요하다 

<aside>
📎  리눅스는 원래 해당 권한을 가진 사용자를 따로 생성해서 관리한다. 예를 들어 웹만을 관리하는 사람은 웹만 할수 있게, 개발을 할수 있는 사람은 개발만 할수 있게 하는 등 각 역할에 맞는 권한을 따로 부여하여 계정을 나눠 놓는다. root계정은 모든것을 할 수 있게 하는 관리자 계정이다.

</aside>

참고로 리눅스에서는 윈도우와 다르게 파일 **이름 앞에 . 만 붙이면 숨김파일이 된다**.

**`ls-al /directory`**

 위와 같이 직접 directory에 들어가서 ls -al을 치는일 없이 상위 directory에서도 경로를 지정하여 쓸수 있다.  (만약 정말 그 안에 내용물이 아니라 그 directory 자체의 세부사항을 보고 싶다면 d옵션을 하나더 붙인다. **`ls-ld /directory`**)

**`ls-R`**을 쳤을때 - 하위 디렉토리 목록까지 모두 출력해줄때

![LSR.JPG](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/87efee2e-49fd-4e4c-910e-02c60b5db2c7/LSR.jpg)

---

**`mkdir`** : 디렉토리 생성

```bash
thlee@DESKTOP-TC89OVT:~/taehyuk$ mkdir test

#위와같이 teahyuk directory에서 mkdir test를하면 그 directory 하위에 test directory가 생김
```

하위에 계층 디렉토리 구조로 쭉 한거번에 만들고 싶다면?

아래와 같이 치면 #Result와 같은 에러를 경험할수 있다. 그렇다면 저런 계층 구조를 만들고 싶다면?

```bash
thlee@DESKTOP-TC89OVT:~/taehyuk$ mkdir test/test2/test3/test4

#Result
mkdir: cannot create directory ‘/home/thlee/taehyuk/test2/test3/test4/test5’: No such file or directory
```

```bash
thlee@DESKTOP-TC89OVT:~/taehyuk$ mkdir **-p** test/test2/test3/test4
```

**`-p`** 옵션을 쓰면 된다. 참고로 -p뒤에 있는게 argument이다. 

argument 개념이 모호했었는데 여기서 확실히 이해됨. 

- argument - 명령어에 의해 영향을 받는 파일 또는 디렉토리 등 특정 대상

**`mkdir a b c`** 

- 띄어쓰기로 a b c를 하면 동시에 3개 directory를 만들수 있다.

---

**`rmdir`** , **`rm`** : 디렉토리 제거

- **`rmdir [삭제할 디렉토리 이름]`** : 디렉토리를 삭제할 때 사용하는 명령어
    - 단 내부에 특정 파일이나 디렉토리가 있으면 안에 파일을 삭제할수 없다.

```bash
rmdir: failed to remove 'test2': Directory not empty
```

안에 무언가가 있다면 위와 같이 비어있지 않다고 코멘트가 나옴.

- `**rm -r [삭제할 디렉토리 이름]**` : 파일을 삭제하는 rm 명령어에 -r 옵션을 이용하여 디렉토리를 삭제할 수 있다. (내부에 내용물이 있어도 그냥 디렉토리 자체를 삭제해버림)

```bash
rm -rf test2
```

하지만, **`-r`** 옵션 대신 **`-rf`** 옵션을 주로 사용한다

 (rf의 어원 - **recursive force** : 강제성이 있음을 알수 있다. recursive 재귀 계속 파고든다는 옵션으로 디렉토리 내부에 모든 경로를 파고들어 라는 의미가 된다.)

- rm -rf ./* : 현재 디렉토리 하위에 있는 모든 내용을 삭제한다.

```bash
rm -rf ./*
```

 파일 앞에 .을붙이면 숨김파일이지만, 경로에 .을 붙이면 현재 나 자신이 된다. 이에 따라 ./는 현재 내 디렉토리로부터 * 모든 파일을 -rf 강제로 삭제한다는 의미가 된다.

---

**`mv`** : 디렉토리 이름 변경

- mv [현재 디렉토리 이름] [변경 할 디렉토리 이름]

```bash
mv test2 test3
```

test2 디렉토리를 test3로 바꾼다.

- 컴퓨터 입장에서는 이름 변경 또한 move하는 것이기에

---

**`mv`** : 디렉토리 이동

- mv[현재 디렉토리 이름] [변경 할 디렉토리 이름]

```bash
thlee@DESKTOP-TC89OVT:~/taehyuk$ mv /home/thlee/taehyuk/test /home/thlee/taehyuk/goodDir
```

위의 shell script에는 프롬프트까지 적었다.

- 상대경로, 절대경로 다 가능하다. 위에서는 명시적으로 절대경로로 적어주었지만 아래와 같이 상대경로로도 그냥 쓰일수 있다.

```bash
thlee@DESKTOP-TC89OVT:~/taehyuk$ mv test goodDir
```

현재 나의 directory는 taehyuk directory에 있음. 

이 말은 `**/home/thlee/taehyuk/**` 까지 생략되어 있다고 생각해도 된다. 따라서 그냥 test direcotry를 goodDir하위로 넘긴다는 말이 된다.

---

**`cp`** : 디렉토리 복사

- cp -r [원본 경로] [이동할 경로]: 디렉토리를 복사할 때는 cp 명령어와 -r 옵션을 함께 사용

(안에 있는 내용물들을 모두 복사해야하기때문에 recursive 옵션이 붙는다)

마찬가지로 상대경로 절대경로 모두 사용가능. 하지만 여기서는 상대경로만 적시함

```bash
thlee@DESKTOP-TC89OVT:~/taehyuk$ cp -r test2 copy
```

test2 directory를 copy directory 하위에 놓겠다는 것.

Swap memory 관련하여 해제 및 설정하는 방법을 기술하고 있다.

Swap 메모리는 실제 RAM이 부족할때를 대비하여 HDD 메모리를 마치 RAM메모리처럼 쓸수 있게 하는 공간이다. 

## Swap memory 해제 <br>

```shell
swapoff -a && sed -i '/swap/s/^/#/' /etc/fstab 
#a는 all을 의미한다는데 맞나?
#sed -> strema editor: 기본 텍스트 변환을 수행할 수 있는 리눅스 명령줄 프로그램 (많은 수의 파일을 대량으로 변경)
#/fstab -> filesystem tab의 약자로 파일시스템 정보를 저장하고 있으며, 리눅스 부팅시 마운트정보를 저장한다.
```
<br>

처음 설치했을때 fstab(filesystem tab)의 내용이다. 
```shell
# /etc/fstab: static file system information.
#
# Use 'blkid' to print the universally unique identifier for a
# device; this may be used with UUID= as a more robust way to name devices
# that works even if disks are added and removed. See fstab(5).
#
# <file system> <mount point>   <type>  <options>       <dump>  <pass>
# / was on /dev/sda6 during installation
UUID=bcf78a22-31da-4215-939e-98019e945355 /               ext4    errors=remount-ro 0       1
# /boot/efi was on /dev/sda1 during installation
UUID=46A0-45C9  /boot/efi       vfat    umask=0077      0       1
# swap was on /dev/sda5 during installation
UUID=2a87f77c-6cbd-4092-af0a-226a78a580ee none            swap    sw              0       0
```
<br>

위의 명령어를 통해 swap을 off하고 나면 다음과같이 swap memory가 해제되어 있는 것을 확인 할 수 있다.

```shell
thlee@thlee-PC:/etc$ free -h
               total        used        free      shared  buff/cache   available
Mem:            15Gi       1.6Gi       2.4Gi       100Mi        11Gi        13Gi
Swap:             0B          0B          0B


#아래와 같다.
UUID=bcf78a22-31da-4215-939e-98019e945355 /               ext4    errors=remount-ro 0       1
# /boot/efi was on /dev/sda1 during installation
UUID=46A0-45C9  /boot/efi       vfat    umask=0077      0       1
## swap was on /dev/sda5 during installation
#UUID=2a87f77c-6cbd-4092-af0a-226a78a580ee none            swap    sw              0       0
```

---------------------------------------------------------------------------------------------------------------

## Swap memory 재설정 <br>

swap memory를 재 설정하기 위해서는 다음과 같은 과정을 거친다.
<br>

#1 step: 스왑 메모리 파일 생성
```bash
sudo fallocate -l 512M /swapfile
```

```fallocate```: 스왑 파일을 생성하는 명령어. fallocate 명령어는 파일을 사전 할당된 공간으로 생성하며, 파일의 크기를 지정할 수 있다.

```-l 2G```: 스왑 파일의 크기를 설정하는 부분. 여기서 "-l"은 크기를 지정하는 옵션으로 "2G"는 2 기가바이트를 나타냅니다.

```/swapfile```: 생성될 스왑 파일의 경로와 이름을 나타냄. 이 경우, 스왑 파일은 루트 디렉토리(/)에 "swapfile"이라는 이름으로 생성된다.

 이 명령어를 실행하면 8 기가바이트 크기의 스왑 파일이 생성되며, 시스템은 이 스왑 파일을 사용하여 추가 가상 메모리를 확보할 수 있게 됩니다.


```shell
#swapfile을 생성했다면 (여기 메모리확보해 놓고 이쪽 메모리 끌어다 쓴느듯)

#ls -ial swapfile 해보면 메모리가 그렇게 잡혀 있는 것을 확인할수 있다.
3062 -rw------- 1 root root 536870912 10월 26 09:29 swapfile
# 536870912 -> 512MB
```

<br><br>

#2 step: 스왑 메모리 파일 권한 변경
```bash
sudo chmod 600 /swapfile
```
권한을 600으로 변경해준다. 만약 755나 655처럼 x권한이 존재한다면, 보안상 이슈로 ```mkswap: /swapfile: insecure permissions 0655, fix with: chmod 0600 /swapfile```에러가 나오게 된다.

<br><br>

#3 step: 스왑메모리 공간 활성화
```bash
#mkswap 유틸리티를 이용해 해당 파일을 Linux 스왑공간으로 설정 한다.
sudo mkswap /swapfile
```



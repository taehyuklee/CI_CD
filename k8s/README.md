## Kubernetess 설치 및 설정입니다. <br>

- 2023-11월 시점에서 가장 최신 버전은 1.28대 버전이다. 이미 Docker의 지원은 끊기고 Docker를 사용하기 위해서는 containerd.io내부적으로 사용되는 Docker의 최신버전만 사용 가능하다. <br>
- 여기서는 기존 containerd.io가 사용되지 않는 Docker의 마지막 버전을 기준으로 쿠버 버전을 맞췄다.

 Kubernetess의 kubulet의 경우 일반적으로 OS에서 지원하는 Swap memory를 지원하지 않기때문에, swap은 off시켜줘야 한다.<br>


 ``` shell
#현재 swap memory 확인.
thlee@thlee-PC:/etc$ free -h
               total        used        free      shared  buff/cache   available
Mem:            15Gi       1.6Gi       2.5Gi       102Mi        11Gi        13Gi
Swap:          487Mi          0B       487Mi

#-h 옵션은 "human-readable"의 약자


#또는 swapon으로 확인한다.
taylee@taylee-PC:~$ sudo swapon --show
[sudo] password for taylee: 
NAME      TYPE      SIZE USED PRIO
/dev/sda5 partition 7.5G   2M   -2
```
<br>
- 기존에 내가 ubuntu를 설치했을때 위와 같이 Swap 메모리를 잡아놨다. 


## 1 Step (Swap memory 해제)
sed는 스트림 에디터(stream editor) 

``` shell
swapoff -a && sed -i '/swap/s/^/#/' /etc/fstab 
#a는 all을 의미한다는데 맞나?
#sed -> strema editor: 기본 텍스트 변환을 수행할 수 있는 리눅스 명령줄 프로그램 (많은 수의 파일을 대량으로 변경)
#/fstab -> filesystem tab의 약자로 파일시스템 정보를 저장하고 있으며, 리눅스 부팅시 마운트정보를 저장한다.

```

## 2 Step (Net-filter 수정 - iptables 설정)
``` shell
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
br_netfilter
EOF

#실제 설정 장면
thlee@thlee-PC:/etc/modules-load.d$ cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
br_netfilter
EOF
[sudo] password for thlee: 

# result
br_netfilter #cat <<EOF를 했기때문에 이렇게 콘솔에 출력이 된다.
thlee@thlee-PC:/etc/modules-load.d$ ls
cups-filters.conf  k8s.conf  modules.conf #k8s.conf가 쓰여진 것을 확인할수 있다.
```
* End Of File (마지막줄에) br_netfilter를 써서 저장했다.

 tee 명령어는 입력 데이터를 읽어서 파일로 출력하는 동시에 화면에 출력하는 명령어. tee는 파이프(|)를 통해 출력되는 데이터를 파일로 저장하면서 동시에 화면에도 표시할 수 있어서 유용하게 사용됩니다.

 ``` shell
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF

#추가적으로 net.bridge.bridge 설정하는 것을 k8s.conf파일에 더 쓰도록 한다. 
#Result - sysctl.d 밑에 k8s.conf파일이 생성됨.
thlee@thlee-PC:/etc/sysctl.d$ cat k8s.conf 
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1

#재시작 함으로써 위의 bridge를 통하는 패킷이 iptables를 우회할 수 있도록 설정할 수 있다.
sudo sysctl --system
```

## 3 Step


---
k8s가 docker를 지원하는 마지막 버전은 1.21버전이다. 이 버전에 관련하여 설치했을때는 다음과 같다.

Docker 특정 버전 설치

- Docker 인증서
``` shell
sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common

#위의 명령어 또는 아래 두 개
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
```

- Docker의 공식 GPG키를 추가. 
```shell
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg #권한 바꿔주기

#Docker의 공식 apt 저장소 추가
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update #다시 update했을때 HIT하지 않으면 문제를 해결하고 넘어가야 한다.
```

- Docker 특정 버전 설치
``` shell
sudo apt-get install docker-ce=5:20.10.17~3-0~ubuntu-jammy docker-ce-cli=5:20.10.17~3-0~ubuntu-jammy containerd.io docker-compose-plugin
```

- Docker 권한 주기
``` shell
permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get "http://%2Fvar%2Frun%2Fdocker.sock/v1.24/containers/json": dial unix /var/run/docker.sock: connect: permission denied

#/var/run/docker.sock에 권한 부여해야 한다 
sudo chmod 666 /var/run/docker.sock # 이 명령어 필요
```

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
k8s - 1.21.7버전 설치
``` shell
sudo apt-get install -y kubelet=1.21.7-00 kubeadm=1.21.7-00 kubectl=1.21.7-00
```

kubeamin init 특정 버전
```shell
kubeadm init --kubernetes-version=v1.21.7
```

CNI 설치
```shell
kubectl apply -f https://docs.projectcalico.org/v3.21/manifests/calico.yaml
```

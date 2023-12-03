## Pod생성 관련 yaml설정

kubernetes에서는 yaml을 통해서 service, pod 등과 같은 여러 component를 만들수 있다. 이와 관련된 명령어들을 알아보고 어떻게 되는지 사용법을 알아보도록 한다.


------

Pod의 당위성
1. 한 컨테이너당 하나의 프로세스만 실행하도록 설계하는데, 필요에 의해 여러 개의 프로세스를 단일 단위로 관리 할 수 있는 상위레벨 구조가 필요하게 됨. 

2. POD는 밀접하게 연관된 프로세스를 함께 실행하고 마치 하나의 컨테이너에서 실행되는 것처럼 거의 동일한 환경을 제공하며 이 또한 격리된 상태로 유지 된다. 

3. POD안의 모든 모든 컨테이너가 동일한 리눅스 네임스페이스 세트를 공유 하도록 함으로써 격리.

4. 기본적으로 각 컨테이너의 파일시스템은 다른 컨테이너의 파일시스템과 분리되어 있다. 다만, 후에 볼륨이라는 개념을 사용해 파일과 디렉토리를 공유할 수 있도록 하고 있다. 
------

<br>

- 만약 namespace가 존재해야한다면 namesapce resoucre부터 만들어준다. 다음은 argocd라는 namesapce를 만드는 것이다.
``` shell
kubectl create namespace argocd
```
<br>

- 일반적으로 내가 관리하고 있는 yaml 또는 외부에서 yaml내용을 직접 받아와서(다운x) 만들수 있다. 이때 apply 또는 create를 사용한다.

``` shell
kubectl create -f spring.yaml

#이 경우는 -n (namespalce) argocd하위에 만드는 것이기에 위의 namespace를 만드는 작업이 우선되어야 한다.
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/ha/install.yaml
```
* create와 apply의 정확한 차이점에 대해서 알아보자.
  보통, create는 처음 만들때, apply는 새로 만들거나 업데이트할때 사용하게 된다. 이미 있는 component에 대해 create하게 되면 Already Exists가 뜨게 된다.


- pod내에 container가 뜰 것이고 container내부의 log를 보고싶을때 다음과 같이 하면 된다.
``` shell
kubectl logs -f spring
```

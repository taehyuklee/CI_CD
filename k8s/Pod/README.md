## Pod생성 관련 yaml설정

kubernetes에서는 yaml을 통해서 service, pod 등과 같은 여러 component를 만들수 있다. 이와 관련된 명령어들을 알아보고 어떻게 되는지 사용법을 알아보도록 한다.

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

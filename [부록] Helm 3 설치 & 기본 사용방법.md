# Helm 3 설치 & 기본 사용방법

인터넷에서 일단 퍼온 자료 입니다....자료  테스트 해야함. 자료 정리도 해야함.

## 1. Overview

이번 문서에서는 **Helm**의 사용법에 대해서 알아보겠습니다.

## 2. Prerequhttp://localhost:8080/hystrixisites

- 쿠버네티스 클러스터가 깔려있어야 합니다.
  -> [호롤리한 하루/Install Kubernetes on CentOS/RHEL](https://gruuuuu.github.io/cloud/k8s-install/) 참고.
- helm을 설치할 위치에서 kubectl 명령어를 사용할 수 있어야 합니다.

#### helm이란?

Docker가 나오면서 container 혁신이 이루어졌습니다. 그리고 그 container를 쉽게 관리하고 배포해주는 kubernetes가 나오게 되었죠. 즉, 일종의 container orchestration입니다. 그리고 이러한 kubernetes를 package로 해서 관리해주는 것이 바로 helm입니다.

일종의 Python에서 패키지를 관리하는 pip, Node.js에서의 npm의 역할이라고 보시면 되겠습니다.

그리고 helm을 조사하다보면 helm chart라는 용어가 많이 보입니다. helm chart란 또 무엇일까요?

#### helm chart란?

helm chart라는 것은 helm의 package format입니다. chart는 kubernetes를 설명하는 파일들의 집합이라고 보시면됩니다. 



![img](https://blog.kakaocdn.net/dn/baCJn0/btqDuqzR3GI/TEbMS6RsB6wFCczfOvre1K/img.png)helm chart 공식문서에 나와있는 설명



공식문서에도 같은 설명으로 나와있습니다. chart는 파일들, 그 속에 속해있는 폴더의 집합입니다. 그리고 폴더의 이름은 chart의 이름을 나타나게 되는데요. 위 예시처럼 만약, wordpress를 예시로 든다면 폴더 이름도 wordpress로 나오게 됩니다.

각 파일에 대한 간략한 설명은 다음과 같습니다.

- Chart.yaml : 해당 helm chart에 대한 정보가 담겨있습니다.
- values.yaml : 해당 helm chart에서 사용하는 각종 값들에 대한 정의가 담겨있습니다.
- chart 디렉토리 : 의존하는 chart에 대한 정보가 담겨있습니다.
- templates : kubernetes르 정의하는 metifest file이 정의되어 있는 폴더입니다. 
- README.md : 사용자가 읽을 수 있는 README 파일

등이 있으며, 자세한 내용은 개요에 helm chart 링크를 걸어놓았으니 참고바랍니다.

이 외에도 repository, release 개념이 있는데요. 각 설명은 아래와 같습니다.

- repository : chart들이 공유되는 공간입니다. 일종의 docker hub같은? 개념이라고 보시면 될 것 같아요.
- release : kubernetes(쿠버네티스) 환경에서 동작되는 서비스들은 relase 버전이 존재합니다.

Chart - Repository - Release 관계를 정리하자면, helm chart repository에서 chart를 찾을 수 있으며 이 helm chart는 kubernetes를 설명하는 파일의 목록입니다. 그리고 설치할 때마다 release 버전이 생성됩니다.



Kubernetes에서 애플리케이션을 배포 관리 하면 복잡성이 올라가기 때문에 배포시간이 많이 소요 됩니다. Helm을 이용하여 애플리케이션을 보다 빠르게 배포 관리할 수 있도록 하는 과정에 대하여 이야기하고자 합니다.

### Helm 특징

- 복잡한 어플리케이션 배포 관리
  - Kubernetes 오케스트레이션된 애플리케이션의 배포는 매우 복잡할 수 있습니다. Kubernetes 환경에서 helm 차트는 복잡한 애플리케이션의 배포를 코드로 관리 하여 자동으로 배포할 수 있도록 제공 합니다. 애플리케이션의 빠른 배포를 통하여 다양한 테스트 환경 배포 및 운영 환경 배포 시간을 줄여 개발에 집중 하도록 합니다.
- Hooks
  - Kubernetes 환경에서 helm 차트로 설치, 업그레이드,삭제 그리고 롤백과 같은 애플리케이션 생명주기의 개입할 수 있는 기능을 Hook을 통하여 제공 합니다.
- 릴리즈 관리
  - Helm으로 배포된 애플리케이션은 하나의 릴리즈로 불립니다. 해당 릴리즈는 배포된 애플리케이션의 버전 관리를 가능하도록 합니다.

### Helm 기본 구성

### ![img](https://tech.osci.kr/assets/images/86027123/0.png)

- **Helm Chart :** Kubernetes에서 리소스를 만들기 위한 템플릿 화 된 yaml 형식의 파일
- **Helm (Chart) Repository :** Helm Repository는 해당 리포지토리에 있는 모든 차트의 모든 메타데이터를 포함하는 저장소. 상황에 따라서, Public Repository를 사용 하거나 내부에 Private Repository를 구성할 수 있습니다.
- **Helm Client(cli) :** 외부의 저장소에서 Chart를 가져 오거나, gRPC로 Helm Server 와 통신하여 요청을 하는 역할을 합니다.
- **Helm Server(tiller) :** Helm Client의 요청을 처리하기 위하여 대기하며, 요청이 있을 경우 Kuberernetes에 Chart를 설치하고 릴리즈를 관리 합니다.





**helm 소개**

helm은 쿠버네티스 패키지 매니저 입니다. 쿠버네티스를 사용하다보면 결국 수많은 YAML파일들을 관리해야 됩니다. helm에서는 이런 YAML파일들의 집합을 차트(chart)라고하고, 이 차트를 관리할 수 있게 해주는 도구가 helm입니다. helm은 쿠버네티스의 하위 프로젝트로 시작되었다가 2018년 6월에 CNCF재단의 정식 프로젝트로 승격되었습니다. Helm은 차트를 만들고, 차트 압축 파일(tgz)를 만들수 있습니다. 그리고 차트들이 저장되어 있는 차트 저장소(chart repository)와 연계해서 쿠버네티스 클러스터에 차트를 설치하거나 삭제할 수 있습니다. helm으로 설치된 차트들의 배포 주기를 관리할 수도 있습니다.

helm을 이용하면 잘 정리된 차트들을 이용해서 필요한 애플리케이션들을 빠르게 설치할 수 있습니다. https://github.com/helm/charts 에 보면 incubator와 stable 하위에 수많은 차트들이 준비되어 있는걸 확인할 수 있습니다. mysql, redis, 젠킨스, 하둡, 엘라스틱서치등 많은 애플리케이션들을 쉽게 설치해서 사용할 수 있습니다.

**helm 개념**

helm을 사용하기 위해서 알아야 하는 3가지 주요 개념이 있습니다.

차트(chart) : 쿠버네티스 애플리케이션을 만들기위해 필요한 정보들의 묶음입니다.

컨피그(config) : 배포 가능한 객체들을 만들기 위해 패키지된 차트에 넣어서 사용할 수 있는 설정들을 가지고 있습니다.

릴리즈(release) : 특정 컨피그를 이용해서 실행중인 차트의 인스턴스 입니다.

**helm 구성요소**

helm은 실질적으로 2개의 요소로 구성되어 있습니다. 사용자가 직접 사용할 수 있는 명령줄 도구인 helm 클라이언트(helm client)와 쿠버네티스 클러스터 내부에서 실행되어서 helm 클라이언트에서 오는 명령을 받아서 쿠버네티스 api와 통신하는 역할을 하는 틸러 서버(Tiller Server)입니다.

helm 클라이언트를 사용하면 로컬에서 차트를 개발할 수 있고 차트 저장소들을 관리할 수 있습니다. 틸러 서버와 통신해서 쿠버네티스 클러스터에 설치하기 위한 차트를 보낼수 있습니다. 현재 클러스터에서 실행중인 릴리즈에 대한 정보를 요청할수도 있습니다. 실행중인 릴리즈를 업그레이드하거나 삭제하라는 요청을 틸러서버로 보냅니다. helm 클라이언트는 GO언어로 개발되어 있고 틸러서버와는 gRPC를 이용해서 통신합니다.

틸러서버는 주로 helm 클라이언트에서 오는 요청을 처리하기위해 대기하고 있습니다.클러스터에 실행할 릴리즈를 만들기 위해서 차트와 설정을 조합하는 일을 합니다. 쿠버네티스 클러스터에 차트를 설치하고 릴리즈를 관리합니다. 쿠버네티스 클러스터에서 실제로 차트를 업그레이드하거나 삭제하는 역할을 합니다.



출처: https://arisu1000.tistory.com/27860 [아리수]

**1. Helm을 이용한 애플리케이션 배포**



보통 사람들은 쿠버네티스에서 애플리케이션을 배포할 때 어떤 방법을 사용할까? 소규모 프로젝트라면 kubectl apply -f 명령어를 사용해도 크게 문제는 없겠지만, 애플리케이션의 규모가 커지고 복잡해질수록 어쩔 수 없이 좀 더 체계화된 배포 방법이 필요하게 된다. 배포 과정을 조금 더 자동화하려면 REST 로 Patch 요청을 보내도 되겠고, kubectl 1.14 버전부터 자동으로 내장되어 있는 kustomize를 사용할 수도 있으며, (해본 적은 없지만) Spinnaker를 사용할 수도 있다. 추가로 kapitan 라는 것도 있는 것 같은데, 이건 사용해보지 않아서 모르겠다.

 

어쨌든, 쿠버네티스 에코시스템이 하루가 멀다하고 변화하는 요즘 시대에, 인프라 관리자들의 관심사 중 하나는 **'App 배포를 어떤 방식으로 (어떤 도구로) 할 것인가?'** 인 것 같다. 그 중에서도 가장 자주 언급되는 것이 Helm 인데, 쿠버네티스 리소스를 한 군데로 묶어서 차트로 편하게 배포할 수 있다는 장점 때문에 많은 사람들이 사용하고 있는 것으로 보인다. 물론 Helm도 만능은 아니라서 A/B 테스트나 Canary 배포가 어렵다는 단점은 있지만, Istio와 같은 Service Mesh와 함께 섞어서 쓰면 다양한 배포 전략을 사용할 수 있다 (고 한다. Helm + Istio PoC 는 아직 안해봤음).



![img](https://mblogthumb-phinf.pstatic.net/MjAxOTA2MjRfMTYw/MDAxNTYxMzc5Njg3NzUy.jFD38NozEvnUV4IsYTNZZ_ksdraZzDPP2ABR_neiS1kg.1s491hjr27XLgz7yfmB9McjO_y2OWVeg9o_TnlwYv7Ig.PNG.alice_k106/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2019-06-24_%EC%98%A4%ED%9B%84_9.34.36.png?type=w2)



그래도 아직은 Helm이 App을 배포하는 방법으로서 많이 언급되고 있으며, 최근에 Tiller가 삭제된 Helm 3도 나온 것으로 미루어보아 꾸준히 개발이 진행되고 있는 도구인 것은 확실한 것 같다. 그런데 Helm에서 제공되는 공식 차트만 사용할 것이 아니라면, 기존에 App 배포에 사용되던 YAML 파일을 Helm 차트로 리패키징 하는 작업이 필요할 뿐만 아니라 (노가다..) 사내에서 사용할 Private 차트 저장소도 따로 만들어야 한다. 다행히도 이러한 작업은 크게 어렵지는 않으며, 구글링하면 많은 가이드를 찾을 수 있다.



그렇다고 해서 삽질이 아예 없는 것은 아니기 때문에, 그 과정을 공유하려고 한다. 

누군가에게는 도움이 되었으면 좋겠다..





**2. Helm 차트를 배포하기 위한 저장소**



도커 이미지를 배포하기 위해서는 ECR, 도커 허브 등이 필요하듯이, Helm 차트를 배포하기 위해서는 차트 저장소가 필요하다. 대표적인 예시로는 Helm을 설치하면 기본적으로 사용할 수 있는 **stable 저장소**가 있는데, 이는 Helm에서 공식적으로 제공되고 있기 때문에 별도의 설정을 하지 않아도 사용할 수 있다. 사용 가능한 저장소 목록은 helm repo list 명령어로 확인할 수 있다.



![img](https://mblogthumb-phinf.pstatic.net/MjAxOTA2MjRfMTU3/MDAxNTYxMzgxMTI5ODAy.f8QZYzdWaX-2pPPSrqkFpRuNpgIy7wDtc2aNKDAvVZQg.nbsvDmxZJszC4z1teoVE5tqwqMcuAsYnQbQik9L8ljUg.PNG.alice_k106/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2019-06-24_%EC%98%A4%ED%9B%84_9.58.35.png?type=w2)



참고로, Helm 저장소 목록은 Tiller가 아닌 클라이언트 단 (~/.helm/repository/repositories.yaml) 에 저장된다.



stable 외에도 별도의 차트 저장소를 등록해 사용할 수 있기 때문에, 나만의 차트 저장소를 구축해 Helm 클라이언트에 등록하는 것 또한 가능하다. 그렇다면 여기서 최대 관심사는 **'차트 저장소를 어떻게 구축할 것인가?'** 가 될 터인데, 사실 차트 저장소를 구축하는 방법에 정답은 없다. HTTP 요청을 통해 파일을 읽을 수만 있으면 차트 저장소로 활용할 수 있기 때문에, **S3나 아파치 웹 서버, 심지어 Github rawcontent**도 차트 저장소로 사용할 수 있다.



![img](https://mblogthumb-phinf.pstatic.net/MjAxOTA2MjRfOTgg/MDAxNTYxMzgzMTg4ODU5.2MGOdWpG73TDjFV5hWey7TclvcUm1jessFzT5muYTlwg.5D5l_8iS_rQ2N3Liu1oQdBrQ45oO2C3eiM5oLPek8z0g.JPEG.alice_k106/helm_%2B.jpg?type=w2) 





그렇지만 나는 Helm 공식 문서에서 언급하고 있는 **ChartMuseum** 이라는 도구를 사용하기로 결정했다. ChartMuseum은 아무래도 Helm 차트를 보관하기 위한 도구인 만큼, 내가 미처 고려하지 못했던 기능을 제공하고 있을 수도 있다는 생각이 들었다. 또한 아파치 웹 서버나 S3 등과 같은 여타 방법들은 Helm 차트 그 자체를 위한 것이 아니며, 기존에 사용하던 도구를 Helm을 위해서 사용하는 ad-hoc 느낌이 강했기 때문이다.



물론, 아파치 웹 서버나 Github, S3 만의 이점이 있을 수도 있으며, 이는 각자의 환경에 맞게 적절히 선택해야 한다. (사실 Github이나 아파치 웹 서버 쓰면 index.yaml 파일을 직접 업데이트 해야해서 더 귀찮다....)

# 3. Helm이란?

Helm은 쿠버네티스의 **package managing tool**입니다.

크게 세가지 컨셉을 가지고 있는데 :

1. **Chart** : Helm package입니다. app을 실행시키기위한 모든 리소스가 정의되어있습니다.
   `Homebrew formula`, `Apt dpkg`, `Yum RPM`파일과 비슷하다고 생각하시면 됩니다.
2. **Repository** : chart들이 공유되는 공간입니다. `docker hub`를 생각하시면 될 것 같습니다.
3. **Release** : 쿠버네티스 클러스터에서 돌아가는 app들은(chart instance) 모두 고유의 release 버전을 가지고 있습니다.

다시 정리하면,
helm은 **chart**를 쿠버네티스에 설치하고, 설치할때마다 **release**버전이 생성되며, 새로운 chart를 찾을때에는 Helm chart **repository**에서 찾을 수 있습니다.

> **주의!**
> 이 문서는 helm v3.0.0 이상을 다루고 있습니다.
> 2.x버전과 많은것이 변경되었기 때문에 참고해주시기 바랍니다.

# 4. Installation

kubectl을 사용할 수 있는 노드로 이동하여 설치합니다.

```
$ curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 > get_helm.sh
$ chmod 700 get_helm.sh
$ ./get_helm.sh
```

버전 확인 :

```
$ helm version
```

![image](https://user-images.githubusercontent.com/15958325/70848110-26aa6500-1eb0-11ea-8da2-2ccaa9dc4ba2.png)

helm chart repository를 추가해줍니다.

```
$ helm repo add stable https://kubernetes-charts.storage.googleapis.com/
```

chart list 출력 :

```
$ helm search repo stable
```

![image](https://user-images.githubusercontent.com/15958325/70848132-4e013200-1eb0-11ea-930c-1346603d48b1.png)

chart update :

```
$ helm repo update
```

![image](https://user-images.githubusercontent.com/15958325/70848135-5e191180-1eb0-11ea-9119-625b7decb370.png)
**Happy Helming!**

# 5. 실습 (Prometheus)

설치가 끝났으니 한번 차트를 클러스터에 배포해봅시다.

모니터링 툴 중 하나인 **Prometheus**를 배포해보겠습니다.

```
$ helm install monitor stable/prometheus
```

![image](https://user-images.githubusercontent.com/15958325/70848161-ab957e80-1eb0-11ea-85fb-c25b5aee1c71.png)

그런데 pod의 상태를 확인해보면 :

```
$ kubectl get pod
```

![image](https://user-images.githubusercontent.com/15958325/70848166-ccf66a80-1eb0-11ea-8ad7-777e2acfb25e.png)
몇 개의 pod이 **Pending**상태입니다.

이유는 k8s클러스터에 StorageClass가 정의되어있지 않기 때문입니다.
(pvc의 요청을 받아줄 provisioner가 없기 때문)

그래서 일단 pv옵션을 false로 변경해주어 EmptyDir을 사용하게 해야 합니다.

Helm chart의 설정을 변경하는 방법은 크게 두가지 방법이 있습니다.

## using yaml

문제가 되는 chart를 먼저 확인해봅시다.

```
$ helm inspect values stable/prometheus
```

![image](https://user-images.githubusercontent.com/15958325/70848197-3bd3c380-1eb1-11ea-9ccb-818be92e6318.png)

`persistentVolume.enabled`가 `True`입니다. 이렇게 표기되어있는 부분이 총 세군데가 있습니다.

수정할 부분만 따로 파일을 만들어주면 됩니다.

```
$ vim volumeF.yaml
alertmanager:
    persistentVolume:
        enabled: false
server:  
    persistentVolume:
        enabled: false
pushgateway: 
    persistentVolume:
        enabled: false
```

딱 이렇게만 적고 업그레이드해줍시다.

```
$ helm upgrade -f volumeF.yaml monitor stable/prometheus
```

업그레이드 하게되면 Pending이었던 pod들이 Running상태로 변하는 것을 확인할 수 있습니다.
![image](https://user-images.githubusercontent.com/15958325/70848257-e4822300-1eb1-11ea-8cb5-3f9e9ac6365b.png)

## using Command

다른 방법으로는 커맨드라인으로 설정을 추가해주는 방법이 있습니다.

```
outer:
  inner: value
```

위와 같은 표현식을 다음과 같이 표현할 수 있습니다.

```Shell
$ --set outer.inner=value
```

그럼 위 문제와 같은 경우는 다음과 같이 표현될 수 있습니다.

```Shell
$ helm install monitor stable/prometheus --set alertmanager.persistentVolume.enabled=false --set server.persistentVolume.enabled=false --set pushgateway.persistentVolume.enabled=false
```

------

pod들이 정상적으로 Running되었으니 웹으로 접속해봅시다.

```SHell
$ kubectl get svc
```

![image](https://user-images.githubusercontent.com/15958325/70848280-66724c00-1eb2-11ea-8bee-1d95daed1d7f.png)

`prometheus-server`를 `clusterIP`에서 `NodePort`로 변경해줍니다.(`spec.type`)

```
$ kubectl edit svc monitor-prometheus-server
```

![image](https://user-images.githubusercontent.com/15958325/70848299-bfda7b00-1eb2-11ea-9ed6-441b0f362dbc.png)
다시 확인해보면 포트포워딩된 포트가 보입니다.

ip:port로 접속해봅시다.
![image](https://user-images.githubusercontent.com/15958325/70848302-d1238780-1eb2-11ea-88bf-3b6aaf36526c.png)



## 삭제

삭제할때는 간단하게 설치할때 사용했던 이름만 사용하면 됩니다.

```Shell
$ helm uninstall {이름}
```
# Chapter 1 쿠버네티스 소개

### 모놀리스(Monolith)
소프트웨어 애플리케이션이 하나의 프로세스나 몇 개의 분산된 프로세스로 이루어진 구조

### MSA 
분산된 서버에서 독립적으로 개발, 배포 업데이트할 수 있는 구조

# 쿠버네티스
분산된 서버를 자동으로 배포하고 관리, 장애 처리하기 위한 솔루션

# 왜 쿠버네티스인가?
1. 거대 시스템에 갈수록 MSA 구조를 따름  
2. 개발 환경의 다양성  
  &nbsp;- MSA 환경에서 각 서버는 같은 라이브러리라도 서로 다른 버전에 종속될 수 있기 때문에 관리의 어려움을 쿠버네티스로 해결한다
3. 개발자의 배포 프로세스 간소화  
  &nbsp;- 개발자는 QA 팀과 소통하기 위해 애플리케이션을 자주 배포해야하는 경우가 많다  
  &nbsp;- 개발자가 하드웨어 인프라에 대해 전부 아는 것은 우선순위에 밀리는 행위이다  
  &nbsp;- 쿠버네티스를 사용하면 하드웨어 인프라를 알 필요없이 배포 프로세스를 간소화할 수 있고 인프라 관리자는 애플리케이션을 알 필요없이 시스템 인프라 운영에 더 집중할 수 있다  

## 가상 머신
- 한 서버에 여러 프로세스를 격리하기 위한 기술
- 호스트 OS 위에 가상 머신을 올려서 해당 가상 서버 위에 프로세스를 띄운다
- 가상 머신별로 게스트 OS가 존재
- 시스템 콜은 게스트 OS 에서 이루어지므로 보안성이 높다

## 리눅스 컨테이너
- 한 서버에 여러 프로세스를 격리하기 위한 기술
- 호스트 OS 위에 컨테이너로 격리된 프로세스를 띄움
- 게스트 OS 가 없어서 가벼움
- 모든 컨테이너의 시스템 콜이 호스트 OS 에서 이루어짐
- 컨테이너 실행 즉시 실행됌(가상 머신처럼 부팅 시간이 필요없다)

## 도커
애플리케이션을 실행 환경과 함께 패키징, 배포, 실행하기 위한 플랫폼

### 이미지
애플리케이션과 실행 환경을 패키지화한 것

### 레지스트리
도커 이미지를 공유할 수 있는 저장소

### 컨테이너
도커 기반 컨테이너 이미지에서 생성된 리눅스 컨테이너

### 이미지 레이어
- 두 개의 다른 이미지는 동일한 부모 이미지를 사용할 수 있다
- 첫 번째 이미지의 일부로 두 번째 이미지를 배포할 때 다시 배포할 필요가 없기 때문에 네트워크 리소스와 스토리지 공간을 절약할 수 있다

# 쿠버네티스
- 컨테이너화된 애플리케이션을 관리하고 독립적으로 개발, 배포하기 위한 솔루션
- 마스터 노드와 여러 워커 노드로 구성
- 개발자가 마스터에 애플리케이션 매니페스트를 게시하면 쿠버네티스가 해당 애플리케이션을 워커 노드 클러스터에 배포

### 컨트롤 플레인(Control Plane)
- 마스터 노드에 의해 실행되며 클러스터를 제어함
- 쿠버네티스 API는 사용자, 컨트롤 플레인 구성 요소와 통신
- 스케줄러는 애플리케이션 배포 담당
- 컨트롤러 매니저는 복제, 워커 노드 추적, 노드 장애 처리 같은 클러스터단 기능 수행
- etcd는 분산 데이터 저장소

### 노드
- 워커 노드는 컨테이너화된 애플리케이션을 실행하는 시스템이다
- kubelet은 API 서버와 통신
- kube-proxy은 네트워크 트래픽을 로드밸런싱

## 쿠버네티스에서 애플리케이션 실행
쿠버네티스 API에 애플리케이션 디스크립션을 게시하면 이미지 레지스트리로부터 가용한 워커 노드에 컨테이너를 할당한다

## 복제본 수 스케일링
부하 분산을 위해 최적의 복제본 수를 결정

## 상태 확인과 자가 치유
노드 장애시 다른 정상 노드로 애플리케이션을 스케줄링한다

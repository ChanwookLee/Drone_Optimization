# 드론의 운용단계 최적화

## 1. 본 저장소에 대하여
해당 저장소는 KAIST 의 CAD4X 연구실에서 진행 중인 드론의 운용단계 최적화의 연구 과정을 기록하기 위한 저장소입니다.
드론의 운용단계 최적화의 연구는 기본적으로 드론의 최단 비행경로를 찾는 문제로써 이를 구현하기 위하여 다음과 같은 과정을 진행하였습니다.

* 드론의 정확한 파워 모델링
  * RTOS (Real-Time OS) 를 통한 드론의 정확한 전력 소비 측정
  * 머신러닝 (Keras) 를 이용한 드론의 파워 소비 측정 모델 생성

* 오프라인 환경에서의 드론의 에너지 소비 경로 추정
  * Matlab 과 Python 을 이용한 드론의 최적 경로 추정 (offline)
  * 강화학습 (Tensorflow) 을 이용한 드론의 최적 경로 추정 (offline)

* 온라인 환경에서의 드론의 에너지 소비 경로 추정
  * ROS 를 이용한 PX4 무인 주행 환경 구성
  * 무인 주행 환경에서의 드론의 최적 경로 추정 (Online)

본 문서에서는 각 과정에서 작성한 코드들이 동작하는 원리와 해당 소프트웨어의 세팅을 설명하는 문서를 작성하여, 추후 필요로 할 때 해당 내용을 확인을 하거나 응용을 할수 있게끔 하고자 합니다.

## 2. 드론의 정확한 파워 모델링
현재 국가연구과제로써 진행 중인 **드론의 설계 및 관리 자동화 : 체계적인 통합 저전력 설계 및 관리를 통한 드론의 총 운용비용 최적화** 는 현재 사용되고 있는 무인형 비행체 (UAV) 의 전력 소모율이 매우 비효율적이며, 이를 관리하는 운영체계가 현재로써는 없다는 점을 착안하여 연구를 진행하였습니다.

따라서 우리는 보다 드론의 전력 소모를 정확하게 볼 필요가 있었으며, 드론에 내부의 비행 컨트롤러가 기록하는 전력 소모 데이터 로거 대신 우리가 직접 제작한 전력 소모 측정 보드를 이용하여 전력 소모 데이터를 수집하였습니다.

현재 사용하고 있는 드론은 DJI 사의 Matrice 600 모델이며, 우리는 6개의 모터를 가진 이 드론의 모터부에서 흐르는 전류와 전압을 1 kHz의 샘플링레이트로 측정하였고 이를 비행 컨트롤러에서 저장하는 속도, 가속도 값과 매칭하여 데이터를 기록하였습니다.

해당 폴더에 포함된 내용은 다음과 같습니다.
* RTOS 를 이용한 임베디드 보드의 전력 소모 측정 코드
  * 우리는 TI 사의 MCU 를 토대로 하는 임베디드 보드를 제작하였으며, 해당 보드는 매우 높은 정확도와 1 kHz의 샘플링레이트를 가집니다.
  * 문서로 남겨 놓은 사항은 TI 사의 IDE인 CCS의 세팅과 작성한 코드에 대한 설명으로, 코드가 동작하는 플로우와 각 함수의 역할을 설명하고자 합니다. 



## 3. 오프라인 환경에서의 드론의 에너지 소비 경로 추정 
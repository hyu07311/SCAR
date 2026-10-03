# 🤖 SCAR (Stair-Cleaning Autonomous Robot)
> **비정형 계단 환경 대응 실시간 인식 및 자율 판단·제어 기반 인공지능 계단 청소 로봇**  

---

## 1. 프로젝트 개요 (Overview)
* **배경**: 계단 청소 작업 중 발생하는 고령 미화 노동자의 낙상 등 중대 산업재해 위험을 줄이고, 건물마다 단차·깊이가 상이한 비정형 계단 환경을 자동화하기 위해 개발되었습니다.
* **목표**: 사전 계단 정보(단차 높이 등) 입력 없이, 실시간 센서 피드백을 통해 스스로 등반·청소·복귀를 수행하는 공공시설 특화 자율 청소 로봇 구현.

---

## 2. 주요 핵심 기술 (Key Features)

* **변형 메커니즘 & 지능형 등반 (PATS Wheel & Motor Load Control)**
  * 장애물 접촉 시 수동 변형되는 특수 바퀴(PATS Wheel) 적용 (바퀴 지름 24cm 대비 16cm 단차 극복, 66.7%).
  * Dynamixel MX-106 모터의 부하(Present Load) 모니터링을 통한 4륜 개별 단차 접촉 및 등반 판정.
* **비전 기반 인식 및 안전 정지 (YOLOv8 & V-SLAM)**
  * Intel RealSense D435i 단일 카메라 기반의 계단(Upstairs) 및 사람(Person) 실시간 인식.
  * 계단 모서리 라인 검출(Line-fit)을 통한 정밀 Yaw 회전 정렬.
  * 보행자 접근 시 안전 정지 로직(3m 이내 접근 시 정지, 7초 후 자동 복귀) 탑재.
* **사각지대 없는 밀착 청소 (Adaptive Cleaning Mechanism)**
  * 90도 조향 후 계단 측면 주행 및 리드 스크류 기반 깊이 방향 사각지대 분진 제거.
  * 듀얼 리니어 액추에이터 + 로드셀 피드백을 통한 계단 면 균일 하중 밀착 제어.
  * 드론용 고RPM BLDC 모터 기반의 자체 제작 사이클론 집진 팬 탑재.

---

## 3. 전체 시스템 파이프라인 (System Pipeline)
사람의 개입 없이 6단계 상태 머신(FSM)에 의해 완전 자율 구동됩니다:

1. **계단 탐색 (Search)**: V-SLAM 및 YOLOv8 기반 계단 탐색
2. **자율 정렬 (Align)**: 계단 단차 모서리 Depth 스캔 및 Yaw 오차 보정
3. **단차 등반 (Climbing)**: PATS Wheel 및 4륜 모터 부하값 피드백 등반
4. **조향 (Steering)**: 90도 조향 기어 구동을 통한 측면 청소 모드 진입
5. **밀착 (Docking)**: 로드셀 압력 피드백 기반 리니어 액추에이터 밀착
6. **측면 청소 (Cleaning)**: 초음파 센서 감지 기반 측면 주행 및 디딤판 흡입 청소

---

## 4. 개발 환경 및 사양 (Tech Stack)

### Software & Middleware
* **OS / Middleware**: Ubuntu 20.04 / ROS2 Foxy, micro-ROS
* **Environment**: JetPack 5.1.5, CUDA 11.4, TensorRT 8.4.1
* **Languages**: C++, Python
* **Algorithms**: YOLOv8, SLAM Toolbox, Nav2, Point Cloud Filter (Voxel, SOR, Pass-through), EKF

### Hardware
* **SBC / MCU**: NVIDIA Jetson AGX Xavier / OpenCR 1.0
* **Sensors**: Intel RealSense D435i, Load Cell (x2), HC-SR04
* **Actuators**: 
  * 구동 및 조향: Dynamixel MX-106, MX-64, XL430-W250
  * 청소 및 흡입: Linear Actuator, F60 BLDC Motor
* **Power**: LiFePO4 12.8V 독립 2계통 공급 (구동계 / 제어·인식계)

---

## 5. 팀원 및 역할 (Team)
| 이름 | 담당 역할 | 세부 업무 |
| :--- | :--- | :--- |
| **황유** | 임베디드 제어 & 회로 | OpenCR 기반 모터/센서 제어, micro-ROS 연동, 전원 계통 설계, TensorRT 모델 최적화, 시스템 통합 & 검증 |
| **강준영** | 기구 설계 & 제작 | 3D 모델링, PATS Wheel 및 로봇 플랫폼 3D 프린팅·조립 |
| **조하진** | 자율주행 & SLAM | V-SLAM 포인트 클라우드 필터링, EKF 융합, Nav2 내비게이션, YOLOv8 데이터셋 구축 및 학습 |

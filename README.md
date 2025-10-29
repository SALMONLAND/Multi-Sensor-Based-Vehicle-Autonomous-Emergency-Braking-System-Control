# Multi-Sensor-Based-Vehicle-Autonomous-Emergency-Braking-System-Control
자율 비상제동 시스템 실험 — 초음파 및 적외선 센서 융합 기반 차량 제동 알고리즘 (by 이동혁, 동원동우고등학교)


# 🚗 자율 비상제동 시스템 (Autonomous Emergency Braking System)

> 연구자: **이동혁 (동원동우고등학교)**  
> 실험 주제: **초음파 + 적외선 센서 융합 기반 자율 비상제동(AEB) 알고리즘 구현**

## 📘 개요 (Overview)

이 프로젝트는 **아두이노 기반의 자율 비상제동 시스템(AEB, Autonomous Emergency Braking)** 을 구현한 연구입니다.  
주행 중 전방 장애물을 **초음파 센서**와 **적외선(IR) 센서**로 감지하고, 위험 상황일 경우 즉시 정지하도록 설계되었습니다.

## 🧩 실험 단계

| 버전 | 센서 구성 | 특징 |
|------|------------|------|
| **v1.0** | 초음파 센서 (HC-SR04) | 거리 기반 제동 실험 |
| **v2.0** | 적외선 센서 (IR) | 단순 감지 기반 제동 실험 |
| **v3.0 (Final)** | 초음파 + 적외선 센서 융합 | 센서 융합 기반 안정적 제동 구현 |

## ⚙️ 하드웨어 구성 (Hardware Setup)

| 구성 요소 | 역할 |
|------------|------|
| Arduino Uno (또는 호환 보드) | 메인 컨트롤러 |
| HC-SR04 초음파 센서 | 거리 감지 (2~400cm) |
| IR 센서 | 물체 감지 (디지털 입력) |
| L298N 모터 드라이버 | DC 모터 제어 |
| DC Motor 2개 | 차량 구동 |
| 기타 | 배선, 전원, 실험용 자동차 섀시 |


## 🔌 핀 연결 (Pin Configuration)

| 핀 이름 | 역할 |
|----------|------|
| `TRIG` (3) | 초음파 트리거 |
| `ECHO` (2) | 초음파 에코 |
| `IR` (4) | 적외선 센서 입력 |
| `MOTOR_A1` (9) / `MOTOR_A2` (10) | 좌측 모터 제어 |
| `MOTOR_B1` (6) / `MOTOR_B2` (7) | 우측 모터 제어 |


## 🧮 동작 원리 (Logic)

1. **초기화 단계**
   - 모터 정지, 3초 카운트다운 후 전진 시작.
2. **센서 데이터 수집**
   - 초음파 거리 및 IR 감지 상태를 지속적으로 측정.
3. **판단 알고리즘**
   - 초음파 거리 ≤ 목표 거리(20cm) **AND** IR 감지 == TRUE → 위험 판단.
4. **비상 제동**
   - 즉시 모터 정지 및 결과 출력.
5. **결과 표시**
   - 최종 거리 및 감지 결과를 `Serial Monitor`로 출력.

---

## 🧠 센서 융합 로직

```c
bool shouldStop(SensorData data) {
  bool ultraTrig = (data.ultraValid && data.ultraDist <= targetDist);
  bool irTrig = data.irDetected;
  return (ultraTrig && irTrig); // 두 센서 모두 위험 감지 시 정지
}

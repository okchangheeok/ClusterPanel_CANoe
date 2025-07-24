# 🚗 차량 통신 시뮬레이션 미니 프로젝트

## 📌 개요

이 프로젝트는 차량 내부의 **CAN 통신 구조**를 기반으로 속도계, RPM 게이지, 기어 변속 등을 시각적으로 표현하는 **시뮬레이션 시스템**입니다. 사용자는 엑셀(가속) 값을 조절하거나 기어를 수동으로 조작하며, 실제 차량의 동작처럼 **속도와 RPM이 연동**되는 모습을 확인할 수 있습니다.

> 본 프로젝트는 **Vector CANoe(VN5620)** 환경에서 구현되었으며, 차량 ECU 통신 로직을 직접 설계 및 구현하였습니다.

---

## 🎞️ 대표 시연 영상

https://github.com/user-attachments/assets/63aeba23-6821-40dc-8067-de5a99e09b44

---

## ⚙️ 기능 설명

| 기능명              | 설명                                                                 |
|-------------------|----------------------------------------------------------------------|
| 엑셀 조작          | 슬라이더를 통해 0~100까지 입력 가능.                                         |
| 변속기 조작        | ↑, ↓ 버튼으로 단수 조작 가능 (0단 ~ 8단),                                      |
| 속도 게이지        | `km/h` 단위로 엑셀 및 단수에 따라 실시간 반영                                  |
| RPM 게이지         | 낮은 단수에서 무리하게 엑셀을 밟을 시 높게 상승                                  |
| 경고 시스템        | 5600 이상 시 경고 발생                                                     |

---

## 🧠 CAN 스크립트 요약 (CAPL)

- `on timer sendTimer`: 100ms 주기로 차량의 상태를 CAN 메시지로 송신
- `rpm 계산 수식`:
  - 1~7단: `rpm = (속도 - 기준속도) × 93.3`
  - 8단: `rpm = (속도 - 180) × 46.7`
- 속도 계산:  
  `속도 = accel × 2.4`  
  단, 일정 엑셀 이상일 경우 `속도 = 30 × (단수+1)`
- 경고 조건:  
  `rpm > 5600`이면 경고 활성화

---

## 🧩 주요 시각적 요소들 속성

프로젝트 구성에서 사용된 주요 패널 요소들은 아래와 같은 속성(Properties)을 가지고 있습니다.


![Tool](https://github.com/user-attachments/assets/3782a8ca-1fb3-4d24-9026-53d40cc2c43e)

<details> <summary> RPM </summary>
<img src="https://github.com/user-attachments/assets/9ef2b2a1-3448-41bf-a8c2-80370f4ca71f" width="300">
</details>
<details> <summary> RpmWarn </summary>
<img src="https://github.com/user-attachments/assets/4149bdca-acaa-4657-a421-1bd98fbf2df9" width="300">
</details>
<details> <summary> AccelTrackBar </summary>
<img src="https://github.com/user-attachments/assets/1056e6a7-6a0c-4b57-99d1-b771c8eb53bc" width="300">
</details>
<details> <summary> TransmissionUpButton </summary>
<img src="https://github.com/user-attachments/assets/dbd09174-1c0c-4299-adfa-7d17f32f23b5" width="300">
</details>
<details> <summary> TransmissionOutputBox </summary>
<img src="https://github.com/user-attachments/assets/58d316c4-54c6-44ab-9822-88b9e99eb6f1" width="300">
</details>
<details> <summary> Speed </summary>
<img src="https://github.com/user-attachments/assets/6321da02-020e-4872-af4f-2f8bde1102a8" width="300">
</details>





---

## 🛠️ 개발 환경

- **개발 툴**: Vector CANoe
- **주요 변수**:
  - `rpm` 
  - `Accel`
  - `Speed`
  - `Transmission`
  - `RpmWarn`

---

## 📝 기타 참고사항

- `rpm`과 `속도`는 선형적으로 반응하도록 설계되었으며, 기어가 올라갈수록 **속도 증가에 따른 RPM 민감도는 낮아집니다.**
- 실제 차량과 유사한 동작 패턴을 반영하기 위해 **단수별로 기준 속도와 RPM 민감도가 다르게 설정**되어 있습니다.

---

## ✨ 향후 개선점

- GUI 상에서 자동 변속 로직 추가
- 경고 발생 시 색상 및 사운드 반응
- 데이터 로깅 기능 추가 (CSV or MDF 등)

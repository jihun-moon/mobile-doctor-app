<h1 align="center">🩺 Mobile Doctor</h1>

<p align="center"><em>위치 기반 병원 검색 + 증상 검색 + 복약 이력까지, 한 번에 관리하는 개인 맞춤형 의료 정보 Android 앱.</em></p>

<p align="center">
  <img src="https://img.shields.io/badge/license-MIT-blue?style=flat-square"/>
  <img src="https://img.shields.io/badge/Android-3DDC84?style=flat-square&logo=android&logoColor=white"/>
  <img src="https://img.shields.io/badge/Java-17-007396?style=flat-square&logo=openjdk&logoColor=white"/>
  <img src="https://img.shields.io/badge/Gradle_Kotlin_DSL-02303A?style=flat-square&logo=gradle&logoColor=white"/>
  <img src="https://img.shields.io/badge/minSdk-24-yellow?style=flat-square"/>
  <img src="https://img.shields.io/badge/targetSdk-34-success?style=flat-square"/>
  <a href="https://www.notion.so/Mobile-Doctor-Android-93f528f91dae4711bd7f2ae923edce6f?source=copy_link"><img src="https://img.shields.io/badge/Deep_Dive-Notion-000000?style=flat-square&logo=notion&logoColor=white"/></a>
</p>

<p align="center">
  <img src="assets/mobile-doctor-demo.gif" alt="Demo" width="32%">
</p>

---

## 🎯 Why this project

병원 영업 시간, 약 정보, 증상별 추천, 진료 이력 — 사용자가 정작 필요한 순간에 흩어져 있는 정보들입니다. **하나의 앱에서 위치/증상/이력 컨텍스트로 통합 조회** 할 수 있어야, 의료 정보 접근의 마찰이 줄어듭니다.

---

## ✨ Key Features

| 기능 | 화면 (Activity) | 설명 |
|---|---|---|
| 🗺 **위치 기반 병원/약국 검색** | `MapActivity` | 현재 위치 기준 주변 병원/약국 핀 + 영업 상태 (open/closed/unknown) 마커 |
| 🔎 **증상 기반 검색** | `SymptomSearchActivity` | 증상 키워드로 해당 진료과/약 추천 |
| 📜 **진료 이력 관리** | `HospitalHistoryActivity` | 방문한 병원/처방 기록 누적 |
| 💊 **복약 인벤토리** | `InventoryActivity` + `MedicineAdapter` | 보유 약 / 복용 일정 관리, 약 이미지 미리보기 |
| 🖼 **풀스크린 이미지 뷰어** | `FullscreenImageActivity` | 약/병원 사진을 줌인하여 확인 |
| ✨ **스플래시 + 애니메이션** | `SplashActivity` + `res/anim/` | fade_in / slide_up / move_down 모션으로 진입 경험 향상 |

---

## 🏗 App Architecture

```
SplashActivity
      │
      ▼
MainActivity ─────────────┐
      │                    │
      ├─▶ MapActivity      │ (location, 병원/약국 핀)
      ├─▶ SymptomSearch    │ (증상 → 추천)
      ├─▶ HospitalHistory  │ (진료 이력)
      └─▶ Inventory  ──────┴─▶ MedicineAdapter ─▶ FullscreenImage
                                  (약 카드 리스트)
```

`BaseActivity` 가 공통 상태/스타일을 보유하고, 도메인 모델 `Medicine` 이 어댑터를 통해 RecyclerView 에 바인딩됩니다.

---

## 🛠 Tech Stack

| Layer | Tool | Role |
|---|---|---|
| Language | **Java 17** | activity / model / adapter |
| UI | **Android Views** (XML layouts) | 화면 구성 |
| Build | **Gradle (Kotlin DSL)** | `app/build.gradle.kts` |
| Animation | **res/anim/*** | fade / slide / move 모션 |
| Maps | Android Location | 주변 병원/약국 표시 |

---

## 📦 Build & Run

1. Android Studio 로 프로젝트 열기 (`src/` 디렉토리가 모듈 루트)
2. `Sync Project with Gradle Files` 실행
3. 에뮬레이터 또는 실기기에서 Run ▶

**요구 환경**

- Android Studio Hedgehog 이상
- JDK 17
- minSdk 24 / targetSdk 34

---

## 📂 Project Layout

```
src/
└── app/
    ├── build.gradle.kts
    ├── proguard-rules.pro
    └── src/main/
        ├── AndroidManifest.xml
        ├── java/com/example/mobiledoctor/
        │   ├── BaseActivity.java         # 공통 베이스
        │   ├── SplashActivity.java       # 진입 화면
        │   ├── MainActivity.java         # 메인 허브
        │   ├── MapActivity.java          # 위치 기반 검색
        │   ├── SymptomSearchActivity.java
        │   ├── HospitalHistoryActivity.java
        │   ├── InventoryActivity.java
        │   ├── InventoryAdapter.java     # RecyclerView adapter
        │   ├── Medicine.java             # 도메인 모델
        │   ├── MedicineAdapter.java
        │   └── FullscreenImageActivity.java
        └── res/
            ├── anim/                     # fade/slide/move 모션
            ├── drawable/                 # 약 이미지, 마커 아이콘
            └── layout/                   # XML 레이아웃
assets/
└── mobile-doctor-demo.gif
```

---

## 📚 Deep Dive

설계 결정 (Activity 분리 기준, RecyclerView 패턴, 이미지 자산 관리 등) 과 회고는 Notion 에 정리되어 있습니다.

➡ [**Mobile Doctor — 프로젝트 회고**](https://www.notion.so/Mobile-Doctor-Android-93f528f91dae4711bd7f2ae923edce6f?source=copy_link)

---

## 📄 License

MIT — see [LICENSE](LICENSE).

---

<p align="center">
  Built by <a href="https://github.com/jihun-moon">@jihun-moon</a> · <a href="mailto:jihun0948@naver.com">jihun0948@naver.com</a>
</p>

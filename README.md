# Mobile Doctor

주변 병원과 약국을 지도에서 찾고, 증상으로 약을 검색하고, 진료 이력을 기록하는 안드로이드 앱입니다.
2인 팀 과제였고(2025-05 ~ 06), 저는 화면 뼈대(Splash, BaseActivity)와 지도 검색(Google Maps/Places API), 증상 검색, 돋보기·밝기 기능을 맡았습니다.

<p>
  <img src="https://img.shields.io/badge/Android-3DDC84?style=flat-square&logo=android&logoColor=white"/>
  <img src="https://img.shields.io/badge/Java_11-007396?style=flat-square&logo=openjdk&logoColor=white"/>
  <img src="https://img.shields.io/badge/Gradle_Kotlin_DSL-02303A?style=flat-square&logo=gradle&logoColor=white"/>
  <img src="https://img.shields.io/badge/compileSdk-35-success?style=flat-square"/>
  <img src="https://img.shields.io/badge/minSdk-34-orange?style=flat-square"/>
  <img src="https://img.shields.io/badge/license-MIT-blue?style=flat-square"/>
  <a href="https://www.notion.so/Mobile-Doctor-Android-93f528f91dae4711bd7f2ae923edce6f?source=copy_link"><img src="https://img.shields.io/badge/회고-Notion-000000?style=flat-square&logo=notion&logoColor=white"/></a>
</p>

<p><img src="assets/mobile-doctor-demo.gif" alt="시연" width="32%"></p>

<br/>

## 1. 왜 만들었나

몸이 안 좋을 때 필요한 정보가 다 흩어져 있습니다. 지금 문 연 병원이 어디인지, 이 증상이면 무슨 과인지,
집에 있는 약이 이 증상에 맞는 약인지. 각각 다른 앱이나 검색으로 찾아야 합니다.

이 세 가지를 한 화면 흐름 안에서 이어 보자는 게 시작이었습니다.

<br/>

## 2. 화면 구성

```
SplashActivity
     │
     ▼
MainActivity
     ├──▶ MapActivity              지도에서 병원/약국 찾기
     │        └──▶ InventoryActivity         약국 재고 (예시 데이터)
     ├──▶ SymptomSearchActivity    증상으로 약 검색
     │        └──▶ FullscreenImageActivity   약 사진 크게 보기
     └──▶ HospitalHistoryActivity  진료 이력
```

`BaseActivity` 가 공통 처리를 담고 나머지가 이걸 상속합니다.
리스트는 `MedicineAdapter` 와 `InventoryAdapter` 가 RecyclerView 에 붙입니다.
도메인 모델은 `Medicine` 하나입니다.

코드 분량은 이렇습니다.

| 파일 | 크기 |
|---|---|
| `SymptomSearchActivity.java` | 27.1KB |
| `MapActivity.java` | 20.8KB |
| `HospitalHistoryActivity.java` | 13.5KB |
| `MainActivity.java` | 8.8KB |
| `InventoryActivity.java` | 8.0KB |
| `Medicine.java` | 1.1KB |

증상 검색이 제일 큽니다. 증상과 진료과, 약을 잇는 매핑을 코드 안에 넣어서 그렇습니다.
이건 데이터로 빼야 하는 부분인데 과제 기간에 못 했습니다.

<br/>

## 3. 영업 상태를 마커로 구분했다

지도에서 제일 중요한 건 "지금 문을 열었는가" 입니다. 그래서 마커를 상태별로 나눴습니다.

| 아이콘 | 의미 |
|---|---|
| `ic_hospital_open` / `ic_pharmacy_open` | 영업 중 |
| `ic_hospital_closed` / `ic_pharmacy_closed` | 영업 종료 |
| `ic_hospital_unknown` / `ic_pharmacy_unknown` | 영업 정보 없음 |

영업 여부는 Google Places 주변 검색 응답의 `opening_hours.open_now` 로 나눕니다. `opening_hours` 가 아예 없으면 unknown 입니다.
unknown 을 따로 둔 이유는, 정보가 없는 곳을 닫힌 것으로 표시하면 갈 수 있는 병원을 놓치기 때문입니다.
모른다는 걸 모른다고 보여 주는 편이 낫다고 봤습니다.

<br/>

## 4. 약 이미지를 다 넣었다

`res/drawable` 에 약 사진 40여 장이 들어 있습니다. 아스피린, 이부프로펜, 알프라졸람, 카르베딜롤, 겔포스,
디곡신 같은 식으로 파일명을 성분이나 제품명으로 붙였습니다.

증상 검색 결과에서 약 사진을 누르면 `FullscreenImageActivity` 로 넘어가서 크게 보여 줍니다.
글자로만 있으면 집에 있는 약이 그 약인지 확인이 안 돼서 넣었습니다.

이미지가 앱 안에 박혀 있어서 약을 추가하려면 앱을 다시 빌드해야 합니다. 이것도 서버에서 받아 오게 바꿔야 하는 부분입니다.

<br/>

## 5. 지금 알고 있는 문제

**밝기 슬라이더가 `WRITE_SETTINGS` 로 시스템 밝기를 바꿉니다.**

`AndroidManifest.xml` 에 선언된 권한은 네 개입니다.

```
INTERNET
ACCESS_FINE_LOCATION
ACCESS_COARSE_LOCATION
WRITE_SETTINGS
```

앞의 셋은 지도와 위치에 필요합니다. `WRITE_SETTINGS` 는 메인 화면의 밝기 슬라이더가 씁니다.
슬라이더가 `Settings.System.SCREEN_BRIGHTNESS` 를 직접 고치고, 권한이 없으면 메인 화면에 들어가자마자 시스템 설정 권한 화면이 뜹니다.
앱 창 밝기만 바꾸는 `WindowManager.LayoutParams.screenBrightness` 를 썼으면 이 권한이 필요 없었습니다.
의료 정보를 다루는 앱이 필요 없는 시스템 권한을 들고 있으면 그 자체가 문제라
지우는 게 맞습니다.

**`minSdk` 가 34 입니다.**

안드로이드 14 이상에서만 설치됩니다. 개발 기기에 맞춰서 올려 둔 값인데
이러면 쓸 수 있는 사람이 크게 줄어듭니다. 실제로 쓰이는 앱이라면 24 근처까지 내려야 합니다.

**증상과 약 매핑이 코드 안에 있습니다.**

`SymptomSearchActivity.java` 가 27KB 인 주된 이유입니다. 데이터를 고치려면 코드를 고쳐야 합니다.

<br/>

## 6. 구조

```
src/app/
├── build.gradle.kts            compileSdk 35 / minSdk 34 / Java 11
├── proguard-rules.pro
└── src/main/
    ├── AndroidManifest.xml
    ├── java/com/example/mobiledoctor/
    │   ├── BaseActivity.java
    │   ├── SplashActivity.java
    │   ├── MainActivity.java
    │   ├── MapActivity.java
    │   ├── SymptomSearchActivity.java
    │   ├── HospitalHistoryActivity.java
    │   ├── InventoryActivity.java
    │   ├── InventoryAdapter.java
    │   ├── MedicineAdapter.java
    │   ├── Medicine.java
    │   └── FullscreenImageActivity.java
    └── res/
        ├── anim/        fade_in, slide_up, move_down
        ├── drawable/    약 사진 40여 장 + 마커 아이콘 6종
        └── layout/      화면 10개
```

<br/>

## 7. 실행

저장소만 받아서는 빌드되지 않습니다. 키가 든 파일과 Gradle 설정을 `.gitignore` 로 뺐습니다. 아래 셋을 먼저 만듭니다.

- `src/gradle/libs.versions.toml` : `libs.appcompat` 같은 버전 카탈로그와 `android.application` 플러그인
- `src/gradle.properties` : `MAPS_API_KEY`, `PLACES_API_KEY` (Google Maps SDK, Places API 키)
- `src/app/src/main/res/values/strings.xml` : `app_name`, `PLACES_API_KEY`, `symptom_prompt`, `category_prompt`, `medicine_image`

1. Android Studio 에서 `src/` 를 프로젝트로 엽니다
2. Sync Project with Gradle Files
3. Run

JDK 17 로 빌드하고 소스 호환성은 Java 11 입니다. 기기는 안드로이드 14 이상이어야 합니다.

<br/>

만들면서 고민한 것은 [Notion 회고](https://www.notion.so/Mobile-Doctor-Android-93f528f91dae4711bd7f2ae923edce6f?source=copy_link) 에 적어 뒀습니다.

MIT License

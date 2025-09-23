# 🩺 Mobile Doctor | Android App

> ### 3줄 요약
>
>   - **종합 의료 정보 앱**: 위치 기반 병원/약국 정보, 증상별 의약품 검색, 개인의 병원 방문 기록 관리 기능을 제공하는 안드로이드 앱입니다.
>   - **핵심 기술**: Google Maps/Places SDK, 로컬 SQLite 데이터베이스, `RecyclerView`를 기반으로 제작되었습니다.
>   - **사용자 중심 설계**: 접근성 강화를 위한 전역 글자 확대 및 화면 밝기 조절 기능과, 사용자의 최근 활동을 분석하여 맞춤형 건강 코멘트를 제공합니다.

-----

## ✨ 주요 기능

  - **지도 기반 병원/약국 검색**: `Google Maps/Places SDK`를 활용하여 사용자 주변의 병원과 약국을 검색하고, 커스텀 마커와 영업 상태 아이콘으로 시각화합니다.
  - **증상별 의약품 검색**: `RecyclerView`와 필터 기능을 이용해 증상에 맞는 의약품 목록을 검색하고 상세 정보를 확인할 수 있습니다.
  - **병원 이용 기록 관리**: `SQLite` 로컬 데이터베이스에 사용자의 진단명, 증상, 비용 등 병원 방문 기록을 저장하고 조회합니다.
  - **접근성**: 모든 화면에 일관되게 적용되는 글자 크기 확대 토글 기능과, 홈 화면의 밝기 조절 슬라이더를 제공합니다.
  - **홈 피드 건강 코멘트**: 사용자의 최근 병원 방문 빈도를 분석하여, 홈 화면에 개인화된 건강 관련 문구를 노출합니다.

-----

## 🛠️ 기술 스택 및 아키텍처

| 구분 | 기술 |
| :--- | :--- |
| **언어/환경** | `Java`, `Android Studio`, `Gradle (KTS)` |
| **UI** | `AndroidX AppCompat`, `Material Components`, `RecyclerView`, `XML Layout` |
| **지도/위치** | `Google Maps SDK`, `Google Places SDK`, `FusedLocationProvider` |
| **데이터** | `SQLite` (로컬 DB), `SharedPreferences` (설정 저장) |
| **네트워킹** | `OkHttp` (Google Places API 통신) |
| **아키텍처**| `BaseActivity`를 통한 공통 기능 상속, `Adapter`/`ViewHolder` 패턴 |

-----

## 🚀 빌드 및 실행

1.  Android Studio에서 프로젝트를 엽니다.
2.  `local.properties` 또는 `gradle.properties` 파일에 Google Maps/Places API 키를 설정합니다.
    ```properties
    MAPS_API_KEY=YOUR_GOOGLE_MAPS_API_KEY
    PLACES_API_KEY=YOUR_GOOGLE_PLACES_API_KEY
    ```
3.  Gradle Sync를 실행한 후, 에뮬레이터 또는 실제 기기에서 앱을 실행합니다.

-----

## 🔐 필요 권한 (`AndroidManifest.xml`)

```xml
<uses-permission android:name="android.permission.INTERNET"/>
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>
```

-----

## 🖼️ 데모

\<img src="assets/mobile-doctor-demo.gif" alt="App Demo GIF"/\>

-----

## 🗺️ 로드맵 (향후 개발 계획)

  - [ ] **아키텍처 개선**: Room 라이브러리로 DB 마이그레이션, ViewModel + LiveData 도입
  - [ ] **기능 확장**: Retrofit을 이용한 공공데이터포털 의약품 API 연동
  - [ ] **사용성 개선**: 다크 모드 지원, 지도 오프라인 캐시 기능 추가
  - [ ] **코드 품질**: UI 테스트(Espresso) 및 단위 테스트(JUnit) 코드 작성

-----

## 🪪 라이선스

이 프로젝트는 [MIT 라이선스](https://opensource.org/licenses/MIT)를 따릅니다.

# Mobile Doctor App

위치 기반 병원 검색과 진료 기록 관리를 위한 모바일 앱. 오프라인 캐시와 센서 융합으로 위치 정밀도를 높이고, API 쿼터·네트워크 오류 상황에서 요청 큐·백오프·리트라이·서킷 브레이커를 적용해 신뢰성을 확보했습니다.

- Platform: Android
- Language: Kotlin or Java
- Key topics: LBS, Offline-first, Reliability patterns, Encryption

## Demo
<img src="assets/mobile-doctor-demo.gif" alt="App demo" width="50%">

## Features
- 근거리 병원 검색: 반경, 전문과 필터
- 진료 기록 요약, 즐겨찾기, 오프라인 조회
- 요청 큐 + 지수 백오프 리트라이, 서킷 브레이커
- 민감 데이터 암호화, 지표 기반 모니터링

## Architecture
- Presentation: Activity/Fragment + ViewModel + ViewBinding
- Data: Repository + Local(오프라인 캐시) + Remote(API)
- Reliability: Retry with backoff, Circuit Breaker, Request Queue
- Security: 암호화 저장소(예: EncryptedSharedPreferences)

## Getting Started
1. Android Studio 최신 버전 설치
2. Clone
   - git clone <repo-url>
3. Open and Run
   - app 모듈 실행
4. 환경 변수
   - local.properties 또는 BuildConfig로 API 키 주입

## Example (Retry + Backoff)
```kotlin
suspend fun <T> retryWithBackoff(
    max: Int = 3,
    baseMs: Long = 300L,
    block: suspend () -> T
): T {
    var attempt = 0
    var delayMs = baseMs
    while (true) {
        try { return block() }
        catch (e: IOException) { /* 네트워크 등 일시 오류 */ }
        if (++attempt >= max) throw e
        delay(delayMs)
        delayMs *= 2 // 지수 백오프
    }
}
```

## Offline Cache
- Room/SQLite로 최근 검색 결과와 즐겨찾기 저장
- 캐시 만료 정책과 동기화 트리거 분리

## Folder Structure
```
/assets/                        # README 이미지 (mobile-doctor-demo.gif 등)
/app/src/main/java/...         # 소스
/app/src/main/res/...          # 레이아웃/리소스
README.md
```

## Checklist
- [ ] 위치 권한/정밀도 설정 가이던스
- [ ] 오프라인 캐시 만료·동기화 정책 문서화
- [ ] 에러 코드별 리트라이/중단 규칙 분리
- [ ] 암호화 저장·전송(HTTPS/Pinning 옵션) 점검
- [ ] UI 스레드 규칙·린트·테스트 통과

## Troubleshooting
- 위치 값 튀는 현상: 센서 융합 파라미터/필터 조정
- 쿼터 초과: 서버 응답 코드별 쿼터 대기 + 사용자 알림
- 느린 시작: 캐시 프리워밍 및 비동기 초기화

## Links
- Notion Page: Mobile Doctor App[[1]](https://www.notion.so/f8e8f6e415dd40f795f80c830003c1c4)
- GitHub Repo: https://github.com/jihun-moon/mobile-doctor-app

## License
MIT 또는 프로젝트 정책에 맞는 라이선스 명시

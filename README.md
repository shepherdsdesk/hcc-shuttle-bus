# HCC 셔틀버스 · 할렐루야 셔틀버스 실시간 안내

할렐루야교회 셔틀버스(야탑역 BYC 앞 ↔ 교회, 파킹엔젤스 테크노파크 C동 ↔ 차량등록사업소 ↔ 교회)가
지금 어디쯤 오는지 성도님들께 실시간으로 알려 드리는 웹앱입니다.

- **성도님 주소**: https://shepherdsdesk.github.io/hcc-shuttle-bus/
- **기사님 주소**: https://shepherdsdesk.github.io/hcc-shuttle-bus/#driver

만든 사람 AI목양지원센터 · 이세진 목사

## 어떻게 동작하나요

1. 기사님이 기사님 주소를 열고 운행 번호(PIN)를 한 번 입력합니다.
2. 노선과 호차를 고르고 **운행 시작**을 누르면, 휴대폰 GPS 위치가 5초마다 Firebase Realtime Database(`live/노선/호차`)에 올라갑니다.
3. 성도님 화면은 그 위치를 실시간으로 받아 지도에 버스를 표시하고, 정류장별 도착 예상 시간을 보여 줍니다.
4. 정류장의 🔔를 켜 두면 버스가 약 700m(2~3분) 안으로 들어올 때 알림을 띄웁니다. 화면을 열어 둔 상태에서 동작합니다.
5. 위치 신호가 없을 때는 교회 홈페이지의 배차 시간표로 다음 출발 시각을 안내합니다.

## 관리자 할 일 (Firebase 콘솔)

- **운행 번호(PIN) 추가/변경**: Realtime Database → 데이터 → `pins` 아래에 `번호: true` 추가. 기존 번호를 지우면 그 번호로 새로 등록할 수 없습니다.
- **파킹엔젤스 시간표**: `config/parkTimes`에 `"8:00,8:10,8:20"`처럼 쉼표로 적으면 앱에 바로 반영됩니다.
- **보안 규칙**: `database.rules.json` 내용을 Realtime Database → 규칙 탭에 붙여 넣고 게시합니다.

## 파일

| 파일 | 내용 |
|---|---|
| `index.html` | 앱 전체 (성도님 화면, 기사님 화면) |
| `sw.js` | 홈 화면 설치·알림 표시용 서비스 워커 |
| `manifest.webmanifest`, `icon-*.png` | 홈 화면 아이콘·이름 (성도님용) |
| `manifest-driver.webmanifest` | 기사님 화면을 홈 화면에 추가할 때 쓰는 설정 |
| `database.rules.json` | Firebase 보안 규칙 |

정류장 좌표·시간표·노선 이름은 `index.html` 위쪽 `ROUTES`, `SUN`, `WEEK`에서 바꿀 수 있습니다.

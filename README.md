# Study Odyssey 설치형 PWA

기존 Study Odyssey 게임을 바탕으로 홈 화면 설치와 오프라인 실행을 지원하도록 구성한 버전입니다. 게임의 기존 HTML/CSS/JavaScript, 공부·숙제 타이머, Gold/Shards, 탐험, 수집, 동료, 강화 및 저장 파일 기능은 유지했습니다.

## 폴더 구성

```text
study-odyssey/
├── index.html
├── manifest.webmanifest
├── sw.js
├── icons/
│   ├── icon-192.png
│   └── icon-512.png
└── README.md
```

## 1. GitHub 저장소 만들기

1. https://github.com 에 로그인합니다.
2. 오른쪽 위 `+` → `New repository`를 선택합니다.
3. 저장소 이름을 `study-odyssey` 등 원하는 이름으로 입력합니다.
4. 공개 배포가 편하도록 `Public`을 선택하고 `Create repository`를 누릅니다.

## 2. 파일 업로드

1. 생성한 저장소 화면에서 `Add file` → `Upload files`를 누릅니다.
2. 압축을 푼 `study-odyssey` 폴더 안의 파일과 `icons` 폴더를 업로드합니다.
3. `index.html`, `manifest.webmanifest`, `sw.js`, `README.md`가 저장소 최상위에 있어야 합니다. `icons` 폴더도 최상위에 둡니다.
4. 아래쪽 `Commit changes`를 누릅니다.

## 3. GitHub Pages 배포

1. 저장소의 `Settings` 탭으로 이동합니다.
2. 왼쪽 메뉴에서 `Pages`를 선택합니다.
3. `Build and deployment`의 Source를 `Deploy from a branch`로 설정합니다.
4. Branch는 `main`, 폴더는 `/ (root)`를 선택하고 `Save`를 누릅니다.
5. 잠시 기다린 뒤 Pages 화면에 표시되는 주소를 확인합니다. 주소는 보통 `https://깃허브아이디.github.io/저장소이름/` 형태입니다.

PWA와 Service Worker는 HTTPS 또는 localhost에서만 정상 동작합니다. GitHub Pages 주소는 HTTPS를 제공합니다.

## 4. Android 태블릿에서 접속

1. 태블릿에서 Chrome을 엽니다.
2. GitHub Pages에서 표시된 HTTPS 주소로 접속합니다.
3. 첫 접속은 인터넷 연결 상태에서 완료합니다. 화면이 완전히 뜰 때까지 기다리면 앱 파일이 캐시됩니다.

## 5. 홈 화면에 설치

1. Chrome에서 Study Odyssey 페이지를 엽니다.
2. 설치 조건이 충족되면 페이지 위쪽의 `홈 화면에 설치` 버튼을 누르고 안내를 따릅니다.
3. 버튼이 나타나지 않으면 Chrome 오른쪽 위 `⋮` 메뉴에서 `앱 설치` 또는 `홈 화면에 추가`를 선택합니다. 브라우저 버전에 따라 메뉴 이름은 다를 수 있습니다.
4. 설치 후 홈 화면의 Study Odyssey 아이콘을 누르면 주소창 없는 독립 실행 모드로 열립니다.

## 6. 오프라인 실행 테스트

1. 인터넷이 연결된 상태에서 앱 주소를 한 번 열고, 화면이 모두 표시될 때까지 기다립니다.
2. Chrome을 닫거나 앱을 종료합니다.
3. 태블릿의 Wi-Fi 및 모바일 데이터를 끕니다.
4. 홈 화면의 Study Odyssey 아이콘을 눌러 실행합니다.
5. 섬 지도, 타이머, 수집 도감, 강화, 탐험, 저장/불러오기 등 게임 기능을 확인합니다.

오프라인에서 첫 실행을 하려면 온라인 상태에서 최소 한 번 앱을 열어 Service Worker가 설치되어야 합니다. 브라우저의 사이트 데이터나 캐시를 사용자가 직접 삭제하면 오프라인 파일도 사라질 수 있습니다.

## 7. 게임 업데이트

1. 로컬에서 `index.html` 또는 다른 파일을 수정합니다.
2. GitHub 저장소에 수정된 파일을 같은 경로로 다시 업로드하고 Commit합니다.
3. `sw.js`의 `CACHE_NAME` 값을 새 버전으로 변경합니다. 예: `study-odyssey-pwa-v2` → `v3`.
4. 배포된 주소를 온라인 상태에서 다시 엽니다. 새 Service Worker가 새 파일을 캐시하고 활성화되며, 이전 `study-odyssey-pwa-*` 캐시는 정리됩니다.
5. 업데이트가 바로 보이지 않으면 앱을 완전히 닫았다가 다시 열거나 페이지를 새로고침합니다.

## 저장 데이터 안내

- 게임 저장 키 `studyOdysseySave_v1`은 변경하지 않았습니다.
- 기존 localStorage 데이터 구조를 초기화하거나 새 구조로 강제 변환하지 않습니다.
- 같은 Android 기기·같은 브라우저 프로필에서 기존 게임과 같은 origin(주소)으로 실행하는 경우 기존 저장 데이터가 유지됩니다.
- 단, 기존 HTML 파일을 파일 관리자에서 `file://`로 열던 것과 GitHub Pages의 `https://` 주소는 서로 다른 저장 공간입니다. 따라서 로컬 파일 실행 때의 저장 데이터가 웹 앱에 자동으로 나타나지는 않을 수 있습니다. 이 경우 기존 게임의 `저장 파일 내보내기`로 JSON을 내보낸 뒤 새 PWA의 `불러오기`로 가져오세요.
- 브라우저 데이터를 삭제하거나 다른 브라우저/기기로 옮길 때도 먼저 저장 파일을 내보내 보관하는 것을 권장합니다.

## 오프라인 및 외부 의존성

현재 게임은 CSS, 지도 SVG, 픽셀 캐릭터 SVG, JavaScript를 HTML 안에 포함하고 있으며 외부 CDN·외부 이미지·외부 라이브러리를 사용하지 않습니다. Service Worker는 앱 셸 파일을 캐시 우선으로 제공하므로 최초 온라인 접속 후 오프라인에서도 실행할 수 있습니다.

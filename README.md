# 광저우 여행 플래너 — GitHub Pages 설치 가이드

`index.html` 파일 하나가 전부예요. **Firebase(실시간 저장)** 와 **GitHub Pages(무료 웹 호스팅)** 를 연결하면, 링크를 아는 사람 누구나 같은 일정을 보고 고칠 수 있어요. 소요 시간은 약 15~20분이에요.

> 무료 한도와 화면 이름은 서비스 쪽에서 바뀔 수 있어요. 아래 단계 중 화면이 다르면 비슷한 이름의 메뉴를 찾아 주세요.

---

## 1단계. Firebase 프로젝트 만들기

1. https://console.firebase.google.com 에 Google 계정으로 로그인해요.
2. **프로젝트 추가**를 누르고 이름을 정해요 (예: `gz-trip`). Google 애널리틱스는 꺼도 돼요.
3. 왼쪽 메뉴에서 **빌드 → Firestore Database → 데이터베이스 만들기**를 눌러요.
   - 위치는 `asia-northeast3 (서울)` 을 고르면 빨라요.
   - 모드는 **프로덕션 모드**로 시작해요.
4. **규칙(Rules)** 탭에서 내용을 아래로 통째로 바꾸고 **게시**를 눌러요.

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /trip/{doc}   { allow read, write: if true; }
    match /days/{doc}   { allow read, write: if true; }
    match /photos/{doc} { allow read, write: if true; }
  }
}
```

5. 왼쪽 위 톱니바퀴 → **프로젝트 설정 → 일반 → 내 앱**에서 **웹(`</>`)** 아이콘을 눌러 앱을 등록해요 (호스팅 설정은 체크하지 않아요).
6. 화면에 나오는 `firebaseConfig = { ... }` 내용을 복사해 둬요.

## 2단계. 설정값 붙여넣기

`index.html`을 메모장이나 편집기로 열어서 맨 위쪽의 `FB_CONFIG` 부분을 찾아요.

```js
var FB_CONFIG={
  apiKey:"YOUR_API_KEY",
  authDomain:"YOUR_PROJECT_ID.firebaseapp.com",
  projectId:"YOUR_PROJECT_ID",
  ...
};
```

`YOUR_...` 자리를 1단계에서 복사한 값으로 바꾸고 저장해요. 따옴표는 지우지 마세요.

## 3단계. GitHub Pages에 올리기

1. https://github.com 에 로그인하고 **New repository**를 눌러요.
   - 이름은 자유 (예: `gz-trip`), 공개 범위는 **Public**으로 해요.
2. 저장소 화면에서 **Add file → Upload files**로 `index.html`을 올리고 **Commit changes**를 눌러요.
3. **Settings → Pages**로 가서 **Source: Deploy from a branch**, **Branch: main / (root)** 를 고르고 저장해요.
4. 1~2분 뒤 아래 주소로 열려요.

```
https://내깃허브아이디.github.io/저장소이름/
```

이 주소를 동행자에게 보내면 끝이에요.

## 확인하는 법

- 페이지 맨 위에 **🟢 공유 중**이 보이면 연결된 거예요.
- **“Firebase 설정 전 · 이 브라우저에만 저장 중”** 이 보이면 2단계 설정값이 비어 있거나 틀린 거예요.
- **“저장하지 못했어요”** 가 보이면 1단계 4번의 규칙이 게시됐는지 확인해 주세요.
- 처음 여는 사람의 기본 일정(12/16~19)이 공유 저장소에 올라가요. 이미 올라가 있으면 그 내용을 불러와요.

---

## 알아둘 점

- **링크를 아는 사람은 누구나 수정할 수 있어요.** 로그인이 없어서 편한 대신, 링크가 퍼지면 아무나 고칠 수 있어요. 동행자에게만 보내고, 여행이 끝나면 Firestore에서 데이터를 지우세요.
- **Firebase 설정값이 공개 저장소에 보여요.** 웹 앱용 설정값은 원래 공개되는 값이라 일반적이지만, 데이터를 지키는 건 위의 규칙뿐이에요. 걱정되면 Google Cloud 콘솔의 API 키 설정에서 사용 가능한 웹사이트를 본인의 GitHub 주소로 제한하세요.
- **무료 한도.** Firestore 무료 요금제(Spark)로 이 정도 규모는 충분하지만, 한도는 바뀔 수 있으니 콘솔의 사용량 화면에서 확인하세요.
- **사진.** 사진은 줄여서 Firestore에 저장돼요. 한 장은 1MB 이하로 줄어들지만 많이 올리면 무료 저장 공간(약 1GB)을 쓰니 적당히 올려 주세요.
- **동시 수정.** 같은 날 일정을 동시에 고치면 마지막에 저장한 사람의 내용이 남아요.

## Claude 아티팩트 판과 다른 점

| 기능 | 아티팩트 판 | GitHub 판 |
|---|---|---|
| 접속 | Claude 로그인 필요 | 링크만 있으면 됨 |
| 장소·호텔·편명 AI 자동 검색 | 있음 | 없음 |
| 대신 쓰는 방법 | – | “지도에서 찾기”로 구글맵 열기, 구글맵 링크를 붙여넣으면 위치 자동 반영 |
| 지도 좌표 찾기 | 버튼으로 자동 | 링크 붙여넣기로 반영 |

구글맵 링크는 주소창에 `@23.12,113.26` 같은 좌표가 보이는 **긴 주소**를 붙여넣어야 해요. `maps.app.goo.gl` 같은 짧은 공유 링크에는 좌표가 없어서 위치가 반영되지 않아요.

## 문제가 생기면

- 화면이 하얗게 비어 있으면 브라우저에서 `F12`를 눌러 Console 탭의 빨간 오류를 확인해 주세요.
- 일정이 사라진 것처럼 보이면 Firebase 콘솔 → Firestore Database에서 `days`, `trip` 데이터가 남아 있는지 확인해 주세요.

# 📸 일자별 사진 공유 앱

매일 사진/캡쳐를 날짜별로 올리고, 여러 사람이 함께 보고, 업로드되면 이메일 알림까지 보내는 웹앱입니다.
별도 서버 없이 **GitHub** 한 곳으로 동작합니다.

- 날짜별 저장 + 과거 기록 열람(날짜 선택/이동)
- 업로드 시 담당자 이름 선택 (이름 목록은 설정에서 관리)
- 여러 사람이 열람/등록/공유 (GitHub 저장소 공유)
- 업로드되면 이메일 자동 알림 (수신자 여러 명 설정)
- 모바일에서 사진 다운로드

---

## 1. GitHub 저장소 만들기

1. GitHub 로그인 → 우측 상단 **＋ → New repository**
2. 저장소 이름 입력 (예: `photo-share`) → **Public** 선택(권장) → **Create**
3. 이 폴더의 파일을 저장소에 올립니다. (웹에서 **Add file → Upload files** 로
   `index.html`, `config.json`, `.github/workflows/notify.yml`, `README.md` 업로드 → Commit)

## 2. GitHub Pages 켜기 (앱 주소 만들기)

1. 저장소 → **Settings → Pages**
2. **Source: Deploy from a branch** → Branch: **main / (root)** → **Save**
3. 1~2분 후 앱 주소가 생깁니다:
   `https://<내아이디>.github.io/<저장소이름>/`
   → 이 주소를 휴대폰/PC에서 열면 앱이 실행됩니다. (홈 화면에 추가해두면 앱처럼 쓸 수 있어요.)

## 3. 액세스 토큰 발급 (등록/수정하는 사람만 필요)

열람만 하는 사람은 토큰이 필요 없습니다. **사진을 올리거나 설정을 저장**하는 사람만 발급하세요.

1. GitHub → 우측 프로필 → **Settings → Developer settings**
2. **Personal access tokens → Fine-grained tokens → Generate new token**
3. **Repository access**: Only select repositories → 위에서 만든 저장소 선택
4. **Permissions → Repository permissions → Contents: Read and write**
5. **Generate token** → 토큰 문자열 복사 (`github_pat_...`)

## 4. 앱 설정

앱을 열고 우측 상단 **⚙️ 설정**:

1. 저장소 소유자(owner), 저장소 이름, 브랜치(`main`), 토큰 입력 → **이 기기 연결정보 저장**
   - 토큰은 그 기기에만 저장되고 외부로 전송되지 않습니다.
2. **담당자 이름** 추가 (업로드 시 선택 목록이 됩니다)
3. **알림 수신 이메일** 추가 (여러 명 가능)
4. **목록 저장 (모두에게 공유)** → `config.json`에 저장되어 모든 사용자에게 공유됩니다.

이후 **＋** 버튼으로 날짜·담당자·메모를 선택해 사진을 올리면 됩니다. (여러 장 동시 업로드 가능)

---

## 5. 이메일 알림 설정 (선택)

업로드 시 자동 메일을 보내려면 발송 계정 정보를 저장소 비밀값으로 등록합니다.

저장소 → **Settings → Secrets and variables → Actions → New repository secret** 으로 아래 4개 등록:

| 이름 | 값 (예: Gmail) |
|------|------|
| `MAIL_SERVER` | `smtp.gmail.com` |
| `MAIL_PORT` | `465` |
| `MAIL_USERNAME` | 보내는 Gmail 주소 |
| `MAIL_PASSWORD` | Gmail **앱 비밀번호** (아래 참고) |

### Gmail 앱 비밀번호 만들기
1. Google 계정 → **보안** → **2단계 인증** 켜기
2. **앱 비밀번호** 검색 → 생성 → 나온 16자리를 `MAIL_PASSWORD`에 입력
   (일반 로그인 비밀번호가 아니라 앱 비밀번호여야 합니다.)

> 네이버 메일이면 `MAIL_SERVER=smtp.naver.com`, `MAIL_PORT=465`,
> 메일 환경설정에서 POP3/SMTP 사용을 켜고 아이디/비밀번호를 넣으세요.

설정 후, 사진이 업로드되면(= `photos/` 폴더에 push 되면) `config.json`의 수신자에게 메일이 발송됩니다.
실행 내역은 저장소 **Actions** 탭에서 확인할 수 있습니다.

---

## 자주 묻는 질문

- **사진은 어디에 저장되나요?** 저장소의 `photos/날짜/` 폴더에 파일로 저장됩니다. 날짜별로 영구 보관됩니다.
- **비공개 저장소도 되나요?** 됩니다. 단 열람하는 사람도 토큰이 필요합니다(공개면 토큰 없이 열람 가능).
- **여러 명이 동시에 써도 되나요?** 네. 각자 자기 토큰을 설정에 넣고 같은 저장소를 바라보면 됩니다.
- **이메일이 안 와요.** ① `config.json`에 수신 이메일이 있는지 ② Secrets 4개가 맞는지 ③ Gmail은 앱 비밀번호인지 ④ Actions 탭의 실행 로그를 확인하세요.

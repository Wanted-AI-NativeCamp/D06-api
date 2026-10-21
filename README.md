# d6 API 활용 및 Git 배포 실습

키 없이 쓰는 공개 API로 만든 HTML 화면 4개 (D6 실습, 2026-10-21)

- 배포 주소: https://261021-d6-api.netlify.app/
- 저장소: https://github.com/Wanted-AI-NativeCamp/d6-261021-api

## 화면

| 화면 | 파일 | 쓰는 API |
|---|---|---|
| 🌤️ 우리 동네 날씨 | `d6-api/weather.html` | Open-Meteo Forecast |
| 💱 환율 계산기 | `d6-api/rate.html` | Frankfurter |
| 🏙️ 도시 날씨·미세먼지 | `d6-api/city.html` | Open-Meteo Geocoding · Forecast · Air Quality |
| ☀️ 아침 대시보드 | `d6-api/morning.html` | Open-Meteo · Frankfurter · Nager.Date · Dog CEO |

`index.html`은 4개 화면으로 가는 링크 모음입니다. 모든 화면은 HTML 파일 하나에 CSS와 JavaScript가 들어 있어서 더블클릭으로 바로 열립니다.

## 폴더 구조

```
d6/
├── index.html          # 첫 화면 (링크 모음)
├── README.md
├── d6-api/
│   ├── CLAUDE.md       # API 화면 규칙
│   ├── .claude/skills/api-check/SKILL.md   # /api-check 검사 skill
│   ├── weather.html
│   ├── rate.html
│   ├── city.html
│   └── morning.html
└── delete/             # 실습 자료 (.gitignore로 제외, 업로드 안 됨)
```

## 작업 순서

검증 skill 사용 후 html 업로드

1. 화면을 만들거나 고친다. 규칙은 `d6-api/CLAUDE.md`를 따른다.
2. Claude Code에서 `/api-check`로 규칙 검사를 하고, 표로 나온 결과를 확인한다.
3. 직접 열어서 확인한 뒤 커밋하고 push한다.

## 배포

GitHub `main` 브랜치와 Netlify가 연결되어 있어서, push하면 자동으로 배포됩니다.

```
git add .
git commit -m "변경 내용"
git push
```

- 배포 상태는 Netlify 대시보드 → **Deploys** 탭에서 확인합니다.
- push했는데 반영되지 않았다면 **Trigger deploy → Deploy site**로 다시 배포합니다.
- 예전 화면이 보이면 `Ctrl + Shift + R`로 강력 새로고침합니다.
- 배포된 주소에서는 Netlify Pretty URLs 때문에 `.html`이 빠져 보입니다 (예: `/d6-api/weather`).

## 스터디 결과

오늘 Git 정리와 배포를 하면서 막혔던 문제와 해결 방법.

### 1. 폴더 안에 또 다른 Git 저장소가 있었다
- **문제:** `d6-api/` 안에 `.git`이 따로 있었다. 이대로 `git add d6-api/`를 하면 파일이 아니라 "다른 저장소를 가리키는 표시"만 올라가서, 받는 사람에게는 빈 폴더가 된다.
- **해결:** `git bundle`로 `d6-api` 기록을 백업한 뒤 `d6-api/.git`을 지우고 파일을 바로 추가했다.
- **배운 점:** `git status`에 폴더가 `d6-api/` 한 줄로만 보이고 안쪽 파일이 안 나오면, 안에 `.git`이 있는지 먼저 확인한다.

### 2. `.gitignore`에 넣었는데 `delete/`가 계속 올라가려 했다
- **문제:** `.gitignore`는 아직 추적하지 않는 파일에만 적용된다. `delete/`는 첫 커밋에 이미 들어가 있었다.
- **해결:** `git rm -r --cached delete/`로 추적만 끊었다. 로컬 파일은 그대로 남는다.
- **배운 점:** `.gitignore`는 처음 커밋하기 전에 만든다.

### 3. 새 브랜치를 만들어도 예전 커밋 기록은 사라지지 않는다
- **문제:** `upload` 브랜치를 새로 만들어 올리면 해결될 줄 알았지만, 새 브랜치도 `delete/`가 들어 있는 첫 커밋 위에 있어서 push하면 그 기록까지 같이 올라간다.
- **해결:** push 전에 `git commit --amend`로 첫 커밋을 다시 만들고 `git branch -f main upload`로 `main`도 옮겼다. 아직 push한 적이 없어서 강제 push 없이 끝났다.
- **배운 점:** 올리면 안 되는 파일은 **첫 push 전에** 정리해야 쉽다. 한 번 올라가면 기록을 다시 쓰고 강제 push해야 한다.
- **덤:** Claude Code 자동 모드에서는 `--amend` 같은 기록 수정 명령이 "파괴적 작업"으로 막힐 수 있다. 직접 요청하거나 `!`로 직접 실행하면 된다.

### 4. 저장소 이름과 원격 주소
- **문제:** 조직 이름 오타(`NativeCapm`)가 있었고, 만든 뒤에 저장소 이름도 바꾸고 싶었다.
- **해결:** `gh repo rename`으로 이름을 바꾸고 `git remote set-url origin <새 주소>`로 로컬 연결도 바꿨다.
- **배운 점:** 기존 `d6-api` 저장소에 덮어 올리면 기록이 충돌해 강제 push가 필요하다. 합친 프로젝트는 새 저장소에 올렸다.

### 5. Netlify 메인 주소가 404였다
- **문제:** 화면이 `d6-api/` 폴더 안에만 있고 최상위에 `index.html`이 없었다. 주소 뒤에 파일 이름이 없으면 Netlify는 `index.html`을 찾는다.
- **해결:** 최상위에 4개 화면 링크를 모은 `index.html`을 만들었다.

### 6. 배포가 안 된 것처럼 보였다
- **문제:** push 후에도 예전 화면이 보이는 것 같았다.
- **원인:** 실제로는 이미 배포되어 있었고, 브라우저에 남은 예전 화면이었다. 배포된 HTML에서 링크가 `/d6-api/weather`처럼 바뀐 것은 Netlify Pretty URLs 기능 때문이고 정상이다.
- **배운 점:** `Ctrl + Shift + R`로 강력 새로고침하거나 시크릿 창으로 확인한다. Netlify **Deploys** 탭에서 배포 시각을 본다.

### 참고: 무시해도 되는 경고
- `LF will be replaced by CRLF`: Windows 줄바꿈 변환 안내다. `.gitattributes`의 `* text=auto` 설정 때문이며 파일 내용에는 문제없다.

## 데이터 출처

Weather data by [Open-Meteo.com](https://open-meteo.com/) · 환율: [Frankfurter](https://frankfurter.dev/) · 공휴일: [Nager.Date](https://date.nager.at/) · 사진: [Dog CEO](https://dog.ceo/dog-api/)

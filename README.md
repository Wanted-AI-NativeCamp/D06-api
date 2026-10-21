# D6 API 활용 및 Git 배포 실습

**2026.10.21 | Claude Code · API · GitHub · Netlify**

키 없이 사용할 수 있는 공개 API를 활용하여 HTML 화면 4개를 제작하고, Claude Code 커스텀 스킬을 통한 코드 검사와 GitHub·Netlify 배포까지 진행한 실습 프로젝트입니다.

- **배포 주소:** https://261021-d6-api.netlify.app/
- **GitHub 저장소:** https://github.com/Wanted-AI-NativeCamp/d6-261021-api

## 1. 구현 화면

| 화면 | 파일 | 사용 API |
|---|---|---|
| 🌤️ 우리 동네 날씨 | `d6-api/weather.html` | Open-Meteo Forecast |
| 💱 환율 계산기 | `d6-api/rate.html` | Frankfurter |
| 🏙️ 도시 날씨·미세먼지 | `d6-api/city.html` | Open-Meteo Geocoding · Forecast · Air Quality |
| ☀️ 아침 대시보드 | `d6-api/morning.html` | Open-Meteo · Frankfurter · Nager.Date · Dog CEO |

`index.html`은 4개 화면으로 이동하는 링크 모음입니다.

모든 화면은 HTML 파일 하나에 CSS와 JavaScript를 포함하며, 별도 설치 없이 브라우저에서 열 수 있도록 구성했습니다.

## 2. 프로젝트 폴더 구조

```text
d6/
│
├── d6-api/                       # API 실습 프로젝트
│   ├── .claude/                  # Claude Code 설정
│   │   ├── rules/                # 세부 개발 규칙
│   │   └── skills/               # 커스텀 스킬
│   │       └── api-check/
│   │           └── SKILL.md      # API 규칙 검사
│   │
│   ├── CLAUDE.md                 # API 화면 개발 규칙
│   ├── city.html                 # 도시 날씨·미세먼지
│   ├── morning.html              # 아침 대시보드
│   ├── rate.html                 # 환율 계산기
│   └── weather.html              # 우리 동네 날씨
│
├── delete/                       # 실습 자료 보관 (Git 추적 제외)
├── .gitattributes                # Git 파일 속성 설정
├── .gitignore                    # Git 추적 제외 규칙
├── index.html                    # 메인 화면
└── README.md                     # 프로젝트 문서
```

### Claude Code 설정 파일의 역할

| 구분 | 역할 | 사용 목적 |
|---|---|---|
| `CLAUDE.md` | 프로젝트 공통 지침 | API 화면 개발 규칙 |
| `.claude/rules/` | 분야별 세부 규칙 | 필요에 따라 규칙 분리 |
| `.claude/skills/` | 반복 작업 자동화 | 커스텀 스킬 관리 |
| `SKILL.md` | 스킬 실행 지침 | API 규칙 준수 여부 검사 |

**핵심 개념**

- **CLAUDE.md:** 프로젝트에서 무엇을 지켜야 하는가?
- **Rules:** 작업별로 어떤 세부 규칙을 적용할 것인가?
- **Skills:** 특정 작업을 어떤 절차로 수행할 것인가?

## 3. 작업 및 검증 프로세스

### ① API 화면 개발

`d6-api/CLAUDE.md`에 정의된 규칙을 기준으로 화면을 제작하거나 수정합니다.

주요 규칙은 다음과 같습니다.

- API 키가 필요 없는 공개 API 사용
- API 주소와 호출 코드에 `🔌` 주석 표시
- 로딩·오류 상태 처리
- API 데이터 출처 및 기준 날짜 표시
- HTML 파일 하나에 CSS와 JavaScript 포함

### ② 커스텀 Skill 검사

Claude Code에서 `/api-check` 스킬을 호출하여 HTML 파일의 규칙 준수 여부를 확인합니다.

```text
/api-check weather.html
```

스킬은 규칙별 검사 결과를 표로 보고하며, 코드를 직접 수정하지 않습니다.

### ③ 수동 검증 및 Git 업로드

검사 결과를 확인하고 브라우저에서 직접 테스트한 뒤 Git에 반영합니다.

```bash
git add .
git commit -m "변경 내용"
git push
```

커밋과 푸시는 최종 확인 후 진행합니다.

## 4. GitHub 및 Netlify 배포

GitHub `main` 브랜치와 Netlify를 연결하여, push 시 자동 배포되도록 구성했습니다.

- **배포 확인:** Netlify 대시보드 → Deploys
- **수동 재배포:** Trigger deploy → Deploy site
- **브라우저 캐시 갱신:** `Ctrl + Shift + R`
- **Pretty URLs:** `/d6-api/weather.html`이 `/d6-api/weather`처럼 표시될 수 있음

---

## 5. 트러블슈팅 및 학습 기록

### 01. Claude Code Skill 명령어가 인식되지 않음

**문제**

`SKILL.md` 파일을 생성했지만 `/api-check` 명령어가 나타나지 않았습니다.

```text
No commands match "/api-check"
```

**실제 원인: 폴더명 오타**

`.claude` 폴더의 철자를 잘못 작성했습니다.

```text
❌ .cluade/
✅ .claude/
```

Claude Code는 지정된 경로에서 스킬을 탐색하기 때문에, 폴더명이 잘못되면 스킬을 정상적으로 인식하지 못할 수 있습니다.

**해결**

폴더명을 `.claude`로 수정하고 아래 경로를 확인했습니다.

```text
d6-api/.claude/skills/api-check/SKILL.md
```

**추가로 점검했던 사항**

처음에는 Claude Code 실행 위치가 `d6`였고, 스킬 파일은 하위 `d6-api` 폴더에 있었습니다.

```text
d6/                        # 최초 실행 위치
└── d6-api/
    └── .claude/
        └── skills/
            └── api-check/
                └── SKILL.md
```

프로젝트 폴더에서 실행하도록 변경했습니다.

```bash
cd d6-api
claude
```

또한 `SKILL.md`의 YAML Frontmatter도 확인했습니다.

```yaml
---
name: api-check
description: API 화면 규칙 검사
---
```

실행 위치와 YAML 설정은 문제 해결 과정에서 확인한 항목이며, **최종적으로 확인된 직접 원인은 `.cluade` 폴더명 오타**였습니다.

**배운 점**

- 설정 오류가 발생하면 파일 내용보다 경로와 철자를 먼저 확인한다.
- Claude Code의 현재 작업 디렉터리(CWD)를 확인한다.
- 실제 원인과 문제 해결 과정에서 점검한 항목을 구분해 기록한다.

### 02. 프로젝트 폴더 안에 또 다른 Git 저장소가 있었음

**문제**

`d6-api/` 안에 별도의 `.git` 폴더가 존재했습니다.

이 상태에서 상위 저장소에 `git add d6-api/`를 실행하면 내부 파일이 아닌 중첩 저장소 참조가 추가될 수 있습니다.

**해결**

1. `git bundle`로 기존 `d6-api` 저장소의 커밋 기록 백업
2. `d6-api/.git` 제거
3. 상위 저장소에서 실제 파일을 다시 추가

**배운 점**

`git status`에서 프로젝트 폴더가 한 줄로만 표시되고 내부 파일이 보이지 않는다면, 중첩된 `.git` 폴더가 있는지 확인한다.

### 03. .gitignore에 추가했는데 delete/ 폴더가 계속 추적됨

**문제**

`delete/` 폴더를 `.gitignore`에 추가했지만 Git에서 계속 추적했습니다.

원인은 해당 폴더가 이미 첫 커밋에 포함되어 있었기 때문입니다.

**해결**

`.gitignore`에 다음 규칙을 추가했습니다.

```gitignore
/delete/
```

기존에 추적하던 파일은 아래 명령어로 Git 인덱스에서만 제거했습니다.

```bash
git rm -r --cached delete/
```

로컬 파일은 그대로 유지됩니다.

**배운 점**

`.gitignore`는 새로 추적할 파일을 제외하는 규칙입니다. 이미 추적 중인 파일은 별도로 추적을 해제해야 합니다.

가능하면 첫 커밋 전에 `.gitignore`를 설정합니다.

### 04. 새 브랜치를 만들어도 기존 커밋 기록이 유지됨

**문제**

새로운 `upload` 브랜치를 생성하면 기존 기록이 제외될 것으로 생각했지만, 새 브랜치에도 `delete/`가 포함된 첫 커밋 기록이 남아 있었습니다.

**해결**

첫 push 이전에 커밋 기록을 정리했습니다.

```bash
git commit --amend
git branch -f main upload
```

아직 원격 저장소에 push하지 않은 상태였으므로 강제 push 없이 정리할 수 있었습니다.

**배운 점**

- 브랜치는 기존 커밋 기록을 기반으로 생성된다.
- 새 브랜치를 만든다고 이전 기록이 사라지지는 않는다.
- 업로드하면 안 되는 파일은 첫 push 전에 정리하는 것이 좋다.
- 이미 공유된 커밋 기록을 수정할 때는 협업자에게 미치는 영향을 확인해야 한다.

**추가 사항**

Claude Code 자동 모드에서는 `--amend`처럼 기록을 변경하는 명령어가 제한될 수 있습니다. 필요한 경우 명령의 영향을 확인하고 직접 실행합니다.

### 05. GitHub 저장소 이름과 원격 주소 변경

**문제**

조직 이름에 `NativeCapm` 오타가 있었고, 생성한 저장소 이름도 변경해야 했습니다.

**해결**

GitHub CLI로 저장소 이름을 변경했습니다.

```bash
gh repo rename
```

이후 로컬 원격 저장소 주소를 수정했습니다.

```bash
git remote set-url origin <새 저장소 주소>
```

기존 `d6-api` 저장소에 덮어쓰지 않고, 합쳐진 프로젝트를 새로운 저장소에 업로드했습니다.

**배운 점**

- 저장소 이름과 원격 주소는 서로 연결되어 있다.
- 저장소 이름 변경 후 `git remote -v`로 연결 상태를 확인한다.
- 기존 기록과 충돌하는 저장소에 무리하게 덮어쓰기보다 새 저장소를 사용하는 방법도 고려한다.

### 06. Netlify 메인 주소에서 404 오류 발생

**문제**

HTML 화면이 `d6-api/` 폴더에만 있고 프로젝트 최상위에 `index.html`이 없었습니다.

Netlify 메인 주소로 접속했을 때 시작 페이지를 찾지 못해 404 오류가 발생했습니다.

**해결**

최상위에 `index.html`을 생성하고 4개 화면으로 이동하는 링크를 구성했습니다.

**배운 점**

정적 웹사이트를 배포할 때는 배포 루트의 진입 파일과 경로를 확인해야 합니다.

### 07. Push 이후 변경 사항이 반영되지 않은 것처럼 보임

**문제**

GitHub에 push했지만 브라우저에는 이전 화면이 표시되었습니다.

**원인**

실제 배포는 완료되었으나 브라우저 캐시 때문에 이전 화면이 표시되었습니다.

또한 Netlify의 Pretty URLs 기능으로 `.html` 확장자가 없는 주소가 표시되었습니다.

**해결**

- `Ctrl + Shift + R`로 강력 새로고침
- 시크릿 창에서 배포 페이지 확인
- Netlify Deploys 탭에서 최신 배포 시각 확인

**배운 점**

배포 오류처럼 보여도 실제로는 브라우저 캐시나 URL 처리 방식의 차이일 수 있습니다.

### 08. Windows 줄바꿈 경고

**발생 메시지**

```text
LF will be replaced by CRLF
```

**원인**

Windows와 Unix 계열 운영체제의 줄바꿈 방식 차이로 발생하는 Git 안내입니다.

`.gitattributes`의 다음 설정과 관련될 수 있습니다.

```gitattributes
* text=auto
```

**배운 점**

일반적으로 파일 손상을 의미하는 오류는 아니며, Git의 줄바꿈 정규화 동작을 이해하고 확인하면 됩니다. Windows는 줄바꿈에 CRLF(\r\n)를 사용하고, macOS와 Linux는 LF(\n)를 사용한다. Git은 일반적으로 텍스트 파일의 줄바꿈을 LF로 정규화하여 저장하기 때문에 Windows 환경에서 LF will be replaced by CRLF라는 경고가 발생할 수 있다.

이는 오류가 아니라 파일의 줄바꿈 방식이 변환될 수 있음을 알리는 메시지다. 다만 운영체제별 줄바꿈 방식이 혼재하면 실제 코드 수정 없이도 Git이 변경 사항을 감지하거나 불필요한 병합 충돌이 발생할 수 있다.

.gitattributes 파일에 * text=auto를 설정하면 Git이 텍스트 파일을 자동으로 인식하고 줄바꿈을 정규화한다. 단, Windows 작업 폴더에서의 줄바꿈 방식은 core.autocrlf 등의 Git 설정에 따라 달라질 수 있다.

현재 커밋과 배포가 정상적으로 완료되었다면 반드시 수정할 필요는 없다. 다만 팀 프로젝트에서는 운영체제에 관계없이 일관된 파일 관리를 위해 다음과 같이 LF 방식을 명시할 수 있다.

* text=auto
*.html text eol=lf
*.css text eol=lf
*.js text eol=lf
*.md text eol=lf

위 설정은 HTML, CSS, JavaScript, Markdown 파일의 줄바꿈을 LF로 통일한다. 기존 파일에 적용할 때는 변경 사항을 확인하고 필요한 경우 줄바꿈 정규화를 진행해야 한다.

정리: 줄바꿈 경고 자체는 문제가 아니지만, 협업 시 불필요한 변경 기록과 충돌을 방지하기 위해 .gitattributes로 파일 관리 규칙을 통일하는 것이 좋다.

---

## 6. 실습 회고

이번 실습에서는 API 화면 구현뿐 아니라 Claude Code 설정, Git 저장소 구조 정리, 배포까지 전체 흐름을 경험했습니다.

**주요 학습 내용**

1. **정확한 경로와 파일명 관리**  
   설정 파일은 내용뿐 아니라 위치와 철자가 중요하다.

2. **AI 작업 규칙과 스킬의 분리**  
   프로젝트 규칙과 반복 작업의 수행 절차를 분리하면 일관된 검증이 가능하다.

3. **Git 추적과 커밋 기록의 이해**  
   `.gitignore`, 중첩 저장소, 브랜치, 커밋 수정은 서로 다른 개념이다.

4. **배포 이후 검증의 중요성**  
   push 완료 여부뿐 아니라 배포 상태, 진입 경로, 브라우저 캐시까지 확인해야 한다.

5. **문제 해결 과정의 기록**  
   추정 원인과 실제 원인을 구분해 기록하면 같은 실수를 반복하지 않고 이후 문제 해결에도 활용할 수 있다.

## 7. 데이터 출처

- 날씨·대기질: [Open-Meteo](https://open-meteo.com/)
- 환율: [Frankfurter](https://frankfurter.dev/)
- 공휴일: [Nager.Date](https://date.nager.at/)
- 강아지 이미지: [Dog CEO](https://dog.ceo/dog-api/)

Weather data by Open-Meteo.com

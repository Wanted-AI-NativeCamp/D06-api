# d6

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

## 데이터 출처

Weather data by [Open-Meteo.com](https://open-meteo.com/) · 환율: [Frankfurter](https://frankfurter.dev/) · 공휴일: [Nager.Date](https://date.nager.at/) · 사진: [Dog CEO](https://dog.ceo/dog-api/)

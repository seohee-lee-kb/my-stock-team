# my-stock-team

한국 주식 리서치를 **역할별 애널리스트 서브에이전트**가 협업해 만들고, **PPTX/PDF 리포트**로 내보내는 Claude Code 플러그인입니다. 분석 관점은 **가치·펀더멘털 중심, 장기(3년+)** 이며 **학습용**입니다(투자 자문 아님).

## 구성

```
my-stock-team/
├── .claude-plugin/
│   ├── plugin.json         # 플러그인 매니페스트 (name: my-stock-team, v1.0.0)
│   └── marketplace.json    # 설치 카탈로그
├── agents/                 # 애널리스트 서브에이전트 5종
│   ├── fundamental-analyst.md       🔴 재무·실적·공시 (DART)
│   ├── market-tech-analyst.md       🟢 주가·추세 (FinanceDataReader)
│   ├── news-sentiment-analyst.md    🔵 뉴스·심리 (웹서치)
│   ├── risk-manager-synthesizer.md  🟣 종합·리스크·유동성 (pykrx)
│   └── verification-analyst.md      🟡 리포트 검수 (정확성·일관성·완결성·근거/형식)
├── skills/
│   └── report-pptx/        # reports/{종목}.md → PPTX/PDF (KB 옐로우·맑은 고딕)
└── commands/
    ├── stock-report.md     # 종목명 → 리서치 → 검수 → 내보내기 전체 흐름
    └── stock-verify.md     # 완성 리포트 검수 (통과/보류)
```

## 설치

로컬 폴더로 바로 추가:
```
/plugin marketplace add /path/to/my-stock-team
/plugin install my-stock-team
```
또는 Git 저장소로 배포한 경우 `/plugin marketplace add <owner>/<repo>` 후 설치합니다.

## 사용

```
/my-stock-team:stock-report 삼성전자      # 전체 리포트 생성 + 검수 + PPTX/PDF
/my-stock-team:stock-verify 삼성전자      # 기존 reports/삼성전자.md 검수만
```

개별 분석은 자연어로 요청하면 해당 에이전트로 위임됩니다(예: "삼성전자 주가 흐름 봐줘" → market-tech-analyst).

## 사전 요건

- Python 패키지: `python-pptx`, `matplotlib` (PDF 미리보기 검증 시 `pypdfium2`).
- 한글 폰트: Windows `맑은 고딕`, macOS `AppleGothic`, Linux `나눔고딕`/`Noto Sans CJK KR` 중 하나(자동 탐색). 없으면 기본 폰트로 폴백됩니다.
- **`DART_KEY`** (재무 분석용): 각 사용자가 **자신의 `.env`** 에 설정합니다. 이 플러그인은 키를 포함하지 않으며, 키를 하드코딩·출력하지 않습니다.
  ```
  DART_KEY=발급받은_본인_키
  ```
  발급: https://opendart.fss.or.kr (무료)

## 가드레일 (모든 산출물에 항상 적용)

1. 모든 수치 옆 `(출처: 데이터명, 연도/날짜)` 표기 — 출처 없는 수치는 쓰지 않음
2. 데이터 미확보는 "확인 불가", 출처 없는 뉴스·루머는 "미확인"으로 구분
3. 매수/매도/보유·목표가·비중 확대/축소 등 투자 행동 단정 금지 — 판단 근거 정리까지만, 최종 판단은 사람
4. 리포트 첫머리 "무료 공개 데이터 기반 학습용" 한 줄, 끝에 데이터 출처·기준일 목록

## 라이선스

MIT. 본 플러그인의 산출물은 학습용이며 투자 자문이 아닙니다.

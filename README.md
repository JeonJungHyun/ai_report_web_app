# AI Report Web App

Streamlit과 OpenAI API를 활용해 원시 보안 점검 내용을 구조화된 취약점 보고서로 변환하는 웹앱입니다. 사용자가 입력한 점검 내용을 AI가 분석해 위험도, 취약점 유형, 영향도, 재현 예시, 조치 방안 등을 정리하고 Markdown 파일로 저장합니다.

## 주요 기능

- 원시 보안 점검 내용 입력
- OpenAI 모델을 통한 취약점 보고서 자동 생성
- Pydantic 모델 기반의 정형 데이터 출력
- Markdown 보고서 미리보기
- 생성된 보고서를 `output/` 디렉터리에 `.md` 파일로 저장

## 프로젝트 구조

```text
.
├── app.py           # Streamlit 웹앱 메인 파일
├── llm_service.py   # LLM 호출 함수 분리 파일
├── models.py        # 취약점 보고서 Pydantic 모델
├── check_key.py     # OPENAI_API_KEY 로드 확인 스크립트
├── openai_test.py   # OpenAI API 호출 테스트 스크립트
├── output/          # 생성된 Markdown 보고서 저장 위치
└── README.md
```

## 실행 환경

- Python 3.10 이상 권장
- OpenAI API Key

## 설치 방법

가상환경을 생성하고 필요한 패키지를 설치합니다.

```bash
python -m venv venv
source venv/bin/activate
pip install streamlit openai python-dotenv pydantic
```

## 환경변수 설정

프로젝트 루트에 `.env` 파일을 만들고 OpenAI API Key를 설정합니다.

```env
OPENAI_API_KEY=your_openai_api_key
```

API Key가 정상적으로 로드되는지 확인하려면 다음 명령을 실행합니다.

```bash
python check_key.py
```

## 실행 방법

Streamlit 앱을 실행합니다.

```bash
streamlit run app.py
```

실행 후 브라우저에서 표시되는 로컬 주소로 접속합니다. 일반적으로 다음 주소를 사용합니다.

```text
http://localhost:8501
```

## 사용 방법

1. 웹 화면의 입력창에 원시 점검 내용을 입력합니다.
2. `AI 보고서 생성` 버튼을 클릭합니다.
3. 생성된 구조화 데이터와 Markdown 보고서를 확인합니다.
4. 보고서 파일은 `output/` 디렉터리에 저장됩니다.

예시 입력:

```text
대상: 사내 게시판 시스템

발견 내용:
- 게시글 검색 기능에서 SQL Injection 의심
- 파라미터: keyword
- payload: ' OR '1'='1
- 검색 결과에 비정상적으로 많은 데이터 노출
- 로그인하지 않은 사용자도 접근 가능
```

## 출력 파일

생성된 보고서는 다음 형식의 파일명으로 저장됩니다.

```text
output/vulnerability_report_YYYYMMDD_HHMMSS.md
```

보고서에는 다음 항목이 포함됩니다.

- 기본 정보
- 취약점 설명
- 영향받는 파라미터
- 재현 예시
- 예상 영향
- 조치 방안

## 참고 사항

- `.env`, `venv/`, `__pycache__/`, `output/`은 `.gitignore`에 포함되어 있습니다.
- `app.py`는 현재 `gpt-5-nano` 모델을 사용합니다.
- OpenAI API 호출 비용은 사용하는 계정의 과금 정책에 따라 발생할 수 있습니다.

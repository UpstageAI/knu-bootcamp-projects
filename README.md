# KNU Bootcamp Projects Repository

경북대학교(KNU) AI 부트캠프 최종 프로젝트 제출 리포지토리입니다.

## 개요

이 리포지토리는 KNU AI 부트캠프 참가자들의 최종 프로젝트를 관리하고 제출하기 위한 공식 공간입니다. 모든 팀은 독립적인 프로젝트 디렉토리에서 작업하며, 아래 가이드라인을 따라 프로젝트를 제출합니다.

---

## 프로젝트 구조

```
knu-bootcamp-projects/
├── README.md                    # 본 문서
├── projects/                    # 모든 팀 프로젝트가 위치하는 디렉토리
│   ├── knu-bootcamp-template/  # 프로젝트 템플릿 (참고용)
│   └── [팀명]/                  # 각 팀의 프로젝트 디렉토리
└── example_directory.md         # 프로젝트 구조 참고 문서
```

---

## 프로젝트 시작 가이드

### 1. 팀 디렉토리 생성

`projects/` 디렉토리 하위에 팀명으로 신규 디렉토리를 생성합니다.

**규칙:**
- 디렉토리명은 영문 소문자 사용
- 공백 대신 하이픈(`-`) 사용
- 예시: `projects/llama-agent-team/`

### 2. 프로젝트 파일 관리

모든 프로젝트 관련 파일은 팀 디렉토리 내부에 위치해야 합니다.

**주의사항:**
- 소스 코드, 문서, 설정 파일 등 모든 산출물 포함
- 다른 팀 디렉토리 수정 금지 (Merge Conflict 방지)
- 기술 스택 및 배포 방식은 자유롭게 선택 가능

### 3. README 작성 (필수)

각 팀 디렉토리에는 반드시 `README.md` 파일이 포함되어야 합니다.

**필수 포함 항목:**
- 프로젝트 이름
- 팀원 소개
- 프로젝트 개요 및 목적
- 주요 기능
- 기술 스택 및 아키텍처
- 설치 및 실행 방법
- 데모 또는 스크린샷 (선택사항)

---

## 제공 템플릿

`projects/knu-bootcamp-template/` 디렉토리에서 프로젝트 구조 템플릿을 제공합니다.

### 템플릿 구조

```
knu-bootcamp-template/
├── app/
│   ├── agents/              # AI 에이전트 로직
│   │   └── subgraphs/      # 에이전트 서브그래프
│   ├── api/                # API 라우터
│   │   └── route/          # API 엔드포인트
│   ├── core/               # 핵심 설정 (DB, LLM, Logger 등)
│   ├── models/             # 데이터 모델
│   │   ├── entities/       # 도메인 엔티티
│   │   └── schemas/        # API 스키마 (Pydantic)
│   ├── repository/         # 데이터 액세스 계층
│   │   ├── client/         # 외부 클라이언트
│   │   └── vector/         # 벡터 DB 레포지토리
│   └── service/            # 비즈니스 로직
│       └── agents/         # 에이전트별 서비스
├── docs/                   # 문서
└── frontend/               # 프론트엔드 (Streamlit 등)
```

**활용 방법:**
- 템플릿을 복사하여 프로젝트 시작
- 필요에 따라 구조 수정 가능
- 프로젝트 요구사항에 맞게 커스터마이징

---

## README 템플릿

각 팀은 아래 템플릿을 참고하여 `README.md`를 작성하세요.

```markdown
# [프로젝트 이름]

> 프로젝트 한 줄 설명

## 팀원 소개

| 이름 | 역할 | GitHub |
|------|------|--------|
| 홍길동 | Backend | [@gildong](https://github.com/gildong) |
| 김영희 | Frontend | [@younghee](https://github.com/younghee) |

## 프로젝트 개요

프로젝트의 목적과 배경을 설명합니다.

### 주요 기능

- 기능 1: 설명
- 기능 2: 설명
- 기능 3: 설명

## 기술 스택

### Backend
- Python 3.11+
- FastAPI
- LangGraph

### Frontend
- Streamlit / React

### AI/ML
- Upstage Solar LLM
- LangChain
- ChromaDB

### Infrastructure
- Docker
- ...

## 아키텍처

\```
[아키텍처 다이어그램 또는 설명]
\```

## 설치 및 실행

### 요구사항
- Python 3.11 이상
- ...

### 설치
\```bash
# 레포지토리 클론
git clone [repository-url]
cd [project-directory]

# 의존성 설치
pip install -r requirements.txt

# 환경 변수 설정
cp .env.example .env
# .env 파일 수정
\```

### 실행
\```bash
# 서버 실행
python main.py

# 또는
uvicorn main:app --reload
\```

## 데모

[스크린샷 또는 데모 영상 링크]

## 라이센스

MIT License
```

---

## 프로젝트 제출 방법

### Fork & Pull Request 방식

1. **Fork**: 본 리포지토리를 본인 계정으로 Fork
2. **Clone**: Fork한 리포지토리를 로컬로 Clone
3. **Branch**: 새로운 브랜치 생성 (`feature/team-name`)
4. **Commit**: 프로젝트 작업 후 커밋
5. **Push**: 본인 Fork 리포지토리에 Push
6. **Pull Request**: 원본 리포지토리로 PR 생성

### Pull Request 작성 가이드

**PR 제목:**
```
[팀명] 프로젝트 제출 - [프로젝트명]
```

**PR 설명:**
```markdown
## 팀 정보
- 팀명: [팀명]
- 팀원: [이름1, 이름2, ...]

## 프로젝트 개요
[간단한 프로젝트 설명]

## 주요 구현 내용
- [구현 내용 1]
- [구현 내용 2]
- ...

## 체크리스트
- [ ] README.md 작성 완료
- [ ] 코드 정리 및 주석 추가
- [ ] 실행 가능한 상태로 제출
```

---

## 참고 자료

- **프로젝트 구조 상세 분석**: [example_directory.md](./example_directory.md)
- **템플릿**: [projects/knu-bootcamp-template](./projects/knu-bootcamp-template)

---

## 문의사항

프로젝트 제출 관련 문의사항은 운영진에게 연락해주세요.

---

## 라이센스

MIT License

Copyright (c) 2025 Upstage AI

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

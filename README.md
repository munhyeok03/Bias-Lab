# Bias Market (Bias-Lab)

Bias Market는 보안 교육 및 내부 실습을 위해 설계된 **의도적으로 취약한** 쇼핑몰 데모입니다.
실제 커머스 사이트처럼 동작하지만, 통제된 환경에서 학습할 수 있도록 일반적인 웹 취약점이 포함되어 있습니다.
**절대 공개 환경에 배포하지 마세요.**

## 빠른 시작 (Docker)

```bash
docker build -t bias-lab:latest .
docker run --rm -p 8000:8000 bias-lab:latest
```

브라우저에서 `http://localhost:8000` 접속

## 아키텍처

- **Backend**: `app.py`의 Flask 애플리케이션 + SQLite 저장소
- **Templates**: `templates/`의 Jinja2 템플릿
- **Static assets**: `static/`의 CSS, JS, 이미지
- **Storage**: 실행 시 `data/`에 SQLite DB 생성, 업로드 파일은 `uploads/`에 저장

## 디렉터리 구조

| 경로 | 설명 |
| --- | --- |
| `app.py` | Flask 애플리케이션, 라우트 및 취약 동작 구현 |
| `templates/` | 모든 페이지 HTML 템플릿 |
| `static/` | CSS, JS, 이미지 등 정적 리소스 |
| `static/images/` | 제품/히어로 이미지 |
| `data/` | 실행 시 생성되는 SQLite DB |
| `uploads/` | 업로드 파일 저장 위치 |
| `public/` | 문서 뷰어에서 노출되는 공개 문서 |
| `secrets/` | 샘플 환경값(첫 실행 시 생성) |

## 취약점 커버리지 (개요)

아래 항목들은 **실습을 위한 학습용 표기**이며, 단계별 공격 방법은 포함하지 않습니다.
UI에는 직접적인 힌트가 없지만, 실제 동작 경로에서 취약점이 드러나도록 구성되어 있습니다.

| 분류 | 표면 | 위치 |
| --- | --- | --- |
| SQLi | 동적 SQL 구성 | `/products`, `/api/products/search` |
| XSS | 무필터 렌더링 | `/search`, `/product/<id>`의 리뷰 영역 |
| CMDi | 쉘 명령 실행 | `/diagnostic`, `/api/diagnostic` |
| Path Traversal | 파일 경로 처리 | `/files`, `/api/file` |
| Auth Bypass | 약한 로그인 로직 | `/login`, `/api/login` |
| IDOR | 직접 객체 접근 | `/account/<id>`, `/orders/<id>` |
| SSRF | 서버 측 요청 | `/fetch`, `/api/fetch` |
| CSRF | 토큰 미적용 | `/transfer`, `/api/transfer` |
| File Upload | 제한 없는 업로드 | `/upload`, `/api/upload` |
| Info Disclosure | 디버그/설정 노출 | `/debug` |

## 주의 사항

- 모든 취약점은 교육/실습 목적입니다.
- 외부 공개 환경에 배포하지 마세요.


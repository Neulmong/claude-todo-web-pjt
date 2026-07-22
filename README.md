# 📋 오늘의 할 일 (To-Do List)

카테고리별로 할 일을 관리하고 진행률을 추적할 수 있는 심플한 웹 애플리케이션입니다.
별도의 빌드 과정 없이 `index.html` 파일 하나로 동작하며, 데이터는 Supabase 데이터베이스에 저장됩니다.

## 주요 기능

- ✅ 할 일 추가 / 완료 체크 / 인라인 수정 / 삭제
- 🗂 카테고리 분류 (전체 / 개인 / 공부 / 취미)
- 📊 전체 및 카테고리별 진행률(완료율) 표시
- ☁️ Supabase(`todo_tbl` 테이블)를 이용한 데이터 저장 (기기·브라우저 간 공유)
- 📱 모바일 화면에 대응하는 반응형 레이아웃

## 사용 방법

1. 이 저장소를 클론하거나 `index.html` 파일을 다운로드합니다.
2. `index.html` 파일을 웹 브라우저에서 엽니다.
3. 할 일을 입력하고 카테고리를 선택한 뒤 추가 버튼을 눌러 사용합니다.

별도의 설치나 서버 실행이 필요 없습니다. (`index.html`에 Supabase 프로젝트 URL과 anon(publishable) 키가 포함되어 있습니다.)

## 기술 스택

- HTML / CSS / Vanilla JavaScript
- [Supabase](https://supabase.com) (`@supabase/supabase-js` CDN) — 데이터베이스 및 클라이언트 연동

## 데이터베이스 스키마 (`todo_tbl`)

| 컬럼         | 타입          | 설명                     |
|--------------|---------------|--------------------------|
| `id`         | bigint (PK)   | 자동 증가 식별자         |
| `text`       | text          | 할 일 내용               |
| `category`   | text          | 카테고리 (개인/공부/취미) |
| `completed`  | boolean       | 완료 여부                |
| `created_at` | timestamptz   | 생성 시각 (기본값 now()) |

RLS(Row Level Security)가 활성화되어 있으며, 별도 인증 없이 anon 키로 전체 CRUD가 가능한 공개 정책이 적용되어 있습니다.

## 프로젝트 구조

```
.
├── index.html   # 전체 애플리케이션 (마크업 + 스타일 + 스크립트, Supabase 연동 포함)
└── README.md
```

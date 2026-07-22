# 📋 오늘의 할 일 (To-Do List)

카테고리별로 할 일을 관리하고 진행률을 추적할 수 있는 심플한 웹 애플리케이션입니다.
별도의 빌드 과정 없이 `index.html` 파일 하나로 동작하며, 데이터는 Supabase 데이터베이스에 저장됩니다.

## 주요 기능

- 🔐 Supabase Auth 기반 로그인 / 회원가입 (이메일 + 비밀번호, 미로그인 시 Todo 화면 대신 로그인/회원가입 화면 표시)
- ✅ 할 일 추가 / 완료 체크 / 인라인 수정 / 삭제 (본인 계정 소유의 할 일만 조회·수정 가능)
- 🗂 카테고리 분류 (전체 / 개인 / 공부 / 취미)
- 📊 전체 및 카테고리별 진행률(완료율) 표시
- ☁️ Supabase(`user_tbl`, `todo_tbl` 1:N 관계)를 이용한 사용자별 데이터 저장
- 📱 모바일 화면에 대응하는 반응형 레이아웃

## 사용 방법

1. 이 저장소를 클론하거나 `index.html` 파일을 다운로드합니다.
2. `index.html` 파일을 웹 브라우저에서 엽니다.
3. 이메일/비밀번호로 회원가입 후(또는 로그인 후) 할 일을 입력하고 카테고리를 선택한 뒤 추가 버튼을 눌러 사용합니다.

별도의 설치나 서버 실행이 필요 없습니다. (`index.html`에 Supabase 프로젝트 URL과 anon(publishable) 키가 포함되어 있습니다.)

## 로그인 / 회원가입

- 회원가입은 `supabase.auth.signUp`, 로그인은 `supabase.auth.signInWithPassword`를 호출하는 방식으로 Supabase Auth와 완전히 연동되어 있습니다.
- 프로젝트의 이메일 확인 설정이 켜져 있으면, 회원가입 후 이메일 인증 링크를 클릭해야 로그인할 수 있습니다(가입 직후 세션이 발급되지 않으면 안내 메시지와 함께 로그인 탭으로 전환됩니다).
- 로그인 세션은 Supabase 클라이언트가 자체적으로 브라우저에 저장·관리합니다.
- 로그인하지 않은 상태에서는 Todo 목록이 아닌 로그인/회원가입 화면이 표시됩니다.

## 기술 스택

- HTML / CSS / Vanilla JavaScript
- [Supabase](https://supabase.com) (`@supabase/supabase-js` CDN) — 인증(Auth), 데이터베이스 및 클라이언트 연동

## 데이터베이스 스키마

`auth.users`(Supabase Auth 관리) 1명당 `user_tbl` 프로필 1개, `user_tbl` 1개당 `todo_tbl` 여러 개가 연결되는 1:N 구조입니다.

### `user_tbl` (auth.users와 1:1)

| 컬럼         | 타입        | 설명                                   |
|--------------|-------------|----------------------------------------|
| `id`         | uuid (PK)   | `auth.users.id`를 참조 (FK, cascade)    |
| `email`      | text        | 가입 이메일                            |
| `created_at` | timestamptz | 생성 시각 (기본값 now())               |

`auth.users`에 새 계정이 생성되면 `on_auth_user_created` 트리거가 `user_tbl`에 프로필 행을 자동으로 생성합니다.

### `todo_tbl` (user_tbl과 1:N)

| 컬럼         | 타입        | 설명                                  |
|--------------|-------------|---------------------------------------|
| `id`         | bigint (PK) | 자동 증가 식별자                      |
| `user_id`    | uuid (FK)   | `user_tbl.id` 참조 (cascade delete)   |
| `text`       | text        | 할 일 내용                            |
| `category`   | text        | 카테고리 (개인/공부/취미)              |
| `completed`  | boolean     | 완료 여부                             |
| `created_at` | timestamptz | 생성 시각 (기본값 now())              |

두 테이블 모두 RLS(Row Level Security)가 활성화되어 있으며, `auth.uid() = id` / `auth.uid() = user_id` 조건으로 각 사용자는 자신의 프로필과 할 일만 조회·수정·삭제할 수 있습니다.

## 프로젝트 구조

```
.
├── index.html   # 전체 애플리케이션 (마크업 + 스타일 + 스크립트, Supabase 연동 포함)
└── README.md
```

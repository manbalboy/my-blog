# MANBALBOY Blog (my-app)

Next.js 기반 블로그/어드민 프로젝트입니다.  
한국어 글 생성/검수/발행 후, 필요 시 영어(미국) 현지화 버전을 `/en/blog/...`로 동시 발행할 수 있습니다.

## 주요 기능
- `/admin`에서 AI 글 생성, 미리보기, Notion 발행
- 발행 시 `영문(미국) 동시 발행` 옵션 지원
- 한국어/영어 언어 토글 (`/` ↔ `/en`)
- 리뷰 이미지 생성 및 Blob 업로드
- Notion을 CMS로 사용 (게시글 조회/발행/수정)

## 시작하기
1. 의존성 설치
```bash
npm install
```

2. 환경변수 파일 생성
```bash
cp .env.example .env.local
```

3. 개발 서버 실행
```bash
npm run dev
```

4. 브라우저 확인
- 사용자 페이지: `http://localhost:3000`
- 어드민 페이지: `http://localhost:3000/admin`

## 스크립트
- `npm run dev`: 개발 서버
- `npm run build`: 프로덕션 빌드
- `npm run start`: 프로덕션 서버
- `npm run lint`: ESLint 검사

## 환경변수 안내
자세한 예시는 `.env.example` 파일 참고.

### 필수
- `OPENAI_API_KEY`
  - 예시: `sk-proj-abc123...`
  - 용도: 글 생성, 영어 현지화 번역, 리뷰 이미지 생성, blog-10 이관 API
- `OPENAI_BASE_URL` (선택)
  - 예시: `https://api.openai.com/v1`
  - 용도: blog-10 이관 API의 OpenAI-compatible endpoint 지정
- `NOTION_API_KEY`
  - 예시: `secret_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`
  - 용도: Notion API 접근
- `NOTION_KEY` (호환용)
  - 예시: `secret_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`
  - 용도: `NOTION_API_KEY`와 동일 값 사용 가능
- `NOTION_DATABASE_ID`
  - 예시: `1234567890abcdef1234567890abcdef`
  - 용도: Notion DB 대상 지정
- `ADMIN_PASSWORD`
  - 예시: `my-strong-admin-password`
  - 용도: `/admin/login` 인증

### 선택
- `BLOB_READ_WRITE_TOKEN`
  - 예시: `vercel_blob_rw_xxxxx`
  - 비워두면 이미지 Blob 업로드 생략
- `REVIEW_IMAGE_MODEL`
  - 기본값: `gpt-image-1`
- `REVIEW_IMAGE_SIZE`
  - 기본값: `1024x1024`
- `REVIEW_IMAGE_QUALITY`
  - 기본값: `low`
  - 권장(비용 절약): `low`
- `OPENAI_IMAGE_QUALITY`
  - `REVIEW_IMAGE_QUALITY` 미설정 시 fallback
- `NOTION_FETCH_TIMEOUT_MS`
  - 기본값: `20000` (ms)
  - Notion 응답 지연 시 타임아웃 완화용
- `NOTION_FETCH_RETRIES`
  - 기본값: `4`
  - Notion 요청 재시도 횟수

## Notion DB 준비사항 (중요)
영문 동시 발행을 위해 Notion DB에 아래 속성이 있어야 합니다.
- `Locale` (Select): `ko`, `en`
- `SourceSlug` (Rich text): 원문 slug 연결용

기존 글에 `Locale`이 비어 있으면 코드에서 한국어(`ko`)로 처리합니다.

## 라우팅
- 한국어 목록: `/`
- 영어 목록: `/en`
- 한국어 상세: `/blog/[slug]`
- 영어 상세: `/en/blog/[slug]`

참고: 영어 상세가 없고 한국어 상세만 있으면 `/en/blog/[slug]` 접근 시 한국어 상세로 자동 이동합니다.

## 운영 팁
- 이미지 비용 절약: `REVIEW_IMAGE_QUALITY=low` 유지
- 영어 발행 실패 시 한국어 발행은 정상 완료되고, 어드민에서 영어 실패 메시지를 확인할 수 있습니다.
# my-blog

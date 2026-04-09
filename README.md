# Wiki.js

Wiki.js 기반의 위키 프로젝트입니다.

---

## 기술 스택

| 분류 | 기술 |
|------|------|
| **Runtime** | Node.js 24 |
| **Frontend** | Vue 2, Vuex, Vuetify, Vue Router |
| **Backend** | Express, Apollo Server (GraphQL) |
| **Database** | PostgreSQL 17 (MySQL, MariaDB, SQLite, MSSQL 지원) |
| **ORM** | Knex.js + Objection.js |
| **Build** | Webpack 4, Babel |
| **인증** | Passport.js (로컬, OAuth2, SAML, LDAP 등 20+ 전략) |
| **컨테이너** | Docker, Docker Compose |
| **패키지 매니저** | Yarn |

---

## 로컬 개발 환경 세팅

### 사전 요구사항

- Node.js `v24.12.0` ([nvm](https://github.com/nvm-sh/nvm) 사용 권장)
- Yarn
- Docker & Docker Compose (DB 실행용)

---

### 1단계: Node.js 버전 맞추기

```bash
nvm install 24.12.0
nvm use
```

### 2단계: 의존성 설치

```bash
yarn install
```

### 3단계: 설정 파일 생성

```bash
cp config.sample.yml config.yml
```

`config.yml`의 DB 정보를 아래와 같이 설정합니다 (Docker 기본값 기준):

```yaml
port: 3000

db:
  type: postgres
  host: localhost
  port: 15432      # Docker Compose에서 15432로 노출됨
  user: wikijs
  pass: wikijsrocks
  db: wiki
  ssl: false
```

### 4단계: 데이터베이스 실행 (Docker)

```bash
cd dev/containers
docker-compose up -d
```

| 서비스 | 주소 |
|--------|------|
| PostgreSQL | `localhost:15432` |
| Adminer (DB GUI) | `http://localhost:3001` |

### 5단계: 개발 서버 실행

```bash
# 프로젝트 루트에서
yarn dev
```

Webpack watch + Node.js 서버가 함께 실행됩니다.

### 6단계: 초기 설정 (최초 1회)

브라우저에서 `http://localhost:3000` 접속 후 Setup Wizard 진행:

1. 관리자 계정 (이메일 / 비밀번호) 생성
2. 사이트 이름 및 URL 설정
3. 완료 후 위키 메인 페이지로 이동

---

## 주요 스크립트

| 명령어 | 설명 |
|--------|------|
| `yarn dev` | 개발 서버 실행 (Webpack watch 포함) |
| `yarn build` | 프로덕션용 정적 파일 빌드 |
| `yarn start` | 프로덕션 서버 실행 |
| `yarn test` | ESLint + Jest 테스트 실행 |

---

## 프로젝트 구조

```
wiki/
├── client/             # Vue.js 프론트엔드
│   ├── components/     # Vue 컴포넌트
│   ├── store/          # Vuex 상태 관리
│   ├── graph/          # GraphQL 쿼리/뮤테이션
│   └── scss/           # 스타일시트
├── server/             # Node.js 백엔드
│   ├── core/           # 핵심 모듈 (auth, db, config 등)
│   ├── controllers/    # 라우트 핸들러
│   ├── graph/          # GraphQL 스키마 & 리졸버
│   ├── models/         # DB 모델 (Objection.js)
│   ├── modules/        # 플러그인 (인증, 스토리지, 검색 등)
│   └── views/          # Pug 템플릿
├── dev/
│   ├── containers/     # 개발용 Docker Compose & Dockerfile
│   ├── build/          # 프로덕션 Docker 빌드
│   └── examples/       # Docker Compose 예시
├── config.sample.yml   # 설정 파일 템플릿
└── config.yml          # 실제 설정 파일 (gitignore 대상, 직접 생성 필요)
```

---

## 환경 변수 / 설정

`config.yml`은 git에서 제외됩니다. `config.sample.yml`을 복사해서 생성하세요.

Docker 환경에서는 아래 환경 변수로도 설정 가능합니다:

| 변수명 | 설명 |
|--------|------|
| `DB_TYPE` | 데이터베이스 종류 (postgres, mysql 등) |
| `DB_HOST` | DB 호스트 |
| `DB_PORT` | DB 포트 |
| `DB_USER` | DB 사용자 |
| `DB_PASS` | DB 비밀번호 |
| `DB_NAME` | DB 이름 |
| `LOG_LEVEL` | 로그 레벨 (info, debug 등) |

---

## 배포 계획

| 분류 | 플랫폼 |
|------|--------|
| Frontend | GitHub Pages |
| Backend | 미정 (무료 플랜 서비스 예정) |
| Database | 미정 |


## 세팅
1. yarn install 
2. psql postgres 명령어로 접속
3. 아래 명령어 실행
```
CREATE USER wikijs WITH PASSWORD 'wikijsrocks';
CREATE DATABASE wiki OWNER wikijs;
\q
```
4. cp config.sample.yml config.yml 로 복사
5. config.yml에 아래의 코드 덮어씌기

```
db:
  type: postgres
  host: localhost
  port: 5432
  user: wikijs
  pass: wikijs
  db: wiki
  ssl: false
```

6. yarn dev로 실행
7. 개인 정보를 입력하고 site 주소를 로컬 http://localhost:3000 로 설정
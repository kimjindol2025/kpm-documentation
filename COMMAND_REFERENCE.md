# KPM v2.0.0 - 명령어 완전 레퍼런스

**버전**: 2.0.0
**작성 날짜**: 2026-03-06
**대상**: CLI 사용자

---

## 목차

1. [기본 명령어](#기본-명령어)
2. [검색 & 정보](#검색--정보)
3. [설치 & 관리](#설치--관리)
4. [고급 기능](#고급-기능)
5. [옵션 & 플래그](#옵션--플래그)
6. [예제](#예제)

---

## 기본 명령어

### kpm --version

패키지 매니저 버전 출력

```bash
$ kpm --version
KPM v2.0.0
```

**옵션**: 없음
**출력**: 버전 번호

---

### kpm --help

도움말 표시

```bash
$ kpm --help
KPM v2.0.0 - Kim Package Manager

사용법:
  kpm <명령어> [옵션] [인자]

명령어:
  search <키워드>     패키지 검색
  tag <태그>         태그별 검색
  info <패키지>      패키지 정보
  install <패키지>    패키지 설치
  list               설치된 패키지 목록
  tree               의존성 트리
  analyze [파일]     npm→KPM 분석
  update [패키지]     패키지 업데이트
  publish            패키지 발행
  stats              레지스트리 통계

옵션:
  --version          버전 표시
  --help             도움말 표시
  --verbose          상세 로그
```

---

## 검색 & 정보

### kpm search <키워드>

패키지 검색

```bash
$ kpm search freelang
1. FreeLang (v2.6.0) [language]
   완전한 언어 런타임
   #freelang #language #runtime

2. freelang-c (v1.0.0) [language]
   C 컴파일러 기반 고성능 런타임
   #freelang #runtime #c

3. freelang-c-final (v2.6.0) [language]
   F-String 보간 및 고급 기능
   #freelang #language #f-string
```

**인자**:
- `<키워드>`: 검색 키워드 (필수)

**옵션**:
- `--limit <수>`: 결과 개수 제한 (기본값: 20)
- `--type <타입>`: 타입 필터 (language, api, web-app, ai, etc.)
- `--tag <태그>`: 태그 필터
- `--sort <정렬>`: 정렬 기준 (name, version, date)

**사용 예**:
```bash
# 기본 검색
kpm search freelang

# 결과 5개만 표시
kpm search freelang --limit 5

# 언어 타입만 검색
kpm search freelang --type language

# AI 태그 필터
kpm search --tag ai

# 버전순 정렬
kpm search freelang --sort version
```

---

### kpm tag <태그>

태그별 패키지 검색

```bash
$ kpm tag ai
1. kim-agent (v1.1.0)
   Google Gemini API 기반 에이전트

2. Proof_ai (v0.0.1-alpha)
   Intent 통합 AI

3. MOSS (v1.0.0)
   AI 플랫폼
```

**인자**:
- `<태그>`: 검색할 태그 (필수)

**옵션**:
- `--limit <수>`: 결과 개수 제한

**사용 예**:
```bash
# AI 패키지 검색
kpm tag ai

# freelang 패키지 검색
kpm tag freelang

# 인프라 패키지 검색
kpm tag infrastructure

# 결과 10개만
kpm tag async --limit 10
```

---

### kpm info <패키지>

패키지 상세 정보

```bash
$ kpm info FreeLang
패키지 정보:
  ID: 1
  이름: FreeLang
  버전: 2.6.0
  설명: 완전한 언어 런타임 (Level 3 구현)
  URL: https://gogs.dclub.kr/kim/v2-freelang-ai.git
  타입: language
  태그: freelang, language, runtime, compiler, vm
  상태: active
  생성: 2026-03-06T13:50:44Z
  수정: 2026-03-06T13:50:44Z
```

**인자**:
- `<패키지>`: 패키지 이름 (필수)

**옵션**:
- `--verbose`: 상세 정보 표시
- `--json`: JSON 형식 출력

**사용 예**:
```bash
# 기본 정보
kpm info FreeLang

# 상세 정보
kpm info FreeLang --verbose

# JSON 형식
kpm info FreeLang --json
```

---

## 설치 & 관리

### kpm install <패키지>

패키지 설치

```bash
$ kpm install FreeLang
Cloning into '/home/kim_modules/FreeLang'...
remote: Enumerating objects: 100, done.
Receiving objects: 100% (100/100), done.
✅ FreeLang (v2.6.0)이 설치되었습니다.
위치: /home/kim_modules/FreeLang
```

**인자**:
- `<패키지>`: 패키지 이름 또는 이름@버전

**옵션**:
- `--path <경로>`: 설치 경로 지정
- `--no-save`: package.json에 추가하지 않음
- `--save`: package.json에 의존성 추가
- `--save-dev`: package.json의 devDependencies에 추가

**사용 예**:
```bash
# 기본 설치
kpm install FreeLang

# 특정 버전 설치
kpm install FreeLang@2.6.0

# 다른 경로에 설치
kpm install FreeLang --path ~/my-packages

# 의존성으로 추가
kpm install FreeLang --save
```

---

### kpm list

설치된 패키지 목록

```bash
$ kpm list
설치된 패키지:
1. FreeLang (v2.6.0)
   위치: /home/kim_modules/FreeLang

2. kim-agent (v1.1.0)
   위치: /home/kim_modules/kim-agent

3. User-Auth-System (v1.0.0)
   위치: /home/kim_modules/User-Auth-System
```

**옵션**:
- `--path <경로>`: 특정 경로의 패키지만 표시
- `--outdated`: 업데이트 가능한 패키지만
- `--all`: 모든 패키지 표시

**사용 예**:
```bash
# 모든 설치된 패키지
kpm list

# 업데이트 가능한 패키지만
kpm list --outdated

# 특정 경로
kpm list --path /home/kim_modules
```

---

### kpm tree

의존성 트리 표시

```bash
$ kpm tree
my-project
├── FreeLang@2.6.0
│   ├── freelang-database-driver@1.0.0
│   └── freelang-c@1.0.0
├── kim-agent@1.1.0
│   └── gemini-bridge@1.0.0
└── User-Auth-System@1.0.0
    ├── redis
    └── postgres
```

**옵션**:
- `--depth <깊이>`: 트리 깊이 제한
- `--flat`: 평면 리스트로 표시
- `--size`: 패키지 크기 표시

**사용 예**:
```bash
# 전체 트리
kpm tree

# 깊이 2까지만
kpm tree --depth 2

# 평면 리스트
kpm tree --flat

# 크기 포함
kpm tree --size
```

---

### kpm update [패키지]

패키지 업데이트

```bash
$ kpm update FreeLang
Updating FreeLang...
Already up to date.

$ kpm update
Updating kim-agent...
v1.1.0 → v1.1.1

Updating User-Auth-System...
Already up to date.
```

**인자**:
- `[패키지]`: 패키지 이름 (생략 시 모든 패키지 업데이트)

**옵션**:
- `--force`: 강제 업데이트
- `--no-save`: package.json 업데이트 안 함

**사용 예**:
```bash
# 특정 패키지 업데이트
kpm update FreeLang

# 모든 패키지 업데이트
kpm update

# 강제 업데이트
kpm update FreeLang --force
```

---

## 고급 기능

### kpm analyze [파일]

npm → KPM 마이그레이션 분석

```bash
$ kpm analyze package.json
분석 결과:

총 의존성: 15개

✅ KPM에서 대체 가능:
  1. express (v4.18.0)
     대체: guestbook, meeting-server-v3

  2. jsonwebtoken (v9.0.0)
     대체: User-Auth-System

⚠️ 부분 호환:
  1. lodash (v4.17.21)
     일부 함수 대체 가능

❌ 직접 지원 안 함:
  1. webpack (v5.0.0)
  2. babel (v7.0.0)

📊 마이그레이션 가능도: 67%
```

**인자**:
- `[파일]`: package.json 경로 (생략 시 현재 디렉토리)

**옵션**:
- `--json`: JSON 형식 출력
- `--detailed`: 상세 분석
- `--strict`: 엄격한 호환성 검사

**사용 예**:
```bash
# 현재 디렉토리 분석
kpm analyze

# 특정 파일 분석
kpm analyze ~/my-project/package.json

# JSON 형식 출력
kpm analyze --json

# 상세 분석
kpm analyze --detailed
```

---

### kpm stats

레지스트리 통계

```bash
$ kpm stats
📊 레지스트리 통계

총 패키지: 903개
총 다운로드: 1.2M (추정)

📁 타입별 분포:
  unknown: 833개 (92%)
  library: 59개 (6%)
  language: 10개 (1%)
  web-app: 1개 (<1%)

🏷️ 상위 15개 태그:
  1. #parser (8개)
  2. #network (8개)
  3. #freelang (8개)
  4. #async (7개)
  5. #database (6개)
  ...

🎯 가장 인기 있는 패키지:
  1. FreeLang (v2.6.0)
  2. User-Auth-System (v1.0.0)
  3. kim-agent (v1.1.0)
```

**옵션**:
- `--json`: JSON 형식 출력
- `--top <수>`: 상위 N개 태그 표시
- `--type <타입>`: 특정 타입 통계만

**사용 예**:
```bash
# 전체 통계
kpm stats

# JSON 형식
kpm stats --json

# 상위 10개 태그
kpm stats --top 10

# 언어 타입만
kpm stats --type language
```

---

### kpm publish

패키지 발행 (레지스트리에 등록)

```bash
$ cd /path/to/my-project
$ kpm publish
Preparing my-project (v1.0.0)...
Creating registry entry...
✅ my-project (v1.0.0)이 KPM에 등록되었습니다
URL: https://gogs.dclub.kr/kim/my-project.git
```

**요구사항**:
- `package.json` 필수
- 프로젝트가 git 저장소여야 함

**옵션**:
- `--force`: 기존 패키지 덮어쓰기
- `--private`: 비공개 패키지로 등록 (향후 구현)
- `--tag <태그>`: 태그 추가

**사용 예**:
```bash
# 현재 프로젝트 발행
kpm publish

# 태그 추가
kpm publish --tag ai --tag async

# 기존 패키지 업데이트
kpm publish --force
```

---

## 옵션 & 플래그

### 글로벌 옵션

```bash
# 상세 로그 출력
kpm --verbose search freelang

# JSON 형식 출력 (지원하는 명령어)
kpm --json info FreeLang

# 버전 표시
kpm --version

# 도움말 표시
kpm --help
```

### 필터 옵션

```bash
# 타입 필터
--type language
--type api
--type web-app
--type ai
--type infrastructure
--type library

# 태그 필터
--tag freelang
--tag ai
--tag async
--tag database

# 정렬 옵션
--sort name         # 이름순
--sort version      # 버전순
--sort date         # 날짜순
--sort relevance    # 관련성순 (기본값)

# 개수 제한
--limit 10
--limit 50
```

---

## 예제

### 예제 1: FreeLang 전체 스택 설치

```bash
# 검색
$ kpm search freelang

# 각 패키지 정보 확인
$ kpm info FreeLang
$ kpm info freelang-c
$ kpm info freelang-database-driver

# 설치
$ kpm install FreeLang --save
$ kpm install freelang-c --save
$ kpm install freelang-database-driver --save

# 확인
$ kpm list
$ kpm tree
```

---

### 예제 2: AI 생태계 구축

```bash
# AI 패키지 검색
$ kpm tag ai

# 각 패키지 정보
$ kpm info kim-agent
$ kpm info Proof_ai
$ kpm info MOSS

# 설치
$ kpm install kim-agent --save
$ kpm install Proof_ai --save
$ kpm install MOSS --save

# 의존성 확인
$ kpm tree
```

---

### 예제 3: npm에서 KPM으로 마이그레이션

```bash
# 현재 프로젝트 분석
$ cd my-project
$ kpm analyze

# 마이그레이션 가능 패키지 확인
# (상세 보고서 참고)

# 호환 패키지 설치
$ kpm install FreeLang --save
$ kpm install User-Auth-System --save

# npm 의존성 제거 (필요한 것만)
$ npm uninstall express
$ kpm install guestbook --save

# 확인
$ kpm list
$ kpm analyze  # 다시 분석
```

---

### 예제 4: 패키지 발행

```bash
# 새 프로젝트 생성
$ mkdir my-awesome-lib
$ cd my-awesome-lib
$ git init
$ npm init -y

# 코드 작성 및 git 커밋
$ echo "module.exports = { ... }" > index.js
$ git add .
$ git commit -m "Initial commit"

# KPM에 발행
$ kpm publish

# 발행 확인
$ kpm search my-awesome-lib
$ kpm info my-awesome-lib
```

---

### 예제 5: 고급 검색

```bash
# 모든 언어 패키지 (결과 50개)
$ kpm search "" --type language --limit 50

# freelang 태그의 라이브러리만
$ kpm search --tag freelang --type library

# AI 플랫폼 (검색 + 정보)
$ kpm search ai --limit 5
$ kpm info MOSS

# 인프라 패키지 정렬 (버전순)
$ kpm search "" --tag infrastructure --sort version --limit 20
```

---

## 🔍 팁 & 트릭

### 1. 빠른 검색
```bash
# 첫 단어만 사용해도 OK
kpm search free  # FreeLang 검색됨
kpm search kim   # kim-agent 검색됨
```

### 2. 자동완성
```bash
# 셸 자동완성 (설정 필요)
kpm search fre<TAB>  # freelang, freelang-c 자동 완성
```

### 3. 파이프라인 활용
```bash
# JSON 출력을 jq로 처리
kpm info FreeLang --json | jq '.version'

# 패키지 목록 필터
kpm list | grep -i freelang
```

### 4. 배치 작업
```bash
# 모든 패키지 업데이트
for pkg in $(kpm list --flat); do
  kpm update $pkg
done
```

---

## 📞 도움말

### 명령어별 도움말
```bash
kpm search --help
kpm install --help
kpm publish --help
```

### 문제 해결
```bash
# 상세 로그로 실행
kpm --verbose search freelang

# 레지스트리 상태 확인
curl http://localhost:50250/api/v1/status
```

---

**KPM v2.0.0 - 완전 명령어 레퍼런스**

생성일: 2026-03-06
상태: ✅ 완성

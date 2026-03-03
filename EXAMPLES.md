# 💡 KPM 사용 예제

## 목차
1. [기본 예제](#기본-예제)
2. [실무 시나리오](#실무-시나리오)
3. [통합 예제](#통합-예제)
4. [스크립트 예제](#스크립트-예제)

---

## 기본 예제

### 예제 1: 패키지 검색 및 설치

```bash
# 상황: REST API 서버를 만들어야 함

# 1단계: KPM 검색 (필수!)
kpm search api
kpm search server
kpm search rest

# 2단계: 결과 분석
# → "api-hub", "RESTful-API" 패키지 발견
# → 가장 많이 사용되는 것: "api-hub" 선택

# 3단계: 패키지 정보 조회
kpm info api-hub
# 출력:
# 패키지명: api-hub
# 버전: v1.0.0
# 설명: 통합 API 관리 솔루션
# 저장소: https://gogs.dclub.kr/kim/api-hub.git

# 4단계: 설치
kpm install api-hub

# 5단계: 설치 확인
kpm list | grep api-hub
# api-hub (v1.0.0)

# 6단계: 사용
cd /home/kimjin/kim_modules/api-hub
cat README.md          # 문서 읽기
npm start              # 시작
```

### 예제 2: 여러 패키지 설치

```bash
# 상황: 전체 인프라 설정 필요

# 한 번에 여러 패키지 설치
kpm install dns-manager
kpm install ssh-hub
kpm install kim-health

# 또는 한 줄로
kpm install dns-manager ssh-hub kim-health

# 설치 완료 확인
kpm list
# 설치된 패키지 (총 3개):
# - dns-manager (v2.1.0)
# - ssh-hub (v1.0.0)
# - kim-health (v1.2.0)
```

---

## 실무 시나리오

### 시나리오 1: 인증 시스템 구축

```bash
# 요구사항: JWT 기반 인증 시스템

# 1단계: KPM 검색
kpm search auth
kpm search jwt
kpm search authentication

# 2단계: 결과 확인 (예상)
# → "jwt-auth" 패키지 발견
# → "oauth2-provider" 패키지 발견
# → "auth-middleware" 패키지 발견

# 3단계: 상세 정보 조회
kpm info jwt-auth
kpm info auth-middleware

# 4단계: 선택 및 설치
kpm install jwt-auth

# 5단계: 프로젝트에서 사용
# app.js
const auth = require('/home/kimjin/kim_modules/jwt-auth');
app.use(auth.middleware());
```

### 시나리오 2: 데이터베이스 연동

```bash
# 요구사항: SQL + NoSQL 하이브리드 DB

# 1단계: KPM 검색
kpm search database
kpm search sql
kpm search mongodb

# 2단계: 패키지 찾기 및 설치
kpm install database-driver  # SQL 드라이버
kpm install KimDB           # NoSQL

# 3단계: 설치 확인
kpm list | grep -i db
# database-driver (v1.0.0)
# KimDB (v1.0.0)

# 4단계: 프로젝트 통합
# project/src/db.js
const SQL = require('/home/kimjin/kim_modules/database-driver');
const NoSQL = require('/home/kimjin/kim_modules/KimDB');

const sqlDb = new SQL.SQLiteDatabase(':memory:');
const noSqlDb = new NoSQL.Database('data.kdb');

module.exports = { sqlDb, noSqlDb };
```

### 시나리오 3: AI 기능 추가

```bash
# 요구사항: 코드 리뷰 AI 통합

# 1단계: KPM 검색
kpm search ai
kpm search review
kpm search code review

# 2단계: 패키지 설치
kpm install AI-Review-API

# 3단계: 사용
cd /home/kimjin/kim_modules/AI-Review-API
npm start

# 4단계: API 호출
curl -X POST http://localhost:5000/api/review \
  -H "Content-Type: application/json" \
  -d '{
    "code": "function test() { ... }",
    "language": "javascript"
  }'

# 응답:
# {
#   "status": "ok",
#   "review": {
#     "issues": [...],
#     "suggestions": [...]
#   }
# }
```

---

## 통합 예제

### 예제: 마이크로서비스 아키텍처

```bash
# 상황: 여러 마이크로서비스를 구축하는 상황

# 1단계: 필수 패키지 검색 및 설치
echo "=== 기반 인프라 ==="
kpm search docker
kpm search container
kpm install kim-docker

echo "=== API 게이트웨이 ==="
kpm search api gateway
kpm install api-hub

echo "=== 인증 ==="
kpm search auth
kpm install jwt-auth

echo "=== 로깅 ==="
kpm search logging
kpm install central-log

echo "=== 모니터링 ==="
kpm search health
kpm install kim-health

# 2단계: 설치 확인
echo "=== 설치 완료 ==="
kpm list

# 3단계: 서비스 시작
echo "=== 서비스 구동 ==="
cd /home/kimjin/kim_modules/api-hub && npm start &
cd /home/kimjin/kim_modules/kim-docker && npm start &
cd /home/kimjin/kim_modules/central-log && npm start &

# 4단계: 헬스 체크
echo "=== 헬스 체크 ==="
curl http://localhost:5000/health    # API Hub
curl http://localhost:3000/health    # Docker Manager
curl http://localhost:9200/health    # Central Log
```

---

## 스크립트 예제

### 스크립트 1: 자동 설정 스크립트 (setup.sh)

```bash
#!/bin/bash

# KPM 자동 설정 스크립트
# 사용법: bash setup.sh

set -e

echo "🚀 KPM 패키지 자동 설정 시작..."

# 1. KPM 버전 확인
echo "📦 KPM 버전: $(kpm --version)"

# 2. 필수 패키지 설치
PACKAGES=(
    "dns-manager"
    "ssh-hub"
    "kim-health"
    "FreeLang"
)

for pkg in "${PACKAGES[@]}"; do
    echo "📥 설치 중: $pkg"
    kpm install "$pkg" || echo "⚠️ $pkg 설치 실패"
done

# 3. 설치 확인
echo ""
echo "✅ 설치된 패키지:"
kpm list

# 4. 디렉토리 생성
echo ""
echo "📁 디렉토리 구조 확인:"
ls -lh /home/kimjin/kim_modules/

echo ""
echo "✨ 설정 완료!"
```

### 스크립트 2: 패키지 검색 및 정보 조회 (search_and_info.sh)

```bash
#!/bin/bash

# 패키지 검색 및 상세 정보 조회
# 사용법: bash search_and_info.sh api server

if [ $# -eq 0 ]; then
    echo "사용법: $0 키워드1 [키워드2] [키워드3]"
    exit 1
fi

echo "🔍 KPM 패키지 검색"
echo "검색어: $@"
echo ""

# 1. 검색
echo "📌 검색 결과:"
for keyword in "$@"; do
    kpm search "$keyword"
    echo ""
done

# 2. 사용자 선택
read -p "설치할 패키지명을 입력하세요: " package_name

if [ -z "$package_name" ]; then
    echo "❌ 패키지명이 입력되지 않았습니다."
    exit 1
fi

# 3. 상세 정보 조회
echo ""
echo "ℹ️  패키지 정보:"
kpm info "$package_name"

# 4. 설치 확인
read -p "설치하시겠습니까? (y/n): " confirm
if [ "$confirm" = "y" ] || [ "$confirm" = "Y" ]; then
    echo "📥 설치 중..."
    kpm install "$package_name"
    echo "✅ 설치 완료!"
    echo "위치: /home/kimjin/kim_modules/$package_name"
else
    echo "❌ 취소됨"
fi
```

### 스크립트 3: 패키지 업데이트 (update_all.sh)

```bash
#!/bin/bash

# 모든 패키지 업데이트
# 사용법: bash update_all.sh

echo "🔄 KPM 패키지 업데이트 시작..."
echo ""

# 1. 현재 설치 목록
echo "📦 현재 설치된 패키지:"
kpm list
echo ""

# 2. 모두 업데이트
read -p "모든 패키지를 업데이트하시겠습니까? (y/n): " confirm

if [ "$confirm" = "y" ] || [ "$confirm" = "Y" ]; then
    echo "🔄 업데이트 중..."
    kpm update
    echo ""
    echo "✅ 업데이트 완료!"
    echo ""
    echo "📦 업데이트된 패키지:"
    kpm list
else
    echo "❌ 취소됨"
fi
```

### 스크립트 4: 설치된 패키지 정보 리포트 (report.sh)

```bash
#!/bin/bash

# 설치된 패키지 정보 리포트 생성
# 사용법: bash report.sh

REPORT_FILE="kpm_report_$(date +%Y%m%d_%H%M%S).md"

echo "# KPM 설치 패키지 리포트" > "$REPORT_FILE"
echo "" >> "$REPORT_FILE"
echo "생성 일시: $(date)" >> "$REPORT_FILE"
echo "" >> "$REPORT_FILE"

echo "## 설치된 패키지" >> "$REPORT_FILE"
echo "" >> "$REPORT_FILE"
kpm list >> "$REPORT_FILE"
echo "" >> "$REPORT_FILE"

echo "## KPM 버전" >> "$REPORT_FILE"
kpm --version >> "$REPORT_FILE"
echo "" >> "$REPORT_FILE"

echo "## 시스템 정보" >> "$REPORT_FILE"
echo "- OS: $(uname -s)" >> "$REPORT_FILE"
echo "- 아키텍처: $(uname -m)" >> "$REPORT_FILE"
echo "- 설치 경로: /home/kimjin/kim_modules" >> "$REPORT_FILE"
echo "- 레지스트리: /home/kimjin/kpm-registry" >> "$REPORT_FILE"

echo "✅ 리포트 생성 완료: $REPORT_FILE"
cat "$REPORT_FILE"
```

---

## 실행 예시

### 기본 워크플로우
```bash
# 1. KPM 설치 확인
$ kpm --version
KPM v1.0.0

# 2. 패키지 검색
$ kpm search rest api server
rest-api-framework (v2.0.0) - RESTful API 프레임워크
api-hub (v1.0.0) - API 통합 관리
server-core (v1.5.0) - 서버 핵심 라이브러리

# 3. 패키지 정보
$ kpm info api-hub
패키지명: api-hub
버전: v1.0.0
설명: API 통합 관리 솔루션
...

# 4. 설치
$ kpm install api-hub
✓ api-hub v1.0.0 설치됨
위치: /home/kimjin/kim_modules/api-hub

# 5. 사용
$ cd /home/kimjin/kim_modules/api-hub
$ npm start
🚀 API Hub started on port 5000
```

---

## 팁 & 트릭

### 💡 효율적인 검색
```bash
# 여러 키워드 조합
kpm search machine learning framework

# 태그로 검색 (지원 시)
kpm search --tag=infrastructure

# 카테고리로 검색
kpm search --category=ai
```

### 🔍 설치된 패키지 찾기
```bash
# 특정 패키지 검색
kpm list | grep freelang

# 모든 인프라 패키지
kpm list | grep -i infra
```

### 📊 패키지 위치 확인
```bash
# 설치된 패키지 경로
ls /home/kimjin/kim_modules/

# 특정 패키지 파일 확인
ls -la /home/kimjin/kim_modules/api-hub/
```

---

더 많은 정보는 [GUIDE.md](GUIDE.md)를 참조하세요.

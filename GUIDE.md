# 📖 KPM 완전 가이드

## 목차
1. [설치 및 설정](#설치-및-설정)
2. [기본 명령어](#기본-명령어)
3. [패키지 검색](#패키지-검색)
4. [패키지 설치](#패키지-설치)
5. [설치된 패키지 관리](#설치된-패키지-관리)
6. [고급 기능](#고급-기능)

---

## 설치 및 설정

### 시스템 요구사항
- Linux/macOS/Windows (WSL)
- bash/zsh 셸
- git (저장소 클론용)

### 설치 확인
```bash
which kpm
# /home/kimjin/.local/bin/kpm

kpm --version
# KPM v1.0.0
```

### 환경 변수
```bash
# KPM 레지스트리 위치
export KPM_REGISTRY_PATH="/home/kimjin/kpm-registry"

# KPM 모듈 설치 위치
export KPM_MODULES_PATH="/home/kimjin/kim_modules"
```

---

## 기본 명령어

### 1️⃣ 버전 확인
```bash
kpm --version
# KPM v1.0.0
```

### 2️⃣ 도움말 보기
```bash
kpm --help
# KPM 모든 명령어 및 옵션 표시
```

### 3️⃣ 설치된 패키지 확인
```bash
kpm list
# 설치된 모든 패키지 목록 표시
# 출력 형식:
# - freelang (v1.5.0)
# - dns-manager (v2.1.0)
# - ssh-hub (v1.0.0)
```

---

## 패키지 검색

### 단일 키워드 검색
```bash
# API 관련 패키지 검색
kpm search api

# 출력:
# api-hub (v1.0.0) - API 통합 허브
# api-gateway (v2.1.0) - API 게이트웨이
# RESTful-API (v1.2.0) - REST API 프레임워크
```

### 다중 키워드 검색 (권장)
```bash
# 여러 키워드로 상세 검색
kpm search database driver
kpm search auth jwt
kpm search ml model training
```

### 검색 팁
- **최소 1단어 이상** 입력
- **3단어** 입력하면 가장 정확한 결과
- **공백**으로 구분

---

## 패키지 정보

### 패키지 상세 정보 조회
```bash
kpm info dns-manager

# 출력:
# 패키지명: dns-manager
# 버전: v2.1.0
# 설명: DNS/Nginx/PM2 통합 관리
# 카테고리: Infrastructure
# 작성자: kim
# 저장소: https://gogs.dclub.kr/kim/dns-manager.git
# 라이선스: MIT
# 태그: [dns, nginx, pm2, infrastructure]
```

### 패키지 존재 여부 확인
```bash
# 설치되지 않은 패키지 정보 조회
kpm info FreeLang
kpm info KimDB

# 설치된 패키지 정보 조회
kpm info dns-manager --installed
```

---

## 패키지 설치

### 기본 설치
```bash
kpm install dns-manager
# /home/kimjin/kim_modules/dns-manager/ 에 설치

kpm install FreeLang
# /home/kimjin/kim_modules/FreeLang/ 에 설치
```

### 특정 버전 설치
```bash
kpm install FreeLang@1.5.0
# FreeLang v1.5.0 설치 (지원 시)

kpm install dns-manager@2.0.0
# dns-manager v2.0.0 설치
```

### 여러 패키지 동시 설치
```bash
kpm install FreeLang dns-manager ssh-hub
# 3개 패키지 모두 설치

# 또는 한 줄씩
kpm install FreeLang
kpm install dns-manager
kpm install ssh-hub
```

### 설치 확인
```bash
kpm list | grep dns-manager
# dns-manager (v2.1.0)

ls /home/kimjin/kim_modules/dns-manager/
# server.js  package.json  public/  README.md
```

---

## 설치된 패키지 관리

### 설치 목록 조회
```bash
kpm list

# 출력 형식:
# 설치된 패키지 (총 15개):
#
# 인프라
#   - dns-manager (v2.1.0)
#   - ssh-hub (v1.0.0)
#   - kim-docker (v1.3.0)
#
# 개발 도구
#   - FreeLang (v1.5.0)
#   - kim-build (v1.0.0)
#
# AI/ML
#   - AI-Review-API (v1.0.0)
```

### 패키지 업데이트
```bash
# 단일 패키지 업데이트
kpm update FreeLang
# FreeLang을 최신 버전으로 업데이트

# 모든 패키지 업데이트
kpm update
# 모든 설치된 패키지를 최신 버전으로 업데이트

# 업데이트 확인
kpm list | grep FreeLang
# FreeLang (v1.6.0)  ← 업데이트됨
```

---

## 고급 기능

### 패키지 경로 확인
```bash
# 설치된 패키지의 절대 경로 확인
ls /home/kimjin/kim_modules/

# 출력:
# dns-manager/
# ssh-hub/
# FreeLang/
# ai-review-api/
# kim-docker/
```

### 패키지 사용 (예시)

#### dns-manager 사용
```bash
# 설치
kpm install dns-manager

# 사용
cd /home/kimjin/kim_modules/dns-manager
npm start
# 또는
node server.js
```

#### FreeLang 사용
```bash
# 설치
kpm install FreeLang

# 사용
/home/kimjin/kim_modules/FreeLang/bin/freelang script.free
```

### 패키지 캐시 정리
```bash
# 설치 완료 후 캐시 정리 (선택사항)
rm -rf /home/kimjin/kim_modules/.cache

# KPM 레지스트리 다시 동기화
# (자동으로 최신 레지스트리 로드)
```

---

## 의사결정 플로우차트

```
코드 작성 시작
    ↓
기능 구현 필요?
    ↓ YES
KPM 검색 3개 키워드 (필수!)
    ↓
패키지 찾음?
    ├─ YES (3개 이상) → kpm install 사용 ✅
    ├─ YES (1-2개) → 검토 후 결정
    └─ NO → 새로 구현 (사용자 승인 후)
```

---

## 명령어 참조표

| 명령어 | 사용법 | 설명 |
|--------|--------|------|
| `search` | `kpm search 키워드` | 패키지 검색 |
| `info` | `kpm info 패키지명` | 패키지 상세 정보 |
| `list` | `kpm list` | 설치된 패키지 목록 |
| `install` | `kpm install 패키지명` | 패키지 설치 |
| `update` | `kpm update [패키지명]` | 패키지 업데이트 |
| `--version` | `kpm --version` | KPM 버전 확인 |
| `--help` | `kpm --help` | 도움말 표시 |

---

## 🎯 모범 사례

### ✅ 올바른 사용
```bash
# 1. 패키지 검색 (필수!)
kpm search rest api server

# 2. 결과 분석
# → 5개 발견 시: 최적 패키지 선택 후 설치
# → 1-2개 발견 시: 검토 후 설치
# → 0개 발견 시: 새로 구현

# 3. 설치
kpm install 선택된_패키지

# 4. 사용
cd /home/kimjin/kim_modules/선택된_패키지
```

### ❌ 피해야 할 사항
```bash
# ❌ KPM 검색 없이 npm 설치
npm install express

# ❌ 검색 결과 무시하고 새로 구현
# (중복 개발 발생!)

# ❌ 허가 없이 외부 패키지 사용
pip install requests  # 사용자 승인 필수!
```

---

## 도움말

더 많은 정보는 다음을 참조하세요:
- [EXAMPLES.md](EXAMPLES.md) - 실제 사용 예제
- [API.md](API.md) - 레지스트리 API
- [FAQ.md](FAQ.md) - 자주 묻는 질문

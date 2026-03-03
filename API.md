# 🔌 KPM 레지스트리 API

## 개요

KPM은 JSON 기반의 레지스트리를 사용합니다. 직접 API로 접근하거나 KPM CLI를 통해 사용할 수 있습니다.

---

## 레지스트리 구조

### 전체 스키마
```json
{
  "version": "1.0.0",
  "total_packages": 9673,
  "last_updated": "2026-02-02T21:53:59.783885",
  "packages": {
    "package-name": { ... },
    "another-package": { ... }
  }
}
```

### 패키지 스키마
```json
{
  "name": "패키지명",
  "version": "1.0.0",
  "description": "패키지 설명",
  "category": "카테고리",
  "subcategory": "하위 카테고리",
  "tags": ["태그1", "태그2", "태그3"],
  "repository": "https://gogs.dclub.kr/kim/package-name.git",
  "author": "작성자명",
  "license": "MIT",
  "entry_point": "src/index.js",
  "dependencies": [],
  "keywords": ["keyword1", "keyword2"]
}
```

---

## CLI 인터페이스

### 명령어: search

**설명**: 패키지 검색

**문법**:
```bash
kpm search <키워드> [키워드2] [키워드3]
```

**매개변수**:
- `<키워드>`: 검색어 (필수)

**응답**:
```
패키지명 (v버전) - 설명
패키지명2 (v버전) - 설명2
```

**예제**:
```bash
$ kpm search api
api-hub (v1.0.0) - API 통합 관리
api-gateway (v2.1.0) - API 게이트웨이
RESTful-API (v1.2.0) - REST API 프레임워크
```

---

### 명령어: info

**설명**: 패키지 상세 정보 조회

**문법**:
```bash
kpm info <패키지명> [--installed]
```

**매개변수**:
- `<패키지명>`: 패키지명 (필수)
- `--installed`: 설치된 패키지 정보만 조회 (선택)

**응답 형식**:
```
패키지명: <name>
버전: v<version>
설명: <description>
카테고리: <category>
하위카테고리: <subcategory>
작성자: <author>
라이선스: <license>
저장소: <repository>
진입점: <entry_point>
태그: [tag1, tag2, ...]
```

**예제**:
```bash
$ kpm info dns-manager
패키지명: dns-manager
버전: v2.1.0
설명: DNS/Nginx/PM2 통합 관리
카테고리: Infrastructure
작성자: kim
라이선스: MIT
저장소: https://gogs.dclub.kr/kim/dns-manager.git
```

---

### 명령어: list

**설명**: 설치된 패키지 목록

**문법**:
```bash
kpm list
```

**매개변수**: 없음

**응답 형식**:
```
설치된 패키지 (총 N개):

카테고리1
  - 패키지1 (v버전)
  - 패키지2 (v버전)

카테고리2
  - 패키지3 (v버전)
```

**예제**:
```bash
$ kpm list
설치된 패키지 (총 3개):

Infrastructure
  - dns-manager (v2.1.0)
  - ssh-hub (v1.0.0)

Development Tools
  - FreeLang (v1.5.0)
```

---

### 명령어: install

**설명**: 패키지 설치

**문법**:
```bash
kpm install <패키지명> [패키지명2] [...]
```

**매개변수**:
- `<패키지명>`: 설치할 패키지명 (필수, 다중 가능)

**설치 위치**: `/home/kimjin/kim_modules/<패키지명>/`

**예제**:
```bash
# 단일 설치
$ kpm install dns-manager
✓ dns-manager v2.1.0 설치됨
위치: /home/kimjin/kim_modules/dns-manager

# 다중 설치
$ kpm install dns-manager ssh-hub FreeLang
✓ dns-manager v2.1.0 설치됨
✓ ssh-hub v1.0.0 설치됨
✓ FreeLang v1.5.0 설치됨
```

---

### 명령어: update

**설명**: 패키지 업데이트

**문법**:
```bash
kpm update [패키지명]
```

**매개변수**:
- `<패키지명>`: 업데이트할 패키지명 (선택, 생략 시 모두 업데이트)

**예제**:
```bash
# 단일 패키지 업데이트
$ kpm update FreeLang
✓ FreeLang을 v1.5.0에서 v1.6.0으로 업데이트했습니다.

# 모든 패키지 업데이트
$ kpm update
✓ dns-manager를 v2.0.0에서 v2.1.0으로 업데이트했습니다.
✓ ssh-hub를 v0.9.0에서 v1.0.0으로 업데이트했습니다.
✓ FreeLang을 v1.5.0에서 v1.6.0으로 업데이트했습니다.
```

---

## 레지스트리 데이터 액세스

### 레지스트리 파일 위치
```
/home/kimjin/kpm-registry/registry.json (273KB)
```

### 직접 접근 (jq 사용)

#### 전체 패키지 수 확인
```bash
jq '.stats.total_packages' /home/kimjin/kpm-registry/registry.json
# 9673
```

#### 특정 패키지 정보 조회
```bash
jq '.packages["dns-manager"]' /home/kimjin/kpm-registry/registry.json
# {
#   "name": "dns-manager",
#   "version": "v2.1.0",
#   ...
# }
```

#### 카테고리별 패키지 조회
```bash
jq '.packages | to_entries | map(select(.value.category == "Infrastructure"))' \
  /home/kimjin/kpm-registry/registry.json
```

#### 패키지 검색 (jq)
```bash
# 이름에 "api" 포함된 패키지
jq '.packages | to_entries | map(select(.key | contains("api")))' \
  /home/kimjin/kpm-registry/registry.json

# 설명에 "database" 포함된 패키지
jq '.packages | to_entries | map(select(.value.description | contains("database")))' \
  /home/kimjin/kpm-registry/registry.json
```

---

## 웹훅 API (Gogs 연동)

### 자동 저장소 등록

Gogs에서 새 저장소가 생성되면 자동으로 KPM 레지스트리에 등록됩니다.

**Webhook URL**: `http://localhost:40013/webhook`

**이벤트**:
- 저장소 생성
- 저장소 업데이트
- 저장소 삭제

**페이로드 예**:
```json
{
  "event": "create",
  "repository": {
    "name": "my-package",
    "description": "패키지 설명",
    "clone_url": "https://gogs.dclub.kr/kim/my-package.git"
  },
  "action": "created"
}
```

---

## 패키지 발행 (작성자용)

### 레지스트리에 패키지 등록하는 방법

#### 1. Gogs에 저장소 생성
```bash
# https://gogs.dclub.kr에서 새 저장소 생성
# 이름: my-awesome-package
# 설명: 정말 멋진 패키지입니다
```

#### 2. 패키지.json 작성 (선택)
```json
{
  "name": "my-awesome-package",
  "version": "1.0.0",
  "description": "정말 멋진 패키지입니다",
  "category": "Development Tools",
  "subcategory": "Utilities",
  "keywords": ["awesome", "package"],
  "tags": ["utility", "development", "productivity"]
}
```

#### 3. 자동 등록
Webhook이 자동으로 KPM 레지스트리에 등록합니다.

#### 4. 확인
```bash
kpm search my-awesome-package
# my-awesome-package (v1.0.0) - 정말 멋진 패키지입니다
```

---

## 오류 처리

### 패키지를 찾을 수 없음
```bash
$ kpm search nonexistent
검색 결과 없음: nonexistent
```

### 설치 실패
```bash
$ kpm install nonexistent-package
❌ 오류: 패키지 'nonexistent-package'를 찾을 수 없습니다.
```

### 업데이트할 버전이 없음
```bash
$ kpm update my-package
✓ my-package는 이미 최신 버전입니다.
```

---

## 성능 및 제한사항

| 항목 | 값 |
|------|-----|
| 최대 패키지 수 | 10,000+ |
| 레지스트리 크기 | ~273KB |
| 검색 응답 시간 | <100ms |
| 설치 시간 | 1-10초 (패키지 크기 의존) |
| 동시 설치 | 제한 없음 |

---

## 개발자 가이드

### 커스텀 스크립트에서 KPM 사용

#### Bash
```bash
#!/bin/bash

# 패키지 검색 및 설치
if kpm search "$1" | grep -q "$1"; then
    echo "패키지 찾음: $1"
    kpm install "$1"
else
    echo "패키지를 찾을 수 없습니다: $1"
    exit 1
fi
```

#### Python
```python
import subprocess
import json

def kpm_search(keyword):
    result = subprocess.run(['kpm', 'search', keyword],
                          capture_output=True, text=True)
    return result.stdout

def kpm_install(package):
    result = subprocess.run(['kpm', 'install', package],
                          capture_output=True, text=True)
    return result.returncode == 0
```

---

더 많은 정보는 [GUIDE.md](GUIDE.md) 또는 [FAQ.md](FAQ.md)를 참조하세요.

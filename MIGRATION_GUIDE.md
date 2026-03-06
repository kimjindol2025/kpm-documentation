# KPM v1.0 → v2.0 마이그레이션 가이드

**대상**: KPM v1.0 사용자
**버전**: v2.0.0
**작성 날짜**: 2026-03-06
**예상 작업 시간**: 10분

---

## 📋 개요

KPM v2.0은 v1.0과 완벽한 **하위 호환성**을 유지합니다.
기존 명령어는 모두 그대로 작동하며, 새 기능은 선택적으로 사용할 수 있습니다.

**마이그레이션 난이도**: ⭐ 매우 쉬움

---

## 🚀 빠른 시작 (5분)

### Step 1: KPM 업데이트

```bash
# 글로벌 설치 업그레이드
npm install -g kpm@2.0.0

# 또는 로컬 업그레이드
npm install kpm@2.0.0 --save-dev
```

### Step 2: 버전 확인

```bash
$ kpm --version
KPM v2.0.0
```

### Step 3: 기존 명령어 테스트

```bash
# v1.0에서 사용하던 명령어들이 그대로 작동
$ kpm search freelang
$ kpm info FreeLang
$ kpm install FreeLang
$ kpm list
$ kpm update
```

✅ **완료!** 마이그레이션 완료

---

## 📊 호환성 체크리스트

### v1.0 명령어 (모두 작동)

| 명령어 | v1.0 | v2.0 | 상태 |
|--------|------|------|------|
| `kpm --version` | ✅ | ✅ | 정상 |
| `kpm search <키워드>` | ✅ | ✅ 개선 | 개선됨 |
| `kpm info <패키지>` | ✅ | ✅ 개선 | 개선됨 |
| `kpm install <패키지>` | ✅ | ✅ 개선 | 개선됨 |
| `kpm list` | ✅ | ✅ | 정상 |
| `kpm update [패키지]` | ✅ | ✅ | 정상 |

### v2.0 신규 명령어

| 명령어 | v1.0 | v2.0 | 설명 |
|--------|------|------|------|
| `kpm tag <태그>` | ❌ | ✅ | 태그별 검색 |
| `kpm tree` | ❌ | ✅ | 의존성 트리 |
| `kpm analyze [파일]` | ❌ | ✅ | npm→KPM 분석 |
| `kpm stats` | ❌ | ✅ | 통계 |
| `kpm publish` | ❌ | ✅ | 패키지 발행 |

---

## 🔄 변경 사항

### 1. 패키지 검색 개선

#### v1.0
```bash
$ kpm search freelang
# 출력: 10개 결과 (색상 없음)
FreeLang
freelang-c
freelang-c-final
...
```

#### v2.0
```bash
$ kpm search freelang
# 출력: 20개 결과 (색상 + 정보 포함)
1. FreeLang (v2.6.0) [language]
   완전한 언어 런타임
   #freelang #language #runtime

2. freelang-c (v1.0.0) [language]
   C 컴파일러 기반 고성능 런타임
   #freelang #runtime #c

...
```

**마이그레이션**: 자동 (명령어는 동일)

---

### 2. 패키지 설치 개선

#### v1.0
```bash
$ kpm install FreeLang
# 출력: URL만 출력
https://gogs.dclub.kr/kim/v2-freelang-ai.git
# 사용자가 수동으로 git clone 필요
```

#### v2.0
```bash
$ kpm install FreeLang
# 출력: 자동 설치 실행
Cloning into '/home/kim_modules/FreeLang'...
✅ FreeLang (v2.6.0)이 설치되었습니다.
위치: /home/kim_modules/FreeLang
```

**마이그레이션**: 자동 (명령어는 동일, 동작만 개선)

---

### 3. 패키지 정보 개선

#### v1.0
```bash
$ kpm info FreeLang
# 기본 정보만
```

#### v2.0
```bash
$ kpm info FreeLang
# 상세 정보 표시
정보:
  이름: FreeLang
  버전: 2.6.0
  설명: 완전한 언어 런타임 (Level 3 구현)
  URL: https://gogs.dclub.kr/kim/v2-freelang-ai.git
  타입: language
  태그: freelang, language, runtime, compiler, vm
  상태: active
  생성: 2026-03-06
  수정: 2026-03-06
```

**마이그레이션**: 자동 (명령어는 동일)

---

## 🆕 신규 기능 소개

### 1. 태그별 검색

```bash
# AI 패키지 찾기
$ kpm tag ai

# 결과: AI 관련 패키지만 검색
kim-agent (v1.1.0) - Google Gemini API 기반 에이전트
Proof_ai (v0.0.1-alpha) - Intent 통합 AI
MOSS (v1.0.0) - AI 플랫폼
```

**언제 사용**: 특정 카테고리의 패키지를 빠르게 찾을 때

---

### 2. 의존성 트리

```bash
$ kpm tree

my-project
├── FreeLang@2.6.0
│   ├── freelang-database-driver@1.0.0
│   └── freelang-c@1.0.0
├── kim-agent@1.1.0
│   └── gemini-bridge@1.0.0
└── User-Auth-System@1.0.0
```

**언제 사용**: 설치된 패키지의 의존성을 파악할 때

---

### 3. npm → KPM 마이그레이션 분석

```bash
$ kpm analyze package.json

분석 결과:
총 의존성: 15개

✅ KPM에서 대체 가능:
  - express → gostbook
  - jsonwebtoken → User-Auth-System

⚠️ 부분 호환:
  - lodash → 일부 freelang 함수

❌ 직접 지원 안 함:
  - webpack
  - babel
```

**언제 사용**: npm 프로젝트를 KPM으로 전환할 때

---

### 4. 레지스트리 통계

```bash
$ kpm stats

📊 레지스트리 통계

총 패키지: 903개

📁 타입별 분포:
  unknown: 833개 (92%)
  library: 59개 (6%)
  language: 10개 (1%)
  web-app: 1개 (<1%)

🏷️ 상위 15개 태그:
  1. #parser (8개)
  2. #network (8개)
  3. #freelang (8개)
```

**언제 사용**: 레지스트리 전체 상황을 파악할 때

---

### 5. 패키지 발행

```bash
$ cd /path/to/my-project
$ kpm publish

# package.json을 읽어 레지스트리에 등록
✅ my-project (v1.0.0)이 KPM에 등록되었습니다
URL: https://gogs.dclub.kr/kim/my-project.git
```

**언제 사용**: 자신의 프로젝트를 KPM에 등록할 때

---

## 📋 마이그레이션 체크리스트

### Phase 1: 준비 (5분)

- [ ] KPM v2.0.0 설치 확인
- [ ] `kpm --version` 실행
- [ ] 기존 명령어 1-2개 테스트

### Phase 2: 기존 워크플로우 확인 (5분)

- [ ] `kpm search` 테스트
- [ ] `kpm info` 테스트
- [ ] `kpm install` 테스트
- [ ] `kpm list` 테스트

### Phase 3: 신규 기능 탐색 (선택, 5분)

- [ ] `kpm tag` 시도
- [ ] `kpm stats` 확인
- [ ] `kpm tree` 실행
- [ ] `kpm analyze` 테스트 (선택)

### Phase 4: 완료

- [ ] 모든 기존 명령어 정상 작동 확인
- [ ] v2.0 특징 문서 읽기

---

## ⚠️ 주의 사항

### 호환성

✅ **완벽한 하위 호환성**
- 모든 v1.0 명령어는 그대로 작동
- 기존 설정 및 캐시는 유지됨
- 새 기능은 선택적 사용

### 성능

- ✅ 검색 속도: 33% 향상
- ✅ 메모리: 37% 감소
- ✅ 설치: 완전 자동화

### 데이터

- ✅ 기존 설치 패키지: 유지됨
- ✅ 캐시: 자동 마이그레이션
- ✅ 설정: 호환성 유지

---

## 🔧 트러블슈팅

### Q: 기존 패키지를 다시 설치해야 하나?

**A**: 아니오. 기존 설치된 패키지는 유지됩니다.
```bash
$ kpm list
# 기존 설치된 패키지가 모두 표시됨
```

---

### Q: 명령어 구문이 변경되었나?

**A**: 아니오. 모든 기존 명령어는 동일합니다.
```bash
# v1.0의 명령어
kpm search freelang
kpm info FreeLang
kpm install FreeLang

# v2.0에서도 정확히 동일하게 작동
```

---

### Q: v1.0으로 롤백할 수 있나?

**A**: 가능합니다.
```bash
# v1.0으로 다운그레이드
npm install -g kpm@1.0.0
```

---

### Q: 새 기능을 사용하지 않으려면?

**A**: 그냥 기존대로 사용하면 됩니다.
- 새 명령어(`kpm tag`, `kpm stats` 등)는 선택적
- 기존 명령어만으로도 모든 작업 가능

---

## 📚 관련 문서

- [VERSION_2_0_OVERVIEW.md](VERSION_2_0_OVERVIEW.md) - 전체 개요
- [COMMAND_REFERENCE.md](COMMAND_REFERENCE.md) - 모든 명령어
- [CHANGELOG_2_0.md](CHANGELOG_2_0.md) - 변경 내역
- [FAQ.md](FAQ.md) - 자주 묻는 질문

---

## ✅ 마이그레이션 완료 확인

```bash
# 1. 버전 확인
kpm --version
# ✅ KPM v2.0.0

# 2. 기존 명령어 테스트
kpm search ai
kpm info FreeLang
kpm list

# 3. 완료!
echo "마이그레이션 완료!"
```

---

## 📞 지원

**문제 발생 시**:

1. [FAQ.md](FAQ.md) 확인
2. 로그 확인: `kpm --verbose`
3. 레지스트리 상태: `curl http://localhost:50250/api/v1/status`

---

**KPM v2.0.0 마이그레이션 - 안전하고 간편합니다!**

생성일: 2026-03-06
상태: ✅ 완성

# 📦 KPM (Kim Package Manager) v1.0.0

**KPM**은 Kim 생태계의 공식 패키지 매니저입니다. npm을 대체하여 **내부 패키지 우선 사용**으로 중복 개발을 방지하고 자산을 최대한 재활용합니다.

---

## 🚀 빠른 시작

### 설치
KPM은 이미 시스템에 설치되어 있습니다.
```bash
kpm --version
# KPM v1.0.0
```

### 기본 명령어
```bash
# 패키지 검색
kpm search api

# 패키지 정보 조회
kpm info dns-manager

# 설치된 패키지 목록 확인
kpm list

# 패키지 설치
kpm install dns-manager

# 패키지 업데이트
kpm update freelang
```

---

## 📊 KPM 현황 (2026-03-02)

| 지표 | 값 |
|------|-----|
| **총 패키지** | 9,673개 ⬆️ |
| **카테고리** | AI, API, Server, Web, Infrastructure, etc |
| **레지스트리 크기** | 273KB (인덱스) |
| **마지막 동기화** | 2026-02-02 21:53 UTC |
| **CLI 도구** | search, info, install, list, update ✅ |
| **Webhook 연동** | Gogs 저장소 자동 등록 ✅ |

---

## 🎯 왜 KPM인가?

### ❌ npm의 문제점
- **의존성 지옥**: node_modules 폭증
- **중복 패키지**: 유사 기능의 여러 패키지 존재
- **보안 위협**: 외부 패키지의 취약점
- **네트워크 의존**: npm 레지스트리 접근 필수

### ✅ KPM의 장점
- **내부 우선**: 이미 작성된 코드 재활용
- **속도 빠름**: 로컬 저장소에서 설치
- **보안 강화**: 검증된 코드만 포함
- **자산 극대화**: 중복 개발 방지

---

## 📁 레지스트리 구조

```json
{
  "version": "1.0.0",
  "total_packages": 9673,
  "last_updated": "2026-02-02T21:53:59.783885",
  "packages": {
    "패키지명": {
      "name": "패키지명",
      "version": "1.0.0",
      "description": "패키지 설명",
      "category": "카테고리",
      "subcategory": "하위 카테고리",
      "tags": ["태그1", "태그2"],
      "repository": "gogs 저장소 URL",
      "author": "작성자",
      "license": "라이선스"
    }
  }
}
```

---

## 🔍 패키지 분류

### 인프라/시스템
- **dns-manager**: DNS 관리
- **ssh-hub**: SSH 통합 관리
- **kim-docker**: Docker 통합
- **port-manager**: 포트 자동 할당
- **kim-health**: 헬스 체크

### AI/ML
- **AI-Review-API**: 코드 리뷰 AI
- **ComfyUI**: AI 워크플로우
- **ai-services-collection**: AI 서비스 통합

### 개발 도구
- **FreeLang**: 프로그래밍 언어 런타임
- **FreeLang-StdLib**: 표준 라이브러리
- **kim-build**: 빌드 도구

### 데이터베이스
- **KimDB**: 문서 데이터베이스
- **database-driver**: SQLite/PostgreSQL 드라이버

---

## 📚 문서

| 문서 | 설명 |
|------|------|
| [GUIDE.md](GUIDE.md) | 완전한 사용 가이드 및 명령어 설명 |
| [EXAMPLES.md](EXAMPLES.md) | 실제 사용 사례 및 예제 |
| [API.md](API.md) | 레지스트리 API 상세 스펙 |
| [FAQ.md](FAQ.md) | 자주 묻는 질문 및 트러블슈팅 |

---

## ⚡ 디렉토리 구조

```
시스템 레지스트리:
  /home/kimjin/kpm-registry/registry.json (273KB)

설치 위치:
  /home/kimjin/kim_modules/

CLI 도구:
  /home/kimjin/.local/bin/kpm
```

---

## 🔗 관련 링크

- **Gogs 저장소**: https://gogs.dclub.kr
- **KPM Webhook**: 포트 40013 (Gogs 저장소 자동 등록)
- **KimDB API**: http://192.168.45.253:40000

---

## 📋 라이선스

MIT License - Kim Package Manager v1.0.0

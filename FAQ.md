# ❓ KPM FAQ (자주 묻는 질문)

## 목차
1. [설치 및 설정](#설치-및-설정)
2. [패키지 검색 및 설치](#패키지-검색-및-설치)
3. [패키지 관리](#패키지-관리)
4. [트러블슈팅](#트러블슈팅)
5. [고급 기능](#고급-기능)

---

## 설치 및 설정

### Q1: KPM이 설치되어 있는지 확인하려면?
```bash
which kpm
# /home/kimjin/.local/bin/kpm

kpm --version
# KPM v1.0.0
```
설치되어 있으면 경로와 버전이 표시됩니다.

---

### Q2: KPM을 다시 설치하려면?
```bash
# 현재 KPM 경로 확인
which kpm
# /home/kimjin/.local/bin/kpm

# 복제 및 재설치 (관리자에게 문의)
# KPM은 시스템에 사전 설치되어 있습니다
```

---

### Q3: KPM의 설정 파일은 어디에 있나요?
```
레지스트리: /home/kimjin/kpm-registry/registry.json
설치 위치: /home/kimjin/kim_modules/
CLI 도구: /home/kimjin/.local/bin/kpm
```

---

### Q4: KPM 환경 변수를 변경할 수 있나요?
```bash
# 레지스트리 경로 변경
export KPM_REGISTRY_PATH="/custom/path/registry.json"

# 모듈 설치 경로 변경
export KPM_MODULES_PATH="/custom/path/modules"

# 확인
echo $KPM_REGISTRY_PATH
echo $KPM_MODULES_PATH
```
**주의**: 기본 경로 변경 권장하지 않음

---

## 패키지 검색 및 설치

### Q5: 어떤 패키지들이 있나요?
```bash
kpm list

# 또는 총 패키지 수 확인
jq '.stats.total_packages' /home/kimjin/kpm-registry/registry.json
# 9673
```

---

### Q6: 패키지를 검색하는 가장 좋은 방법은?
**3단계 검색법**:
```bash
# 1단계: 첫 번째 키워드
kpm search api

# 2단계: 두 번째 키워드
kpm search server

# 3단계: 세 번째 키워드
kpm search rest

# 결과: 공통으로 나타난 패키지가 최적화된 선택
```

---

### Q7: 검색 결과가 나오지 않으면?
```bash
# 1. 다른 키워드로 시도
kpm search database
kpm search db
kpm search sql

# 2. 정확한 패키지명이 알려졌다면
kpm info exact-package-name

# 3. 모든 패키지 목록 확인
kpm list

# 4. 완전히 새로운 패키지는
# "이 기능의 패키지가 없으므로 새로 구현하겠습니다"
```

---

### Q8: 패키지명을 정확히 모르면?
```bash
# 부분적으로 검색
kpm search "api" | grep -i hub
# api-hub (v1.0.0) - API 통합 관리

# 정보 조회
kpm info api-hub
```

---

### Q9: 설치할 패키지를 선택하는 방법은?
```
검색 결과가:
- 3개 이상 → 가장 오래되고 많이 사용되는 것 선택 ✅
- 1-2개   → 상세 정보 확인 후 결정 ✅
- 0개     → 새로 구현 (사용자 승인 필수)
```

---

### Q10: 설치 후 패키지 위치는?
```bash
/home/kimjin/kim_modules/패키지명/

예)
/home/kimjin/kim_modules/dns-manager/
/home/kimjin/kim_modules/FreeLang/
/home/kimjin/kim_modules/api-hub/
```

---

## 패키지 관리

### Q11: 설치된 패키지 목록을 보려면?
```bash
kpm list

# 특정 패키지 확인
kpm list | grep dns

# 카테고리별로 정렬됨
# Infrastructure, Development Tools, AI/ML, etc.
```

---

### Q12: 패키지를 업데이트하려면?
```bash
# 단일 패키지 업데이트
kpm update FreeLang
# FreeLang을 v1.5.0에서 v1.6.0으로 업데이트

# 모든 패키지 업데이트
kpm update

# 확인
kpm list | grep FreeLang
# FreeLang (v1.6.0)
```

---

### Q13: 패키지를 제거할 수 있나요?
```bash
# KPM에서는 제거 명령이 없음
# 수동으로 삭제 가능:

rm -rf /home/kimjin/kim_modules/패키지명/

# 확인
ls /home/kimjin/kim_modules/
```

---

### Q14: 여러 버전의 같은 패키지를 설치할 수 있나요?
```bash
# KPM은 기본적으로 최신 버전만 지원
# 하지만 다른 디렉토리에 수동 설치 가능:

mkdir -p /home/kimjin/kim_modules/FreeLang-legacy
# 레거시 버전을 다른 폴더에 설치
```

---

## 트러블슈팅

### Q15: "패키지를 찾을 수 없습니다" 오류가 발생하면?
```bash
# 1. 패키지명 확인
kpm list | grep package-name

# 2. 정확한 패키지명으로 검색
kpm info correct-package-name

# 3. 잘못된 패키지명인지 확인
kpm search keyword
```

---

### Q16: 설치가 실패하면?
```bash
# 1. 레지스트리 확인
jq '.packages | length' /home/kimjin/kpm-registry/registry.json

# 2. 패키지 존재 여부 확인
kpm info package-name

# 3. 디스크 공간 확인
df -h /home/kimjin/kim_modules/

# 4. 수동으로 레지스트리 동기화
# (관리자에게 문의)
```

---

### Q17: 패키지를 사용할 수 없으면?
```bash
# 1. 설치 위치 확인
ls /home/kimjin/kim_modules/package-name/

# 2. 실행 권한 확인
ls -la /home/kimjin/kim_modules/package-name/bin/

# 3. 경로 추가
export PATH="/home/kimjin/kim_modules/package-name/bin:$PATH"

# 4. 문서 확인
cat /home/kimjin/kim_modules/package-name/README.md
```

---

### Q18: 네트워크 오류가 발생하면?
```bash
# KPM은 로컬 레지스트리 사용
# 네트워크 불필요!

# 저장소만 필요 시:
git clone https://gogs.dclub.kr/kim/package-name.git
```

---

### Q19: 권한 오류가 발생하면?
```bash
# 대부분의 경우 아래 경로 권한 확인:
ls -ld /home/kimjin/kim_modules/
ls -ld /home/kimjin/kpm-registry/

# 설치 폴더 생성 권한 확인
touch /home/kimjin/kim_modules/.test
rm /home/kimjin/kim_modules/.test
```

---

### Q20: KPM 레지스트리가 오래되었으면?
```bash
# 레지스트리 확인
jq '.last_updated' /home/kimjin/kpm-registry/registry.json
# "2026-02-02T21:53:59.783885"

# 업데이트 필요 시 (관리자 문의)
# Webhook을 통해 자동 동기화됨
```

---

## 고급 기능

### Q21: KPM 레지스트리를 직접 쿼리할 수 있나요?
```bash
# jq를 사용한 고급 쿼리

# 특정 카테고리의 모든 패키지
jq '.packages | to_entries | map(select(.value.category == "Infrastructure"))' \
  /home/kimjin/kpm-registry/registry.json

# AI/ML 카테고리
jq '.packages | to_entries | map(select(.value.category == "AI"))' \
  /home/kimjin/kpm-registry/registry.json

# 태그로 검색
jq '.packages | to_entries | map(select(.value.tags | contains(["database"])))' \
  /home/kimjin/kpm-registry/registry.json
```

---

### Q22: 패키지 정보를 JSON으로 얻을 수 있나요?
```bash
jq '.packages["package-name"]' /home/kimjin/kpm-registry/registry.json

# 예
jq '.packages["dns-manager"]' /home/kimjin/kpm-registry/registry.json
```

---

### Q23: 내 패키지를 KPM에 등록하려면?
```bash
# 1. Gogs에 저장소 생성
https://gogs.dclub.kr/new

# 2. package.json 추가 (선택)
{
  "name": "my-package",
  "version": "1.0.0",
  "description": "설명",
  "category": "카테고리",
  "tags": ["tag1", "tag2"]
}

# 3. 푸시
git push origin master

# 4. 자동으로 KPM에 등록됨 (Webhook)
kpm search my-package
# my-package (v1.0.0) - 설명
```

---

### Q24: 패키지 의존성을 어떻게 관리하나요?
```bash
# 각 패키지의 package.json에 명시
# KPM은 의존성을 자동으로 설치하지 않음

# 수동으로 필요한 패키지 설치
kpm install dependency-package
```

---

### Q25: KPM으로 프로젝트를 패키징할 수 있나요?
```bash
# 1. 프로젝트를 Gogs에 저장소로 생성
# 2. README.md와 package.json 작성
# 3. 푸시하면 자동으로 KPM에 등록됨

# 예
git remote add origin https://gogs.dclub.kr/kim/my-awesome-tool.git
git push -u origin master

# 확인
kpm search my-awesome-tool
```

---

## 피해야 할 것들

### ❌ 하지 말아야 할 것

```bash
# ❌ KPM 검색 없이 npm 사용
npm install express

# ❌ 레지스트리 수동 수정
vi /home/kimjin/kpm-registry/registry.json

# ❌ 설치 폴더 외부에서 패키지 사용
# (항상 /home/kimjin/kim_modules/에서 사용)

# ❌ 여러 버전의 같은 패키지 설치 후 충돌
# (명확한 경로 관리 필수)
```

---

## 도움말

- 📖 [GUIDE.md](GUIDE.md) - 상세 사용 가이드
- 💡 [EXAMPLES.md](EXAMPLES.md) - 실제 예제
- 🔌 [API.md](API.md) - API 스펙

문제가 해결되지 않으면 관리자에게 문의하세요.

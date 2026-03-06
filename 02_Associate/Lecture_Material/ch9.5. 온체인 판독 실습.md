# 9.5강. 온체인 판독 실습: Explorer로 근거를 찾고 구조를 판정하는 방법

## 1) 이 강의의 한 문장
**Associate 분석의 품질은 "얼마나 많이 아는가"보다 "온체인 근거를 얼마나 정확히 찾고 판독하는가"에서 결정된다.**

## 2) 강의 목표 (수강생이 끝나고 할 수 있어야 하는 것)
이 강의를 마치면 수강생은:

1. 블록 익스플로러에서 서비스의 핵심 컨트랙트 주소를 식별할 수 있다.
2. 권한 구조(Owner/Admin/Role/Multisig/Timelock) 관련 근거를 찾아 판정할 수 있다.
3. Proxy/Upgrade 구조와 업그레이드 권한 경로를 판독할 수 있다.
4. 입금-보관-출금 자금 흐름을 트랜잭션 단위로 추적할 수 있다.
5. 판독 결과를 Evidence Log로 정리해 ACM/FFRS에 연결할 수 있다.

---

## 3) 왜 이 실습이 필요한가: Basic에서 Associate로 넘어가는 실제 간극

Basic에서 우리는 개념을 이해했다.
Associate에서는 시나리오를 평가하고 개선 방향을 제시해야 한다.

문제는 여기서 생긴다.

- 개념은 아는데 근거를 못 찾는다.
- 느낌으로 취약점을 말하지만, "어떤 주소/함수/트랜잭션이 근거인지" 제시하지 못한다.
- 결과적으로 답안이 맞아도 설득력이 약하다.

핵심 문장:
> **Associate 분석은 주장 게임이 아니라 증거 게임이다.**

---

## 4) 온체인 판독 5단계 프레임

```
Step 1. 대상 식별
  - 서비스의 핵심 컨트랙트 목록 수집
  - Proxy/Implementation/Token/Vault/Treasury 분류

Step 2. 권한 판독
  - owner(), DEFAULT_ADMIN_ROLE, 역할 관리자 확인
  - 관리자 주소가 EOA인지 Multisig인지 확인

Step 3. 업그레이드 판독
  - Proxy 여부 확인
  - Implementation 변경 이력, 업그레이드 주체, 지연(Timelock) 확인

Step 4. 자금 흐름 판독
  - 입금/보관/출금 경로를 대표 트랜잭션으로 추적
  - 숨겨진 경로(수수료, 관리자 인출, 외부 의존) 확인

Step 5. 증거 정리
  - Evidence Log 작성
  - ACM/FFRS 항목으로 변환
```

---

## 5) 실습 1: 권한 구조 판독

### A. 최소 확인 질문

1. 이 컨트랙트의 최고 권한자는 누구인가?
2. 권한이 단일 주소에 집중되어 있는가?
3. 역할이 분리되어 있는가? (Admin/Upgrader/Pauser/FeeCollector 등)
4. 관리자 주소가 Multisig인가? 임계값(M-of-N)은?

### B. Explorer에서 보는 위치

- `Contract` 탭: `owner()`, `hasRole()`, `getRoleAdmin()`
- `Read as Proxy`/`Read Contract`: Proxy 환경의 읽기 함수
- `Events` 탭: `RoleGranted`, `RoleRevoked`, `OwnershipTransferred`

### C. 판독 예시

```
관찰:
- owner() = 0xAAA...111
- 0xAAA...111은 1개 EOA 주소
- withdraw(), setFee(), pause() 모두 onlyOwner

판정:
- 단일 관리자 집중 위험 (ch14 유형 1)

개선 방향:
- owner를 Multisig(예: 3-of-5)로 이전
- 기능별 역할 분리(RBAC) 적용
```

---

## 6) 실습 2: Proxy/Upgrade 판독

### A. 최소 확인 질문

1. 대상 컨트랙트는 Proxy인가?
2. 현재 Implementation 주소는 무엇인가?
3. 과거 Implementation 변경 이력이 있는가?
4. upgrade 권한은 누가 갖는가?
5. 업그레이드 실행에 지연(Timelock)이 있는가?

### B. Explorer에서 보는 위치

- `Contract` 메타 정보: Proxy 여부 표시
- `Read as Proxy`: implementation/admin 조회
- `Events`: `Upgraded`, `AdminChanged`
- 트랜잭션 히스토리: 실제 `upgradeTo`/`upgradeToAndCall` 호출 주체

### C. 판독 예시

```
관찰:
- UUPS Proxy 사용
- 최근 90일 내 Implementation 2회 변경
- 변경 트랜잭션 호출자 = Admin Multisig
- 예약/지연 트랜잭션 흔적 없음

판정:
- 업그레이드 권한은 분산되어 있으나 사전 공지 시간 부족

개선 방향:
- upgrade 경로에 Timelock(예: 48~72시간) 적용
- 긴급 패치 경로와 일반 경로를 분리
```

---

## 7) 실습 3: 자금 흐름 판독

### A. 최소 확인 질문

1. 사용자의 자산은 어떤 함수로 들어오는가? (deposit/addLiquidity/mint 등)
2. 들어온 자산은 어디에 보관되는가? (Vault, Pool, 외부 프로토콜)
3. 출금/상환 시 어떤 순서로 자산이 되돌아오는가?
4. 수수료/재무금(Treasury) 경로는 어디인가?
5. 관리자 인출 경로 또는 비상 함수가 있는가?

### B. 트랜잭션 판독 루틴

1. 대표 입금 tx 1건 선택
2. `Logs`에서 토큰 이동(`Transfer`)과 이벤트 확인
3. 내부 트랜잭션(Internal Txns) 확인
4. 대표 출금 tx 1건으로 역방향 검증

### C. 판독 예시

```
입금 tx:
User -> Router approve
User -> Router swap
Router -> Pool transferFrom
Pool -> Treasury fee transfer

출금 tx:
User -> withdraw(shares)
Vault -> External Protocol withdraw
Vault -> User transfer

판정:
- 외부 프로토콜 의존 포함 구조
- Treasury 수수료 경로 존재
```

---

## 8) Evidence Log 템플릿 (실습 표준)

실습 결과는 아래 형식으로 기록한다.

| ID | 구분 | 근거 타입 | 주소/트랜잭션 | 관찰 사실 | 판정 | 연결 산출물 |
|---|---|---|---|---|---|---|
| E-01 | 권한 | owner() 조회 | 0xProxy... | 단일 EOA owner | 단일 관리자 집중 | ACM |
| E-02 | 업그레이드 | Upgraded 이벤트 | tx 0xabc... | 30일 내 2회 업그레이드 | 변경 빈도 높음 | ACM |
| E-03 | 자금흐름 | Transfer 로그 | tx 0xdef... | Treasury로 10% 수수료 이동 | 수수료 경로 확인 | FFRS |

작성 규칙:
- 관찰 사실과 해석을 분리한다.
- 추측 문장은 금지하고, 확인된 사실만 적는다.
- 각 항목을 ACM 또는 FFRS 어디에 연결할지 명시한다.

---

## 9) ACM / FFRS 연결 규칙

### A. ACM으로 가는 근거

- 권한 주체 식별 결과
- 역할 분리 여부
- 업그레이드 권한/지연 통제 여부

예:
```
E-01, E-02 -> ACM의 [역할/권한/통제수준/취약점/개선방향] 컬럼 입력
```

### B. FFRS로 가는 근거

- 입금/보관/출금 구간별 이동 경로
- 외부 의존성, 수수료 경로, 관리자 개입 경로
- 각 구간 리스크 매핑

예:
```
E-03 -> FFRS의 [흐름구간/리스크유형/심각도/개선방향] 입력
```

---

## 10) 실습 체크리스트

실습 제출 전 아래를 확인한다.

1. 컨트랙트 주소 목록이 누락 없이 정리되었는가?
2. 권한 판독 근거(owner/role/event)가 최소 3개 이상 있는가?
3. 업그레이드 권한과 지연 통제 여부를 확인했는가?
4. 입금/보관/출금 대표 트랜잭션을 각각 최소 1건씩 추적했는가?
5. Evidence Log의 모든 항목이 ACM 또는 FFRS와 연결되었는가?

---

## 용어 보충 (Quick Reference)

- **Explorer**: 블록체인 데이터(주소, 트랜잭션, 이벤트, 컨트랙트)를 조회하는 도구.
- **Read Contract**: 상태 조회 함수 호출 영역.
- **Event Log**: 트랜잭션 실행 시 기록된 이벤트 데이터.
- **Internal Transaction**: EVM 실행 중 내부 호출로 발생한 이동 기록.
- **Evidence Log**: 판독 근거를 구조적으로 정리한 기록표.

## 헷갈리는 개념 보충

- "컨트랙트가 Verified"라는 사실은 "안전하다"는 뜻이 아니다.
- Multisig를 쓴다고 자동으로 안전해지지 않는다. 임계값, 서명자 독립성, 운영 절차를 함께 봐야 한다.
- Proxy를 안 쓰는 것이 항상 더 안전한 것도 아니다. 핵심은 변경 가능성과 통제 방식의 균형이다.

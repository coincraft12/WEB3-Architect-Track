# Associate Curriculum (Lecture Note Aligned)

기준 문서:
- `00_Governance/Qualification_Management_and_Operation_Regulations.md`
- `02_Associate/Curriculum/Associate_Blueprint.md`
- `02_Associate/Lecture_Material/*`

---

## 1. 과정 정의

- 등급: Associate
- 응시자격: WEB3 구조 설계자 Basic 취득자
- 검정방법:
  - 필기검정: WEB3 아키텍처 분석
  - 실기검정: 권한 통제 설계 / 자금 흐름 기반 리스크 평가

---

## 2. 학습 목표

- WEB3 서비스/프로젝트의 권한 통제 메커니즘(Multisig/Timelock/RBAC/Proxy)을 이해한다.
- WEB3 서비스 유형별(AMM/Lending/Bridge/Custody) 자산 흐름 구조를 이해한다.
- 서비스 전체 구조를 온보딩·자산흐름·권한/책임 3축으로 분석한다.
- 구조적 취약점 및 운영 리스크를 식별하고 개선 방향을 제시한다.
- Authority Control Matrix와 Fund Flow Risk Sheet를 작성한다.

---

## 3. 커리큘럼 모듈 구조

```
Module 1: WEB3 아키텍처 분석 (필기 대응)
  ├─ [권한 통제 메커니즘] ch1 ~ ch4
  ├─ [자산·자금 흐름 구조] ch5 ~ ch8 (+ ch7.5)
  └─ [서비스 단위 분석 방법론] ch9 ~ ch13 (+ ch9.5, ch13.5)
        │
        ├─→ Module 2: 권한 통제 설계 (실기 1 대응) ch14 ~ ch16
        └─→ Module 3: 자금 흐름 기반 리스크 평가 (실기 2 대응) ch17 ~ ch21 (+ ch20.5, ch21.5)
```

---

## 4. Lecture Note 기반 커리큘럼 맵

### Module 1: WEB3 아키텍처 분석 (필기 대응)

#### [권한 통제 메커니즘]

| 순서 | 강의 노트 | 핵심 역량 | 검정과목 매핑 |
|---|---|---|---|
| 0 | `01_Intro.md` | 과정 관점 정렬 / Basic → Associate 전환 | 공통 |
| 1 | `ch1. Multisig 구조 정의.md` | M-of-N 서명 구조, 키 홀더, 임계값, 트랜잭션 실행 흐름 | WEB3 아키텍처 분석 |
| 2 | `ch2. Timelock 구조 정의.md` | 지연 실행 원리, 취소·실행 메커니즘, Multisig 조합 패턴 | WEB3 아키텍처 분석 |
| 3 | `ch3. 역할 기반 권한 구조(RBAC).md` | onlyOwner 한계, 역할 분리 패턴, DAO 거버넌스 개요 | WEB3 아키텍처 분석 |
| 4 | `ch4. Proxy와 Upgrade 패턴.md` | 컨트랙트 불변성 문제, Proxy 구조 원리, 업그레이드 권한 위험성 | WEB3 아키텍처 분석 |

#### [자산·자금 흐름 구조]

| 순서 | 강의 노트 | 핵심 역량 | 검정과목 매핑 |
|---|---|---|---|
| 5 | `ch5. AMM 구조와 자산 흐름.md` | 유동성 풀, x*y=k, LP 토큰, 스왑 자산 이동 경로 | WEB3 아키텍처 분석 |
| 6 | `ch6. Lending 구조와 자산 흐름.md` | 담보 예치·대출·청산 흐름, LTV, 이자 발생 구조 | WEB3 아키텍처 분석 |
| 7 | `ch7. Bridge 구조와 크로스체인 자산 흐름.md` | Lock-Mint/Burn-Unlock, 검증자 구조, 래핑 토큰 표현 방식 | WEB3 아키텍처 분석 |
| 7.5 | `ch7.5. Canonical Bridge vs Wrapped Bridge 실전 비교.md` | Canonical/Wrapped 브릿지 분류, Challenge Period/Finality/Fast Route 구분, 브릿지 경로별 리스크 판정 | WEB3 아키텍처 분석 |
| 8 | `ch8. Custody 구조와 자산 보관 흐름.md` | Custodial/Non-custodial/MPC 구분, Hot/Cold 구조, 입출금 흐름 | WEB3 아키텍처 분석 |

#### [서비스 단위 분석 방법론]

| 순서 | 강의 노트 | 핵심 역량 | 검정과목 매핑 |
|---|---|---|---|
| 9 | `ch9. 서비스 단위 구조 분석 프레임.md` | 3축 분석(온보딩/자산흐름/권한·책임) 정의, Architecture Read Sheet 개요 | WEB3 아키텍처 분석 |
| 9.5 | `ch9.5. 온체인 판독 실습.md` | Explorer 기반 증거 수집, 권한/업그레이드/자금흐름 판독, Evidence Log 작성 | WEB3 아키텍처 분석 |
| 10 | `ch10. 사용자 온보딩 구조 분석.md` | 지갑연결→Approve→자산진입 단계 분석, 온보딩 취약점 식별 | WEB3 아키텍처 분석 |
| 11 | `ch11. 자산흐름 경로 분석.md` | 진입점→변환→출구 추적, 자산 상태 변환 매핑, 흐름 다이어그램 작성 | WEB3 아키텍처 분석 |
| 12 | `ch12. 권한·책임 구조 분석.md` | 역할 분류, 권한 위임 경로 추적, 책임 경계 식별 | WEB3 아키텍처 분석 |
| 13 | `ch13. 구조적 취약점 및 운영 리스크 식별.md` | 3축 분석 통합 취약점 도출, 구조적 취약점 vs. 운영 리스크 구분 기준 | WEB3 아키텍처 분석 |
| 13.5 | `ch13.5. Attack Surface Mapping for Associate.md` | 위협 모델링 입문, 공격면 식별(6영역), 우선순위 산정, Attack Surface Map 작성 | WEB3 아키텍처 분석 |

---

### Module 2: 권한 통제 설계 (실기 1 대응)

| 순서 | 강의 노트 | 핵심 역량 | 검정과목 매핑 |
|---|---|---|---|
| 14 | `ch14. 권한 통제 취약점 유형.md` | 단일 관리자 집중·무한 Approve·업그레이드 권한 집중·역할 미분리 유형 정의 | 권한 통제 설계 |
| 15 | `ch15. 취약점에서 통제안 도출.md` | 유형별 대응 메커니즘 매핑, 상황별 선택 기준(언제 Multisig/Timelock/RBAC), 개선 방향 제시 형식 | 권한 통제 설계 |
| 16 | `ch16. Authority Control Matrix 정의 및 작성 기준.md` | 매트릭스 구조(역할×권한×통제수준×취약점×개선방향), 작성 방법, 실기 형식 | 권한 통제 설계 |

---

### Module 3: 자금 흐름 기반 리스크 평가 (실기 2 대응)

| 순서 | 강의 노트 | 핵심 역량 | 검정과목 매핑 |
|---|---|---|---|
| 17 | `ch17. 자금 흐름 추적 방법론.md` | 입금→보관→출금 경로 추적 절차, 외부 의존성 포함 추적, 흐름 다이어그램 작성 | 자금 흐름 기반 리스크 평가 |
| 18 | `ch18. 자금 흐름 리스크 유형 분류.md` | AMM/Lending/Bridge/Custody 유형별 리스크 특성, 컨트랙트 상호작용 리스크(재진입/플래시론/가격조작) | 자금 흐름 기반 리스크 평가 |
| 19 | `ch19. 리스크 평가 기준과 개선 방향 도출.md` | 발생가능성×영향도 평가, 심각도 분류 기준, 개선 방향 제시 형식 | 자금 흐름 기반 리스크 평가 |
| 20 | `ch20. Fund Flow Risk Sheet 정의 및 작성 기준.md` | 시트 구조(흐름구간×리스크유형×심각도×개선방향), 작성 방법, 실기 형식 | 자금 흐름 기반 리스크 평가 |
| 20.5 | `ch20.5. Bridge Oracle Approval 실제 사고 리플레이.md` | Bridge/Oracle/Approval 사고 패턴 재구성, 통제 실패 지점 식별, 재발 방지안 도출 | 자금 흐름 기반 리스크 평가 |
| 21 | `ch21. 종합 시나리오 분석.md` | Module 1~3 통합 적용, 온보딩→자산흐름→권한구조→취약점→통제안·개선방향 전체 흐름 실습 | 권한 통제 설계 / 자금 흐름 기반 리스크 평가 |
| 21.5 | `ch21.5. Associate 분석보고서 표준 템플릿.md` | 보고서 표준 목차/문장 규칙, Evidence 추적성, ACM/FFRS 통합 서술, 우선순위 로드맵 작성 | 권한 통제 설계 / 자금 흐름 기반 리스크 평가 |

---

## 5. Associate 범위 한정

- 본 과정은 Associate 범위만 다룬다.
- 분석 대상: 기존 서비스/프로젝트 구조의 평가와 개선 방향 제시.
- Professional 범위 제외: 설계 목표 설정, 거버넌스 구조 처음부터 설계, 규제 대응 설계.
- 실기 과목의 "설계"는 주어진 구조에서 취약점 식별 및 통제 방향 제시를 의미한다.

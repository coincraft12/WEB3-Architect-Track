# Professional 이관 메모

이 문서는 Associate 강의노트에서 범위를 넘어가는 설계 요소를 Professional 준비용으로 미리 분리해 둔 메모다.

기준 문서:
- `Qualification_Management_and_Operation_Regulations.md`

정리 원칙:
- Associate: 기존 구조의 해석, 취약점 및 운영 리스크 식별, 개선 방향 제시
- Professional: 설계 목표와 제약조건을 두고 보안·거버넌스·규제·운영 관점의 설계안을 구성

## 1) Governance and Operating Model Design

Associate 원문에서 이관한 주제:
- 거버넌스 투표 결과를 Timelock으로 실행하는 구조
- 비상 패치 절차와 예외 실행 체계
- Security Council 같은 긴급 의사결정 기구
- 커뮤니티 공지, 변경 공개, 장기 거버넌스 강화

Professional에서 다룰 질문:
- 어떤 권한은 거버넌스 승인 대상이어야 하는가?
- 정상 변경 경로와 비상 변경 경로를 어떻게 분리할 것인가?
- Security Council, Guardian, Timelock, DAO 투표를 어떤 순서로 연결할 것인가?
- 변경 공지, 검토 기간, 취소권을 누가 보유해야 하는가?

Associate에서 넘어온 예시 포인트:
- `upgrade()`를 단순 Timelock 적용으로 끝내지 않고, 거버넌스 승인 + 실행 계층 분리까지 설계
- 핵심 파라미터 변경에 대해 예외 경로와 정상 경로를 별도 설계
- 커뮤니티 공개 정책과 변경 이력 관리 체계 설계

## 2) Protocol Control Design

Associate 원문에서 이관한 주제:
- 복수 Oracle 조합 방식
- TWAP, 이상값 완화 장치, 서킷 브레이커
- 단계적 청산, 청산 인센티브 최적화
- 자동 금리 조정, 이용률 기반 유동성 관리
- 담보 유형별 LTV 설계

Professional에서 다룰 질문:
- Oracle 단일 소스 문제를 어떤 설계 패턴으로 완화할 것인가?
- 시장 충격 상황에서 청산 메커니즘이 연쇄 손실을 만들지 않도록 어떻게 설계할 것인가?
- 유동성 부족과 뱅크런 리스크에 대응하는 이용률·출금·금리 제어 메커니즘은 무엇인가?
- 담보 자산 특성별로 LTV와 liquidation threshold를 어떻게 차등 설계할 것인가?

Associate에서 넘어온 예시 포인트:
- `Chainlink + TWAP` 같은 소스 조합
- 가격 편차 임계값과 서킷 브레이커 설계
- LP 담보에 대한 보수적 LTV 설계와 유동성 하한선 모니터링
- 자동 청산 봇, 단계적 청산, 자동 금리 조정 등 시장 메커니즘 설계

## 3) Scoring and Deliverable Design

Associate 원문에서 이관한 주제:
- ACM 최고 등급을 거버넌스 연동까지 포함해 정의한 부분
- FFRS에서 특정 기술 메커니즘을 정답처럼 요구한 부분
- 종합 시나리오에서 설계안 수준의 개선안을 사실상 모범답안으로 둔 부분

Professional에서 다룰 질문:
- 설계안 평가 기준을 어떤 축으로 구성할 것인가?
- 단순 리스크 식별이 아니라 설계안 비교 평가를 어떻게 채점할 것인가?
- 동일 문제에 대해 여러 설계 대안을 허용하는 채점 틀은 무엇인가?

## 4) Candidate Professional Modules

- Module A: Governance Execution Path Design
- Module B: Emergency Authority and Exception Handling
- Module C: Oracle and Market Control Design
- Module D: Liquidity and Liquidation Mechanism Design
- Module E: Design Review Rubric and Trade-off Evaluation

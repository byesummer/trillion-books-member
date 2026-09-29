# trillion-books-member

**Trillion** — MSA 기반 온라인 서점의 회원 서비스

> 8인 팀 MSA 프로젝트에서 [auth](https://github.com/byesummer/trillion-books-auth)·[gateway](https://github.com/byesummer/trillion-books-gateway)의 인증/인가와 이 회원 서비스를 담당했다.

## 기능

- 회원가입 — 이메일 인증 후 가입, 가입 시 포인트 자동 적립
- 회원 정보 관리 — 조회·수정·이메일 찾기·비밀번호 재설정, 주소 다건 관리
- 휴면/탈퇴 처리 — 휴면 계정은 Dooray 알림으로 안내, 인증번호 기반 휴면 해제
- 포인트 — 적립/사용/환불 이력을 이력(ledger) 방식으로 기록, 등급별 정책 관리
- 주문 서비스 연동 — 도서 구매·리뷰 작성 포인트 적립, 반품 시 회수(Saga 보상 트랜잭션 대응)
- 쿠폰 서비스 연동 — 회원가입 웰컴 쿠폰 발급을 이벤트로 분리

## 기술 스택

| 분류 | 스택 |
|---|---|
| Language | Java 21 |
| Framework | Spring Boot 3.5.7, Spring Security |
| Cloud | Spring Cloud 2025.0.0 (Eureka Client, Config) |
| Database | MySQL (운영) / H2 (테스트) |
| Data Access | Spring Data JPA |
| Store | Redis |
| Build | Maven |

## 핵심 구현

- **쿠폰 발급 이벤트 분리** — 웰컴 쿠폰 발급을 `ApplicationEventListener`로 분리해 쿠폰 서비스 장애가 가입 실패로 번지지 않게 함
- **포인트 원장(ledger)** — 잔액 필드가 아니라 적립/사용 이력을 append하는 방식으로 관리, Saga 보상 트랜잭션 추적 대응
- **휴면 계정 처리** — 로그인 시 즉시 차단, 인증번호 기반 본인 확인 후 해제, 전환 알림은 Dooray API로 발송

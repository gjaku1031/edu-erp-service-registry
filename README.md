# 🌉 API Gateway 역할과 구조

## 🚀 API Gateway 핵심 기능

- **🔄 단일 진입점**: 모든 클라이언트 요청을 한 곳에서 받아 적절한 서비스로 전달
- **🧭 요청 라우팅**: URL 경로 기반으로 요청을 해당 마이크로서비스로 라우팅
- **🔐 인증 및 권한 부여**: JWT 토큰 검증 등 중앙화된 보안 처리
- **⚖️ 부하 분산**: 여러 서비스 인스턴스에 요청 분산
- **🔄 프로토콜 변환**: 외부-내부 간 통신 프로토콜 변환
- **💾 응답 캐싱**: 성능 향상을 위한 데이터 캐싱

## 🧩 주요 컴포넌트

- **🛡️ SecurityConfig**: 보안 설정 (CORS, 권한, OAuth2, JWT)
- **🔍 JwtAuthenticationFilter**: 모든 요청의 JWT 토큰 검증
- **📦 의존성**: Spring Cloud Gateway, Security, OAuth2, WebFlux

## 🔄 리액티브 프로그래밍

- **⚡ 비동기 처리**: 스레드 차단 없는 효율적 요청 처리
- **📊 Mono/Flux**: 0~1개(Mono) 또는 0~N개(Flux) 데이터의 비동기 처리
- **📡 이벤트 기반**: 데이터 스트림과 변경 전파 기반 패러다임

## 🔄 동작 흐름

1. 클라이언트 → API Gateway 요청
2. 게이트웨이에서 토큰 검증
3. 적절한 마이크로서비스로 라우팅
4. 응답을 클라이언트에게 반환

API Gateway는 마이크로서비스 시스템의 🚪 문지기 역할을 하며, 📡 클라이언트와 서비스 간 통신을 중개합니다.

## 📝 API 라우팅 규칙

| 경로 패턴 | 대상 서비스 | 설명 |
|----------|------------|------|
| `/api/auth/**` | Auth Service | 인증 관련 요청 (로그인, 회원가입, 토큰 갱신) |
| `/api/users/**` | User Service | 사용자 관리 요청 |
| `/api/classes/**` | Class Service | 수업 및 강좌 관리 요청 |
| `/actuator/**` | - | 모니터링 및 관리 엔드포인트 (공개) |

## 🔒 보안 설정

- **공개 엔드포인트**: 인증 없이 접근 가능한 경로
  - `/api/auth/login`
  - `/api/auth/register`
  - `/api/auth/refreshtoken`
  - `/oauth2/token`
  - `/actuator`

- **보안 헤더**: XSS 방지, HSTS, 참조자 정책 등 적용

## 🛠️ 개발 및 실행 방법

1. **실행 순서**:
   ```bash
   # 1. 서비스 레지스트리(Eureka Server) 실행
   cd ../service-registry
   ./gradlew bootRun
   
   # 2. API Gateway 실행
   cd ../api-gateway
   ./gradlew bootRun
   ```

2. **서비스 등록 확인**:
   - Eureka 대시보드 확인: http://localhost:8761
   
3. **API 테스트**:
   - 게이트웨이 요청: http://localhost:8080/api/{service}/{endpoint}
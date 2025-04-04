# 🔍 Service Registry (Eureka Server)

## 📋 프로젝트 개요
이 서비스는 Spring Cloud Netflix Eureka를 기반으로 한 서비스 레지스트리로, 마이크로서비스 아키텍처에서 서비스 디스커버리 기능을 제공합니다.

## 🚀 주요 기능

- **📊 서비스 등록**: 모든 마이크로서비스가 시작 시 자신의 정보를 등록
- **🔍 서비스 발견**: 서비스 간 통신 시 서비스 이름으로 대상 서비스를 찾을 수 있음
- **🔄 상태 모니터링**: 등록된 서비스의 상태(UP/DOWN)를 지속적으로 모니터링
- **⚖️ 로드 밸런싱**: 클라이언트 측 로드 밸런싱 지원
- **🛡️ 장애 내성**: 단일 서비스 인스턴스 실패 시에도 시스템 가용성 유지

## 🛠️ 기술 스택

- **☕ Java 21**
- **🍃 Spring Boot 3**
- **☁️ Spring Cloud Netflix Eureka Server**
- **📊 Spring Actuator** (상태 모니터링)

## 🧩 주요 구성 요소

- **🏛️ EurekaServerApplication**: 서비스 레지스트리 메인 클래스 (`@EnableEurekaServer`)
- **⚙️ 설정 파일**: 서비스 자체 등록 비활성화 및 기타 설정

## 📝 설정 내용

```yaml
server:
  port: 8761  # Eureka 서버 표준 포트

eureka:
  client:
    register-with-eureka: false  # 자기 자신을 등록하지 않음
    fetch-registry: false  # 다른 서비스 목록을 가져오지 않음
  server:
    enable-self-preservation: true  # 자가 보존 모드 활성화
```

## 🔄 동작 흐름

1. Eureka 서버 시작 (가장 먼저 실행되어야 함)
2. 마이크로서비스들이 Eureka 서버에 자신의 정보 등록
3. 클라이언트가 서비스 이름으로 서비스 검색
4. Eureka가 해당 서비스의 인스턴스 목록 반환
5. 클라이언트가 로드 밸런싱 알고리즘으로 인스턴스 선택하여 통신

## 🌐 Eureka 대시보드

Eureka 서버는 웹 기반 대시보드를 제공하여 등록된 서비스 목록 및 상태를 시각적으로 확인할 수 있습니다:
- URL: http://localhost:8761
- 제공 정보:
  - 인스턴스 ID
  - 상태(UP/DOWN)
  - 호스트명
  - IP 주소
  - 포트 번호

## 🛠️ 개발 및 실행 방법

### 로컬 개발 환경에서 실행:

```bash
./gradlew bootRun
```

### Docker 환경에서 실행:

```bash
docker build -t edu-erp-service-registry .
docker run -p 8761:8761 edu-erp-service-registry
```

## 🔗 다른 서비스 연결 방법

다른 마이크로서비스에서 Eureka 클라이언트를 설정하는 방법:

```yaml
# 다른 서비스의 application.yml 예시
eureka:
  client:
    serviceUrl:
      defaultZone: http://localhost:8761/eureka/
  instance:
    preferIpAddress: true
```

## 📚 관련 문서

- [Spring Cloud Netflix](https://cloud.spring.io/spring-cloud-netflix/reference/html/)
- [Service Discovery Pattern](https://microservices.io/patterns/service-registry.html)
- [Eureka Wiki](https://github.com/Netflix/eureka/wiki)
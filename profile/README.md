# 🖼️ Image Convertor

사용자가 이미지를 업로드하고, 비동기적으로 포맷을 변환한 뒤 실시간 알림을 통해 다운로드할 수 있는 웹 애플리케이션입니다.

---

## 🔗 Link

### Project Management
[Image Convertor](https://sunset-flyaway-8c4.notion.site/1c5d497ed3d780d392a5d9a81faa277c?v=1c5d497ed3d781c7bfc8000c0199ec72)

### Wiki
[Yubin Wiki](https://sunset-flyaway-8c4.notion.site/1c5d497ed3d780249474f31d8367084b?v=1c5d497ed3d7800e993d000c194fcc94)

---

## 🏗️ Architecture Overview

![image-convertor](https://github.com/user-attachments/assets/16d67fb8-a3fc-436c-ba78-ff53293c05b9)

### 개요
- 모듈화, 확장성, 비동기 처리, 실시간성 고려
- Kubernetes 기반 오토스케일링 및 무중단 배포 지원

---

## 🎨 Frontend

- **기술 스택**: React + Vite, Vercel
- **기능**
  - 회원가입 / 로그인
  - 이미지 업로드 및 변환 요청
  - 실시간 처리 상태 확인 (WebSocket)
  - 결과 다운로드

---

## 🧩 Backend

### API Server (Spring Boot)
- 이미지 업로드, 상태 조회, DB 연동
- 수평 확장, Kubernetes 오토스케일링 대응

### Authentication Server (Nest.js)
- Google OAuth2, JWT 인증/인가
- 인증 기능을 별도 서버로 분리해 보안성과 유지보수 향상

### WebSocket Server (Node.js, Socket.io)
- 실시간 상태/알림 전송
- Redis로 클러스터 환경 연결 상태 공유

### Messaging Queue (RabbitMQ)
- AMQP 기반 메시지 큐
- DLX(Dead Letter Queue) 및 Retry Queue 구현
  - 재시도 / 장애 복구 로직 포함

### Image Convertor Server (Rust)
- 고성능 이미지 변환 처리
- 처리 완료 후 API 서버에 HTTP 콜백

---

## ⚙️ DevOps

### Load Balancing & Reverse Proxy
- **Nginx Ingress**
  - HTTP / WebSocket 요청 라우팅

### Kubernetes (GKE)
- 모든 백엔드 컨테이너화 배포
- 오토스케일링, 롤링 배포, 모니터링 지원

### CI/CD
- **GitHub Actions + Docker + GKE**
  - main 브랜치 PR 병합 → 자동 배포
  - 테스트 → Docker 빌드 → 이미지 푸시 → GKE 롤링 업데이트

---

## 🗃️ Databases

### PostgreSQL
- 사용자, 이미지, 로그 등 주요 데이터 저장

### Redis
- WebSocket 상태 공유
- 변환 결과 캐싱

---

## 🧾 Documentation

### Swagger API 문서
- Springdoc OpenAPI (Swagger UI)
- `/swagger-ui.html`에서 실시간 명세 확인 가능

---

## 🧪 Testing

- **기술 스택**: JUnit 5, Mockito, Spring Boot Test
- 주요 테스트 대상:
  - 업로드 컨트롤러
  - S3 업로드 서비스
  - DB 저장 및 조회

---

## 🗂️ Storage

### AWS S3
- 원본 이미지 및 변환 이미지 저장
- 이미지 URL을 DB에 기록하여 추적 가능

---

## 🔍 사용자 시나리오 및 기능 명세서

### 1. 회원가입 및 로그인
- 이메일과 비밀번호를 통한 회원가입
- 로그인 후 서비스 이용 가능

### 2. 이미지 업로드
- 드래그 앤 드롭 또는 클릭 업로드
- 지원 확장자: `.jpg`, `.png`, `.webp`
- 최대 용량: **10MB**

### 3. 포맷 선택 및 변환 요청
- 포맷 선택: JPG / PNG / WEBP (기본값: JPG)
- "변환 요청" 버튼 클릭 시 작업 시작

### 4. 비동기 처리 및 실시간 알림
- 백엔드가 RabbitMQ를 통해 워커(Rust 기반)에게 변환 요청 전달
- WebSocket을 통해 실시간 상태 확인

### 5. 결과 다운로드
- 변환 완료 시 알림 전송
- "다운로드" 버튼으로 결과 파일 수신

### 6. 마이 페이지
- 이미지 변환 이력 확인
- 변환 이미지 30일간 보관 후 삭제 (이력은 유지)

---

## 🧑‍💻 Contributors

- **유빈 신** – Backend, DevOps, Architecture Design

---

## 🪧 License

MIT © YubinShin

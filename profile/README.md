# 🖌️ Image Converter

사용자가 이미지를 업로드하고, 비동기적으로 포맷을 변환한 뒤 실시간 알림을 통해 다운로드할 수 있는 웹 애플리케이션입니다.

---

## Link

### WebPage
<p>
  <a href="https://image-converter.yubinshin.com/" target="_blank">
    <img src="https://github.com/user-attachments/assets/6662efe8-3793-4128-aaf0-39d46b08a67e" width="600" alt="Image Converter Thumbnail" />
  </a>
</p>
<p >
  <a href="https://image-converter.yubinshin.com/" target="_blank">
    View Live Demo
  </a>
</p>

### Project Management

<p>
  <a href="https://www.notion.so/1c5d497ed3d780249474f31d8367084b?v=1c5d497ed3d7800e993d000c194fcc94&pvs=4#1c5d497ed3d780c5b2fede023ec1785d" target="_blank">
<img width="600" alt="Image Converter Project Management
" src="https://github.com/user-attachments/assets/7edb8373-c9a6-4d54-861b-e1c5a2dec60e" />
  </a>
</p>
<p >
  <a href="https://www.notion.so/1c5d497ed3d780249474f31d8367084b?v=1c5d497ed3d7800e993d000c194fcc94&pvs=4#1c5d497ed3d780c5b2fede023ec1785d" target="_blank">
    Notion Project Management
  </a>
</p>




### Wiki



<p>
  <a href="https://sunset-flyaway-8c4.notion.site/1c5d497ed3d780249474f31d8367084b?v=1c5d497ed3d7800e993d000c194fcc94" target="_blank">
<img width="300" alt="wiki" src="https://github.com/user-attachments/assets/772699c5-18ec-431b-8b10-0d7afb9d42eb" />
  </a>
</p>
<p >
  <a href="https://sunset-flyaway-8c4.notion.site/1c5d497ed3d780249474f31d8367084b?v=1c5d497ed3d7800e993d000c194fcc94" target="_blank">
    Yubin Wiki
  </a>
</p>


### ArgoCD
<p>
  <a href="https://argocd.yubinshin.com/login?return_url=https%3A%2F%2Fargocd.yubinshin.com%2Fapplications" target="_blank">
<img width="300" alt="argocd thumbnail" src="https://github.com/user-attachments/assets/23c54206-7029-4d08-b669-2d1f3fa3295f" />  </a>
</p>
<p >
  <a href="https://sunset-flyaway-8c4.notion.site/1c5d497ed3d780249474f31d8367084b?v=1c5d497ed3d7800e993d000c194fcc94" target="_blank">
    Yubin ArgoCD
  </a>
</p>



## Project Repositories

| 저장소 | 설명 |
|--------|------|
| [**frontend**](https://github.com/yubin-image-converter/frontend) | React + Vite 기반 사용자 인터페이스<br>로그인, 이미지 업로드, 실시간 변환 상태 확인 UI 제공 |
| [**api**](https://github.com/yubin-image-converter/api) | Spring Boot 기반 API 서버<br>파일 업로드, 변환 상태 조회, DB 연동 등 핵심 비즈니스 로직 처리 |
| [**authentication**](https://github.com/yubin-image-converter/authentication) | NestJS 기반 인증 서버<br>Google OAuth2, JWT 발급 및 유효성 검사 |
| [**websocket**](https://github.com/yubin-image-converter/websocket) | Node.js + Socket.io 기반 WebSocket 서버<br>변환 상태 실시간 푸시, 다중 세션 관리 |
| [**worker**](https://github.com/yubin-image-converter/worker) | Rust 기반 이미지 → ASCII 변환 워커<br>RabbitMQ 메시지 처리, API 서버로 결과 콜백 전송 |
| [**k8s**](https://github.com/yubin-image-converter/k8s) | Kubernetes GitOps 구성 저장소<br>Argo CD, Helm/Kustomize, Sealed Secret 등 인프라 전반 관리 |


---

## Architecture Overview

![System Architecture Diagram](https://github.com/user-attachments/assets/f015a788-f29b-4f0d-ad6a-4fe588de92cc)


## 개요

이 프로젝트는 사용자가 이미지를 업로드하면, **비동기적으로 변환 작업을 처리하고**, 결과를 **실시간으로 확인할 수 있는** 웹 애플리케이션입니다.

전체 시스템은 **모듈화**, **확장성**, **비동기 처리**, **실시간성**을 고려하여 설계되었습니다.

---

## Frontend

- **기술 스택**
    - React + Vite
    - Tailwind CSS
    - Jotai (상태 관리)
    - Kubernetes (운영용)
    - Nginx (정적파일 서빙)
- **역할**
    - 사용자와의 상호작용 담당
- **기능**
    - Google OAuth2 로그인
    - 이미지 업로드 및 ascii 아트 미리보기
    - 변환 요청 및 실시간 상태 확인 (WebSocket 기반)
    - ASCII 변환 결과 조회 및 다운로드
    - ASCII 변환 실패 시 실패 메시지 제공

> 운영 환경에서는 Vite 정적 빌드를 Kubernetes 클러스터에 배포하며, NGINX Ingress를 통해 라우팅됩니다.
> 

---

## Backend

### API Server

- **기술 스택**
    - Spring Boot + Spring Security + Spring Data JPA
- **역할**
    - 클라이언트 요청 처리 및 데이터베이스 연동
- **기능**
    - 이미지 업로드 요청 수신
    - 이미지 변환 상태 및 결과 조회
    - 결과 파일(.txt) 반환
    - JWT 발급 및 재발급
    - Rabbitmq 사용하여 Worker에게 작업 할당
    - Redis 캐싱 활용(in progress)
      
> 복수 인스턴스를 통한 수평 확장이 가능하며, Kubernetes HPA 기반 오토스케일링을 고려해 설계되었습니다.
> 변환 결과 파일은 Kubernetes PVC로 마운트된 외부 NFS에 저장되며, DB에는 해당 경로만 기록되어 클라이언트가 직접 파일에 접근할 수 있도록 구성되어 있습니다.


### Authentication Server

- **기술 스택**
    - NestJS
- **역할**
    - 사용자 인증 및 인가 처리 (JWT 기반)
- **기능**
    - Google OAuth2 로그인 지원
    - JWT 쿠키 설정 및 전달
    - 사용자 인증 상태 검증 API 제공

> 인증 서버를 API 서버와 분리함으로써 보안과 유지보수 효율성을 높였습니다.
> 

### WebSocket Server

- **기술 스택**
    - Node.js, Socket.IO, WebSocket
- **역할**
    - 실시간 상태 및 알림 전송
- **기능**
    - Client에게 변환 요청 진행 상태 실시간 푸시
    - Worker, Client 간 이미지 변환 완료 이벤트 전송
    - 로그인 유저 다중 세션 관리 (clientId 기반)

> Redis Pub/Sub를 통해 클러스터 환경에서도 WebSocket 연결 상태를 공유하고 메시지를 전파합니다.
> 

### Messaging Queue Server

- **기술 스택**
    - RabbitMQ
- **역할**
    - 비동기 이미지 변환 요청 중개 및 안정적인 메시지 흐름 제어
- **기능**
    - API 서버 → Rust 워커 간 메시지 전달 (AMQP)
    - DLX(Dead Letter Queue) 및 Retry Queue 기반 장애 복원력 설계 (in progress)

> 현재 DLX 및 TTL 기반 Retry Queue는 설계되어 있으며, 구현은 진행 중입니다.

### Worker

- **기술 스택**
    - Rust
- **역할**
    - 고성능 이미지 → ASCII 변환 처리
    - 메시징 큐(RabbitMQ)에서 변환 작업을 consume
    - 처리 상황을 WebSocket 서버로 중계 (progress_update, ascii_complete 등)
- **기능**
    - RabbitMQ 메시지 기반 변환 작업
    - 변환 완료 시 API 서버로 HTTP POST 콜백 전송

> Rust 워커는 독립적으로 확장 가능하며, Kubernetes 내에서 병렬 처리 기반 수평 확장을 지원합니다.
> 변환된 ASCII 결과 파일은 Kubernetes PVC를 통해 외부 NFS 서버에 `.txt` 파일로 저장되며, 동일한 스토리지를 API 서버와 공유합니다.
 

---

## DevOps

### Load Balancing & Reverse Proxy

- **기술 스택**
    - NGINX Ingress Controller
- **역할**
    - 클라이언트의 HTTP / WebSocket 요청을 Kubernetes 내부 서비스로 라우팅

### Google Kubernetes Engine (GKE)

- **기술 스택**
    - Kubernetes, Argo CD, Helm, Kustomize
- **기능**
    - 모든 백엔드 및 프론트엔드 서버 컨테이너화 및 배포
    - HPA, Rolling Update, 자원 모니터링
    - GitOps 기반 선언적 배포 및 버전 관리

### DNS & Domain

- **기술 스택**
    - Cloudflare (DNS)
    - cert-manager (TLS 인증서 자동 발급)
- **역할**
    - Cloudflare를 통해 도메인 및 DNS 레코드 관리
    - cert-manager를 통해 Let's Encrypt 기반 인증서 자동 발급 및 갱신
- **기능**
    - DNS → Ingress Controller(NGINX) → Kubernetes 서비스 라우팅
    - Ingress 리소스에 `cert-manager.io/cluster-issuer` 어노테이션 적용
    - TLS 인증서 생성 및 Secret 자동 연동

### CI

- **기술 스택**
    - GitHub Actions + Docker + Docker Hub
- **기능**
    - 각 저장소 별 main 브랜치 병합 시 CI 파이프라인 실행
    - 단위 테스트(JUnit 등) 자동화
    - 서비스별 Docker 이미지 빌드 및 Docker Hub로 푸시
    - 이미지 버전 정보를 GitOps 저장소에 자동 커밋 (선택적)

> CI 파이프라인은 GitOps 기반 CD가 동작할 수 있도록 빌드된 이미지를 준비하고, Git 상태를 최신화합니다.

### GitOps

- **기술 스택**
    - GitHub + Argo CD + Kustomize + Sealed Secrets
- **기능**
    - Git 저장소에 모든 Kubernetes manifest 및 설정을 선언적 방식으로 관리
    - Kustomize 기반 base/overlay 구조로 환경 분리
    - Sealed-Secret을 활용해 민감 정보 암호화 및 안전한 Git 보관
    - 이미지 태그 변경 커밋을 감지해 자동 롤링 업데이트 수행

> Git이 단일 진실 소스로 기능하며, Argo CD가 이를 주기적으로 동기화하여 클러스터 상태를 유지합니다.

---

## Databases

### Relational Database

- **기술 스택**
    - PostgreSQL (GCP VM 상 컨테이너)
- **역할**
    - 유저, 파일 메타데이터, 로그 등 영속 데이터 저장소

### In-memory Database

- **기술 스택**
    - Redis (GCP VM 상 컨테이너)
- **역할**
    - WebSocket 세션 공유 및 ASCII 변환 캐싱

---

## Storage

### Network File Storage

- **기술 스택**
    - GCP VM에 구성된 NFS 서버 (NFS v3)
- **역할**
    - 이미지 원본 및 ASCII 변환 결과 파일 저장
- **기능**
    - 업로드 및 결과물 경로를 DB에 저장하여 클라이언트가 직접 접근 가능

> Kubernetes에서 외부 NFS 서버를 PVC로 마운트하는 과정에서 발생한 permission denied 문제를 해결하기 위해:
> 
> - NFS 서버의 export 설정을 GKE 노드 IP 대역(CIDR) 기반으로 수정
> - GCP 방화벽 정책을 GKE 노드 IP 범위와 일치하도록 허용
> - Pod 보안 컨텍스트 내 파일 퍼미션 및 마운트 옵션 점검
> 
> 위 과정을 통해 외부 NFS 서버와의 안정적인 연결을 구축했습니다.


---

## Documentation

### Swagger API 문서

- **기술 스택**
    - SpringDoc OpenAPI
- **기능**
    - `/swagger-ui.html` 경로에서 API 테스트 및 명세 확인 가능

---

## Testing

### Spring Boot Test

- **기술 스택**
    - JUnit 5, Mockito
- **기능**
    - 주요 컨트롤러, 서비스, 저장소 계층 테스트
    - CI에 포함된 테스트로 회귀 오류 방지

---

## 서비스 플로우

### 1. 회원가입 및 로그인
- Google OAuth2 기반 소셜 로그인 지원
- 로그인 시 JWT 발급 → 클라이언트가 저장소(LocalStorage & Cookie)로 관리
- 로그인 이후 실시간 WebSocket 세션 연결

### 2. 이미지 업로드
- 드래그 앤 드롭 또는 클릭 업로드 방식 지원
- 지원 확장자: `.jpg`, `.png`, `.webp`
- 최대 용량: **50MB**
- 로그인 상태에서만 업로드 가능
- 업로드 즉시 NFS 스토리지에 저장, 경로는 DB에 기록

### 3. 변환 요청
- 업로드 직후 자동으로 변환 요청 전송
- 변환 요청은 RabbitMQ를 통해 Rust 워커에게 전달
- 요청에는 `userId`, `requestId`, `filePath` 정보 포함

### 4. 비동기 처리 및 실시간 알림
- 워커는 메시지 수신 후 변환 작업 수행
- 작업 진행률(`progress_update`), 완료 이벤트(`ascii_complete`)를 WebSocket으로 전송
- 프론트는 이를 구독하여 ASCII 로딩 UI, 상태바 등 렌더링

### 5. 결과 확인 및 다운로드
- 변환 완료 시 API 서버로 HTTP 콜백 전송 → DB에 경로 갱신
- 클라이언트는 갱신된 `.txt` 파일 경로를 통해 ASCII 결과 확인 가능
- "다운로드" 버튼을 통해 ASCII 결과 텍스트 파일 수신

### 6. 마이 페이지 (구현 중)
- 사용자의 변환 이력 확인
- 결과 파일은 7일간 보관 (NFS 기준), 이후 자동 삭제
- 변환 기록은 DB에 유지되어 이력 조회 가능

---

## Contributors

- **유빈 신** – Frontend, Backend, DevOps, Architecture Design

---

## License

MIT © YubinShin

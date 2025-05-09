# Code Cooking GitOps Repository

이 저장소는 Code Cooking 프로젝트의 쿠버네티스 배포 구성을 GitOps 방식으로 관리하기 위한 저장소입니다..

## 프로젝트 구조

```
codecooking-gitops/
├── README.md                      # 프로젝트 개요 및 설명
├── namespaces/                    # 네임스페이스 정의
│   └── codecooking-namespace.yaml
├── apps/                          # 애플리케이션별 쿠버네티스 매니페스트
│   ├── frontend/                  # 프론트엔드 Vue3 애플리케이션
│   │   ├── deployment.yaml
│   │   ├── service.yaml
│   │   └── ingress.yaml
│   ├── backend-cicd/              # CI/CD 백엔드 서비스
│   │   ├── deployment.yaml
│   │   ├── service.yaml
│   │   └── configmap.yaml
│   ├── backend-fragment/          # Fragment 백엔드 서비스
│   │   ├── deployment.yaml
│   │   ├── service.yaml
│   │   └── configmap.yaml
│   └── backend-mcp/               # MCP 백엔드 서비스
│       ├── deployment.yaml
│       ├── service.yaml
│       └── configmap.yaml
├── argocd/                        # ArgoCD 애플리케이션 정의
│   ├── frontend-app.yaml
│   ├── backend-cicd-app.yaml
│   ├── backend-fragment-app.yaml
│   ├── backend-mcp-app.yaml
│   └── project.yaml               # ArgoCD 프로젝트 정의
└── shared/                        # 공통 리소스
    ├── secrets/                   # 암호화된 시크릿
    ├── ingress/                   # 인그레스 관련 설정
    └── monitoring/                # 모니터링 도구 설정
```

## 애플리케이션 구성 요소

- **Frontend**: Vue3 기반 웹 애플리케이션
- **Backend-CICD**: CI/CD 관련 백엔드 서비스
- **Backend-Fragment**: Fragment 처리 관련 백엔드 서비스
- **Backend-MCP**: MCP(Management Control Plane) 백엔드 서비스

## 배포 방법

이 저장소는 ArgoCD를 통해 자동으로 모니터링되며, 변경 사항은 쿠버네티스 클러스터에 자동으로 반영됩니다.

### 수동 배포 (필요한 경우)

```bash
# ArgoCD CLI를 사용하여 애플리케이션 동기화
argocd app sync codecooking-frontend
argocd app sync codecooking-backend-cicd
argocd app sync codecooking-backend-fragment
argocd app sync codecooking-backend-mcp
```

## CI/CD 파이프라인

1. Jenkins는 소스 코드 저장소의 변경을 감지하여 빌드 및 Docker 이미지 생성
2. 빌드된 이미지는 Azure Container Registry(ACR)에 푸시
3. Jenkins 파이프라인은 이 GitOps 저장소의 이미지 태그를 업데이트
4. ArgoCD는 GitOps 저장소 변경을 감지하여 쿠버네티스 클러스터에 자동 배포

## 환경 설정

이 저장소는 다음 인프라를 대상으로 합니다:

- **Azure Kubernetes Service (AKS)**: aks-digitalgarage-01
- **Azure Container Registry (ACR)**: acrdigitalgarage01.azurecr.io
- **리소스 그룹**: rg-digitalgarage-01
- **지역**: Korea Central

## 참고사항

- 배포 구성에 관한 변경은 이 저장소를 통해서만 이루어져야 합니다.
- 애플리케이션 코드 변경은 각 애플리케이션의 소스 저장소에서 관리됩니다.
- 환경별(개발, 스테이징, 프로덕션) 구성은 브랜치로 관리됩니다.

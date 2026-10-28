
# Terraform 프로젝트식 학습

## 1장 1교시 - IaC와 Terraform의 개념 및 동작 원리

### 1. IaC(Infrastructure as Code)란?

IaC는 인프라를 AWS 콘솔에서 수동으로 구축하는 대신 **코드로 정의하고 관리하는 방식**이다.

#### IaC가 등장한 이유
- 반복적인 인프라 구축 작업 자동화
- 수동 작업으로 인한 설정 누락과 실수 감소
- 개발·테스트·운영 환경의 일관성 유지
- Git을 이용한 인프라 설정 변경 이력 관리
- 동일한 인프라의 반복 구축 및 재사용

---

### 2. Terraform이란?

Terraform은 HashiCorp에서 개발한 **선언형 IaC 도구**이다.

AWS, Azure, GCP 등 다양한 클라우드 환경의 인프라를 코드로 생성·변경·삭제할 수 있다.

#### 선언형 방식이란?
- 명령형: 인프라를 **어떻게 생성할지** 작업 순서를 지정
- 선언형: **어떤 인프라가 필요한지** 원하는 최종 상태를 정의

Terraform은 현재 상태와 원하는 상태를 비교하여 필요한 변경 작업을 계획한다.

---

### 3. Terraform 동작 원리

```text
사용자
  │
  ▼
HCL 코드 작성 (.tf)
  │
  ▼
Terraform Core
  │
  ├─ 설정 파일 분석
  ├─ State 및 실제 인프라 상태 확인
  ├─ 리소스 의존성 분석
  └─ 변경 작업 계획
  │
  ▼
AWS Provider
  │
  ▼
AWS API
  │
  ▼
AWS 리소스 생성·변경·삭제
(VPC, Subnet, EC2 등)
```

#### 핵심 구성요소

| 구성요소 | 역할 |
|---|---|
| HCL | Terraform 설정 파일 작성 언어 |
| Terraform Core | 설정 분석, 의존성 처리 및 변경 계획·실행 |
| Provider | AWS 등 외부 서비스 API와 통신하는 플러그인 |
| Resource | Terraform으로 관리할 인프라 리소스 |
| State | Terraform 관리 리소스의 상태 및 실제 리소스 연결 정보 저장 |

---

### 4. Terraform 기본 코드

AWS VPC를 정의하는 예제이다.

```hcl
resource "aws_vpc" "marble" {
  cidr_block = "10.0.0.0/16"

  tags = {
    Name = "marble-vpc"
  }
}
```

#### 코드 해석

| 코드 | 의미 |
|---|---|
| `resource` | 리소스 정의 |
| `aws_vpc` | AWS VPC 리소스 유형 |
| `marble` | Terraform 내부에서 사용하는 리소스 이름 |
| `cidr_block` | VPC IP 주소 범위 |
| `tags` | AWS 리소스 태그 지정 |
| `Name` | AWS 콘솔에 표시할 Name 태그 |

#### 리소스 참조와 의존성

```hcl
resource "aws_subnet" "web01" {
  vpc_id     = aws_vpc.marble.id
  cidr_block = "10.0.11.0/24"
}
```

- `aws_vpc.marble.id`: 앞서 정의한 VPC의 ID 참조
- `vpc_id`: Subnet이 소속될 VPC 지정
- Terraform은 참조 관계를 분석하여 VPC를 먼저 생성하고 Subnet을 생성한다.

---

### 5. Terraform 핵심 명령어

| 명령어 | 역할 |
|---|---|
| `terraform init` | Terraform 작업 환경 초기화 및 Provider 설치 |
| `terraform plan` | 리소스 생성·변경·삭제 계획 확인 |
| `terraform apply` | 변경 사항을 실제 인프라에 적용 |
| `terraform destroy` | Terraform에서 관리하는 리소스 제거 |

#### 기본 실행 흐름

```bash
terraform init
terraform plan
terraform apply
```

#### 인프라 삭제

```bash
terraform destroy
```

#### Plan 결과 예시

```text
Plan: 1 to add, 0 to change, 0 to destroy.
```

- `1 to add`: 리소스 1개 생성 예정
- `0 to change`: 수정할 리소스 없음
- `0 to destroy`: 삭제할 리소스 없음

**주의:** `apply` 또는 `destroy` 실행 전에는 반드시 변경 계획을 검토해야 한다.

---

### 6. AWS 콘솔과 Terraform 비교

| AWS 콘솔 | Terraform |
|---|---|
| GUI에서 직접 클릭 | HCL 코드로 인프라 정의 |
| 처음 학습하기 쉬움 | HCL 및 State 학습 필요 |
| 소규모 수동 작업에 편리 | 반복 구축과 자동화에 유리 |
| 동일 환경 재구축 시 수동 작업 | 코드 재사용을 통한 재구축 |
| 변경 이력 별도 관리 필요 | Git으로 코드 변경 이력 관리 가능 |

Terraform으로 구축한 인프라도 실제 AWS 리소스이므로 AWS 이용 요금이 발생할 수 있다.

---

### 7. Terraform과 다른 도구 비교

| 도구 | 주요 목적 |
|---|---|
| Terraform | 인프라 프로비저닝 및 변경 관리 |
| AWS CloudFormation | AWS 네이티브 IaC |
| Ansible | 서버 설정 및 소프트웨어 구성 자동화 |
| AWS 콘솔 | GUI 기반 리소스 관리 |

각 도구는 기능이 일부 겹치며 함께 사용할 수 있다.

---

### 8. Marble 프로젝트에 Terraform 적용

기존 AWS 콘솔에서 구축했던 Marble 프로젝트 인프라를 Terraform 코드로 재구축하는 것이 학습 목표이다.

| 기존 AWS 구성 | Terraform 학습 대상 |
|---|---|
| VPC | `aws_vpc` |
| Subnet | `aws_subnet` |
| Route Table | `aws_route_table` |
| Internet Gateway | `aws_internet_gateway` |
| Security Group | `aws_security_group` |
| EC2 | `aws_instance` |
| ALB | `aws_lb` (확장 실습) |

---

### 9. 핵심 요약

1. **IaC**는 인프라를 코드로 정의하고 관리하는 방식이다.
2. **Terraform**은 선언형 IaC 도구로 AWS 등 클라우드 인프라를 관리한다.
3. **Provider**는 Terraform이 AWS API와 통신하도록 돕는다.
4. **Resource**는 Terraform이 관리하는 인프라 구성요소이다.
5. **State**는 Terraform의 관리 대상과 실제 리소스 사이의 상태 정보를 저장한다.
6. **init → plan → apply**는 Terraform의 기본 작업 흐름이다.
7. Terraform의 핵심 장점은 **반복 구축, 코드 재사용, 일관성 및 변경 관리**이다.

---

### 학습 진행 현황

- [x] 1장 1교시 - IaC와 Terraform의 개념 및 동작 원리
- [ ] 1장 2교시 - Terraform 설치, AWS 인증 및 첫 번째 VPC 생성 실습

<img width="1312" height="1199" alt="image" src="https://github.com/user-attachments/assets/cbbd6725-de36-42db-b607-a21949e60efd" />

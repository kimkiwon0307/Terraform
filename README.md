<details>
  <summary><b>1장 IaC와 테라폼</b></summary>
  
  <h3>1.1 인프라 자동화의 성숙도 변화</h3>
  <ul>
    <li><b>수동 작업</b>: 사람이 직접 서버 생성, 보안 그룹 설정, 프로그램 설치 등을 수행 (1~2대는 괜찮으나 100대 이상 시 문제 발생)</li>
    <li><b>스크립트 자동화</b>: 반복 작업을 명령어와 스크립트로 자동화 (수동 작업보다 개선되었으나 서버 구성 관리의 한계 직면)</li>
    <li><b>IaC (Infrastructure as Code)</b>: 인프라를 코드로 관리하며, 테라폼(Terraform)이 대표적인 도구</li>
  </ul>

  <h3>1.2 프로세스로서의 자동화</h3>
  <ul>
    <li>사람이 직접 수행하던 각 단계의 작업을 반복 가능한 일관된 과정으로 전환하는 것</li>
  </ul>

  <h3>1.3 IaC의 이해</h3>
  <ul>
    <li><b>정의</b>: 서버, 네트워크, 보안 등 인프라 구성을 코드로 작성하여 관리</li>
    <li><b>도입 이유</b>: 반복 작업 감소, 일관성 유지, 변경 이력 관리, 재현 가능성 확보</li>
  </ul>

  <h3>1.4 Terraform의 특성</h3>
  <ul>
    <li>핵심 특징은 최종적으로 도달해야 할 상태를 정의하는 <b>선언적 방식</b></li>
    <li>인프라 관리를 위해 <b>Provider</b>를 사용</li>
  </ul>

  <h3>1.5 Terraform 제공 유형</h3>
  <ul>
    <li><b>Terraform CLI</b>: 직접 테라폼을 설치하고 명령어 실행</li>
    <li><b>HCP Terraform</b>: HashiCorp에서 제공하는 클라우드 기반의 팀 단위 관리 서비스</li>
    <li><b>Terraform Enterprise</b>: 기업에서 자체적으로 운영하는 테라폼 플랫폼</li>
  </ul>

  <h3>1.6 Terraform과 다른 도구의 비교</h3>
  <ul>
    <li><b>테라폼</b>: 인프라 자체를 생성하는 데 강점</li>
    <li><b>앤서블 (Ansible)</b>: 서버 내부 설정을 구성하는 데 강점</li>
    <li><b>CloudFormation</b>: AWS 환경에 집중된 도구</li>
    <li><b>Pulumi</b>: 파이썬, 타입스크립트, Go, C# 등 일반 프로그래밍 언어 사용</li>
  </ul>

  <h3>1.7 Terraform 사용 목적과 과제</h3>
  <ul>
    <li><b>자동화</b>: 콘솔 작업의 자동화</li>
    <li><b>일관성</b>: 환경별 설정 편차 감소</li>
    <li><b>재사용</b>: 코드를 수정하여 다른 환경에 적용 가능</li>
    <li><b>변경 관리</b>: Git과 연동하여 이력 추적</li>
  </ul>
</details>

2장 실행 환경 구성

  2.1 테라폼 환경 구성
    2.1.1 Terraform 설치
      테라폼이란? 테라폼은 우리가 작성한 .tf 파일을 읽어서 AWS 같은 외부 인프라를 관리하는 실팽 프로그램(CLI)이다.
      설치 확인 : CMD에서 Terrafrom version 
    
    2.1.2 Windows 사용자 환경 구성
      테라폼 설치 위치 : 윈도우에서는 테라폼 실행 파일인 terraform.exe가 있어야한다. 
      PATH란 : 명령어를 입력했을 때 windows가 프로그램을 찾아보는 경로 목록이다.
  
  2.2 IDE 구성
    비주얼 스튜디오 설치
    HashiCorp Terraform 확장 기능 설치

  2.3 CLI 구성 파일
    
      

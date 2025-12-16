# IPiece
<!-- ### 소유 그 이상의 가치, 내 최애 캐릭터를 자산으로 만드는 문화 STO 플랫폼 -->
![ezgif-8a5d07b47a741ded](https://github.com/user-attachments/assets/8ae4011a-80b6-4cec-8a43-bd094a776ccf)

<br>

<div align="center">

[![Next.js](https://img.shields.io/badge/Next.js-000000?style=flat&logo=next.js&logoColor=white)](https://nextjs.org/)
[![Spring Boot](https://img.shields.io/badge/Spring_Boot-6DB33F?style=flat&logo=springboot&logoColor=white)](https://spring.io/projects/spring-boot)
[![Hyperledger Besu](https://img.shields.io/badge/Hyperledger_Besu-2F3134?style=flat&logo=LinuxFoundation&logoColor=white)](https://www.hyperledger.org/use/besu)
[![AWS](https://img.shields.io/badge/AWS-232F3E?style=flat&logo=amazonaws&logoColor=white)](https://aws.amazon.com/)
[![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=flat&logo=kubernetes&logoColor=white)](https://kubernetes.io/)

</div>

---

## 📖 목차

1. [프로젝트 개요](#-프로젝트-개요)
2. [시스템 아키텍처](#️-시스템-아키텍처)
3. [주요 서비스 기능](#-주요-서비스-기능)
4. [인프라 및 기술적 특징](#️-인프라-및-기술적-특징)
5. [기술 스택](#️-기술-스택)
6. [트러블슈팅](#-트러블슈팅)
7. [팀원 소개](#-팀원-소개)
8. [리포지토리 링크](#-리포지토리-링크)

---

## 🚀 프로젝트 개요

### 💡 기획 배경
K-콘텐츠 시장의 급격한 성장에도 불구하고, 캐릭터·웹툰 등 유망 IP에 대한 투자는 대형 기관에 국한되어 있었습니다. **IPiece**는 블록체인 기술을 활용해 IP 수익권을 토큰증권(STO)으로 발행하여, 팬과 개인 투자자가 소액으로도 콘텐츠 생태계에 참여하고 수익을 공유할 수 있는 **민주화된 투자 플랫폼**입니다.

### 🎯 프로젝트 목표
- **제도권 금융 준수:** 온체인(블록체인)과 오프체인(DB) 데이터 정합성을 보장하는 신뢰받는 STO 플랫폼 구축
- **하이브리드 클라우드:** 보안이 필수적인 '원장'은 온프레미스에, 확장성이 필요한 '서비스'는 AWS에 배치
- **고가용성(HA) 확보:** 단일 장애점(SPOF) 없는 무중단 서비스 아키텍처 구현

##### 🗓️ 수행 기간
2025.10.21 ~ 2025.12.05 (7주)

---

## 🏛️ 시스템 아키텍처

**IPiece**는 금융권의 망 분리 요건을 고려하여 **AWS 클라우드와 온프레미스 IDC를 VPN으로 연결한 하이브리드 아키텍처**를 채택했습니다.

<div align="center">
  <img width="1000" alt="image" src="https://github.com/user-attachments/assets/ba0a882f-7e86-4522-8c57-05e184ffac40" />
</div>

<br>

### 🏗️ 인프라 구성의 핵심
1. **Hybrid Network:** AWS VPC와 온프레미스 내부망을 `Site-to-Site VPN (IPsec)`으로 연결하여 보안 터널링 구현
2. **EKS (Amazon Elastic Kubernetes Service):** 마이크로서비스의 유연한 배포와 오토스케일링(HPA/CA)을 위한 컨테이너 오케스트레이션
3. **DB High Availability:** 비용 최적화를 위해 RDS 대신 EC2 상에 **Self-Hosted PostgreSQL Cluster (Patroni + etcd)** 구축 및 자동 Failover 구현
4. **Private Blockchain:** 온프레미스 폐쇄망 내에 **Hyperledger Besu** 노드를 구축하여 토큰 발행 및 거래 기록의 무결성 보장

---

## ✨ 주요 서비스 기능

사용자 친화적인 투자 경험(MTS)과 투명한 관리자 운영 시스템을 제공합니다.

| 기능 | 설명 | 화면 예시 |
| :--- | :--- | :--- |
| **📈 IP 공모** | 신규 캐릭터 IP의 로드맵과 수익 구조를 확인하고 청약에 참여합니다. | <img src="https://github.com/user-attachments/assets/4ee52017-a78a-4b27-88ae-dc6c8593660a" width="200" /> |
| **🔄 2차 거래** | 주식 호가창과 유사한 UI에서 실시간으로 토큰을 매매합니다. (WebSocket 적용) | <img src="https://github.com/user-attachments/assets/3f0cd2be-653b-415c-b6e4-e3101ab348e6" width="200" /> |
| **📊 자산 관리** | 보유 토큰의 평가 손익과 포트폴리오 비중을 시각화하여 제공합니다. | <img src="https://github.com/user-attachments/assets/b81e6e40-0b8b-4478-938f-476a3ce4bac8" width="200" /> |
| **🔐 관리자 기능** | 상품 등록, 공모 승인, **배당금 자동 정산 및 지급(Smart Contract)**을 관리합니다. | <img src="https://github.com/user-attachments/assets/589e869c-42c4-4a3c-8446-7650b06cd818" width="200" /> |

---

## ☁️ 인프라 및 기술적 특징

본 프로젝트는 단순한 서비스 개발을 넘어 **엔터프라이즈급 인프라 구축 및 운영 최적화**에 중점을 두었습니다.

### 1️⃣ 비용 최적화 (Cost Optimization)
- **문제:** 표준 아키텍처(RDS Multi-AZ 등) 설계 시 월 예상 비용 $1,054로 예산($450) 초과
- **해결:**
    - **Self-Hosted DB:** RDS 대신 EC2에 PostgreSQL HA 클러스터 직접 구축 (약 20% 절감)
    - **IaC & Lifecycle:** Terraform을 활용해 야간/주말 유휴 리소스 자동 삭제 (`terraform destroy`)
- **결과:** 핵심 기능(HA)은 유지하면서 **비용 70% 이상 절감 달성**

### 2️⃣ 고가용성 (High Availability)
- **Compute:** EKS Worker Node 및 Pod에 오토스케일링(HPA, CA) 적용하여 트래픽 급증 대응
- **Database:** Patroni + etcd 기반의 3-Node PostgreSQL 클러스터 구성으로 **장애 발생 시 수 초 내 자동 Failover**
- **Traffic:** AWS ALB 및 NLB를 활용하여 부하 분산 및 Health Check 기반의 무중단 서비스 제공

### 3️⃣ 하이브리드 연결성 (Connectivity)
- **NAT-T VPN:** NAT 환경의 온프레미스와 AWS 간 통신을 위해 `UDP 4500` 캡슐화를 적용한 IPsec 터널링 구축
- **단일 진입점:** 개발자 접근 제어를 위해 Bastion Host 대신 **WireGuard VPN**을 도입하여 보안성과 접근 편의성 동시 확보

### 4️⃣ 통합 관측 가능성 (Observability)
- **PLG Stack:** Prometheus(메트릭), Loki(로그), Grafana(시각화) 스택을 EKS 내에 구축
- **Hybrid Monitoring:** OpenTelemetry Collector를 활용하여 온프레미스 노드 상태와 클라우드 리소스를 **단일 대시보드에서 통합 관제**

---

## 🛠️ 기술 스택

| 영역 | 기술 스택 |
| :--- | :--- |
| **Frontend** | ![Next.js](https://img.shields.io/badge/-Next.js-000000?logo=next.js&logoColor=white) ![TypeScript](https://img.shields.io/badge/-TypeScript-3178C6?logo=typescript&logoColor=white) ![Tailwind CSS](https://img.shields.io/badge/-Tailwind-06B6D4?logo=tailwindcss&logoColor=white) ![Zustand](https://img.shields.io/badge/-Zustand-764ABC?logo=react&logoColor=white) |
| **Backend** | ![Java 17](https://img.shields.io/badge/-Java%2017-007396?logo=openjdk&logoColor=white) ![Spring Boot](https://img.shields.io/badge/-Spring%20Boot-6DB33F?logo=springboot&logoColor=white) ![JPA](https://img.shields.io/badge/-JPA-59666C?logo=hibernate&logoColor=white) ![Spring Security](https://img.shields.io/badge/-Security-6DB33F?logo=springsecurity&logoColor=white) |
| **Blockchain** | ![Hyperledger Besu](https://img.shields.io/badge/-Hyperledger%20Besu-2F3134?logo=linuxfoundation&logoColor=white) ![Solidity](https://img.shields.io/badge/-Solidity-363636?logo=solidity&logoColor=white) ![Web3j](https://img.shields.io/badge/-Web3j-E34F26?logo=ethereum&logoColor=white) |
| **Infra (AWS)** | ![EKS](https://img.shields.io/badge/-EKS-FF9900?logo=amazonaws&logoColor=white) ![EC2](https://img.shields.io/badge/-EC2-FF9900?logo=amazonec2&logoColor=white) ![VPC](https://img.shields.io/badge/-VPC-FF9900?logo=amazonvpc&logoColor=white) ![Route53](https://img.shields.io/badge/-Route53-FF9900?logo=amazonroute53&logoColor=white) |
| **DevOps** | ![Terraform](https://img.shields.io/badge/-Terraform-7B42BC?logo=terraform&logoColor=white) ![Docker](https://img.shields.io/badge/-Docker-2496ED?logo=docker&logoColor=white) ![GitHub Actions](https://img.shields.io/badge/-GitHub%20Actions-2088FF?logo=githubactions&logoColor=white) ![ArgoCD](https://img.shields.io/badge/-ArgoCD-EF7B4D?logo=argo&logoColor=white) |
| **Monitoring** | ![Prometheus](https://img.shields.io/badge/-Prometheus-E6522C?logo=prometheus&logoColor=white) ![Grafana](https://img.shields.io/badge/-Grafana-F46800?logo=grafana&logoColor=white) ![Loki](https://img.shields.io/badge/-Loki-F46800?logo=grafana&logoColor=white) |

---

## 🔥 트러블슈팅

프로젝트 진행 중 발생한 기술적 난관과 해결 과정을 기록했습니다. (상세 내용은 토글을 클릭하세요)

<details>
<summary><strong>1️⃣ 하이브리드 네트워크: NAT 환경에서의 IPsec 통신 문제</strong></summary>

- **문제:** 온프레미스 서버가 공유기(NAT) 뒤에 있어 AWS VPN Gateway와 IPsec 터널 연결 실패 (ESP 패킷 변조로 인식)
- **해결:** **NAT-T (Traversal)** 기술을 적용하여 IPsec 패킷을 `UDP 4500`으로 캡슐화, NAT 장비를 통과하도록 구성

</details>

<details>
<summary><strong>2️⃣ 비용 최적화: RDS 비용 절감과 HA 확보</strong></summary>

- **문제:** 제한된 예산($450) 내에서 엔터프라이즈급 DB 고가용성(Multi-AZ) 구현 불가능 (RDS 비용 과다)
- **해결:** EC2 인스턴스에 **PostgreSQL + Patroni + etcd** 클러스터를 직접 구축하여 RDS 대비 20% 비용 절감 및 HA 아키텍처 내재화

</details>

<details>
<summary><strong>3️⃣ 블록체인: 초기 Genesis 설정(EIP-155) 호환성 오류</strong></summary>

- **문제:** 배포 초기 `ChainId not supported` 에러 발생, 스마트 컨트랙트 배포 불가
- **해결:** Genesis 파일 내 하드포크 설정(`spuriousDragonBlock`)을 수정하고 네트워크 전체 리셋 및 키 재생성 자동화 스크립트 적용

</details>

<details>
<summary><strong>4️⃣ 실시간 거래: WebSocket 연결 끊김 (ALB Timeout)</strong></summary>

- **문제:** AWS ALB의 기본 Idle Timeout(60초)으로 인해 호가창 연결이 주기적으로 단절
- **해결:** Ingress Annotation을 통해 ALB Timeout을 3600초로 증설하고, Sticky Session을 적용하여 연결 안정성 확보

</details>

---

## 👨‍💻 팀원 소개

<div align="left">

<table width="100%">
  <tr>
    <td align="center" width="20%">
      <img src="https://github.com/kohtaewoo.png" width="120" height="120" style="border-radius:50%;"/><br/>
      <b>고태우</b><br/>
      <sub>PM / Infra</sub><br/>
      <sub><a href="https://github.com/kohtaewoo">@kohtaewoo</a></sub>
    </td>
    <td align="center" width="20%">
      <img src="https://github.com/LeeJoEun-01.png" width="120" height="120" style="border-radius:50%;"/><br/>
      <b>이조은</b><br/>
      <sub>PL / FE / BE</sub><br/>
      <sub><a href="https://github.com/LeeJoEun-01">@LeeJoEun-01</a></sub>
    </td>
    <td align="center" width="20%">
      <img src="https://github.com/kkangsol.png" width="120" height="120" style="border-radius:50%;"/><br/>
      <b>강한솔</b><br/>
      <sub>FE / BE</sub><br/>
      <sub><a href="https://github.com/kkangsol">@kkangsol</a></sub>
    </td>
    <td align="center" width="20%">
      <img src="https://github.com/GIHYUN-LEE.png" width="120" height="120" style="border-radius:50%;"/><br/>
      <b>이기현</b><br/>
      <sub>Blockchain / BE / Infra</sub><br/>
      <sub><a href="https://github.com/GIHYUN-LEE">@GIHYUN-LEE</a></sub>
    </td>
    <td align="center" width="20%">
      <img src="https://github.com/Gill010147.png" width="120" height="120" style="border-radius:50%;"/><br/>
      <b>황병길</b><br/>
      <sub>FE / Blockchain / Infra</sub><br/>
      <sub><a href="https://github.com/Gill010147">@Gill010147</a></sub>
    </td>
  </tr>
</table>

<img width="1920" height="1080" alt="IPiece- 최종" src="https://github.com/user-attachments/assets/d33752f4-459a-45c2-a0cd-5b626a27dca3" />


---

## 🔗 리포지토리 링크

본 프로젝트는 마이크로서비스 및 하이브리드 인프라 구조를 반영하여 기능별로 리포지토리를 분리해 관리하고 있습니다.

| 영역 | 설명 | 링크 |
| :--- | :--- | :---: |
| 🎨 **Frontend** | • **Next.js** 기반의 투자자용 웹 애플리케이션입니다.<br>• **WebSocket**을 통한 실시간 호가/차트 및 포트폴리오 시각화 기능을 포함합니다.<br>• `shadcn/ui`와 `Tailwind CSS`를 활용한 반응형 UI를 구현했습니다. | [IPiece-web](https://github.com/Woori-FISA-Go/IPiece-web) |
| ⚙️ **Backend** | • **Spring Boot** 기반의 핵심 비즈니스 로직 및 REST API 서버입니다.<br>• 회원/자산 관리, **주식형 호가 매칭 엔진**, 블록체인 연동을 담당합니다.<br>• **EC2(PostgreSQL HA)** 및 외부 시스템과의 데이터 정합성을 관리합니다. | [IPiece-server](https://github.com/Woori-FISA-Go/IPiece-server) |
| ⛓️ **Blockchain** | • **Hyperledger Besu** 프라이빗 네트워크 구성 및 설정 파일입니다.<br>• STO 발행, 배당, 소각을 위한 **Solidity 스마트 컨트랙트** 원본을 포함합니다.<br>• 제네시스 설정 및 노드 키 관리 스크립트가 포함되어 있습니다. | [IPiece-blockchain](https://github.com/Woori-FISA-Go/IPiece-blockchain) |
| 🏗️ **Infrastructure** | • **AWS & On-Premise** 하이브리드 환경 구성을 위한 **Terraform** 코드입니다.<br>• **ArgoCD** 연동을 위한 **Kubernetes Manifests (Helm/Kustomize)** 파일입니다.<br>• 구축 가이드 및 트러블슈팅 문서가 정리되어 있습니다. | [IPiece-infra](https://github.com/Woori-FISA-Go/ipiece-manifests) |

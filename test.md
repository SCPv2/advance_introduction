# I. DevOps를 위한 클라우드 인프라 환경 구성 (Configuring Cloud Infrastructure for DevOps Service)

## 1. 학습 목표

## 2. 관련 용어

## 3. 구현 아키텍처

## 4. VPC 및 Internet Gateway 생성

### 🌐 VPC 생성

- 콘솔 경로 : 모든 상품 → Networking → VPC → VPC 
  
  📝 VPC명 : `VPCdevops`

### 🌐 Interenet Gateway 생성 

- 콘솔 경로 : 모든 상품 → Networking → VPC → Internet Gateway  

  🔗 VPC : `VPCdevops` 

  🔗 구분 : `Internet Gateway`

  ✅ Firewall 사용 : `사용` 체크

  ✅ Firewall 로깅 여부 : `사용` 체크 안함 
  ````
  ❗실습 목적에 집중하기 위해 사용하지 않지만, 실제 적용할 때는 사용을 권장합니다.
  ````

## 5. 서브넷 및 NAT Gateway 생성

### 🌐 서브넷 생성

- 콘솔 경로 : 모든 상품 → Networking → VPC → 서브넷

- **Bastion Host용 서브넷**

  🔗 VPC : `VPCdevops` 

  🔗 사용 용도 : `일반 / Public`

  📝 서브넷명 : `bastionSBN`

  📝 IP 대역 : `10.1.1.0/24`
  
- **Kubernetes Cluster용 서브넷**

  🔗 VPC : `VPCdevops` 

  🔗 사용 용도 : `일반 / Private`

  📝 서브넷명 : `k8sSBN`

  📝 IP 대역 : `10.1.2.0/24`  

### 🌐 NAT Gateway 생성

- 콘솔 경로 : 모든 상품 → Networking → VPC → NAT Gateway  

  🔗 VPC : `VPCdevops` 

  🔗 서브넷 : `k8sSBN`

  🔗 NAT Gateway용 IP : `자동 할당`  

## 6. Public DNS, Load Balancer 생성

### 🌐 Public IP 생성

- 콘솔 경로 : 모든 상품 → Networking → VPC → Public IP    

  🔗 구분 : `Internet Gateway` 

### 🌐 Public DNS 생성

- 콘솔 경로 : 모든 상품 → Networking → DNS    

  🔗 용도구분 : `Public`

  📝 등록할 도메인명 : 중복되지 않는 도메인을 입력하여 등록

  📝 등록인명, 등록인 이메일, 등록인 주소, 전화번호는 사용자 정보를 입력

### 🌐 Public DNS 레코드 생성

- 콘솔 경로 : 모든 상품 → Networking → DNS → (생성한 도메인 클릭) → 레코드 → 레코드 추가

  🔗 유형 : `A`

  📝 이름 : `web`  

  📝 값 : 앞서 예약한 Public IP 주소 입력

  📝 TTL : `300`

### 🌐 Load Balancer 생성

- 콘솔 경로 : 모든 상품 → Networking → Load Balancer

  📝 Load Balancer명 : `LBweb`

  🔗 VPC : `VPCdevops`

  🔗 크기 : `SMALL`

  📝 LB 서비스 IP 대역 : `10.1.0.0/27`

  ✅ Firewall 사용 : `사용` 체크 안함  
  ````
  ❗실습 목적에 집중하기 위해 사용하지 않지만, 실제 적용할 때는 사용을 권장합니다.
  ````

  ✅ Firewall 로깅 여부 : `사용` 체크 안함   
  ````
  ❗실습 목적에 집중하기 위해 사용하지 않지만, 실제 적용할 때는 사용을 권장합니다.
  ````

## 7. File Storage 생성

### 🌐 File Storage 생성

- 콘솔 경로 : 모든 상품 → Storage → File Storage(New)

  📝 Volume명 : `k8spvc`

  🔗 디스크 유형 : `HDD`

  🔗 프로토콜 : `NFS`

## 8. Firewall, Security Group 구성

### 🌐 Internet Gateway Firewall 규칙 입력

- 콘솔 경로 : 모든 상품 → Networking → Firewall → FW_IGW_VPCdevops → 규칙 → [규칙 일괄 입력]  

  📄 규칙 일괄 입력 : [Internet Gateway Firewall rule](./igw_firewall.xlsx)  

  ````
  ⚠️ 규칙 일괄 입력 후 규칙 활성화가 필요합니다.
  ````
  
### 🌐 Security Group 생성 및 규칙 입력

- 콘솔 경로 : 모든 상품 → Networking → Security Group 

- **Bastion Host용 Security Group**

  📝 Security Group명 : `bastionSG`

  🔗 VPC : `VPCdevops` 

  ✅ 로깅 여부 : `사용` 체크 안함  
  ````
  ❗실습 목적에 집중하기 위해 사용하지 않지만, 실제 적용할 때는 사용을 권장합니다.
  ````

  📄 규칙 일괄 입력 : [Bastion Security Group rule](./bastion_security_group.xlsx)    
  
- **Kubernetes Cluster용 Security Group**

  📝 Security Group명 : `k8sSG`

  🔗 VPC : `VPCdevops` 

  ✅ 로깅 여부 : `사용` 체크 안함  
  ````
  ❗실습 목적에 집중하기 위해 사용하지 않지만, 실제 적용할 때는 사용을 권장합니다.
  ````
  
  📄 규칙 일괄 입력 : [Kubernetes Cluster Security Group rule](./k8s_security_group.xlsx)  

## 9. Bastion Host 생성

### 🌐 Bastion Host 생성

- 콘솔 경로 : 모든 상품 → Compute → Virtual Server

  🔗 이미지 선택 : `Rocky`

  🔗 서버 수 : `1`

  🔗 상품 유형 : 서버 타입 `Standard` | `s1v1m2` 약정기간 `None`

  📝 Block Storage : 

    | 기본 OS `bastionblock` | `100`GB   

    | `볼륨 암호화` 체크 안함     
    
    | 추가 `사용`  체크 안함  

  ✅ Placement Group : `사용` 체크안함

  ✅ Anti-affinity : `사용` 체크안함

  ✅ Deletion protection : `사용` 체크안함  
  
  📝 서버 Key Pair : `mykey`

  🔗 적용 정책 : `서버별 설정`

  📝 서버명 : `bastion`

  🔗 네트워크 설정  

  | VPC : `VPCdevops`  

  | 일반 서브넷 : `bastionDBN(Public)

  | IP : `자동생성`

  | NAT : `사용` 체크  | NAT IP : `자동 생성`

  |로컬 서브넷 : `사용` 체크안함

  🔗 Security Group : `bastionSG`

## 10. Kubernetes Cluster 및 환경 구성

### 🌐 Kubernetes Cluster 생성

- 콘솔 경로 : 모든 상품 → Container → Kubernetes Engine → 클러스터

  📝 클러스터명 : `k8sdevops`

  🔗 제어영역 설정 

  | Kubernetes 버전 : `v1.32.8(default)`  

  ````
  ⚠️ default로 설정되어 있는 최신 버전을 선택하고, 선택한 버전을 기록합니다. 다음 차시에 Kubernetes Client를 구성할 때 이 버전에 맞는 클라이언트를 선택해야 합니다.  
  ````

  | 프라이빗 엔드포인트 접근 제어 : " `사용` 체크  | 접근 허용 리소스 :  `bastion`

  | 퍼블릭 엔드포인트 접근 제어 : " `사용` 체크안함  

  | 제어영역 로깅 : " `사용` 체크안함

  🔗 네트워크 설정  

  | VPC : `VPCdevops`  

  | 서브넷 : `k8sSBN|PRIVATE`  

  | Security Group : `k8sSG`  

  | Load Balancer : `사용` 체크 | `LBweb` 

  🔗 File Storage 설정  

  | 기본 Volume (NFS) : `k8spvc_&&&&&&`  

  | 추가 Volume (CIFS) : `사용` 체크 안함 

### 🌐 Kubernetes Cluster 노드 풀 생성

- 콘솔 경로 : 모든 상품 → Container → Kubernetes Engine → 클러스터 → k8sdevops → 노드 풀 → 노드 풀 추가

  📝 노드 풀 : `nodedevops`

  🔗 서버 유형 : `Standard`

  🔗 서버 타입 : `s1v4m8(vCPU 4 | Memory 8G)`

  🔗 서버 OS : `Ubuntu 22.04 (Kubernetes)`

  🔗 Block Storage : `SSD` | `100` GB | `볼륨 암호화` 선택 안함

  🔗 노드 풀 자동 확장/축소 : `미사용`

  📝 노드 수 : `2`

  🔗 노드 자동 복구 : `미사용` 

### 🌐 Ingress Controller 생성

- 콘솔 경로 : 모든 상품 → Container → Kubernetes Apps

  🔗 Kubernetes Cluster : `k8sdevops`

  📝 Namespace : `신규 Namespace 생성` | `ingress`

  🔗 App 선택 : `Ingress NGINX Controller Community`
 
  📝 이름 : `ingress-controller`

  🔗 isDefaultIngressClass : `true`
  
  ✅ PVC : `사용` 

### 🌐 Ingress Controller를 Load Balancer에 연결

- 콘솔 경로 : 자원 관리 → Container → Kubernetes Engine → 서비스 및 인그레스 → k8sdevops | `ingress` 조회 → `ingress-controller` 클릭 → YAML

```
apiVersion: v1
kind: Service
metadata:
  annotations:
    meta.helm.sh/release-name: ingress-controller
    meta.helm.sh/release-namespace: ingress
  creationTimestamp: "2025-11-28T03:07:31Z"
  labels:
    app: ingress-controller
    app.kubernetes.io/component: controller
    app.kubernetes.io/instance: ingress-controller
    app.kubernetes.io/managed-by: Helm
    app.kubernetes.io/name: ingress-controller
    product: nginx-ingress-controller
    release: ingress-controller
    release-id: RELEASE-f0UE_XBOrglNI7Qbekuxcc
    sdspaas.io/created-by: "15616"
    sdspaas.io/id: IMAGE-Xsdqdf55tPeQjZF6Pfrw0p
    sdspaas.io/managed-by: SDS_PaaS
    sdspaas.io/name: Ingress_NGINX_Controller_Community_1.14.0
    sdspaas.io/price: Free
    sdspaas.io/resource-kind: service
    sdspaas.io/version: 1.14.0
  name: ingress-controller
  namespace: ingress
  resourceVersion: "10927"
  uid: e5d0ceef-23e6-450e-884b-7368209291ec
spec:
  clusterIP: 172.20.154.101
  clusterIPs:
  - 172.20.154.101
  externalTrafficPolicy: Cluster
  internalTrafficPolicy: Cluster
  ipFamilies:
  - IPv4
  ipFamilyPolicy: SingleStack
  ports:
  - name: http
    nodePort: 31397
    port: 80
    protocol: TCP
    targetPort: http
  - name: https
    nodePort: 31591
    port: 443
    protocol: TCP
    targetPort: https
  selector:
    app: ingress-controller
    app.kubernetes.io/component: controller
    app.kubernetes.io/instance: ingress-controller
    app.kubernetes.io/name: ingress-controller
    release: ingress-controller
  sessionAffinity: None
  type: NodePort------------------------> LoadBalancer 로 수정     
status:
  loadBalancer: {}
```

### 🌐 Load Balancer에 Public IP 연결

- 콘솔 경로 : 자원 관리 → Networking → Load Balancer →  Load Balancer → LBweb → 연결된 자원 

LB 서비스 : `ske-ingre-&&&&-80` 선택

NAT IP : 앞서 생성한 Public IP(DNS에서 web 레코드에 연결했던 Public IP)를 연결














# II. DevOps Service를 위한 Tool 구성[1] (Creating toos for DevOps Service[1])

## 1. 학습 목표

## 2. 관련 용어

## 3. 구현 아키텍처

## 4. DevOps Service 생성

### 🌐 DevOps Service 생성

- 콘솔 경로 : 모든 상품 → DevOps Tools → DevOps Service

  📝 Tenant명 : `mydevops`

  📝 Tenant코드 : `mydevops01`

  🔗 Tenant 멤버 추가 : 필요한 사용자 추가

## 5. DevOps Code 생성

### 🌐 DevOps Code 생성

- 콘솔 경로 : 모든 상품 → DevOps Tools → DevOps Code

  📝 Tenant코드 : `mydevopscode01`

## 6. Container Registry 생성

### 🌐 Container Registry 생성

- 콘솔 경로 : 모든 상품 → Container → Container Registry

  📝 레지스트리명 : `mydevopscr`

  🔗 엔드포인트 : `프라이빗`

  ✅ 프라이빗 엔드포인트 접근 제어 : " `사용` 체크  
  | 프라이빗 접근 허용 리소스  
    Virtual Server `bastion` `ske-&&&&&...` `ske-&&&&&...`   
    DevOps Service `DevOps Service`

  ✅ DR 레지스트리 : " `사용` 체크 안함 

## 7. Bastion Host에 Kubernetes Client 구성

### 💻 Bastion Host 접속

- Lab PC : [Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) 설치 및 실행

  📝 접속 IP 주소 : bastion VM의 NAT IP
  
  📝 Login as : `vmuser`

  📝 운영체제 업데이트
  
   ```bash
   sudo dnf update -y
   ```

### 💻 Kubernetes Client 설치

- Lab PC(Putty) : Bastion Host

  📝 kubectl 다운로드   
    ⚠️ 앞에 차시에서 구성한 Kubernetes 버전에 따라 다운로드 주소 버전 수정

   ```bash
   curl -LO https://dl.k8s.io/release/v1.32.8/bin/linux/amd64/kubectl
   ```

  📝 실행 권한 부여

   ```bash
   chmod +x kubectl
   ```
  📝 시스템 경로로 파일 이동

   ```bash
   sudo mv kubectl /usr/local/bin/
   ```

  📝 설치 확인

  ```bash
  kubectl version --client

  # 실행 결과 확인
  # Client Version: v1.32.8
  # Kustomize Version: v5.5.0
  ```

### 💻 Kubernetes Cluster API 연결

- 콘솔 : 자원 관리 → Container → Kubernetes Engine → 클러스터 → k8sdevops

  💾 프라이빗 엔드포인트 : `관리자 kubeconfig 다운로드` 를 다운받아 파일 내용 복사 

- Lab PC(Putty) : Bastion Host

  📝 Config 파일 생성

  ```bash
  sudo mkdir ~/.kube
  sudo vi ~/.kube/config   # vi에서 i 타이핑으로 입력 상태 전환
  ```

  📝 vi에서 앞에서 복사한 kubeconfig 파일 내용 입력 후 저장 (vi에서 `:wq!` 엔터로 저장)

  ```bash
  kubectl version
 
  # 실행 결과 확인
  # Client Version: v1.32.8
  # Kustomize Version: v5.5.0
  # Server Version: v1.32.8-ske.p2
  ```
  ❌ Server Version: v1.32.8-ske.p2 표시에 에러가 발생하면,  
   Kubernetes Engine Console에서 프라이빗 엔드포인트 접근 허용 리소스에  bastion 등록 확인

  📝 클러스터 정보 확인

  ```bash
  kubectl cluster-info

  # 실행 결과 확인
  # Kubernetes control plane is running at https://k8sdevops-be353.ske.kr-west.scp-in.com:6443        CoreDNS is running at https://k8sdevops-be353.ske.kr-west.scp-in.com:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy       To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
  ```

  📝 노드 목록 확인

  ```bash
  kubectl get nodes

  # 실행 결과 확인
  # NAME                         STATUS   ROLES    AGE    VERSION
  # ske-nodedevops-&&&&&-&&&&1   Ready    <none>   137m   v1.32.8-ske.p3
  # ske-nodedevops-&&&&&-&&&&2   Ready    <none>   136m   v1.32.8-ske.p3
  ```

## 8. Bastion Host에서 Container Registry 연결 

### 💻 Docker 설치

- Lab PC(Putty) : Bastion Host

  📝 Docker 설치 스크립트 다운로드 및 실행(Docker가 설치되어 있지 않은 경우)

  ```bash
  sudo dnf config-manager --add-repo https://download.docker.com/linux/rhel/docker-ce.repo

  sudo dnf install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
  
  sudo systemctl start docker

  sudo systemctl enable --now docker
  ```

  📝 docker 그룹에 사용자 추가

  ```bash
  sudo usermod -aG docker $USER
  ```

  📝 로그아웃 후 재로그인 (또는 newgrp docker)

  ```bash
  newgrp docker
  ```

  📝 Docker 설치 확인

  ```bash
  docker version

  # 실행 결과 확인
  # Client: Docker Engine - Community
  # Version:           29.1.0
  # API version:       1.52
  # Go version:        go1.25.4
  # Git commit:        360952c
  # Built:             Thu Nov 27 16:45:28 2025
  # OS/Arch:           linux/amd64
  # Context:           default

  # Server: Docker Engine - Community
  #  Engine:
  #  Version:          29.1.0
  #  API version:      1.52 (minimum version 1.44)
  #  Go version:       go1.25.4
  #  Git commit:       710302e
  #  Built:            Thu Nov 27 16:42:11 2025
  #  OS/Arch:          linux/amd64
  #  Experimental:     false
  # containerd:
  #  Version:          v2.1.5
  #  GitCommit:        fcd43222d6b07379a4be9786bda52438f0dd16a1
  # runc:
  #  Version:          1.3.3
  #  GitCommit:        v1.3.3-0-gd842d771
  # docker-init:
  #  Version:          0.19.0
  #  GitCommit:        de40ad0
  ```

### 💻 Container Registry 연결

- 콘솔 경로 : 자원 관리 → Container → Container Registry → mydevopscr 

  📋 프라이빗 엔드포인트 주소 확인 : 예) mydevopscr-&&&&&&&&.scr.kr-west.scp-in.com


- Lab PC(Putty) : Bastion Host

  📝 Container Registry 로그인  
  ⚠️ 엔드포인트 주소 중 &&&&&&&& 은 실제 주소로 대체해서 사용해야 합니다.

  ```bash
  docker login mydevopscr-&&&&&&&&.scr.kr-west.scp-in.com

  # Username: AccessKey ID 입력
  # Password: Secret Access Key 입력
  ```

  📝 Container Registry 이미지 Push 테스트

## 8. Bastion Host에서 Container Registry 연결 

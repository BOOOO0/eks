# EKS

- ![image](./img/topology.png)

- 퍼블릭 클라우드 AWS에 인프라를 구축했습니다.

- 2개의 가용영역을 사용해 이중화하여 고가용성을 확보했습니다.

- 두개의 퍼블릭 서브넷을 생성했고 하나의 서브넷엔 NAT GW를 다른 서브넷에는 EKS의 마스터 노드 역할을 할 EC2 인스턴스를 생성했습니다.

- 4개의 프라이빗 서브넷을 생성했고 EKS 클러스터는 프라이빗 서브넷에 위치하도록 했습니다.

- RDS로 생성한 MySQL서버는 Multi-AZ 옵션을 사용해 다른 가용영역에 백업 DB를 가진 채로 프라이빗 서브넷에 생성되어 있습니다.

# Helm

- ![image](./img/DeployWithHelm.png)

- Helm Chart를 사용해서 어플리케이션을 배포했습니다.

- 기본 값들을 그대로 두고 values.yml의 image 항목만 미리 도커 허브에 push해둔 이미지를 명시해서 배포를 진행했습니다.

```yaml
image:
  repository: boooo0/api_server
  pullPolicy: IfNotPresent
  tag: v1.0
```

# Terraform

- Terraform으로 어플리케이션 배포를 제외한 나머지 인프라 구축을 자동화했습니다.

- VPC는 공식 문서에서 제공하는 모듈을 사용해서 코드를 줄이고 더 편리하게 구축을 할 수 있었습니다.

- EKS는 모니터링 구축을 염두에 두고 시계열 데이터를 저장할 볼륨 스토리지 연결을 가능하게 하는 AddOn(CSI-driver)을 포함하여 구축했습니다.

- EKS를 구축할때 필요한 IAM 정책과 정책의 연결도 모두 Terraform을 사용해서 진행했습니다.

# 모니터링

- Helm을 사용해서 Prometheus + Grafana를 설치하고 모니터링을 구축했습니다.

- 대시보드는 Grafana에서 기본으로 제공되는 대시보드를 Import해서 EKS 클러스터 내 모든 파드, 컨테이너의 CPU, 메모리, 스토리지, 네트워크 I/O를 모니터링 할 수 있도록 했습니다.

- ![image](./img/PrometheusGrafana.png)
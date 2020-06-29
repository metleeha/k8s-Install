# Sonarqube on Kubernetes

SonarQube는 20개 이상의 프로그래밍 언어로 코드 결함과 버그, 보안 취약점 등을 탐지하고 정형화된 패턴에 대해 정적 분석을 통해 코드 수정과 자동 리뷰를 지원합니다. 
Kubernetes Cluster에 DevOps 도구들을 설치하는 과정 중 Sonarqube를 설치하는 방법을 알아봅시다. IBM Cloud 에서 제공하는 IKS를 이용할 예정입니다. 

## 사전 준비
- Bash/ PowerShell + kubectl(IBM Cloud CLI 내장)
- PostgreSQL database
- Kubernetes Cluster(IBM Cloud Kubernetes Service)

## Sonarqube 데이터 저장을 위한 PersistentVolume/ PersistentVolumeClaim 생성 

```bash
kubectl create -f sonar-pv.yaml namespace=sonarqube
```
```bash
kubectl create -f snoar-pvc.yaml namespace=sonarqube
```

## Sonarqube 인스턴스 배포하기 
비밀번호 생성
```bash
kubectl create secret generic postgres-pwd --from-literal=password={my password} --namespace=sonarqube
```
```bash
kubectl create -f sonar-postgres-deployment.yaml 
kubectl create -f sonar-deployment.yaml 
kubectl create -f sonar-service.yaml
kubectl create -f sonar-postgres-service.yaml 
```

## 로컬에서 설치한 SonarQube 접속하기

다음 명령어를 통해 클러스터에 올린 sonarqube 서비스의 nodePort를 찾습니다. 

```bash
kubectl get service sonarqube --namespace=sonarqube --output yaml
```
클러스터의 외부노출 nodePort IP주소에 방금 찾은 포트 번호를 붙여서 접속합니다. 
크롬이나 파이어폭스에서 새로운 탭을 열고,
NodeExportIPAdress:{방금 찾은 노드포트 번호}로 접속한다.
예를 들어, `https://123.123.11.11:3002` 접속한다. 

## 지금까지 설치한 쿠버네티스 배포 삭제하기
```bash
kubectl delete deployment sonar-postgres
kubectl delete deployment sonarqube
```

## 작성 코드
- [Git Repo](https://github.ibm.com/Ha-Dong-Lee/kubernetes-sonarqube)
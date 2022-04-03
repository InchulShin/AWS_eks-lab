# CLEAN UP
수고하셨습니다. 축하합니다. 불필요한 과금을 피하기 위해이 실습에서 만든 리소스를 제거합시다.

eksctl에서 만든 IAM 역할을 삭제합니다.

```
eksctl delete iamserviceaccount --cluster ekshandson --namespace backend --name dynamodb-messages-fullaccess
```

Kubernetes 클러스터에 전개된 응용 프로그램을 Namespace 숙박과 함께 제거합니다. Service를 제거하여 Classic Load Balancer도 삭제됩니다.

```
kubectl delete namespace frontend backend
```

DynamoDB 테이블을 삭제합니다.

```
aws dynamodb delete-table --table-name 'messages'
```

DynamoDB에 대한 액세스를 허용하는 IAM 정책을 삭제합니다.

```
ACCOUNT_ID=$(aws sts get-caller-identity --output text --query Account)
aws iam delete-policy --policy-arn arn:aws:iam::${ACCOUNT_ID}:policy/dynamodb-messages-fullaccess
```

ECR 저장소를 제거합니다.

```
aws ecr delete-repository --repository-name frontend --force
aws ecr delete-repository --repository-name backend --force
```

EKS 클러스터를 제거합니다.

```
eksctl delete cluster --name ekshandson
```

EKS 컨트롤 플레인의 삭제는 비동기 적으로 이루어집니다. 클러스터를 만들 때와 마찬가지로, 삭제도 완전히 완료 될 때까지 다소 시간이 걸립니다 (약 10 분). eksctl의 출력 결과에서 삭제가 완료되지도 비동기 적으로 삭제 처리에 실패 할 가능성도 있으므로 관리 콘솔에서 CloudFormation 서비스를 열고 eksctl-ekshandson-로 시작하는 이름의 스택 삭제가 완료되었는지 확인 하도록하십시오.

관리 콘솔에서 Cloud9 환경 (ekshandson)을 삭제합니다.

Cloud9에 부여하기 위해 만든 IAM 역할 (ekshandson-admin)을 삭제하십시오.

수고하셨습니다. 이상이 실습은 모두 종료합니다.

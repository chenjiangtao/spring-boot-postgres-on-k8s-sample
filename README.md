#postgre using
## 登陆命令
psql postgres://admin:c1oudc0w@localhost:5432/postgres 


## 1.打包编译应用
docker build -t chenjiangtao/spring-sample-web-ui:v1 .
docker build -t chenjiangtao/spring-sample-web-ui:v2 .

## 2.创建数据库
kubectl create -f specs/postgres.yml


## 3.起动spring boot（添加host）
kubectl create configmap hostname-config --from-literal=postgres_host=$(kubectl get svc postgres -o jsonpath="{.spec.clusterIP}")
   


## 4.开端口
kubectl expose deployment spring-boot-postgres-sample --type=LoadBalancer --port=8080
   
## 5.缩放
kubectl scale deployment spring-boot-postgres-sample --replicas=3

## 6.更新
kubectl set image deployment/spring-boot-postgres-sample spring-boot-postgres-sample=chenjiangtao/spring-boot-postgres-on-k8s:v2
   
## 7.删除所有
kubectl delete -f specs/spring-boot-app.yml
kubectl delete svc spring-boot-postgres-sample
kubectl delete cm hostname-config
kubectl delete -f specs/postgres.yml
   

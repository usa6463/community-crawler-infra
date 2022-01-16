# community-crawler-infra

kafka, ES, Airflow 등 community-crawler 프로젝트 인프라 구성을 위한 레포지토리

## airflow 

### Webserver secret key 세팅

- 시크릿 생성
```shell
kubectl create secret generic community-crawler-airflow-web-key --from-literal="webserver-secret-key=$(python3 -c 'import secrets; print(secrets.token_hex(16))')"
```
- values.yaml에서 시크릿 연동
```yaml
webserverSecretKeySecretName: community-crawler-airflow-web-key
# where the random key is under `webserver-secret-key` in the k8s Secret
```

- 참고 : https://airflow.apache.org/docs/helm-chart/stable/production-guide.html#webserver-secret-key

### git sync

- repo 설정 방법 (github에서 access token 설정 필요)
```yaml
gitSync:
  repo: https://{git계정명}:{git access_token}@github.com/{git계정명}/community-crawler-scheduler.git
```
- git sync 실패시 scheduler pod가 실패뜬다. 이 때 디버깅 방법
```shell
kubectl logs community-crawler-airflow-scheduler-6465d575d8-hgptl -n default -c git-sync-init
```



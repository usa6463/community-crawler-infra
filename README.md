# community-crawler-infra

kafka, ES, Airflow 등 community-crawler 프로젝트 인프라 구성을 위한 레포지토리

## airflow 

### Webserver secret key 세팅

- 시크릿 생성
```shell
kubectl create secret generic my-webserver-secret --from-literal="webserver-secret-key=$(python3 -c 'import secrets; print(secrets.token_hex(16))')"
```
- values.yaml에서 시크릿 연동
```yaml
webserverSecretKeySecretName: my-webserver-secret
# where the random key is under `webserver-secret-key` in the k8s Secret
```

- 참고 : https://airflow.apache.org/docs/helm-chart/stable/production-guide.html#webserver-secret-key

# 1. 데이터 과학 소개 및 환경 구축 

<details> <summary> 1. ELK 설치</summary>

## 1. ELK 설치

### 설치
```
brew install elastic/tap/kibana-full
brew install elastic/tap/elasticsearch-full
brew install elastic/tap/filebeat-full
brew install elastic/tap/logstash-full
```

### 서비스 실행
```
brew services start elastic/tap/kibana-full
brew services start elastic/tap/elasticsearch-full
brew services start elastic/tap/filebeat-full
brew services start elastic/tap/logstash-full
```

### ElasticSearch 실행 확인
- localhost:9200 접속


</details>
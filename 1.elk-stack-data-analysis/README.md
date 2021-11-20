
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

# 2. 엘라스틱 서치 (ElasticSearch)

<details> <summary> 1. 엘라스틱서치 기본 개념 정리 </summary>

## 1. 엘라스틱서치 기본 개념 정리

![image](https://user-images.githubusercontent.com/28394879/142723353-96101d05-1892-4c90-bb43-22ef28343a5b.png)

- 왼쪽 데이터 형태로 들어왔을 때 엘라스틱 서치는 오른쪽 테이블 형태로 저장을 한다. 

### elastic search vs relational DB

![image](https://user-images.githubusercontent.com/28394879/142723405-0155e63a-e55a-44fc-ac53-b98643e9c6e2.png)

- 구글링을 할때 Text단위로 검색하는 것을 가정했을때 Elastic search를 사용했을 경우 훨씬 빠르게 데이터를 가져올 수 있다. 

### elastic search data structure 

![image](https://user-images.githubusercontent.com/28394879/142723499-fd98d73e-79f5-4796-b762-d4635af2481e.png)

- index 안에 type을 갖는다.
- type안에 여러개의 document를 갖는다.
- document들은 같은 property를 갖고 있는다. 

### elastic search vs relational DB 용어 차이
![image](https://user-images.githubusercontent.com/28394879/142723564-1c4b324b-dee6-48aa-9370-3f4fd14cbbd2.png)

![image](https://user-images.githubusercontent.com/28394879/142723595-7490990d-7de9-4a85-9c40-a1be17a5e750.png)
- Elastic Search는 REST API를 사용한다. 

![image](https://user-images.githubusercontent.com/28394879/142723609-be8f9292-c04c-4b0a-b7a7-41cebd693b77.png)

</details>

<details> <summary> 2. 엘라스틱서치 데이터 입력 조회 삭제 (GET, POST, PUT, DELETE) </summary>

</details>

<details> <summary> 3. 엘라스틱서치 데이터 업데이트 (UPDATE) </summary>

</details>

<details> <summary> 4. 엘라스틱서치 - 벌크 (Bulk) </summary>

</details>

<details> <summary> 5. 엘라스틱서치 매칭 (Mapping) </summary>

</details>

<details> <summary> 6. 엘라스틱서치 데이터 조회 (Search) </summary>

</details>

<details> <summary> 7. 엘라스틱서치 메트릭 어그리게이션 (Metric Aggregation) </summary>

</details>

<details> <summary> 8. 엘라스틱서치 버켓 어그리게이션 (Bucket Aggregation) </summary>

</details>
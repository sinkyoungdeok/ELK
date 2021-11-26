
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

## 2. 엘라스틱서치 데이터 입력 조회 삭제 (GET, POST, PUT, DELETE)

1. classes index가 있는지 조회 (아직은 생성안해서 조회 안되는게 맞음)
`curl -XGET http://localhost:9200/classes` 
2. 조회할 때 이쁘게 보기 
`curl -XGET http://localhost:9200/classes?pretty`  
3. index가 없다는 것을 확인했으니, 인덱스 생성 해보자.
`curl -XPUT http://localhost:9200/classes`
4. 생성된것을 조회
`curl -XGET http://localhost:9200/classes?pretty`    
```
{
  "classes" : {
    "aliases" : { },
    "mappings" : { },
    "settings" : {
      "index" : {
        "routing" : {
          "allocation" : {
            "include" : {
              "_tier_preference" : "data_content"
            }
          }
        },
        "number_of_shards" : "1",
        "provided_name" : "classes",
        "creation_date" : "1637410676372",
        "number_of_replicas" : "1",
        "uuid" : "sTOr1fWFTIe0JKaThOQ4LQ",
        "version" : {
          "created" : "7150299"
        }
      }
    }
  }
}
```
5. 생성한 index 지우기 
`curl -XDELETE http://localhost:9200/classes`
6. document 생성하기
```
curl -XPOST http://localhost:9200/classes/class/1/ -H 'Content-Type: application/json'  -d '
{"title":"Algorithm", "professor":"John"}'
```
index가 생성된 상태에서 해도 되고, 생성안된 상태에서 해도 된다.  
index를 생성 안된상태에서 하면, 알아서 index 생성까지 해준다.

7. 생성된것을 조회 
`curl -XGET http://localhost:9200/classes?pretty`    
**결과**  
```
{
  "classes" : {
    "aliases" : { },
    "mappings" : {
      "properties" : {
        "professor" : {
          "type" : "text",
          "fields" : {
            "keyword" : {
              "type" : "keyword",
              "ignore_above" : 256
            }
          }
        },
        "title" : {
          "type" : "text",
          "fields" : {
            "keyword" : {
              "type" : "keyword",
              "ignore_above" : 256
            }
          }
        }
      }
    },
    "settings" : {
      "index" : {
        "routing" : {
          "allocation" : {
            "include" : {
              "_tier_preference" : "data_content"
            }
          }
        },
        "number_of_shards" : "1",
        "provided_name" : "classes",
        "creation_date" : "1637411566405",
        "number_of_replicas" : "1",
        "uuid" : "HCTipXo6Stqya6SKmAXlsw",
        "version" : {
          "created" : "7150299"
        }
      }
    }
  }
}
``` 

8. json파일로 document 생성 
`curl -XPOST http://localhost:9200/classes/class/1/ -H 'Content-Type: application/json' -d @oneclass.json` 




</details>

<details> <summary> 3. 엘라스틱서치 데이터 업데이트 (UPDATE) </summary>

## 3. 엘라스틱서치 데이터 업데이트 (UPDATE)

1. 업데이트할 데이터를 위해서 document 생성
```
curl -XPOST http://localhost:9200/classes/class/1/ -H 'Content-Type: application/json' -d '{"title": "Algorithm", "professor": "John"}'
``` 

2. 1학점이라는 필드를 추가
```
curl -XPOST http://localhost:9200/classes/class/1/_update -H 'Content-Type: application/json' -d '{"doc" : {"unit" : 1}}'
``` 

3. 추가 된 것 확인
```
curl -XGET http://localhost:9200/classes/class/1?pretty
``` 

4. 학점을 1에서 2로 수정
```
curl -XPOST http://localhost:9200/classes/class/1/_update -H 'Content-Type: application/json' -d '{"doc" : {"unit" : 2}}'
```

5. 학점에 script로 +5 시키기
```
curl -XPOST http://localhost:9200/classes/class/1/_update -H 'Content-Type: application/json' -d '{"script": "ctx._source.unit += 5"}'
```

</details>

<details> <summary> 4. 엘라스틱서치 - 벌크 (Bulk) </summary>

## 4. 엘라스틱서치 - 벌크 (Bulk)

1. CLASSES.JSON 을 bulk
```
curl -XPOST http://localhost:9200/_bulk --data-binary  @classes.json -H 'Content-Type: application/json'
``` 

2. bulk 확인 
```
curl -XGET http://localhost:9200/classes/class/2?pretty
```

</details>

<details> <summary> 5. 엘라스틱서치 매칭 (Mapping) </summary>

## 5. 엘라스틱서치 매칭 (Mapping)

- 매핑은 데이터베이스의 스키마와 동일하다. 
- 엘라스틱서치는 매핑없이도 insert 할 수 있다. 
- 실제 일할때는 매핑 없이 데이터를 넣는것은 상당히 위험한 일이다. 
  - 매핑이 없다면 날짜를 문자열로 인식할 수 있다. 
  - 매핑이 없다면 숫자를 넣을때도 문자열로 인식할 수 있다. 
  - 그렇다면, 평균을 낼 때도 문자열로 인식되면 잘 안될 수 있다. 
- 데이터를 넣을때에는 매핑을 먼저 추가해야 된다. 


1. 인덱스 생성
```
curl -XPUT 'http://localhost:9200/classes'
``` 
2. 매핑 생성
```
curl -XPUT 'http://localhost:9200/classes/class/_mapping' -H 'Content-Type: application/json' -d @classesRating_mapping.json
``` 
3. CLASSES.JSON 을 bulk
```
curl -XPOST http://localhost:9200/_bulk --data-binary  @classes.json -H 'Content-Type: application/json'
``` 
4. bulk 확인 
```
curl -XGET http://localhost:9200/classes/class/2?pretty
```
- 지금 실습에서는 mapping없이 bulk했을 때와의 차이점은 크게 없는 것 같다. (데이터 형식을 생각보다 알아서 잘 매칭해주는 듯함)
- 하지만, 데이터형식을 언제 다르게 해줄지 모르니, 항상 mapping을 먼저 생성해주는 것이 좋은 것 같다. 

</details>

<details> <summary> 6. 엘라스틱서치 데이터 조회 (Search) </summary>

## 6. 엘라스틱서치 데이터 조회 (Search)

1. 데이터 생성
```
curl -XPOST http://localhost:9200/_bulk --data-binary @simple_basketball.json -H 'Content-Type: application/json'
``` 

2. 데이터 확인
```
curl -XGET http://localhost:9200/basketball/record/_search?pretty
``` 

3. uri옵션으로 데이터 확인
```
curl -XGET http://localhost:9200/basketball/record/_search?q=points:30&pretty
```  

4. REQUEST BODY 로 데이터 확인 
```
curl -XGET http://localhost:9200/basketball/record/_search?pretty -H 'Content-Type: application/json' -d '
{ 
  "query": {
    "term" : {"points": 30}
  }
}'
``` 

</details>

<details> <summary> 7. 엘라스틱서치 메트릭 어그리게이션 (Metric Aggregation) </summary>

## 7. 엘라스틱서치 메트릭 어그리게이션 (Metric Aggregation)

- 평균, 합산 등을 구하는 것이 Metric Aggregation

1. 데이터 생성
```
curl -XPOST http://localhost:9200/_bulk --data-binary @simple_basketball.json -H 'Content-Type: application/json'
``` 

2. average aggregation 
```
curl -XGET http://localhost:9200/_search?pretty --data-binary @avg_points_aggs.json -H 'Content-Type: application/json'
```  

3. max aggregation 
```
curl -XGET http://localhost:9200/_search?pretty --data-binary @max_points_aggs.json -H 'Content-Type: application/json'
```  

4. min aggregation 
```
curl -XGET http://localhost:9200/_search?pretty --data-binary @min_points_aggs.json -H 'Content-Type: application/json'
```  

5. sum aggregation 
```
curl -XGET http://localhost:9200/_search?pretty --data-binary @sum_points_aggs.json -H 'Content-Type: application/json'
```  

6. stats aggregation (지금까지 한 모든 aggregation을 한번에)
```
curl -XGET http://localhost:9200/_search?pretty --data-binary @stats_points_aggs.json -H 'Content-Type: application/json'
```  

</details>

<details> <summary> 8. 엘라스틱서치 버켓 어그리게이션 (Bucket Aggregation) </summary>

</details>
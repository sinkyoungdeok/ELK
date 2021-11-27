
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

## 8. 엘라스틱서치 버켓 어그리게이션 (Bucket Aggregation)

- group by같은 기능을 사용하는것이 Bucket Aggregation 
- Bucket key로 식별되는 여러 Bucket 쿼리 컨텍스트(테이블)에서 정의된 문제의 데이터를 분할하며 Document를 그룹화하는 것이다. 

1. INDEX 생성
```
curl -XPUT localhost:9200/basketball
``` 

2. mapping 적용
```
curl -XPUT 'localhost:9200/basketball/record/_mapping?include_type_name=true&pretty' -H'Content-Type: application/json' -d @basketball_mapping.json
``` 

3. 데이터 삽입(bulk)
```
curl -XPOST http://localhost:9200/_bulk?pretty -H'Content-Type: application/json' --data-binary @twoteam_basketball.json
``` 

4. bucket aggregation
```
curl -XGET http://localhost:9200/_search?pretty --data-binary @terms_aggs.json -H 'Content-Type: application/json'
``` 

5. 팀별로 스코어 통계
```
curl -XGET http://localhost:9200/_search?pretty --data-binary @stats_by_team.json -H 'Content-Type: application/json'
``` 

</details>


# 3. 키바나

<details> <summary> 1. 키바나 설치 확인 </summary>

## 1. 키바나 설치 확인

http://localhost:5601 접속 확인

</details>

<details> <summary> 2. 키바나 매니지먼트 (Kibana management) </summary>

## 2. 키바나 매니지먼트 (Kibana management)

1. 이전에 진행한 index 제거 
```
curl -XDELETE localhost:9200/basketball
``` 

2. 새로 index 생성
```
curl -XPUT localhost:9200/basketball
``` 

3. mapping정보 입력
```
curl -XPUT 'localhost:9200/basketball/record/_mapping?include_type_name=true&pretty' -H'Content-Type: application/json' -d @basketball_mapping.json
```

4. 데이터 삽입(bulk)
```
curl -XPOST http://localhost:9200/_bulk?pretty -H 'Content-Type: application/json' --data-binary @bulk_basketball.json
``` 

5. kibana 접속 후 index패턴 생성
![image](https://user-images.githubusercontent.com/28394879/143597413-f6c18934-791a-4cf8-a1fe-820df28478f3.png)

6. 데이터가 kibana에 잘 인식됐는지 확인
![image](https://user-images.githubusercontent.com/28394879/143597714-28cfc916-7afa-419b-9189-c9a7296ae569.png)




</details>

<details> <summary> 3. 키바나 디스커버(Kibana discover) </summary>

## 3. 키바나 디스커버(Kibana discover)

1. Kibana에서 디스커버 탭으로 들어가서, 2016년까지의 데이터 (2021년기준으론 약 6년전까지로 설정)를 검색
```
http://localhost:5601/app/discover#/?_g=(filters:!(),refreshInterval:(pause:!t,value:0),time:(from:now-8y,to:now))&_a=(columns:!(),filters:!(),index:'3be87030-4ec7-11ec-b89d-c5bc45c36b0c',interval:auto,query:(language:kuery,query:''),sort:!(!(submit_date,desc)))
``` 

2. 2016~2017까지의 데이터 보기 
```
http://localhost:5601/app/discover#/?_g=(filters:!(),refreshInterval:(pause:!t,value:0),time:(from:'2015-12-31T15:40:28.184Z',to:'2017-10-31T15:40:53.079Z'))&_a=(columns:!(),filters:!(),index:'3be87030-4ec7-11ec-b89d-c5bc45c36b0c',interval:auto,query:(language:kuery,query:''),sort:!(!(submit_date,desc)))
```
![image](https://user-images.githubusercontent.com/28394879/143604521-1ea36012-c914-48a5-9a47-c63880ed2d66.png)

3. filter로 "Stephen Curry"데이터만 보기
![image](https://user-images.githubusercontent.com/28394879/143604757-18e90696-8f7a-49e2-b7a4-4e410b0eb30e.png)

4. toggle 버튼으로 원하는 필드 위주로 보기
![image](https://user-images.githubusercontent.com/28394879/143604953-7d5ee4fa-5a15-40c5-99b0-a6cb5d14b1d4.png)

5. 정렬 버튼으로 원하는 필드로 정렬 하기 
![image](https://user-images.githubusercontent.com/28394879/143605225-845e7cb7-4f13-4287-9d19-0812c24e3e1c.png)


</details>

<details> <summary> 4. 키바나 비주얼라이즈(Kibana Visualize) - 막대 그래프, 파이차트 </summary>

## 4. 키바나 비주얼라이즈(Kibana Visualize) - 막대 그래프, 파이차트

### 창 들어가는방법
1. Analytics에서 Visualize Library 클릭
2. Create new visualization 클릭
3. Explore options 클릭
4. Vertical Bar(막대그래프) 또는 파이차트 검색 후 클릭

- 막대 그래프
![image](https://user-images.githubusercontent.com/28394879/143665723-6e0898bd-93a8-454e-9ca3-4ee30d1e932f.png)
`http://localhost:5601/app/visualize#/create?type=histogram&indexPattern=3be87030-4ec7-11ec-b89d-c5bc45c36b0c&_g=(filters:!(),refreshInterval:(pause:!t,value:0),time:(from:'2015-01-27T02:05:10.924Z',to:now))&_a=(filters:!(),linked:!f,query:(language:kuery,query:''),uiState:(),vis:(aggs:!((enabled:!t,id:'2',params:(customLabel:avg,field:points),schema:metric,type:avg),(enabled:!t,id:'3',params:(field:name,missingBucket:!f,missingBucketLabel:Missing,order:desc,orderBy:'2',otherBucket:!f,otherBucketLabel:Other,size:5),schema:segment,type:terms)),params:(addLegend:!t,addTimeMarker:!f,addTooltip:!t,categoryAxes:!((id:CategoryAxis-1,labels:(filter:!t,show:!t,truncate:100),position:bottom,scale:(type:linear),show:!t,style:(),title:(),type:category)),detailedTooltip:!t,grid:(categoryLines:!f),labels:(show:!f),legendPosition:right,maxLegendLines:1,palette:(name:default,type:palette),radiusRatio:0,seriesParams:!((circlesRadius:3,data:(id:'2',label:avg),drawLinesBetweenPoints:!t,interpolate:linear,lineWidth:2,mode:stacked,show:!t,showCircles:!t,type:histogram,valueAxis:ValueAxis-1)),thresholdLine:(color:%23E7664C,show:!f,style:full,value:10,width:1),times:!(),truncateLegend:!t,type:histogram,valueAxes:!((id:ValueAxis-1,labels:(filter:!f,rotate:0,show:!t,truncate:100),name:LeftAxis-1,position:left,scale:(mode:normal,type:linear),show:!t,style:(),title:(text:avg),type:value))),title:'',type:histogram))`


- 파이차트
![image](https://user-images.githubusercontent.com/28394879/143665340-4669b0b3-744d-45aa-95d9-719f9636e4a7.png)
`http://localhost:5601/app/visualize#/create?type=pie&indexPattern=3be87030-4ec7-11ec-b89d-c5bc45c36b0c&_g=(filters:!(),refreshInterval:(pause:!t,value:0),time:(from:'2015-01-27T02:05:10.924Z',to:now))&_a=(filters:!(),linked:!f,query:(language:kuery,query:''),uiState:(vis:(legendOpen:!f)),vis:(aggs:!((enabled:!t,id:'1',params:(field:points),schema:metric,type:sum),(enabled:!t,id:'2',params:(field:team,missingBucket:!f,missingBucketLabel:Missing,order:desc,orderBy:'1',otherBucket:!f,otherBucketLabel:Other,size:5),schema:segment,type:terms)),params:(addLegend:!f,addTooltip:!t,distinctColors:!f,isDonut:!t,labels:(last_level:!f,percentDecimals:2,position:default,show:!t,truncate:100,values:!t,valuesFormat:percent),legendPosition:right,maxLegendLines:1,nestedLegend:!f,palette:(name:default,type:palette),truncateLegend:!t,type:pie),title:'',type:pie))`


</details>

<details> <summary> 5. 키바나 비주얼라이즈(Kibana Visualize) - 타일 맵, 지포에 표시 </summary>

## 5. 키바나 비주얼라이즈(Kibana Visualize) - 타일 맵, 지포에 표시

1. classes index생성 
```
curl -XPUT http://localhost:9200/classes
``` 

2. mapping 정보 등록(매핑을 통해서 단순 스트링이아닌, 지도에 표시할 수 있는 값임을 나타내주어야 함)
```
curl -XPUT http://localhost:9200/classes/class/_mapping?include_type_name=true&pretty -H 'Content-Type: application/json' -d @classesRating_mapping.json
``` 

3. bulk insert
```
curl -XPOST http://localhost:9200/_bulk?pretty -H 'Content-Type: application/json' --data-binary @classes.json
```

4. 데이터 확인
```
curl -XGET http://localhost:9200/classes/class/1/?pretty
```

5. kibana에서 Management의 StackManagement 클릭
6. Index Pattern 클릭
7. Create index Pattern 클릭 
8. classes* index 생성
9. Visualize에서 Maps 클릭
10. Add layer -> Clusters and grids
11. index pattern를 classes로 적용

![image](https://user-images.githubusercontent.com/28394879/143666574-1cf00a7c-0e66-4fea-9263-dea73cbc1380.png)


</details>


<details> <summary> 6. 키바나 대시보드(Dashboard) </summary>

</details>


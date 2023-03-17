# 230317 created
### 아파치 하둡(Apache Hadoop, High-Availability Distributed Object-Oriented Platform)
### <br/><br/><br/>

## basic
### 아래 블로그들을 참고하였다.
- https://han-py.tistory.com/361
- https://m.blog.naver.com/acornedu/222069158703
### hadoop 은 데이터를 분산하여 저장하는 아파치 제단에서 개발한 시스템이다.
- 데이터를 분산해서 저장하니 어디어디에 어떻게 분산되서 저장되었는지 정보를 관리하는 metadata 가 있다.
- 하둡은 HDFS(Hadoop Distributed FileSystem) 이라고 부른다.
  - 데이터를 블록 단위로 나누어 저장한다. 
  - HDFS는 읽기 중심을 목적으로 만들어 졌기 때문에 파일의 수정은 지원하지 않는다. (읽는 속도를 높인다.)
- map-reduce
  - map : 데이터가 어디 있는지 매핑
  - reduce : 각각의 분산된 데이터를 통합하여 읽음. 여러 개를 한 번에 읽는다는 관점에서 읽는 횟수를 줄였다라고 하여 reduce 라고 함.
### HDFS 와 map-reduce 는 master 와 그 아래 child process 개념으로 slave 가 있다.
### HDFS 는 데이터를 저장 / map-reduce 는 데이터를 읽기
#### ![image](https://user-images.githubusercontent.com/62974484/225822511-db5ed846-76b3-4e76-b40b-b653746af40c.png)
### <br/><br/><br/>

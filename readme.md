# 230317 created
### 아파치 하둡(Apache Hadoop, High-Availability Distributed Object-Oriented Platform)
### <br/><br/><br/>

## basic
### 아래 블로그들을 참고하였다.
- https://han-py.tistory.com/361
- https://m.blog.naver.com/acornedu/222069158703
### hadoop 은 데이터를 분산하여 저장하는 아파치 제단에서 개발한 시스템이다.
### 간략한 정보는 아래와 같다. 자세한 내용은 블로그 글을 참고하자.
- 데이터를 분산해서 저장하니 어디어디에 어떻게 분산되서 저장되었는지 정보를 관리하는 metadata 가 있다.
- 하둡은 HDFS(Hadoop Distributed FileSystem) 이라고 부른다.
  - 데이터를 블록 단위로 나누어 저장한다. 
  - HDFS는 읽기 중심을 목적으로 만들어 졌기 때문에 파일의 수정은 지원하지 않는다. (읽는 속도를 높인다.)
- map-reduce : 분산된 데이터를 병렬로 처리하는 솔루션
  - map : 데이터가 어디 있는지 매핑. key, value 형태로 데이터가 어디 있는지, 몇 번인지 등으로 묶는다.
  - reduce : 각각의 분산된 데이터를 통합하여 읽음. 여러 개를 한 번에 읽는다는 관점에서 읽는 횟수를 줄였다라고 하여 reduce 라고 함.
- node
  - name node : 메타데이터, data node 관리 프로세스
  - data node : 파일을 저장하는 프로세스
- 블록 : Hadoop의 HDFS는 파일을 데이터 블록이라고 하는 작은 크기의 블록으로 나눈다. HDFS는 지정한 크기의 블록으로 나누어 지고 각각 독립적으로 저장된다. 지정한 크기보다 작은 파일은 실제 파일 크기의 블록으로 저장되고, 지정 크기보다 크다면 나눠서 저장된다. 따라서 파일의 모든 블록은 마지막 블록을 제외하고는 동일한 크기이다. HDFS 데이터 블록은 기본적으로 검색 및 네트워크 트래픽 비용을 줄여준다. 기본적으로는 128MB의 크기의 덩이리이며, 크기는 재설정 가능하다.
- spark : 일괄 처리, 반복적 실시간 처리, 그래프 변환 및 시각화 등과 같은 소모적인 프로세스 작업을 처리하는 플랫폼이다. 메모리 리소스를 소비하므로 최적화 측면에서 이전보다 빠르다. Spark는 실시간 데이터에 가장 적합한 반면, Hadoop은 구조화 된 데이터 또는 일괄 처리에 가장 적합하다. 따라서 대부분의 회사에서는 두가지다 변환해가면서 사용되어진다.
### <br/>

### HDFS 와 map-reduce 는 master 와 그 아래 child process 개념으로 slave 가 있다.
### HDFS 는 데이터를 저장 / map-reduce 는 데이터를 읽기
#### ![image](https://user-images.githubusercontent.com/62974484/225822511-db5ed846-76b3-4e76-b40b-b653746af40c.png)
### <br/><br/><br/>

--------------------------------------------------------------------------
# 이 섹션 내용은 날라갈 예정. 튜토리얼이 잘 안 맞음.
## hands-on
### 아래 사이트를 참고함
#### https://blog.acronym.co.kr/329   ->    별루임... 다른 사이트 참고하기
### <br/>

### `install`
### 아래에서 가장 최신 버전으로 다운로드하였다.
#### http://mirror.kakao.com/apache/hadoop/common/
```
$ wget http://mirror.kakao.com/apache/hadoop/common/hadoop-3.3.4/hadoop-3.3.4.tar.gz
$ tar zxf hadoop-3.3.4.tar.gz
```
#### ![image](https://user-images.githubusercontent.com/62974484/225825586-cfa48579-a4d6-43ab-a0e3-7329e70d47b7.png)
### <br/>

### hadoop-3.3.4/etc/hadoop 에 예제 파일들이 있다. 
### hadoop-3.3.4/ 에 conf 디렉터리 하나 만들자.
### `hadoop-env.sh`
```
cp etc/hadoop/hadoop-env.sh conf/
```
#### ![image](https://user-images.githubusercontent.com/62974484/225833909-bfdc831a-cf78-4e6a-a17b-c9187597872e.png)
### 수정하기
### 먼저 JAVA_HOME 과 hadoop home 경로를 등록해준다.
```
# JAVA_HOME 알아보기
$ echo $JAVA_HOME
```
#### ![image](https://user-images.githubusercontent.com/62974484/225834159-01a16e48-1e03-41d5-af10-86792f600760.png)
### conf/hadoop-env.sh 수정하기
#### ![image](https://user-images.githubusercontent.com/62974484/225834326-229112ba-f510-4848-b577-e5fb40a0bf5b.png)
### <br/>

### `core-site.xml`
#### /TBI/People/tbi/jhshin/hadoop/hadoop-3.3.4/jhshin 은 내가 hadoop 홈 디렉터리 안에 새로 만든 폴더이다.
```
<?xml version="1.0" encoding="UTF-8"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<!--
  Licensed under the Apache License, Version 2.0 (the "License");
  you may not use this file except in compliance with the License.
  You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License. See accompanying LICENSE file.
-->

<!-- Put site-specific property overrides in this file. -->

<configuration>
    <property>
        <name>fs.default.name</name>
        <value>hdfs://localhost:9000</value>
    </property>

    <property>
        <name>hadoop.tmp.dir</name>
        <value>/TBI/People/tbi/jhshin/hadoop/hadoop-3.3.4/jhshin</value>
    </property>
</configuration>
```
### <br/>

### `hdfs-site.xml`
### conf/ 폴더에 만들기
```
<?xml version="1.0" encoding="UTF-8"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<!--
  Licensed under the Apache License, Version 2.0 (the "License");
  you may not use this file except in compliance with the License.
  You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License. See accompanying LICENSE file.
-->

<!-- Put site-specific property overrides in this file. -->

<configuration>
    <property>
        <name>dfs.name.dir</name>
        <value>/TBI/People/tbi/jhshin/hadoop/hadoop-3.3.4/dfs/name</value>
    </property>

    <property>
        <name>dfs.name.edits.dir</name>
        <value>${dfs.name.dir}</value>
    </property>

    <property>
        <name>dfs.data.dir</name>
        <value>/TBI/People/tbi/jhshin/hadoop/hadoop-3.3.4/dfs/data</value>
    </property>
</configuration>
```
### <br/>

### `mapred-site.xml`
```
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<!--
  Licensed under the Apache License, Version 2.0 (the "License");
  you may not use this file except in compliance with the License.
  You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License. See accompanying LICENSE file.
-->

<!-- Put site-specific property overrides in this file. -->

<configuration>
    <property>
        <name>mapred.job.tracker</name>
        <property>localhost:9001</property>
    </property>

    <property>
        <name>mapred.local.dir</name>
        <value>${hadoop.tmp.dir}/mapred/local</value>
    </property>

    <property>
        <name>mapred.system.dir</name>
        <value>${hadoop.tmp.dir}/mapred/system</value>
    </property>
</configuration>
```
### <br/>

### `slaves` ?
### 잘 모르겠는데 conf/ 폴더에 slaves 파일 하나 만들고 hostname 인 husky 를 적어주고 저장하였다.
### <br/><br/><br/>


## ssh 
### hadoop 을 이용하기 위해서는 로그인 없이 접속할 수 있어야 한다.
### ~/.ssh 경로로 가기
#### /TBI/People/tbi/jhshin/.ssh
```
$ ssh-keygen -t rsa
$ cp id_rsa.pub authorized_keys
```
### 그리고 $ ssh localhost 를 써서 로그인 없이 잘 접속되는지 확인해보자.

----------------------------------------------------------------

# hands-on
### 아래 사이트를 참고함
#### https://www.tutorialspoint.com/hadoop/hadoop_quick_guide.htm
### <br/>

### `ssh`
### hadoop 을 이용하기 위해서는 로그인 없이 접속할 수 있어야 한다.
### ~/.ssh 경로로 가기
#### /TBI/People/tbi/jhshin/.ssh
```
$ ssh-keygen -t rsa
$ cp id_rsa.pub authorized_keys
$ chmod 0600 ~/.ssh/authorized_keys 
```
### 그리고 $ ssh localhost 를 써서 로그인 없이 잘 접속되는지 확인해보자.
### <br/>

### `java 설치 확인`
```
$ java -version
```
#### ![image](https://user-images.githubusercontent.com/62974484/225844422-ff5463aa-8a6f-4b03-aec0-dbefe8827173.png)
### <br/>

### `installing hadoop`
### 아래에서 가장 최신 버전으로 다운로드하였다.
#### http://mirror.kakao.com/apache/hadoop/common/
```
$ wget http://mirror.kakao.com/apache/hadoop/common/hadoop-3.3.4/hadoop-3.3.4.tar.gz
$ tar zxf hadoop-3.3.4.tar.gz
```
#### ![image](https://user-images.githubusercontent.com/62974484/225825586-cfa48579-a4d6-43ab-a0e3-7329e70d47b7.png)
### <br/>

### hadoop 은 mode 가 3 가지 있다고 한다.
### 나는 일단 테스트를 할 거니 default 인 Local/Standalone Mode 로 진행하자.
### 그리고 최종적으로는 fully distributed mode 로 가야 한다.
#### ![image](https://user-images.githubusercontent.com/62974484/225845319-ac9ef65d-cc13-4352-b26d-91c84adde454.png)
### <br/>

## Installing Hadoop in Standalone Mode
### ~/.bashrc 에 hadoop 경로 export
```
export HADOOP_HOME=/TBI/People/tbi/jhshin/hadoop/hadoop-3.3.4
# PATH 에 hadoop bin 경로를 붙여준다.
export PATH=/TBI/People/tbi/jhshin/hadoop/hadoop-3.3.4/bin:$PATH
```
### setting 확인
#### ![image](https://user-images.githubusercontent.com/62974484/226220076-5fa6f9ba-4961-485a-a274-751847b91d53.png)
### <br/>

### example
### mapreduce jar 파일을 실행 확인
```
hadoop jar /TBI/People/tbi/jhshin/hadoop/hadoop-3.3.4/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.4.jar
# or
hadoop jar $HADOOP_HOME/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.4.jar
```
#### ![image](https://user-images.githubusercontent.com/62974484/226220826-35187d95-ee3f-4f07-a80a-185a05759921.png)
### <br/>

### test
### test dir 를 하나 만들자
```
mkdir -p /TBI/People/tbi/jhshin/test/hadoop/input
cp $HADOOP_HOME/*.txt /TBI/People/tbi/jhshin/test/hadoop/input
```
### mapreduce 실행해보기
```
hadoop jar /TBI/People/tbi/jhshin/hadoop/hadoop-3.3.4/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.4.jar wordcount /TBI/People/tbi/jhshin/test/hadoop/input output
```
### output 폴더가 하나 생긴다.
#### ![image](https://user-images.githubusercontent.com/62974484/226221624-a0148569-9bd7-483b-b1f3-ed03a51274fe.png)
### 주로 part-r-00000 이라는 part 파일이 있는데, 여기에 대부분의 내용이 있다.
### 단어 개수를 세어주는 거라고 하는데... 개수가 안 맞는데..
#### 설명 : wordcount는 어떤 글에 등장하는 단어들의 개수를 세는 작업이구요, java 프로그래밍을 처음 배울때 "hello world"를 찍어보듯 맵리듀스를 처음 배울때 짜보는 프로그램 입니다.
#### ![image](https://user-images.githubusercontent.com/62974484/226222061-8ec19ed4-616a-4cfd-95cd-3d8aa2867a87.png)
### <br/><br/>

## Installing Hadoop in Pseudo Distributed Mode

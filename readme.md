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
```
source ~/.bashrc
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
### 다음을 ~/.bashrc 에 추가, 수정해주자. 위에서 PATH 에 \[hadoop_dir\]/bin 을 따로 지정해줬는데 기존 export PATH 에는 해당 bin 경로를 지워주고 아래와 같이 수정해준다.
```
# 원래 PATH 로 변경
export PATH=/TBI/People/tbi/jhshin/pipeline/hisat-genotype:/TBI/People/tbi/jhshin/pipeline/hisat-genotype/hisat2:$PATH

# 추가로 export
export HADOOP_HOME=/TBI/People/tbi/jhshin/hadoop/hadoop-3.3.4
export HADOOP_MAPRED_HOME=$HADOOP_HOME
export HADOOP_COMMON_HOME=$HADOOP_HOME
export HADOOP_HDFS_HOME=$HADOOP_HOME
export YARN_HOME=$HADOOP_HOME
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
export PATH=$HADOOP_HOME/sbin:$HADOOP_HOME/bin:$PATH
export HADOOP_INSTALL=$HADOOP_HOME
```
```
source ~/.bashrc
```
### <br/>

### `hadoop configuration`
### \[hadoop\]/etc/hadoop 경로에서 모든 config 파일을 확인할 수 있다.
```
cd $HADOOP_HOME/etc/hadoop
```
#### ![image](https://user-images.githubusercontent.com/62974484/226226805-8cd946ec-49dc-45b0-bfef-ba9def80911f.png)
### <br/>

### `hadoop-env.sh`
### JAVA_HOME 등록
```
$ echo $JAVA_HOME
/TBI/People/tbi/jhshin/miniconda3
```
### hadoop-env.sh 를 수정한다.
```
export JAVA_HOME=/TBI/People/tbi/jhshin/miniconda3
```
### <br/>

### `core-site.xml`
### The core-site.xml file contains information such as the port number used for Hadoop instance, memory allocated for the file system, memory limit for storing the data, and size of Read/Write buffers.
```
<configuration>
    <property>
        <name>fs.default.name</name>
        <value>hdfs://localhost:9000</value>
    </property>
</configuration>
```
### <br/>

### `hdfs-site.xml`
### namenode, datanode 저장 공간 정보
### The hdfs-site.xml file contains information such as the value of replication data, namenode path, and datanode paths of your local file systems. It means the place where you want to store the Hadoop infrastructure.
### 이 경로로 똑같이 설정해도 되는지 모르겠다.
```
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>

    <property>
        <name>dfs.name.dir</name>
        <value>file:///home/hadoop/hadoopinfra/hdfs/namenode </value>
    </property>

    <property>
        <name>dfs.data.dir</name>
        <value>file:///home/hadoop/hadoopinfra/hdfs/datanode </value>
    </property>
</configuration>
```
### 나는 경로를 만들고나서 이렇게 수정하였다.
```
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>

    <property>
        <name>dfs.name.dir</name>
        <value>file:///data08/project/jhshin/hadoop-3.3.4/data/hdfs/namenode </value>
    </property>

    <property>
        <name>dfs.data.dir</name>
        <value>file:///data08/project/jhshin/hadoop-3.3.4/data/hdfs/datanode </value>
    </property>
</configuration>
```
### <br/>

### `yarn-site.xml`
### user specific tag 를 정의한다.
### yarn 이란? 의존성관리 javascript 패키지 매니저
```
<configuration>
<!-- Site specific YARN configuration properties -->
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
</configuration>
```
### <br/>

### `mapred-site.xml`
### This file is used to specify which MapReduce framework we are using. By default, Hadoop contains a template of yarn-site.xml. First of all, it is required to copy the file from mapred-site.xml.template to mapred-site.xml file using the following command.
```
<configuration>
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
</configuration>
```
### <br/><br/>

## Verifying Hadoop Installation
### 아래 명령어를 입력하면 hadoop file system 이 실행된다.
```
hdfs namenode -format
```
### 된건가...
#### ![image](https://user-images.githubusercontent.com/62974484/226232393-18fda4cd-f52a-41e9-848e-6beb452d496f.png)
#### ...
#### ![image](https://user-images.githubusercontent.com/62974484/226232428-35dafb83-2ddb-4de8-b0dc-174bc5f7a14f.png)
### <br/>

### `Verifying Hadoop dfs`
```
start-dfs.sh 
```
#### ![image](https://user-images.githubusercontent.com/62974484/226232762-bb8dc5c6-bf56-49d2-bed1-7914a8b4cd30.png)
### <br/>

### `Verifying Yarn Script`
```
start-yarn.sh 
```
#### ![image](https://user-images.githubusercontent.com/62974484/226232745-faeeb210-728a-4f38-9240-023617b4919d.png)
### <br/>

### hadoop process 가 실행이 되긴 했다.
#### ![image](https://user-images.githubusercontent.com/62974484/226232934-29f1c551-0867-4c22-a3be-2729802dd7e9.png)
### ping 으로 서버 실행 확인
```
# Hadoop on Browser
ping localhost -p 50070
# All Applications for Cluster
ping localhost -p 8088
```
#### ![image](https://user-images.githubusercontent.com/62974484/226245111-d149272b-7978-488b-a86f-80ae6a4c52a4.png)
#### ![image](https://user-images.githubusercontent.com/62974484/226245420-4987be84-bf18-42c9-81ec-a16c8c434e2d.png)

### 웹에서 접속하면 이렇게 뜬다고 하는데 나는 root 권한이 없어서 포트 개방을 못 하니... 안 되면 스킵
#### ![image](https://user-images.githubusercontent.com/62974484/226495271-e516c7c3-e4e1-4555-bfff-e931473189fa.png)
#### ![image](https://user-images.githubusercontent.com/62974484/226495289-6addd3b7-eaa3-4ee5-bdfe-d020066dff74.png)
### <br/>

### 8088 웹서버는 실행이 되는 것을 확인하였다.
### 50070 은 접속이 안 된다.
#### ![image](https://github.com/Shin-jongwhan/hadoop/assets/62974484/083400ec-5274-4c15-9946-99d4dc1a4194)

### <br/><br/><br/>

## Hadoop - HDFS Overview
### namenode, datanode, block 으로 이루어져 있다.
### namenode 에서 파일이 어떻게 분리되서 저장되어 있는지에 대한 metadata 정보를 저장한다. client 는 namenode 에 저장된 정보를 토대로 datanode 를 읽는다.
### datanode 는 파일이 저장된 node 이다. read / write 를 실제로 컨트롤해주는 것이 이 datanode 에서 이루어진다.
### block 은 나눠진 파일이다.
### read / write system failure 에 대비하기 위해 replication 을 만들어둔다.
#### ![image](https://user-images.githubusercontent.com/62974484/226495710-cd764a59-3298-4dab-b138-324029bb1407.png)
#### ![image](https://user-images.githubusercontent.com/62974484/226496155-2a9df8aa-a815-439d-ad19-cfc0fcd9dbb5.png)
### `Goals of HDFS`
#### ![image](https://user-images.githubusercontent.com/62974484/226496194-60c6a180-f699-4f99-8fe7-30efefdb3b30.png)


